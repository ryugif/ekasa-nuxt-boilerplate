<template>
    <div>
        <p>
            <b>{{ this.id ? "Update" : "Create" }} Blog</b>
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
                <el-button type="primary" @click="submitForm('ruleForm')">{{ this.id ? "Update" : "Create" }}
                </el-button>
                <el-button @click="resetForm('ruleForm')">Reset</el-button>
                <el-button @click="backToHome">Back</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
export default {
    async fetch() {
        if (this.$route.params.id) {
            this.id = this.$route.params.id;
            await this.getDetail();
        }
    },
    data() {
        return {
            options: ["Tag 1", "Tag 2"],
            id: null,
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
                    { min: 5, message: 'Content at least 5 characters', trigger: 'blur' },
                ],
                description: [
                    { required: true, message: 'Please input description', trigger: 'blur' },
                    { min: 5, message: 'Content at least 5 characters', trigger: 'blur' },
                ],
                tags: [
                    { required: true, message: 'Please input tags', trigger: 'blur' },
                ],
            }
        }
    },
    methods: {
        backToHome() {
            this.$router.push("/");
        },
        submitForm(formName) {
            this.$refs[formName].validate(async (valid) => {
                if (valid) {
                    await this.$axios({
                        method: this.id ? 'patch' : 'post',
                        url: '/blog' + (this.id ? '/' + this.id : ''),
                        data: this.form,
                    });

                    this.backToHome();
                    this.$message({
                        message: 'Successfully saved',
                        type: 'success'
                    });
                } else {
                    this.$message({
                        message: 'Please check the form',
                        type: 'error'
                    });
                }
            });
        },
        resetForm(formName) {
            this.$refs[formName].resetFields();
        },
        async getDetail() {
            const { data } = await this.$axios.get(`/blog/${this.id}`);
            console.log(data);

            // loop this.form to set value
            for (let key in this.form) {
                this.form[key] = data[key];
            }
        }
    }
}
</script>
