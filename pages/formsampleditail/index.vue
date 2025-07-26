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


    <div style="text-align: center; margin-top: 32px;">
      <button
        @click="goToUpdate"
        style="padding: 10px 24px; font-size:16px; cursor:pointer;"
      >
        更新
      </button>
    </div>


    <div style="text-align: center; margin-top: 16px;">
      <button
        @click="goBack"
        style="padding: 8px 24px; font-size:14px; cursor:pointer;"
      >
        戻る
      </button>
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
  methods: {
    goToUpdate() {
      const id = this.resp?.details?.topics_id;
      if (!id) {
        alert('IDが取得できません');
        return;
      }
      // "di"というクエリ名で遷移させる
      this.$router.push({ path: '/fromsampleupdate', query: { id: id } });
    },

    goBack() {
      window.history.back();
    },

  },
};
</script>
