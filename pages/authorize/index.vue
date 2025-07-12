<template>
	<view class="login-container">
	  <u-image 
	    src="/static/image/logo.jpg" 
	    mode="aspectFill" 
	    class="default-avatar"
	  ></u-image>
	  <text class="login-tip">登录后享受更多功能</text>
	  <u-button 
	    type="primary" 
	    text="微信快捷登录" 
	    @click="loginByWechat"
	    class="login-btn"
	  ></u-button>
	</view>
</template>

<script>
</script>

<style>
</style>