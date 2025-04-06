<template>
  <div class="bg-gray-100 p-6 min-h-screen font-roboto">
    <div class=" mx-auto p-6 rounded-lg shadow-md" style="font-family: 'Poppins',serif;width: 932px; margin-top: 122px; background: white;">
      <h1 class="text-2xl font-bold mb-2">Offered Subjects</h1>
      <p class="text-gray-600 mb-6">
        <span class="text-blue-600 font-bold">35 hours</span> totally spent
      </p>

      <!-- Scrollable container with fixed height -->
      <div class="overflow-y-auto max-h-[990px] pr-4" style="scrollbar-width: thin;">
        <!-- Grid container with gap between items -->
        <div class="grid grid-cols-1 gap-6">
          <SubjectCard
              v-for="(subject, index) in subjects"
              :key="index"
              :subjectName="subject.subjectName"
              :description="subject.description"
              :iconClass="subject.iconClass"
              :iconBgColor="subject.iconBgColor"
              :progressColor="subject.progressColor"
              :progress="subject.progress"
              :timeSpent="subject.timeSpent"
              :difficultyLevel="subject.difficultyLevel"
              :currentGrade="subject.currentGrade"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SubjectCard from './SubjectCard.vue';

export default {
  name: "Subjects",
  components: {
    SubjectCard,
  },
  data() {
    return {
      subjects: [
        {
          subjectName: "Mathematics",
          description: "Advanced calculus and algebra",
          iconClass: "https://cdn-icons-png.flaticon.com/512/43/43102.png",
          iconBgColor: "blue-500",
          progressColor: "blue-600",
          progress: 36,
          timeSpent: "26 hours 26 min",
          difficultyLevel: 4,
          currentGrade: "B"
        },
        {
          subjectName: "Physics",
          description: "Classical mechanics and thermodynamics",
          iconClass: "https://cdn-icons-png.flaticon.com/512/15190/15190364.png",
          iconBgColor: "purple-500",
          progressColor: "violet-600",
          progress: 82,
          timeSpent: "38 hours 15 min",
          difficultyLevel: 3,
          currentGrade: "B+"
        },
        {
          subjectName: "Chemistry",
          description: "Organic chemistry and chemical reactions",
          iconClass: "https://cdn-icons-png.flaticon.com/512/620/620401.png",
          iconBgColor: "green-600",
          progressColor: "green-600",
          progress: 42,
          timeSpent: "22 hours 10 min",
          difficultyLevel: 4,
          currentGrade: "B-"
        },
        {
          subjectName: "Science Philosophy",
          description: "Philosophical foundations of science",
          iconClass: "https://cdn-icons-png.flaticon.com/512/9922/9922644.png",
          iconBgColor: "cyan-600",
          progressColor: "cyan-600",
          progress: 65,
          timeSpent: "30 hours 15 min",
          difficultyLevel: 2,
          currentGrade: "A"
        },
        {
          subjectName: "French",
          description: "Advanced vocabulary and grammar in French",
          iconClass: "https://cdn-icons-png.flaticon.com/512/4937/4937337.png",
          iconBgColor: "orange-600",
          progressColor: "orange-600",
          progress: 78,
          timeSpent: "50 hours 40 min",
          difficultyLevel: 3,
          currentGrade: "A-"
        },
        {
          subjectName: "English",
          description: "English literature and advanced writing skills",
          iconClass: "https://cdn-icons-png.flaticon.com/512/15190/15190364.png",
          iconBgColor: "rose-600",
          progressColor: "rose-600",
          progress: 92,
          timeSpent: "120 hours 5 min",
          difficultyLevel: 2,
          currentGrade: "A+"
        },
        {
          subjectName: "Arabic",
          description: "Intermediate Arabic language skills",
          iconClass: "https://cdn-icons-png.flaticon.com/512/11145/11145207.png",
          iconBgColor: "violet-600",
          progressColor: "violet-600",
          progress: 33,
          timeSpent: "15 hours 30 min",
          difficultyLevel: 3,
          currentGrade: "C+"
        },
        {
          subjectName: "Technology",
          description: "Programming and software development",
          iconClass: "https://cdn-icons-png.flaticon.com/512/3247/3247956.png",
          iconBgColor: "teal-600",
          progressColor: "teal-600",
          progress: 58,
          timeSpent: "42 hours 20 min",
          difficultyLevel: 4,
          currentGrade: "B+"
        },
        {
          subjectName: "History",
          description: "Major events and figures in world history",
          iconClass: "https://cdn-icons-png.flaticon.com/512/1183/1183858.png",
          iconBgColor: "orange-600",
          progressColor: "orange-600",
          progress: 81,
          timeSpent: "78 hours 55 min",
          difficultyLevel: 2,
          currentGrade: "A-"
        }
      ]
    };
  }
};
</script>

<style>


@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

.font-roboto {
font-family: 'Roboto', sans-serif;
}

//* Custom scrollbar styles */
::-webkit-scrollbar {
width: 12px;
height: 12px;
}

::-webkit-scrollbar-track {
background: #f8f9fa;
border: 1px solid #e9ecef;
border-radius: 8px;
box-shadow: inset 0 0 4px rgba(0, 0, 0, 0.1);
}

::-webkit-scrollbar-thumb {
background: linear-gradient(45deg, #2c3e50, #3498db);
border: 2px solid #f8f9fa;
border-radius: 8px;
transition: all 0.3s ease;
}

::-webkit-scrollbar-thumb:hover {
background: linear-gradient(45deg, #1a252f, #2980b9);
box-shadow: 0 0 8px rgba(52, 152, 219, 0.5);
}

::-webkit-scrollbar-corner {
background: #f8f9fa;
}

/* Scrollbar Button Styles */
::-webkit-scrollbar-button:single-button {
background-color: #f8f9fa;
display: block;
border-style: solid;
height: 13px;
width: 12px;
}

/* Up */
::-webkit-scrollbar-button:single-button:vertical:decrement {
border-width: 0 6px 8px 6px;
border-color: transparent transparent #2c3e50 transparent;
}

::-webkit-scrollbar-button:single-button:vertical:decrement:hover {
border-color: transparent transparent #1a252f transparent;
}

/* Down */
::-webkit-scrollbar-button:single-button:vertical:increment {
border-width: 8px 6px 0 6px;
border-color: #2c3e50 transparent transparent transparent;
}

::-webkit-scrollbar-button:single-button:vertical:increment:hover {
border-color: #1a252f transparent transparent transparent;
}

/* Firefox scrollbar - needs to be more specific */
* {
scrollbar-width: thin;
scrollbar-color: #3498db #f8f9fa;
}

/* Additional smooth scroll behavior */
.overflow-y-auto {
scroll-behavior: smooth;
-webkit-overflow-scrolling: touch; /* For smooth scrolling on iOS */
}

/* Optional: Add a fancy hover effect to the scrollable container */
.overflow-y-auto:hover::-webkit-scrollbar-thumb {
background: linear-gradient(45deg, #2c3e50, #3498db);
box-shadow: 0 0 12px rgba(52, 152, 219, 0.3);
}

/* Optional: Add subtle transition when scrolling */
.overflow-y-auto {
transition: all 0.3s ease;
}

/* Optional: Custom selection color for academic feel */
::selection {
background: rgba(92, 169, 220, 0.3);
color: #2c3e50;
}/* Keep the same style as before */
</style>