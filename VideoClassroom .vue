<template>
  <div class="video-classroom bg-slate-100 p-6">
    <!-- Teacher View -->
    <div v-if="isTeacher" class="teacher-view">
      <!-- Teacher's camera preview -->
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <div class="col-span-3">
          <div class="relative bg-slate-800 rounded-lg overflow-hidden aspect-video">
            <video
                ref="localVideo"
                autoplay
                playsinline
                muted
                class="w-full h-full object-cover"
            ></video>
            <div class="absolute bottom-4 left-4 bg-slate-900/75 px-3 py-1 rounded-full text-white">
              You (Teacher)
            </div>
          </div>
        </div>

        <!-- Connected students list -->
        <div class="col-span-1">
          <div class="bg-white rounded-lg p-4 h-full">
            <h3 class="text-lg font-semibold mb-4">Connected Students</h3>
            <ul class="space-y-2">
              <li
                  v-for="student in connectedStudents"
                  :key="student.id"
                  class="flex items-center space-x-2 p-2 bg-slate-50 rounded"
              >
                <div class="w-2 h-2 rounded-full bg-green-500"></div>
                <span>{{ student.name }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <!-- Student View -->
    <div v-else class="student-view">
      <div class="max-w-4xl mx-auto">
        <div class="relative bg-slate-800 rounded-lg overflow-hidden aspect-video">
          <video
              ref="teacherVideo"
              autoplay
              playsinline
              class="w-full h-full object-cover"
          ></video>
          <div class="absolute bottom-4 left-4 bg-slate-900/75 px-3 py-1 rounded-full text-white">
            Teacher
          </div>
        </div>
      </div>
    </div>

    <!-- Controls -->
    <div class="mt-6 flex justify-center space-x-4">
      <button
          @click="toggleVideo"
          class="px-4 py-2 rounded-full flex items-center space-x-2"
          :class="isVideoOn ? 'bg-slate-200' : 'bg-red-500 text-white'"
      >
        <span v-if="isVideoOn">
          <Camera class="w-5 h-5" />
        </span>
        <span v-else>
          <CameraOff class="w-5 h-5" />
        </span>
      </button>

      <button
          @click="toggleAudio"
          class="px-4 py-2 rounded-full flex items-center space-x-2"
          :class="isAudioOn ? 'bg-slate-200' : 'bg-red-500 text-white'"
      >
        <span v-if="isAudioOn">
          <Mic class="w-5 h-5" />
        </span>
        <span v-else>
          <MicOff class="w-5 h-5" />
        </span>
      </button>

      <button
          @click="endCall"
          class="px-4 py-2 rounded-full bg-red-500 text-white flex items-center space-x-2"
      >
        <PhoneOff class="w-5 h-5" />
        <span>End</span>
      </button>
    </div>
  </div>
</template>

<script>
import { Camera, CameraOff, Mic, MicOff, PhoneOff } from 'lucide-vue-next'
import { ref, onMounted, onUnmounted } from 'vue'

export default {
  name: 'VideoClassroom',

  components: {
    Camera,
    CameraOff,
    Mic,
    MicOff,
    PhoneOff
  },

  props: {
    isTeacher: {
      type: Boolean,
      default: false
    },
    roomId: {
      type: String,
      required: true
    }
  },

  setup(props) {
    const localVideo = ref(null)
    const teacherVideo = ref(null)
    const localStream = ref(null)
    const peerConnections = ref({})
    const isVideoOn = ref(true)
    const isAudioOn = ref(true)
    const connectedStudents = ref([])
    const socket = ref(null)

    const configuration = {
      iceServers: [
        { urls: 'stun:stun.l.google.com:19302' }
      ]
    }

    // Initialize WebSocket connection
    const initializeSocket = () => {
      socket.value = new WebSocket('YOUR_WEBSOCKET_SERVER_URL')

      socket.value.onmessage = async (event) => {
        const data = JSON.parse(event.data)

        switch (data.type) {
          case 'student-joined':
            if (props.isTeacher) {
              const peerConnection = new RTCPeerConnection(configuration)
              peerConnections.value[data.studentId] = peerConnection

              // Add local stream to peer connection
              localStream.value.getTracks().forEach(track => {
                peerConnection.addTrack(track, localStream.value)
              })

              // Create and send offer
              const offer = await peerConnection.createOffer()
              await peerConnection.setLocalDescription(offer)

              socket.value.send(JSON.stringify({
                type: 'offer',
                offer: offer,
                studentId: data.studentId
              }))

              // Add student to connected list
              connectedStudents.value.push({
                id: data.studentId,
                name: data.studentName
              })
            }
            break

          case 'offer':
            if (!props.isTeacher) {
              const peerConnection = new RTCPeerConnection(configuration)
              peerConnections.value['teacher'] = peerConnection

              peerConnection.ontrack = (event) => {
                if (teacherVideo.value) {
                  teacherVideo.value.srcObject = event.streams[0]
                }
              }

              await peerConnection.setRemoteDescription(new RTCSessionDescription(data.offer))
              const answer = await peerConnection.createAnswer()
              await peerConnection.setLocalDescription(answer)

              socket.value.send(JSON.stringify({
                type: 'answer',
                answer: answer
              }))
            }
            break

          case 'answer':
            if (props.isTeacher) {
              const peerConnection = peerConnections.value[data.studentId]
              await peerConnection.setRemoteDescription(new RTCSessionDescription(data.answer))
            }
            break

          case 'student-left':
            if (props.isTeacher) {
              // Remove peer connection
              if (peerConnections.value[data.studentId]) {
                peerConnections.value[data.studentId].close()
                delete peerConnections.value[data.studentId]
              }

              // Remove from connected students list
              connectedStudents.value = connectedStudents.value.filter(
                  student => student.id !== data.studentId
              )
            }
            break
        }
      }
    }

    // Initialize media stream
    const initializeStream = async () => {
      try {
        localStream.value = await navigator.mediaDevices.getUserMedia({
          video: true,
          audio: true
        })

        if (props.isTeacher && localVideo.value) {
          localVideo.value.srcObject = localStream.value
        }
      } catch (error) {
        console.error('Error accessing media devices:', error)
      }
    }

    // Toggle video
    const toggleVideo = () => {
      if (localStream.value) {
        const videoTrack = localStream.value.getVideoTracks()[0]
        videoTrack.enabled = !videoTrack.enabled
        isVideoOn.value = videoTrack.enabled
      }
    }

    // Toggle audio
    const toggleAudio = () => {
      if (localStream.value) {
        const audioTrack = localStream.value.getAudioTracks()[0]
        audioTrack.enabled = !audioTrack.enabled
        isAudioOn.value = audioTrack.enabled
      }
    }

    // End call
    const endCall = () => {
      if (localStream.value) {
        localStream.value.getTracks().forEach(track => track.stop())
      }

      Object.values(peerConnections.value).forEach(connection => {
        connection.close()
      })

      peerConnections.value = {}

      if (socket.value) {
        socket.value.close()
      }

      // Emit event to parent component
      emit('call-ended')
    }

    // Lifecycle hooks
    onMounted(async () => {
      await initializeStream()
      initializeSocket()
    })

    onUnmounted(() => {
      endCall()
    })

    return {
      localVideo,
      teacherVideo,
      isVideoOn,
      isAudioOn,
      connectedStudents,
      toggleVideo,
      toggleAudio,
      endCall
    }
  }
}
</script>