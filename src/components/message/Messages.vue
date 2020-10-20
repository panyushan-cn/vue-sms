<template>
    <div>
        <el-row style="height: 840px;">
            <search-bar @onSearch="searchResult" ref="searchBar"></search-bar>
            <el-popover placement="right" trigger="hover" width="250"
                        v-for="item in messages"
                        v-bind:key="item.id">
                <div>主题：{{item.theme}}<br/><br/>详细信息：{{item.detail}} <br/>发件人：{{item.send}}<br/>
                    收件人：{{item.receive}}<br/><br/>状态：{{item.abs}}</div>

                <el-card slot="reference" style="width: 135px;margin-bottom: 20px;height: 233px;float: left;margin-right: 15px" class="book"
                         body-style="padding:10px" shadow="hover">
                    <div class="attachment" @click="editBook(item)">
                        <img :src="item.attachment" alt="附件">
                    </div>
                    <div class="info">
                        <div class="theme">
                            <a href="">主题：{{item.theme}}</a>
                        </div>
                        <i class="el-icon-delete" @click="deleteBook(item.id)"></i>
                    </div>
                    <div class="detail">详细信息：{{item.detail}}/<br/>发件人：{{item.send}}</div>
                </el-card>
            </el-popover>
            <edit-form @onSubmit="loadMessages()" ref="edit"></edit-form>
        </el-row>
        <el-row>
            <el-pagination
                    @current-change="handleCurrentChange"
                    :current-page="messages.currentPage"
                    :page-size="messages.pagesize"
                    :total="messages.length">
            </el-pagination>
        </el-row>
    </div>
</template>

<script>
    import EditForm from './EditForm'
    import SearchBar from './SearchBar'

    export default {
        name: 'Messages',
        components: {EditForm, SearchBar},
        data () {
            return {
                messages: [],
            }
        },
        mounted: function () {
            this.loadMessages()
        },
        methods: {
            loadMessages () {
                let _this = this;
                //let receive='潘玉山';
                let receive=JSON.parse(window.localStorage.getItem('username' || '[]')).username;
                let url = 'receive/' + receive + '/messages';
                this.$axios.get(url).then(resp => {
                    if (resp && resp.status === 200) {
                        _this.messages = resp.data
                        _this.currentPage = 1
                    }
                })
            },
            handleCurrentChange: function (currentPage) {
                this.currentPage = currentPage
                console.log(this.currentPage)
            },
            searchResult () {
                let _this = this;
                this.$axios
                    .get('/search?keywords=' + this.$refs.searchBar.keywords, {
                    }).then(resp => {
                    if (resp && resp.status === 200) {
                        _this.messages = resp.data
                    }
                })
            },
            deleteBook (id) {
                this.$alert('此操作将永久删除该消息 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                        this.$axios
                            .post('/admin/content/messages/delete', {id: id}).then(resp => {
                            if (resp && resp.status === 200) {
                                this.loadMessages()
                            }
                        })
                    }
                ).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消删除'
                    })
                })
                // alert(id)
            },
            editBook (item) {
                this.$refs.edit.dialogFormVisible = true
                this.$refs.edit.form = {
                    id: item.id,
                    attachment: item.attachment,
                    theme: item.theme,
                    detail: item.detail,
                    send: item.send,
                    receive: item.receive,
                    abs: item.abs,
                    category: {
                        id: item.category.id.toString(),
                        name: item.category.name
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .attachment {
        width: 115px;
        height: 122px;
        margin-bottom: 7px;
        overflow: hidden;
        cursor: pointer;
    }

    img {
        width: 115px;
        height: 122px;
        /*margin: 0 auto;*/
    }

    .theme {
        font-size: 14px;
        text-align: left;
    }

    .detail {
        color: #333;
        width: 102px;
        font-size: 13px;
        margin-bottom: 6px;
        margin-top: 10px;
        text-align: left;
    }

    .el-icon-delete {
        cursor: pointer;
        float: right;
        margin-top: 22px;
    }

    .switch {
        display: flex;
        position: absolute;
        left: 780px;
        top: 25px;
    }

    a {
        text-decoration: none;
    }

    a:link, a:visited, a:focus {
        color: #3377aa;
    }
</style>

