<template>
  <view class="my-page">

    
    <!-- 用户信息卡片 -->
    <view class="user-card" v-if="userInfo.nickName">
      <u-avatar 
        :src="userInfo.avatarUrl" 
        :size="100" 
        mode="circle" 
        class="user-avatar"
      ></u-avatar>
      <view class="user-info">
        <text class="user-name">{{ userInfo.nickName }}</text>
      </view>
    </view>
    
    
    
    <!-- 功能列表 -->
    <view class="function-list" v-if="userInfo.nickName">
      <u-list border-bottom>
        <u-list-item 
          title="我的资产" 
          :showArrow="true"
          @click="navigateTo('assets')"
        >
          <template #icon>
            <u-icon name="wallet" color="#165DFF" size="24"></u-icon>
          </template>
        </u-list-item>
        <u-list-item 
          title="操作记录" 
          :showArrow="true"
          @click="navigateTo('records')"
        >
          <template #icon>
            <u-icon name="document" color="#165DFF" size="24"></u-icon>
          </template>
        </u-list-item>
        <u-list-item 
          title="收益排行" 
          :showArrow="true"
          @click="navigateTo('ranking')"
        >
          <template #icon>
            <u-icon name="ranking" color="#165DFF" size="24"></u-icon>
          </template>
        </u-list-item>
        <u-list-item 
          title="市场资讯" 
          :showArrow="true"
          @click="navigateTo('news')"
        >
          <template #icon>
            <u-icon name="newspaper" color="#165DFF" size="24"></u-icon>
          </template>
        </u-list-item>
        <u-list-item 
          title="设置" 
          :showArrow="true"
          @click="navigateTo('settings')"
        >
          <template #icon>
            <u-icon name="setting" color="#165DFF" size="24"></u-icon>
          </template>
        </u-list-item>
      </u-list>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {},
      bgImage: '/static/images/my_bg.png',
      userId: '',
      isLogin: false
    }
  },
  onLoad() {
    this.checkLoginStatus();
  },
  methods: {
    // 检查登录状态
    checkLoginStatus() {
      const userInfo = uni.getStorageSync('userInfo');
      if (userInfo && userInfo.nickName) {
        this.userInfo = userInfo;
        this.userId = this.generateUserId();
        this.isLogin = true;
      }
    },
    
    // 生成用户ID
    generateUserId() {
      const timestamp = new Date().getTime().toString().slice(-6);
      const random = Math.floor(Math.random() * 10000).toString().padStart(4, '0');
      return `U${timestamp}${random}`;
    },
    
    // 微信登录
    loginByWechat() {
      uni.getUserProfile({
        desc: '用于完善用户资料',
        success: (res) => {
          this.userInfo = res.userInfo;
          this.userId = this.generateUserId();
          this.isLogin = true;
          
          // 存储用户信息到本地
          uni.setStorageSync('userInfo', this.userInfo);
          
          // 登录成功提示
          uni.showToast({
            title: '登录成功',
            icon: 'success',
            duration: 2000
          });
        },
        fail: (err) => {
          console.log('获取用户信息失败', err);
          uni.showToast({
            title: '授权失败，部分功能将受限',
            icon: 'none',
            duration: 2000
          });
        }
      });
    },
    
    // 退出登录
    logout() {
      uni.showModal({
        title: '确认退出',
        content: '退出后将无法使用个人功能，是否继续？',
        success: (res) => {
          if (res.confirm) {
            // 清除本地存储的用户信息
            uni.removeStorageSync('userInfo');
            this.userInfo = {};
            this.isLogin = false;
            
            uni.showToast({
              title: '已退出登录',
              icon: 'success',
              duration: 2000
            });
          }
        }
      });
    },
    
    // 页面跳转
    navigateTo(page) {
      if (!this.isLogin) {
        uni.showToast({
          title: '请先登录',
          icon: 'none',
          duration: 2000
        });
        return;
      }
      
      let url = '';
      switch (page) {
        case 'assets':
          url = '/pages/assets/index';
          break;
        case 'records':
          url = '/pages/records/index';
          break;
        case 'ranking':
          url = '/pages/ranking/index';
          break;
        case 'news':
          url = '/pages/news/index';
          break;
        case 'settings':
          url = '/pages/settings/index';
          break;
      }
      
      uni.navigateTo({
        url: url
      });
    }
  }
}
</script>

<style lang="scss" scoped>
.my-page {
  background-color: #F5F7FA;
  min-height: 100vh;
}

.header-bg {
  height: 200rpx;
  overflow: hidden;
  position: relative;
  
  .bg-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.user-card {
  background-color: #FFFFFF;
  border-radius: 16rpx;
  margin: 60rpx 30rpx 40rpx;
  padding: 40rpx 30rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.08);
  position: relative;
  z-index: 10;
  
  .user-avatar {
    margin: 0 auto 20rpx;
    border: 4rpx solid #FFFFFF;
    box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);
  }
  
  .user-info {
    text-align: center;
    margin-bottom: 30rpx;
    
    .user-name {
      font-size: 36rpx;
      font-weight: bold;
      color: #333333;
      display: block;
      margin-bottom: 10rpx;
    }
    
    .user-id {
      font-size: 24rpx;
      color: #999999;
    }
  }
  
  .logout-btn {
    width: 200rpx;
    margin: 0 auto;
    font-size: 28rpx;
    padding: 10rpx 0;
  }
}

.login-container {
  padding: 80rpx 0;
  text-align: center;
  
  .default-avatar {
    width: 160rpx;
    height: 160rpx;
    border-radius: 50%;
    margin: 0 auto 40rpx;
    background-color: #E5E9F2;
    padding: 20rpx;
  }
  
  .login-tip {
    font-size: 28rpx;
    color: #999999;
    display: block;
    margin-bottom: 40rpx;
  }
  
  .login-btn {
    width: 500rpx;
    margin: 0 auto;
    font-size: 32rpx;
  }
}

.function-list {
  margin: 0 30rpx;
}
</style>    