<template>
  <div class="home">
    <item msg="electry test" />
  </div>
</template>

<script>
// @ is an alias to /src
import item from '@/components/item.vue';

export default {
  name: 'home',
  components: {
    item
  }
};
</script>
