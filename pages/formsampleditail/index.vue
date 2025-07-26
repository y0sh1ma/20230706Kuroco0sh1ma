


<template>
  <div>
    <hr>
    <div>{{resp.details.topics_id}}</div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, query }) {
    const id = query.id;
    const resp = await $axios.$get(`/rcms-api/10/topics/${id}`);
    return { resp };
  },
};
</script>