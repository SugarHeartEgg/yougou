<template>
    <div>
        <div class="header">
            <!-- 搜索框 -->
            <icon type="search" size="16"></icon>
            <input type="text" placeholder="搜索">
        </div>
        <!-- 轮播图 -->
        <swiper indicator-dots autoplay circular>
            <swiper-item v-for="item in swiperList" :key="item.name">
                <image mode="aspectFill" :src="item.image_src"></image>
            </swiper-item>
        </swiper>

        <!-- 分类区域 -->
        <ul class="cate-box">
            <li class="item" v-for="item in cateList" :key="item.name">
                <img class="img" :src="item.image_src" alt="">
                <span class="span">{{item.name}}</span>
            </li>
        </ul>

        <!-- 楼层区域 -->
        <div class="section" v-for="(item, index) in floorList" :key="index">
            <div class="title">
                <span>{{item.floor_title.name}}</span>
                <img class="img" :src="item.floor_title.image_src" alt="">
            </div>
            <div class="content">
                <image v-for="(it, i) in item.product_list" :key="i" :src="it.image_src"></image>
            </div>
        </div>
    </div>
</template>

<script>
// 导入ajax模块
import tool from "../../utils/index.js";

export default {
    // 数据
    data: function() {
        return {
            // 轮播图数据
            swiperList: [],
            // 分类区域
            cateList: [],
            //楼层区域
            floorList: []
        };
    },
    // 生命周期函数
    created() {
        // // 获取轮播图数据
        // tool
        //     .thenAjax({
        //         url: "api/public/v1/home/swiperdata"
        //     })
        //     .then(res => {
        //         // 绑定数据
        //         // console.log(res);
        //         this.swiperList = res.data.message;
        //     });
        (async () => {
            // await
            let data1 = await tool.thenAjax({
                url: "api/public/v1/home/swiperdata"
            });
            // console.log(data1);
            this.swiperList = data1.data.message;
            let data2 = await tool.thenAjax({
                url: "api/public/v1/home/catitems"
            });
            // console.log(data2);
            this.cateList = data2.data.message;
            let data3 = await tool.thenAjax({
                url: "api/public/v1/home/floordata"
            });
            // console.log(data3);
            this.floorList = data3.data.message;
        })();
    }
};
</script>

<style lang="less">
.header {
    padding: 20rpx 16rpx;
    position: relative;
    background-color: #ff2d4a;
    > icon {
        position: absolute;
        top: 34rpx;
        left: 50%;
        transform: translateX(-200%);
        z-index: 998;
    }
    input {
        height: 60rpx;
        border-radius: 10rpx;
        background-color: white;
        width: 100%;
        color: #bbb;
        text-align: center;
        font-size: 24rpx;
    }
}
// 轮播图
swiper {
    height: 340rpx;
    swiper-item {
        image {
            display: block;
            width: 100%;
            height: 100%;
        }
    }
}
// 分类选项
.cate-box {
    padding-top: 24rpx;
    padding-bottom: 30rpx;
    display: flex;
    .item {
        flex: 1;
        .img {
            width: 100rpx;
            height: 100rpx;
            display: block;
            margin: 0 auto 20rpx;
        
        }
        .span {
            display: block;
            text-align: center;
            font-size: 24rpx;
            color: #ff2d4a;
        }
    }
}
// 楼层区域
.section {
    .title {
        position: relative;
        ._span {
            position: absolute;
            color: #ff2d4a;
            font-weight: 700;
            line-height:90rpx; 
        }
        ._img {
            height: 90rpx;
            width: 100%;
        }
    }
    .content {
        overflow: hidden;
        image {
            width: 240rpx;
            float: left;
            margin: 5rpx;
        }
        image:not(:first-child) {
            height: 240rpx;
        }
    }
}
</style>
