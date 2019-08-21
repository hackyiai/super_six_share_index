<template>
    <div class="home">
        <div class="wrapper">
            <div class="liuhe_head">
                <div class="liuhe_headleft">
                    <img src="../images/logo.png" class="logoimg" @click="jumptocommon" />
                </div>
                <div class="liuhe_headmiddle">
                    <div class="invitecode">
                        <span>邀请码：</span>
                        <span id="code">{{datalist.invitationCode}}</span>
                    </div>
                    <div
                        class="copybtn"
                        ref="copy"
                        data-clipboard-action="copy"
                        data-clipboard-target="#code"
                        @click="copyCode"
                    >复制</div>
                </div>
                <div
                    class="liuhe_headright"
                    :style="{backgroundImage: 'url(' + bg2 + ')' }"
                    @click="jumptocommon"
                >APP下载</div>
            </div>
            <!-- 广播 -->
            <div class="noticeList">
                <div class="notice" v-if="noticeList && noticeList.length">{{noticeList[0].content}}</div>
            </div>

            <div class="liuhe_nav" v-if="moduleType == 1" @click="jumptocommon">
                <div class="liuhe_navleft">
                    <div class="navleftup">
                        <img v-if="personimg" :src="personimg " />
                        <img v-else src="../images/defaultimg.png" />
                    </div>
                    <div class="navleftdown">
                        <p class="p_up">{{nickname}}</p>
                        <p class="p_down">
                            <span class="sleft">楼主</span>
                            <span class="sright">{{diffTimes}}</span>
                        </p>
                    </div>
                </div>
                <div class="liuhe_navright">
                    <img src="../images/shang.png" />
                    <div class="shouchang">已收藏</div>
                    <div class="attention">关注楼主</div>
                </div>
            </div>

            <div class="title" v-if="moduleType != 1">
                {{ '第' + periods + '期' + ':' +title}}
            </div>

            <div class="rule" v-if="moduleType == 2">公式規律持續計算中，如本期公式結果未出，請回頭再看</div>

           <!-- 發帖人 -->
            <div id="uInfo" v-if="moduleType == 5">
                <div class="uInfo-t">
                    作者：<span>管理员</span>
                </div>
                <div class="uInfo-b">
                    <div class="uInfo-b-l">
                        時間：<span>{{createTime}}</span>
                    </div>
                    <div class="uInfo-b-r">
                        閲讀：<span>{{browsePoint}}</span>次
                    </div>
                </div>
                <div class="update update-mark"></div>
            </div>

            <div class="main">
                <!-- <h2>{{title}}</h2> -->
                <p class="maincontent" v-html="postcontent"></p>
            </div>
            <div class="contentimgsinge" v-if="ISsingeimg">
                <div v-if="newimglist.length > 1">
                    <div
                        :style="{'width':imgWidth + 'rem','height': imgWidth + 'rem'}"
                        v-for="(itemy,indexy) in newimglist"
                        :key="indexy"
                        class="singeimgbox"
                    >
                        <img :src="itemy" class="singeimg" />
                    </div>
                </div>
                <div v-else>
                    <img :src="newimglist[0]" class="singeimg" :style="{'max-width':imgWidth + 'rem','max-height': imgWidth + 'rem','width':'auto','height':'auto'}"/>
                </div>
            </div>
            <div class="comment" v-for="(item,index) in datalist.replyList" :key="index">
                <div class="commentleft">
                    <img :src="item.senderHeadImg" />
                </div>
                <div class="commentright">
                    <div class="commentright_up">
                        <span>{{item.senderNickname}}</span>
                    </div>
                    <div class="commenttime">
                        <span class="times">{{commenttime[index]}}</span>
                        <div class="commenttimeright">
                            <div class="comment_zan" @click="jumptocommon">
                                <img src="../images/zan.png" />
                                <span>{{item.nicePoint}}</span>
                            </div>
                            <img src="../images/pinglun.png" class="pinglun" @click="jumptocommon" />
                        </div>
                    </div>
                    <div class="commentcontent">
                        <p class="commenttitle">{{item.content}}</p>
                        <div v-for="(itemss,indexss) in imglist[index]" :key="indexss">
                            <img :src="itemss" class="commentimg" />
                        </div>
                    </div>
                    <div class="commentreply">
                        <div v-for="(items,indexs) in showsublist[index]" :key="indexs">
                            <p>
                                <img :src="items.senderHeadImg" />
                                <label>
                                    {{items.senderNickname}}
                                    <span>:</span>
                                </label>
                            </p>
                            <p>{{items.content}}</p>
                        </div>
                        <div v-show="isShow==index">
                            <div v-for="(itemx,indexx) in hidesublist[index]" :key="indexx">
                                <p>
                                    <img :src="itemx.senderHeadImg" />
                                    <label>
                                        {{itemx.senderNickname}}
                                        <span>:</span>
                                    </label>
                                </p>
                                <p>{{itemx.content}}</p>
                            </div>
                            <span class="hidemore" @click.stop="scale(item.senderId)">收起</span>
                        </div>
                        <p
                            class="showmore"
                            v-show="isShow!=index"
                            v-if="hidenum[index]!=0"
                            @click.stop="spread(index)"
                        >
                            查看全部
                            <span>{{hidenum[index]}}</span>条回复
                            <img src="../images/rightarrow.png" />
                        </p>
                    </div>
                </div>
            </div>
            <div class="kongbai"></div>
            <div class="liuhe-footer">
                <img src="../images/defaultimg.png" class="footerimg" />
                <div class="textbox">
                    <input type="text" placeholder="说点什么……" class="say" disabled />
                </div>
                <div class="footerright">
                    <ul class="footerlist">
                        <li class="footerpointer" @click="jumptocommon">
                            <img src="../images/icons_Fabulous@3x.png" />
                            <span>{{nicepoint}}</span>
                        </li>
                        <li class="footercomment" @click="jumptocommon">
                            <img src="../images/icons_liuyan@3x.png" />
                            <span>{{replypoint}}</span>
                        </li>
                        <li class="singetubiao" @click="jumptocommon">
                            <img src="../images/icons_shoucang@3x.png" />
                        </li>
                        <li class="singetubiao" @click="jumptocommon">
                            <img src="../images/icons_fenxiang@3x.png" />
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import Bg2 from "@/images/icon_Button.png";
export default {
    name: "home",
    data() {
        return {
            ISsingeimg: false,
            nicepoint: "",
            replypoint: "",
            title: "",
            periods:-1,
            nickname: "",
            personimg: "",
            commenttime: [],
            diffTimes: "",
            isShow: -1,
            datalist: [],
            imglist: [],
            postcontent: "",
            commentsubList: [],
            showsublist: [],
            hidesublist: [],
            hidenum: [],
            newimglist: [],
            copyBtn: null,
            bg2: Bg2,
            imgWidth: 0,
            noticeList: null,
            moduleType: 0,
            createTime:'',
            browsePoint:0
        };
    },
    methods: {
        spread(x) {
            this.isShow = x;
        },
        scale(x) {
            this.isShow = x;
        },
        jumptocommon() {
            location.href = "http://www.61888.com";
        },
        copyCode() {
            let _this = this;
            let clipboard = _this.copyBtn;
            clipboard.on("success", function() {
                _this.$message({
                    /*这是使用了element-UI的信息弹框*/
                    message: "复制成功！",
                    type: "success"
                });
            });
            clipboard.on("error", function() {
                _this.$message({
                    message: "复制失败，请手动选择复制！",
                    type: "error"
                });
            });
        },
        // 用0占位
        placeholder(value, num) {
            if (String(value).length < num) {
                value = this.placeholder(("0" + value), num);
            }
            return value;
        },
        /**
         * 根據毫秒數返回年、月、日、時、分、秒
         * @param {int} time 
         */
        timeFormat(time) {
            var myDate = new Date(time);
            return {
                year: myDate.getFullYear(),
                month: this.placeholder(myDate.getMonth() + 1, 2),
                date: this.placeholder(myDate.getDate(), 2),
                hours: this.placeholder(myDate.getHours(), 2),
                min: this.placeholder(myDate.getMinutes(), 2),
                sec: this.placeholder(myDate.getSeconds(), 2)
            }
        },
        parseQueryString(url) {
            var obj = {};
            var keyvalue = [];
            var key = "",
                value = "";
            var paraString = url.substring(url.indexOf("?") + 1, url.length).split("&");
            for (var i in paraString) {
                keyvalue = paraString[i].split("=");
                key = keyvalue[0];
                value = keyvalue[1];
                obj[key] = value;
            }
            return obj;
        }
    },
    mounted() {
        this.copyBtn = new this.clipboard(this.$refs.copy);

        var obj = this.parseQueryString(location.href);

        var str = obj.params;
        var uri = "http://api.lhbbapp.com/posts/getPostsDetails";
        if (obj.devModel) {
            var devModel = obj.devModel;
            //根据devModel = 1 调用测试环境的接口
            if (devModel && devModel == 1) {
                uri = "http://app.lhbbapp.com:8080/posts/getPostsDetails";
            }
        }
        var plate = obj.plate;
        
        switch (plate) {
            case "capture": // 高手論壇列表
            case "exchange": // 精料轉帖列表
                this.moduleType = 1;
                break;
            case "formula": // 公式规律列表
                this.moduleType = 2;
                break;
            case "mystery": // 每期玄机列表
                this.moduleType = 3;
                break;
            case "image_depot": // 六合图库列表
                this.moduleType = 4;
                break;
            case "humor_guess": // 幽默猜测列表
            case "run_dog": // 跑狗玄机列表
            case "four_unlike": // 四不像列表
                this.moduleType = 5;
                break;
        }
        axios
            .get(uri, {
                params: {
                    params: str
                }
            })
            .then(res => {
                this.datalist = res.data.data;
                this.personimg = this.datalist.posts.senderHeadImg;
                this.nickname = this.datalist.posts.senderNickname;
                this.title = this.datalist.posts.title;
                this.periods = this.placeholder(this.datalist.posts.periods,3);
                let myDate = this.timeFormat(this.datalist.posts.createTime);
                this.createTime = myDate.year + '-' + myDate.month + '-' + myDate.date + '  ' + myDate.hours + ':' + myDate.min;
                this.datalist.posts.browsePoint && (this.browsePoint = this.datalist.posts.browsePoint);
                this.postcontent = this.datalist.posts.content;
                this.replypoint = this.datalist.posts.replyPoint;
                this.noticeList = this.datalist.noticeList;
                if (this.datalist.posts.nicePoint > 10000) {
                    this.nicepoint =
                        this.datalist.posts.nicePoint / 10000 + "万";
                } else if (
                    this.datalist.posts.nicePoint > 1000 &&
                    this.datalist.posts.nicePoint < 10000
                ) {
                    this.nicepoint =
                        this.datalist.posts.nicePoint / 1000 + "千";
                } else {
                    this.nicepoint = this.datalist.posts.nicePoint;
                }
                this.newimglist =
                    (this.datalist.posts.img &&
                        this.datalist.posts.img.split(",")) ||
                    [];

                if (this.newimglist.length > 0) {
                    this.ISsingeimg = true;
                    // 换算为rem
                    var boxwidth = (750 - 20 * 2) / 100;
                    if (this.newimglist.length < 3) {
                        this.imgWidth = boxwidth / this.newimglist.length;
                    } else {
                        this.imgWidth = boxwidth / 3;
                    }
                } else {
                    this.ISsingeimg = false;
                }
                var difftime =
                    (Number(new Date().getTime()) -
                        Number(this.datalist.posts.createTime)) /
                    1000;
                var diffhour = difftime / 60 / 60;
                if (diffhour < 0.5) {
                    this.diffTimes = "半小时内";
                } else if (diffhour > 0.5 && diffhour < 1) {
                    this.diffTimes = "半小时前";
                } else if (diffhour > 1 && diffhour < 2) {
                    this.diffTimes = "1小时前";
                } else if (diffhour > 2 && diffhour < 24) {
                    this.diffTimes = "2小时前";
                } else {
                    this.diffTimes = "1天前";
                }
                for (var i = 0; i < this.datalist.replyList.length; i++) {
                    var riqi = new Date(this.datalist.replyList[i].createTime);
                    var ymr, sfm;
                    if (
                        Number(riqi.getDate()) >= 10 &&
                        Number(riqi.getMonth()) >= 9
                    ) {
                        ymr =
                            riqi.getFullYear() +
                            "-" +
                            (riqi.getMonth() + 1) +
                            "-" +
                            riqi.getDate();
                    } else {
                        if (Number(riqi.getMonth()) < 9) {
                            ymr =
                                riqi.getFullYear() +
                                "-0" +
                                (riqi.getMonth() + 1) +
                                "-" +
                                riqi.getDate();
                        }
                        if (Number(riqi.getDate()) < 10) {
                            ymr =
                                riqi.getFullYear() +
                                "-" +
                                (riqi.getMonth() + 1) +
                                "-0" +
                                riqi.getDate();
                        }
                        if (
                            Number(riqi.getDate()) < 10 &&
                            Number(riqi.getMonth()) < 9
                        ) {
                            ymr =
                                riqi.getFullYear() +
                                "-0" +
                                (riqi.getMonth() + 1) +
                                "-0" +
                                riqi.getDate();
                        }
                    }
                    if (
                        Number(riqi.getHours()) >= 10 &&
                        Number(riqi.getMinutes() >= 10)
                    ) {
                        sfm = riqi.getHours() + ":" + riqi.getMinutes();
                    } else {
                        if (Number(riqi.getHours()) < 10) {
                            sfm =
                                "0" + riqi.getHours() + ":" + riqi.getMinutes();
                        }
                        if (Number(riqi.getMinutes()) < 10) {
                            sfm = riqi.getHours() + ":0" + riqi.getMinutes();
                        }
                        if (
                            Number(riqi.getMinutes()) < 10 &&
                            Number(riqi.getHours()) < 10
                        ) {
                            sfm =
                                "0" +
                                riqi.getHours() +
                                ":0" +
                                riqi.getMinutes();
                        }
                    }
                    this.commenttime.push(ymr + " " + sfm);
                    // console.log(this.datalist.replyList[i].img);
                    if (this.datalist.replyList[i].img != null) {
                        this.imglist[i] = this.datalist.replyList[i].img.split(
                            ","
                        );
                    } else {
                        this.imglist[i] = null;
                    }
                    var newlist = this.datalist.replyList[i].subList; //newlist仍然是个数组
                    this.commentsubList.push(newlist); //这样this.commentsubList是一个二维数组
                    //console.log(newlist)
                    if (newlist != null) {
                        if (newlist.length >= 2) {
                            this.showsublist[i] = newlist.slice(0, 2);
                            this.hidesublist[i] = newlist.slice(
                                2,
                                newlist.length
                            );
                            this.hidenum[i] = newlist.length;
                            // console.log(newlist.length)
                        } else {
                            this.showsublist[i] = newlist;
                            this.hidesublist[i] = null;
                            this.hidenum[i] = 0;
                        }
                    } else {
                        this.showsublist[i] = null;
                        this.hidesublist[i] = null;
                        this.hidenum[i] = 0;
                    }
                }
            });
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss'>
body {
    overflow: auto;
    .wrapper {
        overflow-y: scroll;
        background: #f5f5f5;
        .liuhe_head {
            position: fixed;
            top: 0;
            width: 100%;
            box-sizing: border-box;
            padding: 0.2rem;
            border-bottom: 2px solid #f4f4f4;
            background: #fff;
            overflow: hidden;
            z-index: 2;

            .liuhe_headleft {
                float: left;
                width: 23%;
                vertical-align: middle;

                .logoimg {
                    width: 0.84rem;
                    height: 0.84rem;
                    float: left;
                }

                .logotitle {
                    float: left;
                    width: 1.16rem;
                    margin: 0.2rem 0 0 0.2rem;

                    span {
                        font-size: 0.28rem;
                    }
                }
            }

            .liuhe_headmiddle {
                float: left;
                width: 50%;
                height: 0.84rem;
                line-height: 0.84rem;
                text-align: center;
                font-size: 0.28rem;
                vertical-align: middle;
                .invitecode {
                    display: inline-block;
                    background: #f7caa9;
                    width: 70%;
                    color: #d75227;
                    border-bottom-left-radius: 0.42rem;
                    border-top-left-radius: 0.42rem;
                    text-align: center;
                }
                .copybtn {
                    display: inline-block;
                    background: #fd5378;
                    width: 30%;
                    color: #fff;
                    border-bottom-right-radius: 0.42rem;
                    border-top-right-radius: 0.42rem;
                    text-align: center;
                }
            }

            .liuhe_headright {
                float: right;
                width: 22%;
                height: 0.64rem;
                background: #e01022;
                border-radius: 0.2rem;
                color: white;
                line-height: 0.64rem;
                text-align: center;
                font-size: 0.2rem;
                margin-top: 0.1rem;
            }
        }

        .noticeList {
            margin-top: 1.3rem;
            background: #ffffff;

            .notice {
                font-size: 0.14rem;
                margin-bottom: 0.05rem;
                line-height: 0.5rem;
                background: url("../images/notice.png") 0.2rem center no-repeat;
                background-size: 0.4rem 0.4rem;
                padding-left: 0.7rem;
                overflow: hidden; //超出的文本隐藏
                text-overflow: ellipsis; //溢出用省略号显示
                white-space: nowrap; //溢出不换
            }
        }

        .title{
            font-size: .3rem;
            font-weight: bold;
            text-align: center;
            line-height: .7rem;
            overflow: hidden; //超出的文本隐藏
            text-overflow: ellipsis; //溢出用省略号显示
            white-space: nowrap; //溢出不换
        }

        .liuhe_nav {
            padding: 0.2rem;
            margin-bottom: 0.2rem;
            overflow: hidden;
            background: #fff;
            .liuhe_navleft {
                font-size: 0.28rem;
                float: left;
                width: 46%;
                .navleftup {
                    float: left;
                    img {
                        width: 0.87rem;
                        height: 0.87rem;
                    }
                }
                .navleftdown {
                    float: left;
                    width: 50%;
                    overflow: hidden;
                    margin-left: 0.12rem;
                    .p_up {
                        color: #303030;
                        height: 0.44rem;
                        line-height: 0.44rem;
                        font-weight: 700;
                    }
                    .p_down {
                        color: #696969;
                        height: 0.44rem;
                        line-height: 0.44rem;
                        overflow: hidden;
                        .sleft {
                            float: left;
                        }
                        .sright {
                            float: right;
                        }
                    }
                }
            }

            .liuhe_navright {
                width: 54%;
                font-size: 0.28rem;
                float: right;
                margin-top: 0.1rem;
                img {
                    float: left;
                    width: 20%;
                }
                .attention {
                    float: right;
                    width: 40%;
                    height: 0.6rem;
                    background: #ff6781;
                    border-radius: 0.16rem;
                    text-align: center;
                    margin-top: 0.06rem;
                    line-height: 0.6rem;
                    color: white;
                }
                .shouchang {
                    float: left;
                    margin: 0.06rem 0 0 0.08rem;
                    width: 35%;
                    height: 0.6rem;
                    background: #ff6781;
                    border-radius: 0.16rem;
                    text-align: center;
                    line-height: 0.6rem;
                    color: white;
                }
            }
        }

        .rule{
            display: block;
            height: 0.6rem;
            font-size: 0.26rem;
            color: rgba(255, 104, 128, 1);
            background: rgba(255, 253, 237, 1);
            overflow: hidden;
            text-align: center;
            line-height: 0.6rem;
        }

        #uInfo{
            background: #FFFFFF;
            padding: .17rem .3rem;
            position: relative;
        }

        #uInfo .uInfo-t{
            height:0.4rem;
            font-size:0.24rem;
            font-family:PingFang-SC-Medium;
            font-weight:500;
            color:rgba(102,102,102,1);
        }

        #uInfo .uInfo-b{
            height:0.4rem;
            font-size:0.24rem;
            font-family:PingFang-SC-Medium;
            font-weight:500;
            color:rgba(153,153,153,1);
        }

        #uInfo .uInfo-b-l{
            float: left;
        }

        #uInfo .uInfo-b-r{
            float: right;
        }

        .main {
            padding: 0.2rem;
            background: #fff;

            h2 {
                text-align: center;
                padding: 0.1rem 0;
                font-size: 0.44rem;
                color: #353535;
                font-weight: 700;
            }
            .maincontent {
                white-space: pre-line;
                font-size: 0.32rem;
                color: #4a4a4a;
            }
        }
        .contentimg {
            background: #fff;
            padding: 0 0.2rem 0.4rem 0.2rem;
            overflow: hidden;
            div {
                width: 1.8rem;
                height: 1.8rem;
                float: left;
                margin: 0 0.1rem 0.1rem 0;
                img {
                    width: 100%;
                    height: 100%;
                }
            }
            div:nth-of-type(3n) {
                margin-right: 0.2rem;
            }
        }
        .contentimgsinge {
            box-sizing: border-box;
            width: 100%;
            background: #fff;
            padding: 0 0.2rem 0.4rem 0.2rem;
            overflow: hidden;

            .singeimgbox {
                float: left;
                overflow: hidden;
                margin-bottom: 0.1rem;
                box-sizing: border-box;

                &:nth-child(3n-1) {
                    padding: 0 0.1rem;
                }

                img {
                    width: 100%;
                    height: 100%;
                }
            }
        }
        .comment {
            padding: 0.2rem;
            overflow: hidden;
            background: #fff;
            .commentleft {
                width: 10%;
                float: left;
                img {
                    margin-top: 0.1rem;
                    width: 0.64rem;
                    height: 0.64rem;
                }
            }
            .commentright {
                width: 90%;
                margin-left: 0.66rem;
                .commentright_up {
                    overflow: hidden;
                    span {
                        font-size: 0.36rem;
                        color: #6b6b6b;
                        float: left;
                    }
                }
                .commenttime {
                    font-size: 0.2rem;
                    color: #9a9a9a;
                    overflow: hidden;
                    .commenttimeright {
                        float: right;
                        overflow: hidden;
                        .comment_zan {
                            float: left;
                            position: relative;
                            img {
                                margin-right: 0.56rem;
                                width: 0.34rem;
                                height: 0.34rem;
                            }
                            span {
                                position: absolute;
                                color: #5f7298;
                                right: 0.26rem;
                                top: -0.06rem;
                                font-size: 0.16rem;
                            }
                        }
                        .pinglun {
                            float: right;
                            width: 0.34rem;
                            height: 0.34rem;
                        }
                    }
                }
                .commentcontent {
                    margin-left: 0.14rem;
                    .commenttitle {
                        font-size: 0.32rem;
                        color: #252525;
                    }
                    div {
                        overflow: hidden;
                        width: 30%;
                        float: left;
                        .commentimg {
                            width: 1.3rem;
                            height: 1.3rem;
                            float: left;
                        }
                    }
                }
                .commentreply {
                    float: left;
                    width: 100%;
                    background: #f5f5f5;
                    padding: 0.1rem;
                    font-size: 0.28rem;
                    .showmore {
                        img {
                            width: 0.24rem;
                            height: 0.2rem;
                        }
                    }
                    p {
                        margin-bottom: 0.14rem;
                        line-height: 0.32rem;
                        color: #5d5d5d;
                        label {
                            color: #169bd4;
                            font-size: 0.32rem;
                        }
                        img {
                            width: 0.36rem;
                            height: 0.36rem;
                        }
                    }
                }
            }
        }
        .kongbai {
            width: 100%;
            height: 0.66rem;
        }
        .liuhe-footer {
            position: fixed;
            width: 100%;
            left: 0;
            bottom: 0;
            padding: 0.2rem 0;
            background: #fff;
            overflow: hidden;
            .footerimg {
                float: left;
                margin-left: 0.2rem;
                width: 0.64rem;
                height: 0.64rem;
            }
            .textbox {
                float: left;
                border: 2px solid #000;
                height: 0.64rem;
                line-height: 0.64rem;
                border-radius: 0.32rem;
                width: 30%;
                background: #ebebe4;
                margin: 0 0 0 0.34rem;
                .say {
                    height: 0.5rem;
                    border: 0;
                    outline: none;
                    width: 90%;
                    margin: -0.2rem 0 0 0.1rem;
                }
            }
            .footerright {
                float: right;
                width: 48%;
                margin: 0.1rem 0 0 0.1rem;
                .footerlist {
                    float: left;
                    width: 100%;
                    .footerpointer {
                        width: 28%;
                        position: relative;
                        span {
                            position: absolute;
                            top: -0.1rem;
                        }
                    }
                    .footercomment {
                        width: 28%;
                        position: relative;
                        span {
                            position: absolute;
                            top: -0.1rem;
                        }
                    }
                    .singetubiao {
                        text-align: center;
                    }
                    .singetubiao:nth-of-type(3) {
                        text-align: left;
                    }
                    li {
                        float: left;
                        width: 22%;
                        img {
                            width: 0.44rem;
                            height: 0.44rem;
                        }
                    }
                }
            }
        }
    }
}
</style>
