<template>
    <!-- <div class="container"> -->
      <div class="container" :catchtouchmove="showPreview">
        <scroll-view class="collectHead" scroll-x :scroll-left="scrollLeft">
            <div v-for="(item,index) in firstData" v-if="item.num!==0" class="firstCollect" :class="nowFirstCollect==index?'active':''" :key="index" @click="selectFirstCollect(item,index)">{{item.name}}({{item.num}})</div>
        </scroll-view>
        <view class='view_img' v-for="(item,index) in imgList" :key="index">
         <img :src="item.imgUrl" :data-src="item.imgUrl" @tap="previewImage(index)"/>
       </view>
        <img-preview :imgList="imgList" :showPreview="showPreview" @closePreview="closePreview"></img-preview>
        <!-- <swiper @change="tabChange" :current="nowFirstCollect" class="swiper">
            <swiper-item v-for="(itemA,indexA) in firstData" :key="indexA">
                <view class="collectNav">
                    <div v-for="(item,index) in secondData" v-if="item.num!==0" class="secondCollect" :class="nowSecondCollect==index?'active':''" :key="index" @click="selectSecondCollect(item,index)">{{item.name}}({{item.num}})</div>
                </view>
                <block v-for="(item, index) in dataTab" :key=index>
                    <wpzr @itemClick=itemClick(index)
                            :img=imgUrls[index%3]
                            title="拱墅万达广场旺铺，杰拉查克拉超级电竞网咖"
                            area="1000㎡"
                            region="杭州-拱墅"
                            price="5000万"
                            location="万达广场"
                            :tags=tags
                            v-if="canShowIndex=='wpzr'">
                    </wpzr>
                    <design-case :imgUrl="item.imgUrl" v-if="canShowIndex=='designCase'"></design-case>
                    <atelierlist :imgUrl="item.imgUrl" :infoName="item.infoName" :infoTags="item.infoTags" :address="item.address" v-if="canShowIndex=='atelierlist'"></atelierlist>
                    <decorate-case :imgUrl="item.imgUrl" :infoName="item.infoName" :infoArea="item.infoArea" :infoDate="item.infoDate" v-if="canShowIndex=='decorateCase'"></decorate-case>
                    <purchase-goods :imgUrl="item.imgUrl" :purGoodsInfo="item.purGoodsInfo" :purGoodsPrice="item.purGoodsPrice" v-if="canShowIndex=='purchaseGoods'"></purchase-goods>
                    <computer-buy :imgUrl="item.imgUrl" :infoName="item.infoName" :address="item.address" v-if="canShowIndex=='computerBuy'"></computer-buy>
                    <decorate-team :imgUrl="item.imgUrl" :infoName="item.infoName" :otherInfo="item.otherInfo" :address="item.address" v-if="canShowIndex=='decorateTeam'"></decorate-team>
                    <water-bar :imgUrl="item.imgUrl" :infoName="item.infoName" :address="item.address" v-if="canShowIndex=='waterBar'"></water-bar>
                </block>
            </swiper-item>
        </swiper> -->
    </div>
</template>
<script>
import Wpzr from '../../components/wpzr'
import designCase from '../../components/collect//designcase'
import atelierlist from '../../components/collect/atelierlist'
import decorateCase from '../../components/collect/decoratecase'
import purchaseGoods from '../../components/collect/purchasegoods'
import computerBuy from '../../components/collect/computerbuy'
import decorateTeam from '../../components/collect/decorateteam'
import waterBar from '../../components/collect/waterbar'
import imgPreview from '../../components/imgpreview'
export default {
  components: {
    Wpzr,
    designCase,
    atelierlist,
    decorateCase,
    purchaseGoods,
    computerBuy,
    decorateTeam,
    waterBar,
    imgPreview
  },
  data () {
    return {
      firstData: [
        {name: '二手交易', num: 10, secondData: [{name: '旺铺转让', num: 3, type: 'wpzr'}, {name: '桌椅转让', num: 3, type: 'wpzr'}, {name: '电脑外设', num: 3, type: 'wpzr'}, {name: '其他转让', num: 3, type: 'wpzr'}]},
        {name: '专业设计', num: 1, secondData: [{name: '案例', num: 3, type: 'designCase'}, {name: '工作室', num: 3, type: 'atelierlist'}]},
        {name: '装修团队', num: 10, secondData: [{name: '案例', num: 3, type: 'decorateCase'}, {name: '公司', num: 3, type: 'decorateTeam'}]},
        {name: '桌椅采购', num: 20, secondData: [{name: '商品', num: 3, type: 'purchaseGoods'}, {name: '代理商', num: 3, type: 'computerBuy'}]},
        {name: '电脑采购', num: 10, secondData: [], type: 'computerBuy'},
        {name: '外设采购', num: 5, secondData: [{name: '商品', num: 3, type: 'purchaseGoods'}, {name: '代理商', num: 3, type: 'computerBuy'}]},
        {name: '显示器采购', num: 10, secondData: [{name: '商品', num: 3, type: 'purchaseGoods'}, {name: '代理商', num: 5, type: 'computerBuy'}]},
        {name: '水吧供货', num: 2, secondData: [], type: 'waterBar'},
        {name: '网维', num: 2, secondData: [], type: 'waterBar'}
      ],
      secondData: [],
      imgList: [
        {imgUrl: 'http://photo.16pic.com/00/06/68/16pic_668787_b.jpg', describe: '吧台设计'},
        {imgUrl: 'http://pptdown.pptbz.com/pptbeijing/%E7%B2%89%E7%BA%A2%E7%8C%AA%E5%B0%8F%E5%A6%B9PPT%E8%83%8C%E6%99%AF%E5%9B%BE%E7%89%87.jpg', describe: '商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺Lorem ipsumsit amet consectetur adipiscing elit'},
        {imgUrl: 'http://pic.58pic.com/58pic/15/14/12/46358PICPMH_1024.jpg', describe: '商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺'},
        {imgUrl: 'http://pic.58pic.com/58pic/15/36/93/37W58PICi8G_1024.jpg', describe: '商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺描述商铺Lorem ipsumsit amet consectetur adipiscing elit'}
      ],
      imgUrls: [
        'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
        'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
        'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
      ],
      tags: ['上水', '停车位', '商圈成熟'],
      dataTab: [{imgUrl: 'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg', infoName: '蜜帅歪脸工作室', infoTags: ['网咖', '电竞馆'], address: '杭州', infoArea: '1000㎡', infoDate: '300天', purGoodsInfo: '傲风电竞桌网吧游戏桌椅电脑台式可躺着玩的台式电脑桌椅一体', purGoodsPrice: '299.90', otherInfo: '蜜帅歪脸装饰有限公司'}, {imgUrl: 'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg', infoName: '蜜帅歪脸工作室', infoTags: ['网咖', '电竞馆'], address: '杭州', infoArea: '1000㎡', infoDate: '300天', purGoodsInfo: '傲风电竞桌网吧游戏桌椅电脑台式可躺着玩的台式电脑桌椅一体', purGoodsPrice: '299.90', otherInfo: '蜜帅歪脸装饰有限公司'}, {imgUrl: 'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg', infoName: '蜜帅歪脸工作室', infoTags: ['网咖', '电竞馆'], address: '杭州', infoArea: '1000㎡', infoDate: '300天', purGoodsInfo: '傲风电竞桌网吧游戏桌椅电脑台式可躺着玩的台式电脑桌椅一体', purGoodsPrice: '299.90', otherInfo: '蜜帅歪脸装饰有限公司'}
      ],
      nowFirstCollect: 0,
      nowSecondCollect: 0,
      canShowIndex: 'wpzr',
      screenWidth: null,
      showPreview: false
    }
  },
  computed: {
    scrollLeft: function () {
      if (this.nowFirstCollect > 2) {
        return (this.screenWidth / this.firstData.length) * (this.nowFirstCollect - 2)
      } else {
        return 0
      }
    }
  },
  created () {
    this.secondData = this.firstData[0].secondData
  },
  mounted () {
    const that = this
    const aa = wx.createSelectorQuery()
    aa.selectAll('.firstCollect').boundingClientRect(function (rect) {
      that.screenWidth = rect[rect.length - 1].right
    }).exec()
  },
  methods: {
    previewImage (index) {
      this.$store.commit('setCurrentIndex', index)
      this.showPreview = true
      this.wx.title((this.$store.state.currentIndex + 1) + '/' + this.imgList.length)
    },
    closePreview () {
      this.showPreview = false
      this.wx.title('我的收藏')
    },
    selectFirstCollect (item, index) {
      this.secondData = item.secondData
      this.nowFirstCollect = index
      this.nowSecondCollect = 0
      this.canShowIndex = (item.secondData.length === 0) ? item.type : item.secondData[0].type
    },
    selectSecondCollect (item, index) {
      this.nowSecondCollect = index
      console.log(this.nowSecondCollect)
      this.canShowIndex = item.type
    },
    tabChange (e) {
      this.nowFirstCollect = e.mp.detail.current
      this.secondData = this.firstData[this.nowFirstCollect].secondData
      this.nowSecondCollect = 0
      this.canShowIndex = (this.secondData.length === 0) ? this.firstData[this.nowFirstCollect].type : this.secondData[0].type
    }
  }
}
</script>
<style lang="scss" scoped>
.container{
    margin-bottom:10px;
    .collectHead{
        box-sizing: border-box;
        white-space: nowrap;
        padding: 8px 0px 8px 10px;
        background: #FFFFFF;
        box-shadow: 0 1px 0 0 rgba(0,0,0,0.05);
        height:36px;
        line-height: 20px;
        .firstCollect {
            display: inline-block;
            height:20px;
            margin: 0px 10px;
            font-family: PingFang-SC-Medium;
            font-size: 14px;
            color: #666666;
            letter-spacing: 0;
        }
        .active {
            font-family: PingFang-SC-Bold;
            color: #FFA740;
        }
    }
    .swiper{
        width:100%;
        height: calc(100vh - 36px);
        .collectNav{
            display:flex;
            flex-direction: row;
            width:100%;
            .secondCollect{
                flex:auto;
                padding:8px 10px;
                box-sizing: border-box;
                height:36px;
                background: #FFFFFF;
                font-family: PingFang-SC-Medium;
                font-size: 14px;
                color: #999999;
                letter-spacing: 0;
                text-align: center;
            }
            .active {
                color: #FFA740;
            }
        }
    }
}
</style>