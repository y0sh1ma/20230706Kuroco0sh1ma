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
        <strong>詳細へ：</strong>
        <button
          type="button"
          @click="goToDetail(n.topics_id)"
          class="detail-button"
          aria-label="詳細ページへ移動"
        >
          詳細を見る
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    return { resp: await $axios.$get('/rcms-api/10/list') };
  },
  methods: {
    goToDetail(id) {
      // Vue Routerがあればこれで推奨。なければ window.location.href でも可
      this.$router
        ? this.$router.push({ path: '/formsampleditail/', query: { id } })
        : (window.location.href = `/formsampleditail/?id=${id}`);
    },
  },
};
</script>

<style scoped>
.detail-button {
  padding: 8px 20px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #f5f5f5;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
.detail-button:hover {
  background-color: #e0e0e0;
}
</style>