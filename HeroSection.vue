<template>
  <div class="relative w-full h-screen bg-slate-900 flex items-center justify-center p-4 overflow-hidden">
    <!-- Center Card Container - Acts as origin point for calculations -->
    <div ref="cardContainer" class="relative w-80">
      <!-- Connection Points Overlay -->
      <div class="absolute -inset-2">
        <div v-for="(point, index) in connectionPoints" :key="'point-'+index"
             :style="{
            position: 'absolute',
            left: `${point.x}%`,
            top: `${point.y}%`,
            transform: 'translate(-50%, -50%)'
          }"
             class="w-3 h-3">
          <div class="relative">
            <div class="absolute inset-0 bg-emerald-400/30 rounded-full animate-ping"></div>
            <div class="relative w-full h-full bg-emerald-400 rounded-full border border-emerald-300"></div>
          </div>
        </div>
      </div>

      <!-- Main Card -->
      <div class="bg-slate-800 rounded-lg border border-slate-700 shadow-xl">
        <!-- Header with Network Stats -->
        <div class="p-4 border-b border-slate-700">
          <div class="flex items-center justify-between mb-3">
            <h3 class="text-lg font-semibold text-slate-200">Network Hub</h3>
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 bg-emerald-400 rounded-full animate-pulse"></span>
              <span class="text-sm text-slate-400">Active</span>
            </div>
          </div>
          <!-- Network Stats -->
          <div class="grid grid-cols-3 gap-2">
            <div v-for="(stat, index) in networkStats" :key="index"
                 class="bg-slate-700/50 rounded-lg p-2 text-center">
              <div class="text-emerald-400 text-sm font-mono">{{ stat.value }}</div>
              <div class="text-xs text-slate-400">{{ stat.label }}</div>
            </div>
          </div>
        </div>

        <!-- Activity Log -->
        <div class="h-48 overflow-y-auto custom-scrollbar p-4 space-y-2">
          <div v-for="(log, index) in logs" :key="index"
               :class="['text-sm font-mono flex items-start gap-2 transition-all duration-300',
              log.new ? 'text-emerald-400' : 'text-slate-300']">
            <span class="text-slate-500 text-xs">{{ log.time }}</span>
            <span>{{ log.message }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- SVG Connection Layer -->
    <svg class="absolute inset-0 w-full h-full" style="pointer-events: none;">
      <defs>
        <linearGradient id="lineGradient" gradientUnits="userSpaceOnUse">
          <stop offset="0%" stop-color="#10b981" stop-opacity="0.1"/>
          <stop offset="50%" stop-color="#10b981" stop-opacity="0.5"/>
          <stop offset="100%" stop-color="#10b981" stop-opacity="0.1"/>
        </linearGradient>

        <!-- Glowing Filter -->
        <filter id="glow">
          <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
          <feMerge>
            <feMergeNode in="coloredBlur"/>
            <feMergeNode in="SourceGraphic"/>
          </feMerge>
        </filter>
      </defs>

      <!-- Connection Lines -->
      <g v-for="(node, index) in nodes" :key="'node-'+index">
        <path :d="calculatePath(node)"
              class="connection-line"
              stroke="url(#lineGradient)"
              stroke-width="2"
              fill="none"
              filter="url(#glow)"/>

        <!-- Data Flow Animation -->
        <circle v-for="packet in node.packets" :key="packet.id"
                r="3"
                class="data-packet">
          <animateMotion
              :dur="`${packet.duration}s`"
              :path="calculatePath(node)"
              fill="freeze"/>
        </circle>
      </g>
    </svg>

    <!-- Network Nodes -->
    <template v-for="(node, index) in nodes" :key="'node-'+index">
      <div class="absolute transform -translate-x-1/2 -translate-y-1/2 transition-all duration-300"
           :style="{
          left: `${node.x}%`,
          top: `${node.y}%`
        }">
        <div class="relative group">
          <!-- Node Container -->
          <div class="bg-slate-800 p-3 rounded-lg border border-slate-700 shadow-lg
            transition-transform duration-300 hover:scale-110">
            <div class="w-12 h-12 rounded-lg bg-slate-700/50 flex items-center justify-center">
              <svg class="w-6 h-6 text-emerald-400" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                      d="M12 6v6m0 0v6m0-6h6m-6 0H6"/>
              </svg>
            </div>
            <!-- Status Indicator -->
            <div class="absolute -right-1 -bottom-1 w-3 h-3 bg-emerald-400 rounded-full
              border-2 border-slate-800 animate-pulse"/>
          </div>
          <!-- Tooltip -->
          <div class="absolute left-full ml-3 top-1/2 -translate-y-1/2 px-2 py-1
            bg-slate-800 rounded text-xs text-slate-300 whitespace-nowrap opacity-0
            group-hover:opacity-100 transition-opacity">
            Node {{ index + 1 }} | {{ node.status }}
          </div>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  data() {
    return {
      connectionPoints: [
        { x: 0, y: 50 },    // Left
        { x: 100, y: 50 },  // Right
        { x: 50, y: 0 },    // Top
        { x: 50, y: 100 },  // Bottom
        { x: 15, y: 15 },   // Top Left
        { x: 85, y: 15 },   // Top Right
        { x: 15, y: 85 },   // Bottom Left
        { x: 85, y: 85 }    // Bottom Right
      ],
      nodes: [
        { x: 20, y: 20, status: 'Active', packets: [] },
        { x: 80, y: 20, status: 'Active', packets: [] },
        { x: 20, y: 80, status: 'Active', packets: [] },
        { x: 80, y: 80, status: 'Active', packets: [] },
        { x: 50, y: 10, status: 'Active', packets: [] },
        { x: 50, y: 90, status: 'Active', packets: [] }
      ],
      logs: [],
      networkStats: [
        { label: 'Connections', value: '6/8' },
        { label: 'Latency', value: '12ms' },
        { label: 'Bandwidth', value: '8.5MB/s' }
      ]
    }
  },
  methods: {
    calculatePath(node) {
      // Find closest connection point
      const closestPoint = this.findClosestPoint(node)

      // Calculate control points for smooth curve
      const dx = closestPoint.x - node.x
      const dy = closestPoint.y - node.y
      const midX = (node.x + closestPoint.x) / 2
      const midY = (node.y + closestPoint.y) / 2

      // Create smooth curve using cubic bezier
      return `M ${node.x},${node.y}
              C ${midX},${node.y}
                ${midX},${midY}
                ${closestPoint.x},${closestPoint.y}`
    },
    findClosestPoint(node) {
      return this.connectionPoints.reduce((closest, point) => {
        const distance = Math.hypot(point.x - node.x, point.y - node.y)
        return distance < closest.distance ? { ...point, distance } : closest
      }, { distance: Infinity }).x ?
          this.connectionPoints.reduce((closest, point) => {
            const distance = Math.hypot(point.x - node.x, point.y - node.y)
            return distance < closest.distance ? { ...point, distance } : closest
          }, { distance: Infinity }) :
          this.connectionPoints[0]
    },
    startNetworkSimulation() {
      setInterval(() => {
        const nodeIndex = Math.floor(Math.random() * this.nodes.length)
        const duration = 1 + Math.random()

        // Add packet
        this.nodes[nodeIndex].packets.push({
          id: Date.now(),
          duration
        })

        // Add log
        this.logs.unshift({
          time: new Date().toLocaleTimeString(),
          message: `Data transfer: Node ${nodeIndex + 1}`,
          new: true
        })

        // Cleanup
        if (this.logs.length > 8) this.logs.pop()
        setTimeout(() => {
          this.nodes[nodeIndex].packets.shift()
        }, duration * 1000)

        // Update stats randomly
        this.networkStats[1].value = `${10 + Math.floor(Math.random() * 5)}ms`
        this.networkStats[2].value = `${(8 + Math.random()).toFixed(1)}MB/s`
      }, 2000)
    }
  },
  mounted() {
    this.startNetworkSimulation()
  }
}
</script>

<style >
.connection-line {
  transition: all 0.3s ease;
}

.data-packet {
  fill: #10b981;
  filter: drop-shadow(0 0 4px #10b981);
}

.custom-scrollbar {
  scrollbar-width: thin;
  scrollbar-color: #475569 #1e293b;
}

.custom-scrollbar::-webkit-scrollbar {
  width: 4px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  background: #1e293b;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: #475569;
  border-radius: 4px;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>