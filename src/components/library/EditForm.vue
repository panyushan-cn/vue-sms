<template>
    <div>
        <!--        TODO: 添加修改分开-->
        <i class="el-icon-circle-plus-outline" @click="dialogFormVisible = true"></i>
        <el-dialog
                title="添加/修改短消息"
                :visible.sync="dialogFormVisible"
                @close="clear">
            <el-form v-model="form" style="text-align: left" ref="dataForm">
                <el-form-item label="主题" :label-width="formLabelWidth" prop="theme">
                    <el-input v-model="form.theme" autocomplete="off" placeholder="例如：账号消息"></el-input>
                </el-form-item>
                <el-form-item label="文本" :label-width="formLabelWidth" prop="detail">
                    <el-input type="textarea" :rows="5" v-model="form.detail" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="发件人" :label-width="formLabelWidth" prop="send">
                    <el-input v-model="form.send" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="收件人" :label-width="formLabelWidth" prop="receive">
                    <el-input v-model="form.receive" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="附件" :label-width="formLabelWidth" prop="attachment">
                    <el-input v-model="form.attachment" autocomplete="off" placeholder="图片 URL"></el-input>
                    <img-upload @onUpload="uploadImg" ref="imgUpload"></img-upload>
                </el-form-item>
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
                    theme: '',
                    detail: '',
                    send: JSON.parse(window.localStorage.getItem('username' || '[]')).username,
                    receive:'',
                    attachment: '',
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
                    theme: '',
                    detail: '',
                    send:  JSON.parse(window.localStorage.getItem('username' || '[]')).username,
                    receive: '',
                    attachment: '',
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
                    .post('/admin/content/messages', {
                        id: this.form.id,
                        attachment: this.form.attachment,
                        theme: this.form.theme,
                        detail: this.form.detail,
                        send: this.form.send,
                        receive: this.form.receive,
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
                this.form.attachment = this.$refs.imgUpload.url
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