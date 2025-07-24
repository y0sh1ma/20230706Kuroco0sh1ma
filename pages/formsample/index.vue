

<template>
  <div>

    <div v-for="n in resp.list" :key="n.slag" >
      <div>{{ n.subject }}</div>
      <div>{{ n.contents }}</div>
      <div>{{ n.topics_id }}</div>
    </div>

  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    return { resp: await $axios.$get('/rcms-api/10/list') };
  },
};
</script>