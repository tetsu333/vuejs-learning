<script setup lang="ts">
import TweetForm from "./components/TweetForm.vue";
import TweetList from "./components/TweetList.vue";
import SettingsModal from "./components/SettingsModal.vue";
import TweetDeleteModal from "./components/TweetDeleteModal.vue";
import { provide, ref } from "vue";
import { Tweet } from "./types/Tweet";
import { updateUserNameKey } from "./key";

const tweets = ref<Tweet[]>([
  {id: "1", text: "Hello, Vue3!", userName: "test1"},
  {id: "2", text: "Hello, Vite!", userName: "test2"},
]);

const onSubmitForm = (tweet: string) => {
  tweets.value.push({id: String(Math.random()), text: tweet, userName: userName.value});
};

const isModalOpen = ref(false);
const onClickSettings = () => {
  isModalOpen.value = true;
}

const onSubmitSettings = (userName: string) => {
  console.log(userName);
  isModalOpen.value = false;
}

const userName = ref("");

const updateUserName = (name: string) => {
  userName.value = name;
};
provide(updateUserNameKey, updateUserName);

const isShowDeleteModal = ref(false);
const isDeletingID = ref("");
const onClickTweet = (id: string) => {
  isShowDeleteModal.value = true
  isDeletingID.value = id;
};

const onDelete = () => {
  deleteTweet(isDeletingID.value);
}

const deleteTweet = (id: string) => {
  tweets.value = tweets.value.filter((tweet) => tweet.id !== id);
  isShowDeleteModal.value = false;
}
</script>

<template>
  <div class="container">
    <div class="header">
      <button @click="onClickSettings">Settings</button>
    </div>
    <Teleport to="body">
      <SettingsModal @submit="onSubmitSettings" v-if="isModalOpen" />
    </Teleport>
    <Teleport to="body">
      <TweetDeleteModal v-if="isShowDeleteModal" @submit="onDelete" @cancel="isShowDeleteModal = false" />
    </Teleport>
    {{ userName }}
    <TweetForm @submit="onSubmitForm" />
    <TweetList :tweets="tweets" @click="(id) => onClickTweet(id)" />
    </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}
.header {
  display: flex;
  justify-content: flex-end;
  padding: 1em;
  button {
    border-radius: 0.5em;
    background-color: #81d6ee;
    color: white;
    height: 50px;
    font-size: 1em;
  }
}
</style>
