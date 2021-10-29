<template>
  <el-form ref="form" :model="form" :rules="rules" size="small">
      <el-form-item
        v-for="item of layout"
        :key="item"
        :label="fields[item].label || item"
        :prop="item"
        :error="errors && errors[item]">

        <el-input
          v-model="form[item]"
          :placeholder="fields[item].placeholder" />
        
        <div v-if="fields[item].help" v-text="fields[item].help" class="form-help"></div>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">提交</el-button>
        <el-button @click="onCancel">取消</el-button>
      </el-form-item>
  </el-form>
</template>

<style>
.form-help {
  color: #909399;
}
</style>

<script>
export default {
  name: 'form-render',
  props: ['layout', 'fields', 'errors'],
  data() {
    return {
      rules: null,
      form: null,
    }
  },
  created() {
    const form = {};
    const rules = {};
    for (const [k, o] of Object.entries(this.fields)) {
      form[k] = o.default || '';
      if (!rules[k]) rules[k] = [];
      if (o.required) {
        // 必填项
        rules[k].push({ required: true, message: typeof o.required === 'string' ? o.required : undefined, trigger: 'blur' });
      }
      if (o.validate) {
        // 长度验证
        if (o.validate.maxLength || o.validate.minLength) {
          const _lenObj = { trigger: 'blur' };
          o.validate.maxLength && (_lenObj['max'] = o.validate.maxLength);
          o.validate.minLength && (_lenObj['min'] = o.validate.minLength);
          rules[k].push(_lenObj);
        }
      }
    }
    this.form = form;
    this.rules = rules;
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.$emit('submit', this.form);
        } else {
          return false;
        }
      });
    },
    onCancel() {
      this.$emit('cancel');
    }
  }
}
</script>
