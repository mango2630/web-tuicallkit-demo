<script lang="ts" setup>
import { ref } from "vue"
import { TUICallKit, TUICallKitServer } from "./components/TUICallKit/Web";
import * as GenerateTestUserSig from "./components/TUICallKit/Web/demos/basic/public/debug/GenerateTestUserSig.js";

const SDKAppID = 0;
const SecretKey = "xxx";

const userID = ref<string>("");
const isCalledUser = ref<string>("");

const login = () => {
  console.log(typeof userID.value);
  const { userSig } = GenerateTestUserSig.genTestUserSig(userID.value, SDKAppID, SecretKey);
  TUICallKitServer.init({ SDKAppID, userID: userID.value, userSig }); 
}

const audioCall = () => {
  TUICallKitServer.call({ userID: isCalledUser.value, type: 1 }); // 双人语音通话
}
const videoCall = () => {
  TUICallKitServer.call({ userID: isCalledUser.value, type: 2 }); // 双人视频通话
}


</script>
  
<template>

  <div class="call-kit-container">
    <TUICallKit />
  </div>
  
  <div class="debug">
    <p>当前用户userID是：{{ userID ? userID : "" }}</p>
    <span>请输入userID: </span>
    <input 
      v-model="userID"
      placeholder="userID"
    />
    <button @click="login">用户登陆</button> <br>
    <span>请输入通话用户userID: </span> 
    <input 
      v-model="isCalledUser"
      placeholder="请输入通话用户userID"
    />
    <button @click="audioCall">语音1v1通话</button>
    <button @click="videoCall">视频1v1通话</button>
  </div>
</template>


<style scoped>
.call-kit-container {
  width: 50rem;
  height: 35rem;
  border-radius: 16px;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 3px 6px, rgba(0, 0, 0, 0.23) 0px 3px 6px;
}
</style>
