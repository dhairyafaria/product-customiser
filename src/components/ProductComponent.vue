<template>
    <div class="product-component">
        <div class="product-name">{{ productComponent.name }}</div>
        <div class="product-variants">
            <div v-for="(variant, index) in productComponent.variants"
                :key="index"
                class="product-variant"
                :class="{'selected': selectedIndex === index}"
                @click="SelectVariant(productComponent.name, variant.price, index)"
            >
                <span class="product-variant-title">{{ variant.title }}</span>
                <span class="product-variant-price" v-if="CalculatePrice(variant.price)">
                    Rs.{{ CalculatePrice(variant.price) }}
                </span>
            </div>
        </div>
    </div>
</template>
<script>
export default {
  name: 'ProductComponent',
  data() {
    return {
      selectedVariantPrice: 0,
      selectedIndex: 0,
    };
  },
  props: {
    productComponent: {
      type: Object,
      default: () => {},
    },
  },
  methods: {
    SelectVariant(productComponentName, price, index) {
      this.selectedVariantPrice = price;
      this.selectedIndex = index;
      this.$emit('variantSelected', productComponentName, index);
    },
    CalculatePrice(price) {
      return price - this.selectedVariantPrice;
    },
  },
};
</script>
<style lang="scss" scoped>
    .product-component {
        text-align: left;
        .product-name {
            margin-bottom: 20px;
        }
        .product-variants {
            margin-bottom: 20px;
            .product-variant {
                border: 1px solid black;
                margin-bottom: 10px;
                cursor: pointer;
                &.selected {
                  background-color: aqua;
                }
                .product-variant-price {
                    text-align: right;
                }
            }
        }
    }
</style>
