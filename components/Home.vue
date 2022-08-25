<template>
  <div>
    <p>
      <b>Create Blog</b>
    </p>
    <el-form ref="ruleForm" :model="form" label-width="120px" :rules="rules">
      <el-form-item label="Title" prop="title">
        <el-input v-model="form.title" placeholder="Please input title"></el-input>
      </el-form-item>
      <el-form-item label="Slug" prop="slug">
        <el-input v-model="form.slug" placeholder="Please input slug"></el-input>
      </el-form-item>
      <el-form-item label="Content" prop="content">
        <el-input type="textarea" v-model="form.content" placeholder="Please input content"></el-input>
      </el-form-item>
      <el-form-item label="Description" prop="description">
        <el-input type="textarea" v-model="form.description" placeholder="Please input description"></el-input>
      </el-form-item>
      <el-form-item label="Tags" prop="tags">
        <el-select v-model="form.tags" multiple allow-create placeholder="Please input tags" filterable>
          <el-option v-for="(item, index) in options" :key="index" :label="item" :value="item">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">Create</el-button>
        <el-button @click="resetForm('ruleForm')">Reset</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  async fetch() {
    await this.getData();
  },
  data() {
    return {
      options: ["Tag 1", "Tag 2"],
      form: {
        title: null,
        slug: null,
        content: null,
        tags: [],
        description: null,
      },
      rules: {
        title: [
          { required: true, message: 'Please input title', trigger: 'blur' },
        ],
        content: [
          { required: true, message: 'Please input content', trigger: 'blur' },
          { min: 3, message: 'Content at least 3 characters', trigger: 'blur' },
        ],
        tags: [
          { required: true, message: 'Please input tags', trigger: 'blur' },
        ],
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    async getData() {
      const { data } = await this.$axios.get('/blog');
      console.log(data);
    }
  }
}
</script>
