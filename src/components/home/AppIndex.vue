<template>
    <div class="ap">
        <div id="mission"><p class="mission">主题</p>
            <p class="desc">
                短消息管理系统  课程设计
            </p>
        </div>
        <a-carousel autoplay>
            <div id="ymd"><h3>欢迎来到短消息管理系统</h3>
            </div>
        </a-carousel>
    </div>
</template>
<script>
    export default {
        name: 'AppIndex',
        methods: {}
    };
</script>
<style scoped>

    .ap {
        margin: auto;
        width: 100%;
    }

    .ant-carousel >>> .slick-slide {
        text-align: center;
        height: 100%;
        line-height: 650px;
        background: #6fa8dc;
        overflow: hidden;
    }

    .ant-carousel >>> .ap .custom-slick-arrow {
        width: 25px;
        height: 25px;
        font-size: 25px;
        color: #fff;
        background-color: rgba(31, 45, 61, 0.11);
        opacity: 0.3;
    }

    .ant-carousel >>> .ap .custom-slick-arrow:before {
        display: none;
    }

    .ant-carousel >>> .ap .custom-slick-arrow:hover {
        opacity: 0.9;
    }

    .ant-carousel >>> .slick-slide h3 {
        color: #fff;
    }

    .ap h3 {
        font-size: 55px;
        font-weight: lighter;
    }

    #mission {
        position: absolute;
        top: 61px;
        left: 8vw;
        width: 450px;
        height: 230px;
        padding: 50px .42rem 0 30px;
        background-color: #eff1d4;
        border-radius: 0 0 115px;
        z-index: 1;
    }

    #mission > .mission {
        font-weight: 500;
        font-size: 28px;
        line-height: .31rem;
        color: #828282;
        text-align: left;
    }

    #mission > .desc {
        margin-top: .35rem;
        font-weight: 500;
        font-size: 48px;
        line-height: 1.25;
        text-align: left;
    }

    p {
        margin-top: 0;
        margin-bottom: 1em;
    }
</style>
