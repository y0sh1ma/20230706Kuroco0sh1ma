<template>
  <div
    style="border:1px solid #ccc; padding:24px; border-radius:8px; max-width:600px; margin:32px auto;"
  >
    <form @submit.prevent="handleSubmit">
      <div style="margin-bottom:16px;" v-if="form.topics_id">
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

      <div class="button-group">
        <button type="submit" class="btn-primary">保存</button>
        <button type="button" @click="goBack" class="btn-primary">戻る</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        // topics_id: '', // 新規なら不要。編集時だけ残す
        subject: '',
        update_ymdhi: '', // 必要なら残す
        contents: '',
        open_flg: '1',    // 必要ならここでセット
      },
    };
  },
  methods: {
    async handleSubmit() {
      try {
        await this.$axios.$post(
          '/rcms-api/10/insert',
          this.form,
          {
            headers: {
              'X-RCMS-API-ACCESS-TOKEN': '03dc65148cbdad0e9123d14b407282e57c0fee7b69859bf17b16d6e883f0cbb8'
            }
          }
        );
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