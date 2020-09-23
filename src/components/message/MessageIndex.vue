<template>
    <el-container>
        <el-aside style="width: 200px;margin-top: 20px">
            <switch></switch>
            <SideMenu @indexSelect="listByCategory" ref="sideMenu"></SideMenu>
        </el-aside>
        <el-main>
            <Messages class="messages-area" ref="messagesArea"></Messages>
        </el-main>
    </el-container>
</template>

<script>
    import SideMenu from './SideMenu'
    import Messages from './Messages'
    export default {
        name: 'MessageIndex',
        components: {SideMenu,Messages},
        methods: {
            listByCategory () {
                let _this = this;
                let cid = this.$refs.sideMenu.cid;
                let press=JSON.parse(window.localStorage.getItem('username' || '[]')).username;
                let url = 'categories/' + cid +'/messages/'+ press;
                let url1 = 'categories/' + cid +'/messages1/'+ press;
                let url2 = 'categories/' + cid +'/messages2/'+ press;
                let url3 = 'categories/' + cid +'/messages3/'+ press;
                let url4 = 'categories/' + cid +'/messages4/'+ press;
                if(cid == 1){
                    console.log("1")
                    this.$axios.get(url1).then(resp => {
                        if (resp && resp.status === 200) {
                            _this.$refs.messagesArea.messages = resp.data
                            _this.$refs.messagesArea.currentPage = 1
                        }
                    })
                }
                else if(cid == 2){
                    console.log("2")
                    this.$axios.get(url2).then(resp => {
                        if (resp && resp.status === 200) {
                            _this.$refs.messagesArea.messages = resp.data
                            _this.$refs.messagesArea.currentPage = 1
                        }
                    })
                }
                else if(cid == 3){
                    console.log("3")
                    this.$axios.get(url3).then(resp => {
                        if (resp && resp.status === 200) {
                            _this.$refs.messagesArea.messages = resp.data
                            _this.$refs.messagesArea.currentPage = 1
                        }
                    })
                }
                else if(cid == 4){
                    console.log("4")
                    this.$axios.get(url4).then(resp => {
                        if (resp && resp.status === 200) {
                            _this.$refs.messagesArea.messages = resp.data
                            _this.$refs.messagesArea.currentPage = 1
                        }
                    })
                }
                else{
                    console.log("else")
                    this.$axios.get(url).then(resp => {
                        if (resp && resp.status === 200) {
                            _this.$refs.messagesArea.messages = resp.data
                            _this.$refs.messagesArea.currentPage = 1
                        }
                    })
                }

            }
        }
    }
</script>

<style scoped>
    .messages-area {
        width: 990px;
        margin-left: auto;
        margin-right: auto;
    }
</style>

