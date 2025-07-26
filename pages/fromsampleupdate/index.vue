<template>
  <div
    style="border:1px solid #ccc; padding:24px; border-radius:8px; max-width:600px; margin:32px auto;"
  >
    <form @submit.prevent="handleSubmit">
      <div style="margin-bottom:16px;">
        <strong>ID：</strong>
        <!-- 編集不可なのでspanで表示 -->
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
        <!-- 編集不可、保存時に自動セット -->
        <span>{{ form.update_ymdhi || '未更新' }}</span>
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

      <div class="button-group">
        <button type="submit" class="btn-primary">保存</button>
        <button type="button" @click="goBack" class="btn-primary">戻る</button>
      </div>

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
      // 更新日時を自動セット
      const now = new Date();
      const pad = (n) => (n < 10 ? '0' + n : n);
      this.form.update_ymdhi =
        now.getFullYear().toString() +
        pad(now.getMonth() + 1) +
        pad(now.getDate()) +
        pad(now.getHours()) +
        pad(now.getMinutes());

      // 保存API呼び出し（エンドポイントはご指定の /rcms-api/10/topics/update）
      try {
        await this.$axios.$post('/rcms-api/10/topics/update', this.form);
        alert('保存しました（更新日時は自動セットされました）');
      } catch (e) {
        alert('保存に失敗しました');
        console.error(e);
      }
    },

    goBack() {
      window.history.back();
    },

  },
};
</script>