<template>
  <div v-loading="loading">
    <form-render
      v-if="layout && fields"
      :layout="layout"
      :fields="fields"
      :errors="errors"
      @submit="submit"
      @cancel="cancel" />
    <div v-else class="loading">正在载入表单</div>
  </div>
</template>

<style>
.loading {
  text-align: center;
  line-height: 300px;
}
</style>

<script>
import FormRender from './FormRender.vue'

export default {
  name: 'form-api',
  props: ['url'],
  components: {
    FormRender
  },
  data() {
    return {
      loading: true,
      layout: null,
      fields: null,
      errors: null,
    }
  },
  async mounted() {
    const data = await this.$api.get(this.url);
    this.layout = data.layout;
    this.fields = data.fields;
    this.loading = false;
  },
  methods: {
    submit(data) {
      this.$api.post(this.url, data).then(r => {
        this.$message.success('提交完成');
        this.$emit('success', r);
      }, e => {
        this.errors = e.data.errors;
        this.$message.error('表单错误，请检查输入项');
      });
    },
    cancel() {
      this.$emit('cancel');
    }
  }
}
</script>
