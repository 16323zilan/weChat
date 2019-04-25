<template>
  <div class="wrap">
    <header>
      <dl>
        <dt>
          <img :src="userInfo.avatarUrl" alt>
        </dt>
        <dd>
          <p class="name">
            {{userInfo.nickName}}
            <!-- <img src="/static/images/黄金会员-v1@2x.png" alt> -->
          </p>
          <p class="codeNum">
            邀请码：{{inviteCode}}
            <span @click="copy(inviteCode)">复制</span>
          </p>
        </dd>
      </dl>
    </header>
    <section>
      <div class="sec-top">
        <h3 @click="goTolist">我的订单</h3>
        <div class="wallet">
          <dl @click="goTolist">
            <dt>
              <img src="/static/images/待付款.png" alt>
            </dt>
            <dd>待付款</dd>
          </dl>
          <dl @click="goTolist">
            <dt>
              <img src="/static/images/dfh.png" alt>
            </dt>
            <dd>待付款</dd>
          </dl>
          <dl @click="goTolist">
            <dt>
              <span class="num">12</span>
              <img src="/static/images/dsh.png" alt>
            </dt>
            <dd>待付款</dd>
          </dl>
        </div>
      </div>
      <div class="msg">
        <ul>
          <li @click="goto(1)">
            <div>
              <img class="img1" src="/static/images/yhj.png" alt>
              <span>我的优惠卷</span>
            </div>
            <img class="img2" src="/static/images/jt.png" alt>
          </li>
          <li @click="goto(2)">
            <div>
              <img class="img1" src="/static/images/dz.png" alt>
              <span>收货地址</span>
            </div>
            <img class="img2" src="/static/images/jt.png" alt>
          </li>
          <li @click="goto(3)">
            <div>
              <img class="img1" src="/static/images/kf.png" alt>
              <span>联系客服</span>
            </div>
            <img class="img2" src="/static/images/jt.png" alt>
          </li>
          <li @click="goto(4)">
            <div>
              <img class="img1" src="/static/images/sm.png" alt>
              <span>实名认证</span>
            </div>
            <img class="img2" src="/static/images/jt.png" alt>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import Login from "../../components/user/login";
import request from "@/utils/request";
import { ddNum } from "@/api/index";
// /{version}
export default {
  data() {
    return {
      userInfo: "",
      inviteCode: "",
      boo: false,
      trackId: ""
    };
  },
  components: {
    Login
  },
  onShow() {
    var that = this;
    wx.getStorage({
      key: "user",
      success: function(res) {
        console.log("res.data.rawData", res.data.rawData);
        that.userInfo = JSON.parse(res.data.rawData);
      }
    });
    wx.getStorage({
      key: "inviteCode",
      success: function(res) {
        console.log("666666666", res);
        that.inviteCode = res.data;
      }
    });
  },
  methods: {
    copy(val) {
      wx.setClipboardData({
        data: val,
        success(res) {
          wx.showToast({
            title: "已复制邀请码到剪切板", //提示的内容,
            icon: "none" //图标,
          });
        }
      });
    },
    goTolist() {
      wx.getStorage({
        key: "trackId",
        success: res => {
          this.trackId = res.data;
          console.log(res.data);
        }
      });
      wx.request({
        method: "POST",
        url: "http://test.api.jinaup.com/api/open/order/tips/1", // 仅为示例，并非真实的接口地址
        data: {
          trackId: this.trackId
        },
        header: {
          "content-type": "application/x-www-form-urlencoded" // 默认值
        },
        success(res) {
          console.log("res...订单数量", res);
        }
      });
      wx.navigateTo({
        url: "/pages/myorder/main"
      });
    },
    goto(index) {
      if (index == 1) {
        console.log("去优惠券界面");
        wx.navigateTo({
          url: "/pages/conponlist/main"
        });
      } else if (index == 2) {
        console.log("去收货地址界面");
        wx.navigateTo({
          url: "/pages/shoppingadress/main"
        });
      } else if (index == 3) {
        console.log("联系客服");
        wx.navigateTo({
          url: "/pages/artificial/main"
        });
      } else if (index == 4) {
        console.log("实名认证");
        wx.navigateTo({
          url: "/pages/autonym/main"
        });
      }
    }
  }
};
</script>

<style scoped>
.wrap {
  position: fixed;
  height: 100%;
  width: 100%;
  background: rgba(243, 247, 247, 1);
}
header {
  width: 100%;
  height: 110px;
  background: skyblue;
  background-image: url("/static/images/32.png");
}
header dl {
  display: flex;
}
header dl dt img {
  width: 64px;
  height: 64px;
  margin: 19px;
  border-radius: 50%;
}
header dl dd {
  margin: 11px 10px 0 10px;
}
header dl dd .name {
  font-size: 20px;
  line-height: 2;
}
header dl dd .codeNum {
  font-size: 14px;
}
header dl dd .codeNum span {
  margin-left: 5px;
  line-height: 2;
  font-size: 10px;
  background: #35cdd0;
  color: #fff;
  padding: 3px 5px;
  box-sizing: border-box;
  border-radius: 5px;
  opacity: 0.7239;
}
header dl dd img {
  width: 31px;
  height: 17px;
}
section {
  width: 100%;
  position: absolute;
  top: 90px;
}
section .sec-top {
  width: 96%;
  margin: 0 2%;
  height: 110px;
  background: #fff;
  border-radius: 5px;
}

section .sec-top h3 {
  line-height: 2;
  padding-left: 10px;
  box-sizing: border-box;
}
section .sec-top .wallet {
  display: flex;
  justify-content: space-around;
}
section .sec-top .wallet dl dt img {
  width: 40px;
  height: 40px;
  box-sizing: border-box;
}
section .sec-top .wallet dl dt {
  position: relative;
}
section .sec-top .wallet dl dt .num {
  position: absolute;
  left: 30px;
  top: -5px;
  padding: 1px;
  font-size: 10px;
  border: 1px solid rgba(252, 93, 123, 1);
  border-radius: 50%;
  color: #fc5d7b;
}
section .sec-top .wallet dl dd {
  font-size: 12px;
  color: #70757d;
  text-align: center;
}
section .msg {
  width: 96%;
  margin: 0 2%;
  margin-top: 10px;
}
section .msg ul {
  background: #fff;
}
section .msg ul li {
  display: flex;
  height: 48px;
  justify-content: space-between;
}
section .msg ul li div {
  display: flex;
}
section .msg ul li div .img1 {
  width: 18px;
  height: 18px;
  margin: 15px;
}
section .msg ul li div span {
  font-size: 14px;
  margin: 15px 15px 0 0;
}
section .msg ul li .img2 {
  width: 8px;
  height: 12px;
  margin: 17px 18px 0 0;
}
</style>
