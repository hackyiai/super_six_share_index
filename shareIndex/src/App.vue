<template>
    <div id="app">
        <Home2 v-if="isQQAndWx" :phoneType='phoneType'></Home2>
        <Home v-else></Home>
    </div>
</template>

<script>
import Home from "./components/home.vue";
import Home2 from "./components/home2.vue";

export default {
    name: "app",
    data() {
        return {
            phoneType: "",
            isQQAndWx: false
        };
    },
    components: {
        Home,
        Home2
    },
    beforeMount() {
        //判断访问终端
        var u = navigator.userAgent;
        if (u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/)) {
            this.phoneType = "ios";
        } else if (u.indexOf("Android") > -1 || u.indexOf("Adr") > -1) {
            this.phoneType = "android";
        }
        if(u.indexOf('MicroMessenger') > -1 || u.indexOf('QQ') > -1){
            this.isQQAndWx = true;
        }
    }
};
</script>

<style lang="scss">
@import "./styles/base.scss";
</style>
