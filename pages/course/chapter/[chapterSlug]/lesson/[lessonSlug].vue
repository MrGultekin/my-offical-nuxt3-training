<template>
  <h2>Lesson</h2>
  <p>This is a lesson</p>
<!--  <p>Chapter slug: {{ $route.params.chapterSlug }}</p>-->
<!--  <p>Lesson slug: {{ $route.params.lessonSlug }}</p>-->
  <p>{{ chapter.title }}</p>
  <p>{{ lesson.title }}</p>
</template>

<script setup>
const course = useCourse()
const route = useRoute() // This is the same as $route

const chapter = computed(() =>{
  return course.chapters.find(chapter => chapter.slug === route.params.chapterSlug)
})

const lesson = computed(() => {
  const {lessonSlug} = route.params;
  return chapter.value.lessons.find(lesson => lesson.slug === lessonSlug)
})
</script>