<template>
    <div class="page">
        <header class="header" id="header">
          <img src="../assets/logo.png" alt="" class="logo">
          <span>Vue-scroll-tab</span>
        </header>
        <div class="container">
            <div class="tabBar-area" id="tabBarArea">
                <div class="tabbar">
                    <div
                        @click="handleNavBarClick(item.id)"
                        v-for="item in tabList"
                        :key="item.id"
                        :class="activeNav == item.id ? 'tabbar-item active' : 'tabbar-item'"
                    >{{item.name}}</div>
                </div>
            </div>
            <div class="content">
                <div class="scrollItem" id="sale">商品销售</div>
                <div class="scrollItem" id="intro">商品介绍</div>
                <div class="scrollItem" id="detail">商品详情</div>
                <div class="scrollItem" id="after">商品售后</div>
            </div>
        </div>
        <!-- 顶部导航 -->
        <div class="topNavBar" v-if="isShowTopNavBar" id="topNav">
            <div class="tabbar">
                <div
                    @click="handleNavBarClick(item.id)"
                    v-for="item in tabList"
                    :key="item.id"
                    :class="activeNav == item.id ? 'tabbar-item active' : 'tabbar-item'"
                >{{item.name}}</div>
            </div>
        </div>
        <footer class="footer"></footer>
    </div>
</template>

<script>
export default {
    name: "main",
    data() {
        return {
            activeNav: "1",
            isShowTopNavBar: false,
            tabList: [
                {
                    id: 1,
                    name: "商品销售"
                },
                {
                    id: 2,
                    name: "商品介绍"
                },
                {
                    id: 3,
                    name: "商品详情"
                },
                {
                    id: 4,
                    name: "商品售后"
                }
            ],
            activeDom: []
        };
    },
    mounted() {
        this.activeDom = document.getElementsByClassName("scrollItem");
        window.addEventListener("scroll", this.handlePageScroll, true);
    },
    methods: {
        //点击 NavBar
        handleNavBarClick(id) {
            if (id != this.activeNav) {
                this.activeNav = id;
            }
            //导航高度计算
            let tabBarOffsetTop = document.querySelector("#tabBarArea").offsetTop;
            let headerHeight = document.querySelector(`#header`).offsetTop - tabBarOffsetTop;
            //页面锚点高度
            if (id == 1) {
                document.documentElement.scrollTop =
                    document.querySelector(`#sale`).offsetTop + headerHeight;
            } else if (id == 2) {
                document.documentElement.scrollTop =
                    document.querySelector(`#intro`).offsetTop + headerHeight;
            } else if (id == 3) {
                document.documentElement.scrollTop =
                    document.querySelector(`#detail`).offsetTop + headerHeight;
            } else if (id == 4) {
                document.documentElement.scrollTop =
                    document.querySelector(`#after`).offsetTop + headerHeight;
            }
        },
        //页面滚动tab切换
        handlePageScroll() {
            let scrollTop =
                window.pageYOffset ||
                document.documentElement.scrollTop ||
                document.body.scrollTop;
            let tabBarOffsetTop = document.querySelector("#tabBarArea").offsetTop;

            this.isShowTopNavBar = scrollTop >= tabBarOffsetTop;
            for (let i = 0; i < this.activeDom.length; i++) {
                if (
                    this.activeDom[this.activeDom.length - 1].offsetTop -
                        scrollTop > tabBarOffsetTop
                ) {
                    if (
                        this.activeDom[i].offsetTop - scrollTop <= tabBarOffsetTop &&
                        this.activeDom[i + 1].offsetTop - scrollTop > tabBarOffsetTop
                    ) {
                        this.activeNav = i + 1;
                    }
                } else {
                    this.activeNav = this.activeDom.length;
                }
            }
        }
    },
    destroyed() {
        window.removeEventListener("scroll", this.handlePageScroll);
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
body {
    scroll-behavior: smooth;
}
.page {
    .header {
        width: 100%;
        min-height: 120px;
        line-height: 120px;
        background: #eee;
        text-align: center;
        color: #ffffff;
        font-size: 32px;
        font-weight: bold;
        padding: 20px;
        .logo{
          width: 100px;
          height: 100px;
          vertical-align: middle;
        }
    }
    .container {
        // 导航
        .tabBar-area {
            width: 100%;
            height: 80px;
            line-height: 80px;
            background: #f7f8fa;
            display: flex;
            align-items: center;
            justify-content: space-around;

            .tabbar {
                .tabbar-item {
                    width: 175px;
                    text-align: center;
                    float: left;
                    cursor: pointer;
                }

                .active {
                    background: #ffffff;
                    border-top: 3px solid #ff615b;
                    color: #ff615b;
                }
            }
        }
        .content {
            #sale,
            #intro,
            #detail,
            #after {
                width: 100%;
                color: #ffffff;
                font-size: 42px;
                font-weight: bold;
            }
            #sale {
                height: 300px;
                background: orange;
            }
            #intro {
                height: 500px;
                background: pink;
            }
            #detail {
                height: 800px;
                background: paleturquoise;
            }
            #after {
                height: 900px;
                background: yellow;
            }
        }
    }
    .topNavBar {
        width: 100%;
        height: 80px;
        line-height: 80px;
        background: #ffffff;
        display: flex;
        align-items: center;
        justify-content: space-around;
        position: fixed;
        top: 0;
        left: 0;
        cursor: pointer;
        .tabbar {
            width: 895px;
            display: inline-block;
            margin-left: 171px;
            .tabbar-item {
                width: 175px;
                text-align: center;
                float: left;
                cursor: pointer;
            }

            .active {
                background: #ffffff;
                color: #ff615b;
            }
        }
    }
    .footer {
        width: 100%;
        min-height: 120px;
        background: #000000;
    }
}
</style>
