<template>
  <div>
    <Profile />
  </div>
</template>

<script>
import Profile from '@/components/Profile.vue';

export default {
  name: 'App',
  components: {
    Profile,
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
}
</style>
