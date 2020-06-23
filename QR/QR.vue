 <template xlang="wxml">
  <view class="container">
    <view class="qrimg">
      <view class="qrimg-i">
        <tki-qrcode
          v-if="ifShow"
          cid="qrcode1"
          ref="qrcode"
          :val="val"
          :size="size"
          :unit="unit"
          :background="background"
          :foreground="foreground"
          :pdground="pdground"
          :icon="icon"
          :iconSize="iconsize"
          :lv="lv"
          :onval="onval"
          :loadMake="loadMake"
          :usingComponents="true"
          @result="qrR"
        />
      </view>
    </view>
    <view class="but">
      <button type="primary" @tap="saveQrcode">保存到图库</button>
    </view>
  </view>
</template>
 <script>
import tkiQrcode from "@/components/tki-qrcode/tki-qrcode.vue";
export default {
  data() {
    return {
      ifShow: true,
      val: "1", // 要生成的二维码值
      size: 300, // 二维码大小
      unit: "rpx", // 单位
      background: "#f1f9ff", // 背景色
      foreground: "#000000", // 前景色
      pdground: "#000000", // 角标色
      icon: "", // 二维码图标
      iconsize: 100, // 二维码图标大小
      lv: 3, // 二维码容错级别 ， 一般不用设置，默认就行
      onval: false, // val值变化时自动重新生成二维码
      loadMake: true, // 组件加载完成后自动生成二维码
      src: "" // 二维码生成后的图片地址或base64
    };
  },
  methods: {
    sliderchange(e) {
      this.size = e.detail.value;
    },
    creatQrcode() {
      this.$refs.qrcode._makeCode();
    },
    saveQrcode() {
      this.$refs.qrcode._saveCode();
    },
    qrR(res) {
      this.src = res;
    },
    clearQrcode() {
      this.$refs.qrcode._clearCode();
      this.val = "";
    },
    ifQrcode() {
      this.ifShow = !this.ifShow;
    },
    selectIcon() {
      let that = this;
      uni.chooseImage({
        count: 1, //默认9
        sizeType: ["original", "compressed"], //可以指定是原图还是压缩图，默认二者都有
        sourceType: ["album"], //从相册选择
        success: function(res) {
          that.icon = res.tempFilePaths[0];
          setTimeout(() => {
            that.creatQrcode();
          }, 100);
          // console.log(res.tempFilePaths);
        }
      });
    }
  },
  components: {
    tkiQrcode
  },
  onLoad: function(value) {
    console.log("hello");
    console.log(value);
    const list = decodeURIComponent(value.item);
    console.log(list);
    this.val = list;
    this.creatQrcode();
  },
  updated: () => {
    console.log("created");
  }
};
</script>
 
 <style>
/* @import "../../../common/icon.css"; */
.container {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.qrimg {
  margin: 0 auto;
}
.qrimg-i {
  width: 100%;
  height: 400rpx;
}
slider {
  width: 100%;
}
input {
  width: 100%;
  margin-bottom: 20upx;
}
.btns {
  display: flex;
  flex-direction: column;
  width: 100%;
}
button {
  width: 90%;
  margin: 0 auto;
}

.but {
  margin-top: 300rpx;
}
</style>