<template >
  <main>
    <section class="py-5">
      <div class="container">
        <div class="row mb-5">
          <div class="col-lg-6">
            <div id="carouselExampleDark" class="carousel carousel-dark slide" data-bs-ride="carousel">
              <div class="carousel-inner">
                <div v-for="(id, index) in reSpecIds" :key="index" :class="{ 'carousel-item': true, 'active': index === 0 }" :data-bs-interval="index === 0 ? 10000 : 2000">
                  <img :src="`http://localhost:8080/mall/productSpec/photo/${id}`" class="d-block w-100" style="height: 300px; width: 100%; object-fit: contain " :alt="reProductName">
                </div>
              </div>
              <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleDark" data-bs-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Previous</span>
              </button>
              <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleDark" data-bs-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Next</span>
              </button>
            </div>
          </div>
          <div class="col-lg-6">
            <h1>{{ reProductName }}</h1>
            <p class="text-muted lead">${{ rePrice }}</p>
            <p class="text-sm mb-4">{{ reProductDescription }}</p>
            <div class="row align-items-stretch mb-4">
              <select class="form-select" v-model="selectedSpec">
                <option disabled value="">請選擇商品顏色</option>
                <option v-for="spec in productSpecs" :key="spec.id" :value="spec">
                  {{ spec.color }}
                </option>
              </select>
              <div class="quantity-container">
                <span class="quantity-label">Quantity</span>
                <button class="quantity-btn dec-btn" @click="decrement">-</button>
                <input class="quantity-input" type="number" v-model="product.quantity" min="1">
                <button class="quantity-btn inc-btn" @click="increment">+</button>
                <button class="add-to-cart-btn" @click.prevent="addToCart">Add to Cart</button>
              </div>
            </div>

            <template v-if="!isTracked">
              <button class="btn btn-link text-dark p-0 mb-4 no-underline" @click="submitUpdate(this.UserID,this.reSpecIds)"><i class="far fa-heart me-2"></i>Add to wish list</button>
            </template>
            <template v-if="isTracked">
              <button class="btn btn-link text-dark p-0 mb-4 no-underline" @click="deleteTrack(this.UserID,this.reSpecIds)"><i class="fas fa-heart me-2 text-danger"></i>Delete this wish</button>
            </template>
            <br>
            <ul class="list-unstyled small d-inline-block">
            </ul>
          </div>
        </div>
        <!-- DETAILS TABS-->
        <ul class="nav nav-tabs border-0" id="myTab" role="tablist">
          <li class="nav-item"><a class="nav-link text-uppercase active" id="description-tab" data-bs-toggle="tab" href="#description" role="tab" aria-controls="description" aria-selected="true">Description</a></li>
          <li class="nav-item"><a class="nav-link text-uppercase" id="reviews-tab" data-bs-toggle="tab" href="#reviews" role="tab" aria-controls="reviews" aria-selected="false">Reviews</a></li>
        </ul>
        <div class="tab-content mb-5" id="myTabContent">
          <div class="tab-pane fade show active" id="description" role="tabpanel" aria-labelledby="description-tab">
            <div class="p-4 p-lg-5 bg-white">
              <h6 class="text-uppercase">Product description </h6>
              <p class="text-muted text-sm mb-0">{{reProductDescription}}</p>
            </div>
          </div>
          <div class="tab-pane fade" id="reviews" role="tabpanel" aria-labelledby="reviews-tab">
            <div class="p-4 p-lg-5 bg-white">
              <div class="row">
                <div class="col-lg-8">
                  <div class="d-flex mb-3">
                    <div class="flex-shrink-0"><img class="rounded-circle" src="../assets/img/customer-1.png" alt="" width="50"/></div>
                    <div class="ms-3 flex-shrink-1">
                      <h6 class="mb-0 text-uppercase">Peggy Lin</h6>
                      <p class="small text-muted mb-0 text-uppercase">11 April 2024</p>
                      <ul class="list-inline mb-1 text-xs">
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star-half-alt text-warning"></i></li>
                      </ul>
                      <p class="text-sm mb-0 text-muted">金屬質感佳，符合期待!</p>
                    </div>
                  </div>
                  <div class="d-flex">
                    <div class="flex-shrink-0"><img class="rounded-circle" src="../assets/img/customer-2.png" alt="" width="50"/></div>
                    <div class="ms-3 flex-shrink-1">
                      <h6 class="mb-0 text-uppercase">Ron Wu</h6>
                      <p class="small text-muted mb-0 text-uppercase">10 April 2024</p>
                      <ul class="list-inline mb-1 text-xs">
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star text-warning"></i></li>
                        <li class="list-inline-item m-0"><i class="fas fa-star-half-alt text-warning"></i></li>
                      </ul>
                      <p class="text-sm mb-0 text-muted">送貨速度快，包裝仔細，有機會再回購其他產品。</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
  <div class="modal fade" id="checkColorSelectModal" tabindex="-1" aria-labelledby="checkColorSelectModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header bg-light text-black">
          <h5 class="modal-title" id="checkColorSelectModalLabel">提醒！</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          請選擇商品顏色！<br>
          選擇顏色後，才可放入購物車！
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
  <div class="modal fade" id="addToCartModal" tabindex="-1" aria-labelledby="checkColorSelectModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header bg-light text-black">
          <h5 class="modal-title" id="checkColorSelectModalLabel">加入完成！</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          已加入購物車！<br>
          可至購物車下訂單！
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
  <div class="modal fade" id="pleaseLogInModal" tabindex="-1" aria-labelledby="checkColorSelectModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header bg-light text-black">
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          請先登入！
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
  <div class="modal fade" id="quantityLessThenOneModal" tabindex="-1" aria-labelledby="checkColorSelectModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header bg-light text-black">
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          數量不能少於一！
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import {useUserStore} from "@/stores/userStore.js";

function injectSvgSprite(path) {
  var ajax = new XMLHttpRequest();
  ajax.open("GET", path, true);
  ajax.send();
  ajax.onload = function(e) {
    var div = document.createElement("div");
    div.className = 'd-none';
    div.innerHTML = ajax.responseText;
    document.body.insertBefore(div, document.body.childNodes[0]);
  }
}

export default {
  data() {
    return {
      item: {
        quantity: 1,
        specId: null,
      },
      product: {
        id: null,
        quantity: 1,
        specId: ''
      },
      selectedSpec: '',
      productSpecs:[],
      reProductId: this.$route.query.reProductId,
      reProductName: this.$route.query.reProductName,
      rePrice: this.$route.query.rePrice,
      rePhotoId: this.$route.query.rePhotoId,
      reProductDescription: this.$route.query.reProductDescription,
      reSpecIds: this.$route.query.reSpecIds,
      isTracked: false,
      UserID: null,
      trackDTO: {
        userID: null,
        specID: null,
      },
    };
  },
  computed: {
    computedSpecId() {
      return this.reSpecIds && this.reSpecIds.length > 0 ? this.reSpecIds[0] : null;
    }
  },
  methods: {
    increment() {
      this.product.quantity++;
    },
    decrement() {
      if (this.product.quantity > 1) {
        this.product.quantity--;
      } else {
        this.showModal('quantityLessThenOneModal');
      }
    },
    showModal(modalId) {
      var myModal = new bootstrap.Modal(document.getElementById(modalId));
      myModal.show();
    },
    async addToCart() {
      const store = useUserStore();
      if (!store.isLoggedIn) {
        var myModal = new bootstrap.Modal(document.getElementById('pleaseLogInModal'));
        myModal.show();
        this.$router.push('/login');
        return;
      }
      if (!this.selectedSpec.specId) {
        var myModal = new bootstrap.Modal(document.getElementById('checkColorSelectModal'));
        myModal.show();
      }else{
        const payload = {
          userId: store.userId,
          specId: this.selectedSpec.specId,
          quantity: this.product.quantity,
        };
        console.log('Sending to backend:', payload);
        try {
          const response = await axios.post('http://localhost:8080/mall/cart/add', payload);
          console.log('Added to cart successfully:', response.data);
          var myModal = new bootstrap.Modal(document.getElementById('addToCartModal'));
          myModal.show();
        } catch (error) {
          console.error('Failed to add to cart:', error);
          if (error.response) {
            console.error('Response data:', error.response.data);
            console.error('Response status:', error.response.status);
          }
        }
      }
    },
    IsSpectRacked(userId, SpecIds) {
      const data = {
        userID: userId,
        specID: SpecIds
      };
      axios
          .post(`${this.API_URL}/check/track`, data)
          .then((response) => {
            console.log(response);
            this.isTracked = response.data;
          })
          .catch((error) => {
            console.log(error);
          });
    },
    submitUpdate(userId, SpecIds) {
      console.log(userId, SpecIds);
      const data = {
        userID: userId,
        specID: SpecIds
      };
      axios
          .post(`${this.API_URL}/create/track`, data)
          .then((response) => {
            console.log(response);
            this.isTracked = true;
          })
          .catch((error) => {
            console.log(error);
          });
    },
    deleteTrack(userId, SpecIds) {
      this.trackDTO.userID =userId;
      this.trackDTO.specID =SpecIds;
      console.log(userId, SpecIds);
      axios.delete(`${this.API_URL}/Dedelete/track`,{
        data: this.trackDTO,
      })
          .then((response) => {
            console.log(response);
            this.isTracked = false;
          })
          .catch((error) => {
            console.error('Error deleting:', error);
          });
    },
  },
  mounted() {
    console.log("Query Parameters:", this.$route.query);
    const reSpecIds = this.$route.query.reSpecIds;
    if (!reSpecIds) {
      console.error("No specIds found in the route query.");
      return;
    }
    const spId = Array.isArray(reSpecIds) ? reSpecIds : JSON.parse(reSpecIds);
    console.log("Parsed specIds:", spId);
    const spIdt = this.$route.query.reSpecIds;
    const userStore = useUserStore();
    if (userStore.isLoggedIn) {
      this.IsSpectRacked(userStore.userId, spIdt);
      this.UserID = userStore.userId;
    } else {
      console.log("會員未登入");
    }

    if (spId && spId.length > 0) {
      const requests = spId.map(specId => {
        return axios.get(`http://localhost:8080/mall/products/findProductSpecBySpecId/${specId}`);
      });
      console.log('All requests resolved:', requests);
      Promise.all(requests)
          .then(responses => {
            responses.forEach((response, index) => {
              console.log(`Response for specId ${spId[index]}:`, response.data);
              this.productSpecs[index] = response.data;
            });
            this.product.specId = spId[0];
          })
          .catch(error => {
            console.error('Error with specId requests:', error);
          });
    } else {
      console.error("SpecIds array is empty or undefined.");
    }
  }
}
</script>

<style scoped>
.text-danger {
  color: red;
}
.no-underline {
  text-decoration: none !important;
}
.quantity-container {
  display: flex;
  align-items: center;
  padding: 0.5rem;
  background-color: white;
  border: 1px solid #dee2e6;
  display: flex;
  align-items: center;
}

.quantity-label {
  margin-right: 0.75rem;
  text-transform: uppercase;
  color: #6c757d;
}

.quantity-btn {
  padding: 0.25rem 0.5rem;
  border: 1px solid #dee2e6;
  background-color: #f8f9fa;
  cursor: pointer;
}

.quantity-input {
  width: 3rem;
  text-align: center;
  margin: 0 0.5rem;
  border: 1px solid #ced4da;
}

.quantity-input::-webkit-inner-spin-button,
.quantity-input::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.quantity-input[type=number] {
  -moz-appearance: textfield;
}
.add-to-cart-btn {
  flex-grow: 1;
  padding: 0.5rem 1rem;
  color: white;
  background-color: #586074;
  border: none;
  cursor: pointer;
  margin-left: 16px;
  flex-grow: 1;
}
</style>
