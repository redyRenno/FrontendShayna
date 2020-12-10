<template>
  <div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container" id="app">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" v-bind:src="gambarDefault" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" 
                                        :nav="false"
                                        :dots="false">
                                    <div
                                      v-for="ss in productDetails.galleries"
                                      v-bind:key="ss.id" 
                                      class="pt" 
                                      @click="changeImage(ss.photo)" 
                                      v-bind:class="ss.photo == gambarDefault ? 'active' : ''">
                                        <img :src="ss.photo" alt="" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>${{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/shoppingCart">
                                        <a href="#" class="primary-btn pd-cart" 
                                          @click="saveKeranjang(productDetails.id, 
                                            productDetails.name, productDetails.price, productDetails.galleries[0].photo)">
                                            Add To Cart
                                        </a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedShayna />
    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from '@/components/HeaderShayna.vue';
import RelatedShayna from '@/components/RelatedShayna.vue';
import carousel from 'vue-owl-carousel';
import FooterShayna from '@/components/FooterShayna';
import axios from "axios";

export default {
  name: 'Product',
  components: {
    HeaderShayna,
    RelatedShayna,
    carousel,
    FooterShayna
  },
  data() {
    return {
        gambarDefault: "",
        productDetails: [],
        keranjangUser: []
    }
  },
  methods: {
    changeImage(ulrImage) {
        this.gambarDefault = ulrImage;
    },
    setDataPicture(data) {
        // replace object productDetails dengan data dari API
        this.productDetails = data;
        // replace value gambarDefault dengan data dari API (galleries)
        this.gambarDefault = data.galleries[0].photo;
    },
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
        var productStored = {
            "id": idProduct,
            "name": nameProduct,
            "price": priceProduct,
            "photo": photoProduct
        }

        this.keranjangUser.push(productStored);
        const parsed = JSON.stringify(this.keranjangUser);
        localStorage.setItem('keranjangUser', parsed);
    }
  },
  mounted() {
        if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }
        axios
            .get("http://127.0.0.1:8000/api/products", {
                params: {
                    id: this.$route.params.id
                }
            })
            .then(res => (this.setDataPicture(res.data.data)))
            // eslint-disable-next-line no-console
            .catch(err =>console.log(err))
    }
}
</script>

<style scoped>
    .product-thumbs .pt{
        margin-right: 10px;
    }
</style>