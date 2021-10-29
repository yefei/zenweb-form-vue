<template>
  <div id="app">
    <h1>zenweb-form-vue 自动表单演示</h1>
    <el-row :gutter="20">
      <el-col :span="16"><el-input v-model="url" placeholder="输入表单接口地址"></el-input></el-col>
      <el-col :span="8"><el-button type="primary" @click="openDialog">打开表单</el-button></el-col>
    </el-row>

    <el-dialog
      title="表单"
      :visible.sync="open">
      <form-api v-if="open" :url="url" @cancel="open = false" @success="success" />
    </el-dialog>
  </div>
</template>

<script>
import FormApi from './components/FormApi.vue'

export default {
  name: 'App',
  components: {
    FormApi
  },
  data() {
    return {
      url: 'http://localhost:7003/app/android/1',
      open: false,
    }
  },
  methods: {
    openDialog() {
      this.open = true;
    },
    success() {
      this.open = false;
    }
  }
}
</script>
