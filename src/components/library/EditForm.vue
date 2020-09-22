<template>
    <div>
        <!--        TODO: 添加修改分开-->
        <i class="el-icon-circle-plus-outline" @click="dialogFormVisible = true"></i>
        <el-dialog
                title="添加/修改短消息"
                :visible.sync="dialogFormVisible"
                @close="clear">
            <el-form v-model="form" style="text-align: left" ref="dataForm">
                <el-form-item label="主题" :label-width="formLabelWidth" prop="bookname">
                    <el-input v-model="form.bookname" autocomplete="off" placeholder="例如：账号消息"></el-input>
                </el-form-item>
                <el-form-item label="文本" :label-width="formLabelWidth" prop="author">
                    <el-input type="textarea" :rows="5" v-model="form.author" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="发件人" :label-width="formLabelWidth" prop="date">
                    <el-input v-model="form.date" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="收件人" :label-width="formLabelWidth" prop="press">
                    <el-input v-model="form.press" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="附件" :label-width="formLabelWidth" prop="cover">
                    <el-input v-model="form.cover" autocomplete="off" placeholder="图片 URL"></el-input>
                    <img-upload @onUpload="uploadImg" ref="imgUpload"></img-upload>
                </el-form-item>

                <!-- <el-form-item label="重要性" :label-width="formLabelWidth" prop="abs">
                     <el-input type="textarea" v-model="form.abs" autocomplete="off"></el-input>
                 </el-form-item>-->
                <el-form-item label="重要性" :label-width="formLabelWidth" prop="abs">
                    <el-select v-model="form.abs" placeholder="请选择分类" >
                        <el-option label="紧急" value="紧急"></el-option>
                        <el-option label="重要" value="重要"></el-option>
                        <el-option label="普通" value="普通"></el-option>
                    </el-select>
                </el-form-item>

                <el-form-item label="状态" :label-width="formLabelWidth" prop="cid">
                    <el-select v-model="form.category.id" placeholder="请选择分类" >
                        <el-option label="已读" value="1"></el-option>
                        <el-option label="未读" value="2"></el-option>
                        <el-option label="已发送" value="3" ></el-option>
                        <el-option label="保存到草稿" value="4"></el-option>
                        <!--                        <el-option label="经管" value="5"></el-option>-->
                        <!--                        <el-option label="科技" value="6"></el-option>-->
                    </el-select>
                </el-form-item>
                <el-form-item prop="id" style="height: 0">
                    <el-input type="hidden" v-model="form.id" autocomplete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="onSubmit">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
    import ImgUpload from './ImgUpload'
    export default {
        name: "EditForm",
        components: {ImgUpload},
        data() {
            return {
                dialogFormVisible: false,
                form: {
                    id: '',
                    bookname: '',
                    author: '',
                    date: JSON.parse(window.localStorage.getItem('username' || '[]')).username,
                    press:'',
                    cover: '',
                    abs: '',
                    category: {
                        id: '3',
                        name: ''
                    }
                },
                formLabelWidth: '120px'
            }
        },
        methods: {
            clear() {
                this.form = {
                    id: '',
                    bookname: '',
                    author: '',
                    date:  JSON.parse(window.localStorage.getItem('username' || '[]')).username,
                    press: '',
                    cover: '',
                    abs: '',
                    category: {
                        id: '3',
                        name: ''
                    }
                }
                this.$refs.imgUpload.$refs.upload.clearFiles()
            },
            onSubmit() {
                this.$axios
                    .post('/admin/content/books', {
                        id: this.form.id,
                        cover: this.form.cover,
                        bookname: this.form.bookname,
                        author: this.form.author,
                        date: this.form.date,
                        press: this.form.press,
                        abs: this.form.abs,
                        category: this.form.category
                    }).then(resp => {
                    if (resp && resp.status === 200) {
                        this.dialogFormVisible = false
                        this.$emit('onSubmit')
                        this.$message.success('修改成功')
                    }
                })
            },
            uploadImg () {
                this.form.cover = this.$refs.imgUpload.url
            }

        }
    }
</script>

<style scoped>
    .el-icon-circle-plus-outline {
        margin: 50px 0 0 20px;
        font-size: 100px;
        float: left;
        cursor: pointer;
    }
</style>