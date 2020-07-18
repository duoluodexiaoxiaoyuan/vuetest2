<template>
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" style="width: 40%" class="demo-ruleForm">
        <el-form-item label="ID" >
            <el-input v-model="ruleForm.id" readonly></el-input>
        </el-form-item>

        <el-form-item label="图书名称" prop="name">
            <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="作者" prop="author">
            <el-input v-model="ruleForm.author"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">修改</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>

        </el-form-item>
    </el-form>
</template>

<script>
    export default {
        data() {
            return {
                ruleForm: {
                    name: '',
                    author:'',
                    id:''
                },
                rules: {
                    name: [
                        { required: true, message: '请输入图书名称', trigger: 'blur' },

                    ],
                    author: [
                        { required: true, message: '请输入图书作者', trigger: 'blur' },

                    ]

                }
            };
        },
        methods: {
            submitForm(formName) {
                const _this=this
                // console.log(_this)
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        axios.put('http://localhost:8181/book/update',this.ruleForm).then(function (resp) {
                            if(resp.data=='success'){
                                _this.$message('修改成功')
                                _this.$router.push('/bookManage')
                            }
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        },
        created() {
            // alert(this.$route.query.id)
            const _this=this
            axios.get("http://localhost:8181/book/findById/"+this.$route.query.id).then(function (resp) {
                // console.log(resp.data)
                _this.ruleForm=resp.data
            })
        }
    }
</script>