<template>
    <UiPageHeader
      :subject="response.details.subject"
      subheading="Privacy Policy"
    />
  <div>
    <div>{{response.details.ext_3}}</div>
    <div v-html="response.details.ext_3"></div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    return { response: await $axios.$get('/rcms-api/3/service/3') };
  },
};
</script>
