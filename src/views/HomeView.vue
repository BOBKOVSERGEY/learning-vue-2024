<script setup lang="ts">
import axios from 'axios';
import { ref } from 'vue';

type User = {
  name: string;
  bio: string;
  avatar_url: string;
  twitter_username: string;
  blog: string;
//...
};

const user = ref<User | null>(null);
const error = ref<Error | null>(null);

async function getUser () {
  try {
    const response = await axios.get<User>('https://api.github.com/users/bobkovsergey')
    user.value = response.data
  } catch (err) {
    error.value  = err as Error;
  }
}
getUser()
</script>

<template>
  <div class="user-profile" v-if="user">
    <img :src="user.avatar_url" alt="`${user.name} Avatar`" width="200" />
    <div>
      <h1>{{ user.name }}</h1>
      <p>{{ user.bio }}</p>
      <p>Twitter: {{ user.twitter_username }}</p>
      <p>Blog: {{ user.blog }}</p>
    </div>
  </div>
  <div class="error" v-else-if="error">
    {{ error.message }}
  </div>
<pre>
  {{ user }}
</pre>
</template>
