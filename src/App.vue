<template>
  <div>
    <h1>Line LIFF SDK Example</h1>
    <div v-if="userProfile">
      <p>User ID: {{ userProfile.userId }}</p>
      <p>Display Name: {{ userProfile.displayName }}</p>
      <p>Status Message: {{ userProfile.statusMessage }}</p>
      <img :src="userProfile.pictureUrl" alt="User Picture" />
    </div>
    <button @click="sendMessage">Send Message</button>
    <!-- show err message -->
     <p v-if="errMsg">{{ errMsg }}</p>
  </div>
</template>

<script>
import liff from '@line/liff';

export default {
  name: 'LiffExample',
  data() {
    return {
      liffId: '2006455856-3BE8l7mx', // 替换为你的 LIFF ID
      userProfile: null,
      errMsg: null
    };
  },
  mounted() {
    this.initializeLiff();
  },
  methods: {
    async initializeLiff() {
      try {
        await liff.init({ liffId: this.liffId });
        console.log('LIFF 初始化成功');
        this.getUserProfile();
      } catch (error) {
        console.error('LIFF 初始化失败:', error);
      }
    },
    async getUserProfile() {
      if (!liff.isLoggedIn()) {
        liff.login();
        return;
      }

      try {
        const profile = await liff.getProfile();
        this.userProfile = profile;
        console.log('获取用户信息成功:', profile);
      } catch (error) {
        console.error('获取用户信息失败:', error);
      }
    },
    async sendMessage() {
      if (!liff.isLoggedIn()) {
        liff.login();
        return;
      }

      try {
        await liff.sendMessages([
          {
            type: 'text',
            text: 'Hello, this is a message from LIFF!'
          }
        ]);
         this.errMsg='消息发送成功';
      } catch (error) {
       this.errMsg= error;
      }
    }
  }
};
</script>

<style scoped>
/* 你的样式 */
</style>