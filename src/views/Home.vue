<template>
  <div class="home">
    <ProductCustomiser />
  </div>
</template>

<script>
// @ is an alias to /src
import ProductCustomiser from '@/components/ProductCustomiser.vue';

export default {
  name: 'Home',
  components: {
    ProductCustomiser,
  },
};
</script>
