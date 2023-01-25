<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <NuxtLink
          v-if="lesson.sourceUrl"
          class="font-normal text-md text-gray-500"
          :to="lesson.sourceUrl"
      >
        Download Source Code
      </NuxtLink>
      <NuxtLink
          v-if="lesson.downloadUrl"
          class="font-normal text-md text-gray-500"
          :to="lesson.downloadUrl"
      >
        Download Video
      </NuxtLink>
    </div>
    <VideoPlayer
        v-if="lesson.videoId"
        :videoId="lesson.videoId"
    />
    <p>{{ lesson.text }}</p>
<!-- doesnt work v-modal anymore bcoz progress is too complex now  <LessonCompleteButton v-model="progress"/>-->
    <LessonCompleteButton
    :modelValue="isLessonComplete"
    @update:modelValue="toggleComplete"
    />
  </div>
</template>

<script setup>
const course = useCourse();
const route = useRoute();

const chapter = computed(() => {
  const {chapterSlug} = route.params;
  return course.chapters.find(
      (chapter) => chapter.slug === chapterSlug
  );
});

const lesson = computed(() => {
  return chapter.value.lessons.find(
      (lesson) => lesson.slug === route.params.lessonSlug
  );
});

const title = computed(() => {
  // return `${lesson.value.title} - ${chapter.value.title} - ${course.title}`;
  return `${lesson.value.title} - ${course.title}`;
});
useHead({
  title,
})
//change into useLocalStorage
// const progress = useState('progress', () => {
//  return [];
// });
const progress = useLocalStorage('progress', []);



// some couple of Guards before get the right value
// if chapter dosent exist
const isLessonComplete = computed(() => {
  if (!progress.value[chapter.value.number-1]) {
    return false;
  }
  // if lesson dosent exist
  // if (!progress.value[chapter.value.number-1].lessons[lesson.value.number-1]) {
  //   return false;
  // }

  // if lesson dosent exist , same as above
  if (!progress.value[chapter.value.number-1][lesson.value.number-1]) {
    return false;
  }
  // if value exists than return the value
  return progress.value[chapter.value.number-1][lesson.value.number-1];
});

// Similar Guards

 const toggleComplete = () => {
   //if chapter exists or not we create chapter array
   if (!progress.value[chapter.value.number-1]) {
     progress.value[chapter.value.number-1] = [];
   }
   // when chapter array exists then we set the value
   progress.value[chapter.value.number-1][lesson.value.number-1] = !isLessonComplete.value;
 };

</script>
