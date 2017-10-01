<template>
    <div class="index">
        <div class="top">
            <router-link class="scenic js-opencity" :to="'/city'">
                <!-- <span>请输入目的地城市/景点</span> -->
                <span>{{cityInfo.cname}}</span>
                <span class="sceicon">&#xe648</span>
            </router-link>
            <div class="travel flex-box" @click="changeCity()">
                <p class="ticon">
                    <span>旅游专线</span>
                </p>
                <p class="flex1">往返{{cityInfo.cname}}景区</p>
            </div>
        </div>
        <div class="station" v-if="isHasScenic">
            <div class="box" v-for="son in scenicList" :key="son.silyscenicId" @click="selectScenic()">
                <div class="image"><img :src="son.sipicture"></div>
                <p class="title">{{son.siname}}</p>
                <p class="tiptwo" v-if="son.siactivityDesc && son.siactivityDesc.length <= 2">
                    <span class="tiptext">{{son.siactivityDesc}}</span>
                </p>
                <p class="tipfour" v-if="son.siactivityDesc && son.siactivityDesc.length > 2 && son.siactivityDesc.length <= 4">
                    <span class="tiptext">{{son.siactivityDesc}}</span>
                </p>
            </div>
        </div>
        <div class="station" v-if="!isHasScenic">
            <p class="none">抱歉哟，暂时未开通覆盖该城市的线路</p>
        </div>
        <div class="float">
            <p class="name">请输入目的地城市/景点</p>
        </div>
        <div class="line" v-bind:class="{'line-show': isShowScenicLine}">
            <p class="title">前往 苏州</p>
            <ul class="lbox" @click="selectLine()">
                <li class="text">
                    <span class="text-ellipsis">上海 出发</span>
                </li>
            </ul>
            <p class="title">苏州 出发</p>
            <ul class="lbox">
                <li class="text">
                    <span class="text-ellipsis">前往 上海</span>
                </li>
            </ul>
        </div>
        <div class="line-mask" v-show="isShowScenicLine" @click="isShowScenicLine = !isShowScenicLine"></div>
    </div>
</template>

<script>
    // import Vue from 'vue'

    import { Bridge, City, Refresh } from 'tcvui2' //非混合
    import { mapState, mapActions } from 'vuex';
    import tempData from '../utils/temp';

    const { bar, util, map, user } = Bridge;  //非混合
    // Vue.component(Refresh.name,Refresh); //非混合
    // Vue.component(City.name, City); //非混合
    // Vue.component(bar.setNavbar.name, bar.setNavbar)

    export default {
        data() {
            return {
                scenicList: [],
                scenicLineList: {},
                isShowScenicLine: false,
                isHasScenic: true,
                isHasSetoff: false,
                isHasGoto: false
            }
        },
        mounted() {
            this.getScenicData();
            console.log(JSON.stringify(this.cityInfo));
        },
        computed: {
            ...mapState([
                'cityInfo'
            ]),
        },
        methods: {
            getScenicData() {
                let data = tempData.scenicList,
                    searchScenic = [],
                    isHas = false;

                if (data.code == 0) {

                    if (this.cityInfo.sid && this.cityInfo.sname) {
                        for (let son of data.value) {
                            if (son.siid == this.cityInfo.sid && son.siname == this.cityInfo.sname) {
                                searchScenic.push(son);
                                isHas = true;
                                break;
                            }
                        }

                        if (isHas) {
                            this.scenicList = searchScenic;
                            this.isHasScenic = true;
                        } else {
                            this.isHasScenic = false;
                        }

                    } else {
                        this.scenicList = data.value;
                        this.isHasScenic = true;
                    }

                } else {
                    this.isHasScenic = false;
                }
            },
            getScenicLineData() {
                let data = tempData.scenicLineList;

                if (data.code == 0) {
                    this.scenicLineList = data.value;
                    console.log(this.scenicLineList);
                }
            },
            selectScenic() {
                this.isShowScenicLine = !this.isShowScenicLine;
                this.getScenicLineData();
            },
            selectLine() {
                // this.setcityInfo("乌鲁木齐")


                // fetch('http://m.qa.ly.com/tcsceniccarbasiccontainer/clientResourceApi/queryCity').then(function(response) {
                // fetch('/clientResourceApi/queryCity').then(function(response) {
                //     console.log(111)
                //     console.log(response)
                //     return response.json();
                // }).then(function(json) {
                //     console.log(222)
                //     console.log(json)
                // }).catch(function(error) {
                //     console.log(333);
                //     console.log(error)
                // })


                // util.getData({
                //     requrl: 'http://m.qa.ly.com/tcsceniccarbasiccontainer/clientResourceApi',
                //     reqbody: {},
                //     servicename: 'queryCity'
                // }, 'http://m.qa.ly.com/tcsceniccarbasiccontainer/clientResourceApi').then(function(data) {
                //     alert(data)
                // }).catch(function(e){
                //     console.log(111)
                //     console.log(e)
                // })
            }
        }
    }

</script>

<style lang="less" scoped>
    .index {
        position: relative;
        overflow-x: hidden;
        overflow-y: scroll;
        background-color: #fff;
        height: 100%;
        -webkit-overflow-scrolling: touch;
    }

    .top {
        padding: 50px 15px 0;
        margin-bottom: 10px;
        line-height: 1;
        font-size: 16px;
        color: #333;

        .scenic {
            display: block;
            position: relative;
            background-color: #f5f5f5;
            height: 40px;
            padding-left: 15px;
            margin-bottom: 40px;
            border-radius: 9999px;
            line-height: 40px;
            font-size: 14px;

            .sceicon {
                font-family: 'iconfont' !important;
                position: absolute;
                top: 0;
                right: 0;
                background-color: @main-color;
                width: 40px;
                height: 40px;
                border-radius: 50%;
                text-align: center;
                font-size: 18px;
                color: #fff;
            }

            &:visited {
                color: #333;
            }
        }

        .travel {
            overflow: hidden;
            height: 28px;
            line-height: 28px;

            .ticon {
                position: relative;
                width: 70px;
                height: 28px;
                padding-left: 5px;
                margin-right: 9px;
                font-size: 14px;
                color: #fff;

                span {
                    position: relative;
                    z-index: 10;
                }

                .common() {
                    content: '';
                    position: absolute;
                    top: 0;
                    left: -5px;
                    right: 0;
                    height: 28px;
                    z-index: 9;
                }

                &::before {
                    .common;
                    background-color: lighten(@main-color, 25%);
                    -webkit-transform: skew(-20deg);
                    transform: skew(-20deg);
                }

                &::after {
                    .common;
                    background-color: @main-color;
                    -webkit-transform: skew(20deg);
                    transform: skew(20deg);
                }
            }
        }
    }

    .station {
        padding: 0 15px;

        .box {
            position: relative;
            margin-bottom: 10px;

            .title {
                position: absolute;
                bottom: 10px;
                left: 10px;
                font-size: 20px;
                font-weight: 700;
                color: #fff;
                text-shadow: 1px 1px 1px #333;
            }

            .image {
                overflow: hidden;
                height: 150px;
                border-radius: 3px;

                img {
                    position: relative;
                    top: 50%;
                    width: 100%;
                    min-height: 100%;
                    -webkit-transform: translateY(-50%);
                    transform: translateY(-50%);
                }
            }
        }

        .tip-common() {
            position: absolute;
            top: -2px;
            right: -2px;
            z-index: 80;
        }

        .tip-textcommon() {
            position: absolute;
            width: 25px;
            margin: auto;
            line-height: 1.2;
            text-align: center;
            font-size: 12px;
            color: #fff;
            -webkit-transform: rotate(19deg);
            transform: rotate(19deg);
        }

        .tipfour {
            .tip-common;
            background: url(http://img1.40017.cn/touch/bus/wx_bus/sctips.png) 0 0 no-repeat;
            background-size: 44px 65px;
            width: 44px;
            height: 65px;

            .tiptext {
                .tip-textcommon;
                left: 9px;
                bottom: 8px;
            }
        }

        .tiptwo {
            .tip-common;
            background: url(http://img1.40017.cn/touch/bus/wx_bus/sctips2.png) 0 0 no-repeat;
            background-size: 40px 52px;
            width: 40px;
            height: 52px;

            .tiptext {
                .tip-textcommon;
                left: 7px;
                bottom: 9px;
            }
        }

        .none {
            padding: 20px 0;
            text-align: center;
            font-weight: 400;
            font-size: 16px;
            color: #999;
        }
    }

    .float {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        background-color: #fff;
        padding: 10px 15px;
        z-index: 16;

        .name {
            position: relative;
            background-color: #f5f5f5;
            height: 40px;
            padding-left: 40px;
            border-radius: 5px;
            line-height: 40px;
            font-size: 14px;
            color: #333;

            &::after {
                content: "\e648";
                font-family: iconfont !important;
                position: absolute;
                top: 0;
                left: 12px;
                font-size: 16px;
                color: #333;
            }
        }
    }

    .line {
        position: fixed;
        left: 0;
        right: 0;
        bottom: 0;
        overflow-x: hidden;
        overflow-y: scroll;
        background-color: #fff;
        max-height: 380px;
        padding-bottom: 20px;
        line-height: 1;
        font-size: 14px;
        color: #333;
        -webkit-transition: all .3s ease-in-out;
        transition: all .3s ease-in-out;
        -webkit-transform: translate3d(0, 200%, 0);
        transform: translate3d(0, 200%, 0);
        -webkit-overflow-scrolling: touch;
        z-index: 96;

        .title {
            padding: 35px 0 5px 15px;
            color: #999;
        }

        .lbox {
            padding-left: 15px;
            font-size: 0;

            .text {
                display: inline-block;
                width: 50%;
                height: 50px;
                padding: 10px 15px 0 0;
                text-align: center;
                font-size: 14px;
                overflow: hidden;

                span {
                    display: block;
                    position: relative;
                    height: 40px;
                    line-height: 40px;
                    padding: 0 5px;

                    &::after {
                        position: absolute;
                        content: "";
                        width: 200%;
                        height: 200%;
                        top: 0;
                        left: 0;
                        border: 1px solid #dcdcdc;
                        border-radius: 5px;
                        -webkit-transform: scale(.5, .5);
                        transform: scale(.5, .5);
                        -webkit-transform-origin: 0 0;
                        transform-origin: 0 0;
                        -webkit-box-sizing: border-box;
                        box-sizing: border-box;
                    }
                }
            }
        }
    }

    .line-show {
        -webkit-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0);
    }

    .line-mask {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0, 0, 0, .5);
        z-index: 95;
    }
</style>
