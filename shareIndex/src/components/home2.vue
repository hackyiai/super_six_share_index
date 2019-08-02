<template>
    <div class="home2">
        <div class="box">
            <div class="p1">
                {{phoneType == 'ios'?'苹果手机' : '安卓手机'}}
            </div>
            <div class="p2">
                <span class="sp1">1</span>
                <span>点击左上角的</span>
                <span class="sp3">按钮</span>
            </div>
            <div class="p3">
                <span class="sp1">2</span>
                <span class="sp2" :class="phoneType == 'ios'?'ios' : 'android'">选择</span>
            </div>
            <div class="arrow">
                <img src="../images/icon_arrow.png" alt="">
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props:['phoneType']
}
</script>

<style lang="scss" scoped>
    .home2{
        width: 100%;
        height: 100%;
        background: #4e4c4d;
        font-size: .38rem;
        line-height: .7rem;

        .box{
            width: max-content;
            margin: 0 auto;
            position: relative;

            .p1{
                margin-top: .6rem;
                color: #e15829;
                font-weight: bold;
            }

            .p2{
                color: #FFFFFF;
                margin: .2rem 0 .55rem 0;
                
                .sp3{
                    display: inline-block;
                    background: url('../images/icon_menu.png') .2rem center no-repeat;
                    background-size: contain;
                    padding-left: 1.4rem;
                }
            }

            .sp1{
                background: #e15829;
                color: #FFFFFF;
                font-size: .32rem;
                border-radius: 50%;
                padding: .09rem .2rem; 
                margin-right: .2rem;
            }

            .p3{
                
                .sp2{
                    padding-right: 4rem;
                    display: inline-block;
                    color: #FFFFFF;
                    
                    &.android{
                        background: url('../images/icon_open.png') .9rem center no-repeat;
                        background-size: contain;
                    }

                    &.ios{
                        background: url('../images/icon_safari.png') .9rem center no-repeat;
                        background-size: contain;
                    }
                }
            }

            .arrow{
                width: 1.07rem;
                height: 1.4rem;
                position: absolute;
                top: -.5rem;
                right: -.7rem;

                img{
                    max-height: 100%;
                }
            }
        }
    }
</style>

