<template>
  <div>
    <div style="max-width: 600px; margin: 32px auto; display: flex; justify-content: flex-end; padding: 0 16px 16px 0;">
      <button
        type="button"
        @click="goToInsert"
        class="insert-button"
        aria-label="新規作成ページへ移動"
      >
        新規作成
      </button>
    </div>

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
    return {
      resp: await $axios.$get(
        'https://diverta-oshima.g.kuroco.app/rcms-api/10/list',
        {
          headers: {
            'X-RCMS-API-ACCESS-TOKEN': '42275dcbd8c48d924ff658cef0f25d1ac18985ea4f09f3caf9936c509c6db132'
          }
        }
      )
    };
  },
  methods: {
    goToDetail(id) {
      this.$router
        ? this.$router.push({ path: '/formsampleditail/', query: { id } })
        : (window.location.href = `/formsampleditail/?id=${id}`);
    },
    goToInsert() {
      if (this.$router) {
        this.$router.push('/formsampleinsert');
      } else {
        window.location.href = '/formsampleinsert';
      }
    },
  },
};
</script>
