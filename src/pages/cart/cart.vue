<template>
  <div class="container">
    <van-tabs :v-active="active" class="tabs" @change="changeCategory">
      <van-tab v-for="(category, categoryIndex) in categories" :key='categoryIndex' :title='category' class="category-tab" >
        <div v-for="(product, productIndex) in currentItems" :key='productIndex' class="product">
          <van-card
            :desc='product.nameEn'
            :title='product.nameCn'
            :thumb='product.posterSrc'
          />
        </div>
      </van-tab>
    </van-tabs>
    <van-toast id="van-toast" />
  </div>
</template>

<script>
import card from '@/components/card'
import Toast from '@/../static/vant/toast/toast'
// fly.interceptors.request.use((config,promise)=>{
//     //给所有请求添加自定义header
//     config.headers["X-Tag"]="flyio";
//     return config;
// })

export default {
  data () {
    return {
      motto: 'Hello',
      userInfo: {},
      value: "",
      active: 0,
      lineWidth: "1px",
      categories: ["化学品", "建筑", "电子电气", "能源与资源", "家居护理与工业", "生物试剂", "服装工业"],
      singleItem: {id: "1", cas: "872-50-4", posterSrc: "https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3769537293,3873568774&fm=11&gp=0.jpg", nameEn: "1-Methyl-2-pyrrolidinone", nameCn: "N-甲基吡络烷酮"},
      currentItems: []
    }
  },

  components: {
    card
  },

  methods: {
    changeCategory (e) {
      Toast.loading({
        mask: true,
        message: '加载中...'
      })
      this.currentItems = []
      for (let i = 0; i < e.mp.detail.index + 1; i++) {
        this.currentItems.push(this.singleItem)
      }
      wx.setNavigationBarTitle({
        title: "产品-" + e.mp.detail.title
      })
      Toast.clear()
    },
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    testClick () {
      this.$flyio.get("inventory").then((d) => {
        console.log(d.data)
      })
  
      // wx.chooseImage({
      //   success (res) {
      //     const tempFilePaths = res.tempFilePaths
      //     console.log(tempFilePaths)
      //     wx.uploadFile({
      //       url: 'https://example.weixin.qq.com/upload', //仅为示例，非真实的接口地址
      //       filePath: tempFilePaths[0],
      //       name: 'file',
      //       formData: {
      //         'user': 'test'
      //       },
      //       success (res){
      //         const data = res.data
      //         //do something
      //       }
      //     })
      //   }
      // })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    }
  },

  mounted () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
    this.currentItems.push(this.singleItem)
    wx.setNavigationBarTitle({
      title: "产品-" + this.categories[0]
    })
  }
}
</script>

<style lang="less">
@import "./cart.less";
</style> 