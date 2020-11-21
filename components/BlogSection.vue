<template>
  <main>
    <div class="relative bg-gray-50 pt-16 pb-20 px-4 sm:px-6 lg:pt-24 lg:pb-28 lg:px-8">
      <div class="absolute inset-0">
        <div class="bg-background-secondary h-1/3 sm:h-2/3" />
      </div>
      <div class="relative max-w-7xl mx-auto">
        <div class="text-center">
          <h2 class="text-3xl text-copy-primary leading-9 tracking-tight font-extrabold text-white-900 sm:text-4xl sm:leading-10">
            From my blog
          </h2>
          <p class="mt-3 max-w-2xl mx-auto text-xl leading-7 text-gray-500 sm:mt-4">
            This is a place for me to write and explore ideas, tools, process, etc.
          </p>
        </div>
        <div class="flex items-strech mt-12 grid gap-5 max-w-xlg mx-auto lg:grid-cols-2 lg:max-w-none">
          <BlogSectionCard
            v-for="post in sortedPostsNotCurrentView"
            :key="post.fields.slug"
            :title="post.fields.title"
            :slug="post.fields.slug"
            :description="post.fields.description"
            :author="post.fields.author.fields.name"
            :heroImageUrl="post.fields.heroImage.fields.file.url"
            :publishDate="post.fields.publishDate"
            :body="post.fields.body"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import BlogSectionCard from '@/components/BlogSectionCard.vue';

function dateMostRecentSortFunction(postA, postB) {
  var dateA = new Date(postA.fields.publishDate).getTime();
  var dateB = new Date(postB.fields.publishDate).getTime();
  return dateA < dateB ? 1 : -1;
}

export default {
  components: {
    BlogSectionCard,
  },
 computed: {
   posts() {
     return this.$store.state.posts;
   },
   sortedPosts() {
     var unSortedPosts = this.$store.state.posts;
     return unSortedPosts.slice().sort(dateMostRecentSortFunction);
   },
   sortedPostsNotCurrentView(currentPost="") {
     const array = this.sortedPosts;
     const index = array.indexOf(currentPost);
     if (index > -1) {
       array.splice(index, 1);
     }
     return array;
   }
 },
};
</script>
