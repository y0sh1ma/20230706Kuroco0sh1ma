

<template>
  <div>
    <div
      v-for="n in resp.list"
      :key="n.topics_id"
      style="border:1px solid #ccc; padding:16px; margin-bottom:16px; border-radius:8px; max-width:600px; margin:32px auto;"
    >
      <div>
        <strong>id：</strong>
        {{ n.topics_id }}
      </div>
      <div>
        <strong>タイトル：</strong>
        {{ n.subject }}
      </div>
      <div>
        <strong>ID：</strong>
        <a :href="`/formsampleditail/?id=${n.topics_id}`">{{ n.topics_id }}</a>
      </div>
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