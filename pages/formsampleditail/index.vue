

<template>
  <div>
    <hr>
    <div>{{resp.details.topics_id}}</div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    const id = params.id;
    const resp = await $axios.$get(`/rcms-api/10/topics/${id}`);
    return { resp };
  },
};
</script>