<script setup lang="ts">
import { ref, watch } from "vue";
const tweet = ref("");

const onSubmit = () => {
  console.log(tweet.value);
  emits("submit", tweet.value);
  tweet.value = "";
}

const isValidInput = ref(true);

watch(tweet, (newTweet) => {
  if (newTweet.length > 140 || newTweet.length < 0 ) {
    isValidInput.value = false;
  } else {
    isValidInput.value = true;
  }
});

const emits = defineEmits(["submit"]);
</script>

<template>
  <div>
    <form class="tweet-form">
      <textarea :style="{color: isValidInput ? 'black' : 'red'}" placeholder="what's happening?" v-model="tweet"></textarea>
      <button :disabled="!isValidInput" @click.prevent="onSubmit" type="submit">Tweet</button>
    </form>
  </div>
</template>

<style scoped>
.tweet-form {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.tweet-form textarea {
  height: 6em;
  border: 1px solid #ccc;
  border-radius: 0.5em;
  padding: 0.5em;
  font-size: 1.5em;
  border: 1px solid #ccc;
}
.tweet-form button {
  margin-top: 20px;
  border-radius: 0.5em;
  background-color: #81d6ee;
  color: white;
  width: 200px;
}
</style>
