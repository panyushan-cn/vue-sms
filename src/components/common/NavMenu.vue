<template>
    <!--   使用router模式时会在激活导航时以index为path进行跳转-->
    <el-menu
            :default-active="$route.path"
            router
            mode="horizontal"
            background-color="white"
            text-color="#222"
            active-text-color="red"
            style="min-width: 1300px">
        <el-menu-item v-for="(item,i) in navList" :key="i" :index="item.name">
            {{ item.navItem }}
        </el-menu-item>
        <i class="el-icon-switch-button" v-on:click="logout" style="font-size: 25px;float: right"></i>
        <a href="#" style="width: 200px; color: #222;float: right;padding: 20px;" @click="clickDialog">当前用户：{{press}}</a>
        <i class="el-icon-message" style="font-size: 30px; margin-top:20px; margin-left: 140px"></i>
        <span style="position: absolute;padding-top: 20px;right: 43%;font-size: 20px;font-weight: bold">短消息管理系统</span>
    </el-menu>
</template>

<script>
    export default {
        name: "NavMenu",
        data () {
            return {
                navList: [
                    {name: '/index', navItem: '首页'},
                    {name: '/wantedlist', navItem: '通讯录'},
                    {name: '/library', navItem: '消息中心'},
                    {name: '/admin', navItem: '个人中心'}
                ],
                activeIndex: "",
                press:JSON.parse(window.localStorage.getItem('username' || '[]')).username

            }
        },
        methods: {
            clickDialog() {
                this.$message.info("孟老弟牛逼")
            },
            logout () {
                let _this = this;
                this.$axios.get('/logout').then(resp => {
                    if (resp.data.code === 200) {
                        // 前后端状态保持一致
                        _this.$store.commit('logout')
                        _this.$router.replace('/login')
                    }
                })
            }
        }
    }
</script>

<style scoped>
    a{
        text-decoration: none;
    }
    i{
        margin-top: 15px;
    }
    span {
        pointer-events: none;
    }
</style>