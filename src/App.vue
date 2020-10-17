<template>
  <github-link />
  <fake-chuck-norris-tweet />
</template>

<script>
import FakeChuckNorrisTweet from './components/FakeChuckNorrisTweet';
import GithubLink from './components/GithubLink';

export default {
  name: 'App',
  components: {
    GithubLink,
    FakeChuckNorrisTweet
  }
};
</script>
