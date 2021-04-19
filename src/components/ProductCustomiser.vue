<template>
    <div class="product-customiser">
        <div class="title">Mac Book Pro</div>
        <div class="body">
            <div class="left-body">
                <img class="product-image" src="../assets/macbook.jpeg" />
            </div>
            <div class="right-body">
                <div class="product-description">
                    <div class="product-name">Customise your 16‑inch MacBook Pro - Space Grey</div>
                    <div class="product-summary">
                        <p v-for="(productComponent, index) in productComponents"
                          :key="index"
                        >
                          {{getSelectedVariantTitle(productComponent)}}
                        </p>
                        <p>16-inch Retina display with True Tone</p>
                        <p>Four Thunderbolt 3 ports</p>
                        <p>Touch Bar and Touch ID</p>
                        <p>Backlit Magic Keyboard - US English</p>
                    </div>
                </div>
                <div class="product-components">
                    <ProductComponent v-for="(productComponent, index) in productComponents"
                        :key="index"
                        :product-component="productComponent"
                        @variantSelected="AddVariants"
                    />
                </div>
            </div>
            <div class="clear-float"></div>
        </div>
        <div class="footer">
            <div>Total Price: Rs.{{ totalPrice }}</div>
            <div>EMI per Month: Rs.{{ emi }}</div>
        </div>
    </div>
</template>

<script>
import ProductComponent from './ProductComponent.vue';
import ProductComponents from '../assets/product-components.json';

export default {
  name: 'ProductCustomiser',
  components: {
    ProductComponent,
  },
  data() {
    return {
      productComponents: ProductComponents,
      productComponentsSelected: {},
      basePrice: 239900,
      totalPrice: 0,
      emi: 0,
    };
  },
  created() {
    this.CalculateTotalPrice();
  },
  methods: {
    AddVariants(productComponentName, variantIndex) {
      this.productComponentsSelected[productComponentName] = variantIndex;
      this.CalculateTotalPrice();
    },
    CalculateEmi() {
      let emi = 0;
      emi = (this.totalPrice + (this.totalPrice * (13 / 100))) / 12;
      return Math.round(emi);
    },
    CalculateTotalPrice() {
      let price = this.basePrice;
      const totalVariants = Object.keys(this.productComponentsSelected).length;
      if (totalVariants > 0) {
        for (let i = 0; i < totalVariants; i += 1) {
          const productComponentName = Object.keys(this.productComponentsSelected)[i];
          const variantIndex = this.productComponentsSelected[productComponentName];
          // eslint-disable-next-line max-len
          const productComponentSelected = this.productComponents.find((j) => j.name === productComponentName);
          price += productComponentSelected.variants[variantIndex].price;
        }
      }
      this.totalPrice = price;
      this.emi = this.CalculateEmi();
    },
    getSelectedVariantTitle(productComponent) {
      const variantIndex = this.productComponentsSelected[productComponent.name] || 0;
      return productComponent.variants[variantIndex].title;
    },
  },
};
</script>

<style lang="scss" scoped>
    .product-customiser {
        font-family: 'Courier New', Courier, monospace;
        margin-left: auto;
        margin-right: auto;
        width: 1024px;

        .title {
            font-size: 40px;
            font-weight: bold;
            margin-bottom: 20px;
        }
        .body {
            margin-bottom: 20px;
            .left-body {
                width: 50%;
                float: left;
                .product-image {
                    width: 410px;
                    height: auto;
                }
            }
            .right-body {
                width: 50%;
                float: left;
                .product-description {
                    text-align: left;
                    .product-name {
                        font-size: 32px;
                        font-weight: bold;
                    }
                    .product-summary {
                        font-size: 14px;
                    }
                }
            }
            .clear-float {
                clear: both;
            }
        }
        .footer {
            min-height: 150px;
        }
    }
</style>
