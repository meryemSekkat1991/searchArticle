<script lang="ts">
import {defineComponent, ref, computed} from 'vue'

export default defineComponent({
  name: "SearchBox",
  setup() {
    const query = ref("")
    const articles = ref([
      { title: "NIL Policies", content: "The purpose of this document is to provide San Jose State University student-athletes, coaches, faculty, staff, sponsors, and boosters, as well as professional service providers, third parties, and/or other individuals or entities related to San Jose State University, with policies regarding the usage of a student-athlete’s name, image or likeness (NIL)." },
      { title: "Institutional NIL Activities. NiL", content: "Name, Image and Likeness. Name, image and/or likeness (NIL) are the three components of a person’s “rights of publicity.” These are independent or collectively identifiable aspects of a person that make them unique, including but not limited to, their name, image, likeness, nickname, signature, social media account(s), any symbol, design or any combination thereof that readily identifies a student-athlete." },
      { title: " Uncompensated Noninstitutional NIL Activities", content: "Institutional NIL activities are any activities in which a prospective student-athlete or currently enrolled student-athlete’s NIL or personal appearance is used for promotional purposes by an institution, conference, or association (including but not limited to the NCAA, or an affiliate of such entities). Such use of NIL may not be compensated." }
    ])

    const searchArticles = computed(() => {
      if(!query.value) {
        return articles.value;
      }
      const serchQuery = query.value.trim().toLowerCase();
      return articles.value.filter(article =>
        article.title.toLowerCase().includes(serchQuery)
        || article.content.toLowerCase().includes(serchQuery)
      );
    });

    const searchCount = computed(() => {
      const serchQuery = query.value.trim().toLowerCase();
      if (!serchQuery) return 0
      return articles.value.reduce((count, article) => {
        const titleCount = (article.title.toLowerCase().match(new RegExp(serchQuery, 'g')) || []).length
        const contentCount = (article.content.toLowerCase().match(new RegExp(serchQuery, 'g')) || []).length
        return count + titleCount + contentCount;
      }, 0)
    });

    return {
      query,
      searchArticles,
      searchCount,
    };
  },
})
</script>

<template>
  <div class="max-w-4xl mx-auto mt-10 p-4">
    <input
      v-model="query"
      type="text"
      class="w-ful p-3 border rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
      placeholder="search articles ..."
    >
    <div v-if="query.trim()" class="mt-4 text-sm mx-auto text-gray-600">
      found "{{query.trim()}}" {{searchCount}} time<span v-if="searchCount !== 1">s</span>
    </div>
    <div v-if="searchArticles.length > 0" class="mt-6 space-y-4">
      <Article  v-for="(article , index) in searchArticles"
        :key="index"
        :article="article"
        :query="query"
      />
    </div>
    <div v-else class="text-center text-gray-600">No articles found.</div>
  </div>

</template>

<style scoped lang="scss">

</style>