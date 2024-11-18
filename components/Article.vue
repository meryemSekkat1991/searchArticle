<script lang="ts">
import {defineComponent, computed } from 'vue'

export default defineComponent({
  name: "Article",
  props: {
    article: {
      typw: Object as () => {title: string; content:string},
      required: true ,
    },
    query: {
      type: String,
      default: ''
    },
  },
  setup(props) {
    const highlightText = (text: string) => {
      if(!props.query) return text;
      const regex = new RegExp(props.query, 'gi');
      return text.replace(regex, `<span class="text-yellow-500">${props.query}</span>`);
    };

    return {
      highlightText
    };
  },
});
</script>

<template>
 <div class="bg-white p-4 rounded-lg shadow md">
   <h2 class="text-xl font-bold mb-2">
     <span v-html="highlightText(article.title)"></span>
   </h2>

   <p class="text-gray-700" v-html="highlightText(article.content)"></p>
 </div>
</template>

<style lang="scss">

</style>