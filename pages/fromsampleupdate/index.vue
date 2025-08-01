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

      <div class="button-group">
        <button type="submit" class="btn-primary">保存</button>
        <button type="button" @click="goBack" class="btn-primary">戻る</button>
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
    const resp = await $axios.$get(`https://diverta-oshima.g.kuroco.app/rcms-api/10/topics/${id}`
      ,{
        headers: { 'X-RCMS-API-ACCESS-TOKEN': '42275dcbd8c48d924ff658cef0f25d1ac18985ea4f09f3caf9936c509c6db132' }
      }
	);
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
      contents: d.contents || ''
    };
  },
  methods: {
    async handleSubmit() {
      try {
        const id = this.form.topics_id;
        const url = `https://diverta-oshima.g.kuroco.app/rcms-api/10/upsert/${id}`;
		
		const postData = {
          subject: this.form.subject,
          contents: this.form.contents,
		};
		
        await this.$axios.$post(url, postData
       ,{
	  	    headers: { 'X-RCMS-API-ACCESS-TOKEN': '42275dcbd8c48d924ff658cef0f25d1ac18985ea4f09f3caf9936c509c6db132' }
        }
		);
		
        alert('保存しました');
        window.history.back();s
      } catch (e) {
        alert('保存に失敗しました');
        // --- 詳細デバッグ出力 ---
        if (e.response) {
          // サーバーレスポンスが返っている場合
          console.error('POST /rcms-api/10/upsert error:');
          console.error('status:', e.response.status);
          console.error('response data:', e.response.data);
          console.error('response headers:', e.response.headers);
        } else if (e.request) {
          // リクエスト送信済だがレスポンス受信できず
          console.error('No response received:');
          console.error('request:', e.request);
        } else {
          // リクエスト前のJSエラーなど
          console.error('Error message:', e.message);
        }
        console.error('config:', e.config);
      }
    },
	
    goBack() {
      window.history.back();
    },

  },
};
</script>