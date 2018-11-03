<template>
       <swiper :current="currentIndex" class="previewBack" :style="'height:'+screenHeight" v-if="showPreview" @animationfinish="changeCurrentImg">
           <swiper-item v-for="(item,index) in imgList" :key="index" class="currentImgShow">
               <div class="currentImgBack" @tap="close">
                   <image :src="item.imgUrl" mode="widthFix"/>
               </div>
               <div class="bottomDescribe">
                    <div class="describeLeft">{{item.describe}}</div>
                    <div class="downLoadRight" @tap="downLoadImg(item.imgUrl)"><img src="/static/assets/downLoad.png"/></div>
              </div>
           </swiper-item>
       </swiper>
</template>

<script>
export default {
  props: {
    imgList: {
      type: Array
    },
    showPreview: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      screenHeight: null
    }
  },
  computed: {
    currentIndex () {
      return this.$store.state.currentIndex
    }
  },
  onReady () {
    const that = this
    wx.getSystemInfo({
      success (r) {
        that.screenHeight = r.windowHeight + 'px'
      }
    })
  },
  methods: {
    downLoadImg (imgurl) {
      wx.authorize({
        scope: 'scope.writePhotosAlbum',
        success () {
          wx.downloadFile({
            url: imgurl,
            success (res) {
              wx.saveImageToPhotosAlbum({
                filePath: res.tempFilePath,
                fail (ress) {
                  wx.showToast({title: '已取消', icon: 'none'})
                },
                success (ress) {
                  wx.showToast({title: '保存成功'})
                }
              })
            }
          })
        },
        fail () {
          wx.showToast({title: '授权失败', icon: 'none'})
        }
      })
    },
    close () {
      this.$emit('closePreview')
    },
    changeCurrentImg (e) {
      this.$store.commit('setCurrentIndex', e.mp.detail.current)
      this.wx.title((this.$store.state.currentIndex + 1) + '/' + this.imgList.length)
    }
  }
}
</script>
<style lang="scss" scoped>
.previewBack {
    background: #000;
    width:100%;
    position: fixed;
    top:0px;
    .currentImgShow {
        width:100%;
        height:100%;
        position: relative;
        .currentImgBack {
            width:100%;
            height:100%;
            display: flex;
            align-items: center;
            image {
                width:100% !important;
            }
        }
        .bottomDescribe{
            padding:0px 20px;
            position:absolute;
            bottom:20px;
            color:#fff;
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-family: PingFang-SC-Medium;
            font-size: 14px;
            .describeLeft{
                width:315px;
            }
            .downLoadRight{
                width:35px;
                height:35px;
                flex-shrink: 0;
                display: flex;
                align-items: center;
                justify-content: center;
                img{
                    width:20px;
                    height:20px;
                }
            }
        }
    }
}
</style>

