<template>
  <div
    style="border:1px solid #ccc; padding:24px; border-radius:8px; max-width:600px; margin:32px auto;"
  >
    <form @submit.prevent="handleSubmit">
      <div style="margin-bottom:16px;">
        <strong>ID：</strong>
        <span>{{ form.topics_id }}</span>
      </div>
      <div style="margin-bottom:16px;">
        <strong>タイトル：</strong>
        <input
          v-model="form.subject"
          type="text"
          style="width:100%; padding:6px;"
          required
        />
      </div>
      <div style="margin-bottom:16px;">
        <strong>更新日時：</strong>
        <input
          v-model="form.update_ymdhi"
          type="text"
          style="width:100%; padding:6px;"
          required
        />
      </div>
      <div style="margin-bottom:16px;">
        <strong>内容：</strong>
        <textarea
          v-model="form.contents"
          rows="6"
          style="width:100%; padding:6px;"
          required
        ></textarea>
      </div>
      <!-- 必要なフィールドがあればここに追加 -->
      <div>
        <button type="submit" style="padding:8px 32px;">保存</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, query }) {
    // 詳細画面と同じAPIでデータ取得
    const id = query.id;
    if (!id) {
      return {  resp: { details: {} } };
    }
    const resp = await $axios.$get(`/rcms-api/10/topics/${id}`);
    return { resp };
  },
  data() {
    return {
      form: {
        topics_id: '',
        subject: '',
        update_ymdhi: '',
        contents: '',
      },
    };
  },
  mounted() {
    // APIで取得した内容をフォーム初期値にセット
    if (this.resp && this.resp.details) {
      this.form = { ...this.resp.details };
    }
  },
  methods: {
    // 保存処理（ここでは仮でconsole出力）
    async handleSubmit() {
      // 例: 保存API送信
      // await this.$axios.$post('/rcms-api/10/insert', this.form);
      console.log('保存データ:', this.form);
      alert('保存しました');
    },
  },
};
</script>