

<template>
  <div>
    <hr>
    <div>{{resp.details.topics_id}}</div>
    <div>{{resp.details.subject}}</div>
    <div v-html="response.details.contents"></div>

  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    return { resp: await $axios.$get('/rcms-api/10/topics/3703') };
  },
};
</script>