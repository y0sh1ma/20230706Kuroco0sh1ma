<template>
  <div
    style="border:1px solid #ccc; padding:24px; border-radius:8px; max-width:600px; margin:32px auto;"
  >
    <div style="margin-bottom:16px;">
      <strong>ID：</strong>
      <span>{{ resp.details.topics_id }}</span>
    </div>
    <div style="margin-bottom:16px;">
      <strong>タイトル：</strong>
      <span>{{ resp.details.subject }}</span>
    </div>
    <div style="margin-bottom:16px;">
      <strong>更新日時：</strong>
      <span>{{ resp.details.update_ymdhi }}</span>
    </div>
    <div style="margin-bottom:16px;">
      <strong>内容：</strong>
      <span>{{ resp.details.contents }}</span>
    </div>
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