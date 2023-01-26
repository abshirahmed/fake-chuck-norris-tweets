<template>
  <div
    class="min-w-screen min-h-screen flex items-center justify-center px-5 py-5"
  >
    <div
      class="mx-auto rounded-lg bg-white shadow p-5 text-gray-800"
      style="max-width: 400px"
    >
      <div class="w-full flex mb-4">
        <div class="overflow-hidden rounded-full w-12 h-12">
          <a href="https://twitter.com/chucknorris" target="_blank">
            <img
              alt="image of Chuck Norris"
              src="https://pbs.twimg.com/profile_images/1407346896/89_400x400.jpg"
          /></a>
        </div>
        <div class="flex-grow pl-3">
          <h6 class="font-bold text-md">Chuck Norris</h6>
          <a
            class="text-xs text-gray-600"
            href="https://twitter.com/chucknorris"
            target="_blank"
            >@chucknorris</a
          >
        </div>
        <div class="w-12 text-right">
          <a
            class="mdi mdi-twitter text-blue-400 text-3xl"
            href="https://twitter.com/__abshir"
            target="_blank"
          ></a>
        </div>
      </div>
      <div class="w-full mb-4">
        <p class="text-sm" v-html="joke"></p>
      </div>
      <div class="w-full">
        <p class="text-xs text-gray-500 text-right">{{ fakeTweetTimestamp }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import * as diffusion from 'diffusion';

const moment = require('moment');
import { ref } from 'vue';

export default {
  name: 'FakeChuckNorrisTweet',
  setup() {
    const joke = ref('');
    const fakeTweetTimestamp = ref('');
    diffusion
      .connect({
        host: 'clouddemo.eu.diffusion.cloud',
        secure: true
      })
      .then((session) => {
        session.addStream('*.*', diffusion.datatypes.json()).on({
          value(topic, specification, newValue) {
            joke.value = newValue.get();
            fakeTweetTimestamp.value = moment().format('MMM Do h:mma');
          }
        });
        session.select('chucknorris/value');
        return session;
      })
      .catch((error) => console.error(error.message));
    return { joke, fakeTweetTimestamp };
  }
};
</script>
