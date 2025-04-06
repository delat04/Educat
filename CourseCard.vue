<template>
  <div class="hero" :style="{ backgroundColor: course.color }">
    <img class="hero-profile-img" :src="course.image" :alt="`${course.subject} banner`" />

    <!-- Background overlay with course color theme -->
    <div class="hero-description-bk" :style="{ backgroundImage: `linear-gradient(0deg, ${course.color}, #fc466b)` }"></div>

    <!-- Course logo in the lower left corner -->
    <div class="hero-logo">
      <img :src="course.logo" :alt="`${course.subject} logo`" />
    </div>

    <!-- Course description with subject, enrolled count, and completion status -->
    <div class="hero-description">
      <p>{{ course.subject }} - {{ course.enrolled }} lessons</p>
    </div>
    <div class="hero-date">
      <p>Start Date: {{ course.startDate }}</p>
    </div>

    <!-- Progress bar for course completion -->
    <div class="course-progress-container">
      <div class="course-progress-bar" :style="{ width: `${course.progress}%` }"></div>
    </div>
    <p class="progress-label">{{ course.progress }}% complete</p>

    <!-- Button to view more details -->
    <div class="hero-btn">
      <a :href="course.link" target="_blank">Learn More</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "CourseCard",
  props: {
    course: {
      type: Object,
      required: true,
      default: () => ({
        subject: "Math",
        color: "#2bc27a",
        logo: "https://cdn-icons-png.flaticon.com/512/3426/3426679.png",
        image: "https://i.pinimg.com/736x/51/7e/89/517e890cd1d9606b78ccbdd08f0fdf1d.jpg",
        enrolled: 0,
        startDate: "N/A",
        progress: 0,
        link: "#"
      })
    }
  }
};
</script>

<style scoped>
/* BEGIN CARD DESIGN */
.hero {
  display: inline-block;
  position: relative;
  width: 400px;
  min-width: 400px;
  height: 400px;
  border-radius: 30px;
  overflow:hidden;
  box-shadow: 5px 5px 30px rgba(0, 0, 0, 0.3);
  margin: 30px;
}

.hero-profile-img {
  height: 70%;
}

.hero-description-bk {
  background-image: linear-gradient(0deg , #3f5efb, #fc466b);
  border-radius: 30px;
  position: absolute;
  top: 55%;
  left: -5px;
  height: 65%;
  width: 108%;
  transform: skew(19deg, -9deg);
}

.second .hero-description-bk {
  background-image: linear-gradient(-20deg , #bb7413, #e7d25c)
}

.hero-logo {
  height: 80px;
  width: 80px;
  border-radius: 20px;
  background-color: #fff;
  position: absolute;
  bottom: 30%;
  left: 30px;
  overflow:hidden;
  box-shadow: 5px 5px 30px rgba(0, 0, 0, 0.7);
}

.hero-logo img {
  height: 100%;
}

.hero-description {
  position: absolute;
  color: #fff;
  font-weight: 900;
  left: 150px;
  bottom: 26%;
}

.hero-btn {
  position: absolute;
  color: #fff;
  right: 30px;
  bottom: 10%;
  padding: 10px 20px;
  border: 1px solid #fff;
}

.hero-btn a {
  color: #fff;
}

.hero-date {
  position: absolute;
  color: #fff;
  left: 30px;
  bottom: 10%;
}
/* END CARD DESIGN */

body {
  font-family: 'Open Sans', sans-serif;
  margin: 0;
  background-color: #eee;
  min-height: 100vh;
}
.btn i:before {
  width: 14px;
  height: 14px;
  position: fixed;
  color: #fff;
  background: #0077B5;
  padding: 10px;
  border-radius: 50%;
  top:5px;
  right:5px;
}
</style>
