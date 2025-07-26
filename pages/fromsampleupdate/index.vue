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
        <strong>内容：</strong>
        <textarea
          v-model="form.contents"
          rows="6"
          style="width:100%; padding:6px;"
          required
        ></textarea>
      </div>
      <div>
        <button type="submit" style="padding:8px 32px;">保存</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, query }) {
    const id = query.id;
    if (!id) {
      return { resp: { details: {} } };
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
  created() {
    const d = this.resp?.details || {};
    this.form = {
      topics_id: d.topics_id || '',
      subject: d.subject || '',
      update_ymdhi: d.update_ymdhi || '',
      contents: d.contents || '',
    };
  },
  methods: {
    async handleSubmit() {
      // 保存前に更新日時を現在日時(YYYYMMDDHHmm形式)で設定
      const now = new Date();
      const pad = (n) => (n < 10 ? '0' + n : n);
      this.form.update_ymdhi =
        now.getFullYear().toString() +
        pad(now.getMonth() + 1) +
        pad(now.getDate()) +
        pad(now.getHours()) +
        pad(now.getMinutes());

      // ここに保存API呼び出しを実装してください
      // 例:
      // await this.$axios.$post('/rcms-api/10/topics/update', this.form);

      console.log('保存データ:', this.form);
      alert('保存しました（更新日時は自動セットされました）');
    },
  },
};
</script>