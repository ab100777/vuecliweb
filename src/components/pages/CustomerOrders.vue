<template>
  <div>
    <loading :active.sync="isLoading"></loading>
    <div class="container">
      <b-alert
        v-model="showDismissibleAlert"
        class="cart-alert"
        variant="success"
        dismissible
      >成功加入購物車</b-alert>
      <div class="row mt-5">
        <div class="col-sm-3 mb-5 px-sm-3 px-0">
          <div
            class="list-group"
            style="font-weight: bolder;font-size: 24px;background-color: rgb(43, 42, 65)"
          >
            <a
              href="#all"
              class="list-group-item list-group-item-action text-center text-primary border-0 active"
              style="background-color: rgb(43, 42, 65)"
              data-toggle="list"
            >所有類別</a>
            <a
              href="#RPG"
              class="list-group-item list-group-item-action text-center text-primary border-0"
              style="background-color: rgb(43, 42, 65)"
              data-toggle="list"
              @click.prevent="link='RPG'"
            >角色扮演</a>
            <a
              href="#shooting"
              class="list-group-item list-group-item-action text-center text-primary border-0"
              style="background-color: rgb(43, 42, 65)"
              data-toggle="list"
            >射擊</a>
            <a
              href="#action"
              class="list-group-item list-group-item-action text-center text-primary border-0"
              style="background-color: rgb(43, 42, 65)"
              data-toggle="list"
            >動作</a>
            <a
              href="#adventure"
              class="list-group-item list-group-item-action text-center text-primary border-0"
              style="background-color: rgb(43, 42, 65)"
              data-toggle="list"
            >冒險</a>
          </div>
        </div>
        <div class="col-sm-9">
          <div class="tab-content">
            <div class="search-input mb-3" style="display:flex">
              <span class="text-secondary ml-auto">搜尋</span>
              <input type="text" name id class="bg-primary border-0" style v-model.trim="search" />
            </div>
            <div
              class="text-primary text-center mt-5"
              style="font-size:40px"
              v-if="searchTitle.length==0"
            >抱歉，找不到此產品</div>
            <div class="tab-pane active" id="all" role="tabpanel">
              <div class="row">
                <div class="col-sm-6 mb-4" v-for="item in searchTitle" :key="item.id">
                  <a href="#" @click="getProduct(item.id)">
                    <div
                      class="bg-cover item-img img-hover"
                      :style="{backgroundImage:`url(${item.imageUrl})`}"
                    ></div>
                  </a>
                  <div class="row">
                    <div class="col-sm-6">
                      <div
                        class="item-name text-secondary py-2 text-center border-right-0"
                      >{{item.title}}</div>
                    </div>
                    <div class="col-sm-6">
                      <div
                        class="item-price text-secondary py-2 text-center"
                      >{{item.price | currency}}</div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-12">
                      <button
                        class="item-btn btn btn-block btn-primary text-info rounded-0 py-3"
                        @click="addToCart(item.id)"
                      >加入購物車</button>
                    </div>
                  </div>
                </div>
              </div>
              <nav aria-label="Page navigation example" v-if="searchTitle.length!==0">
                <ul class="pagination">
                  <li class="page-item" :class="{'disabled':!pagination.has_pre}">
                    <a
                      class="page-link"
                      href="#"
                      aria-label="Previous"
                      @click.prevent="getProducts(pagination.current_page - 1)"
                    >
                      <span aria-hidden="true">&laquo;</span>
                    </a>
                  </li>
                  <li
                    class="page-item"
                    v-for="page in pagination.total_pages"
                    :key="page"
                    :class="{'active':pagination.current_page===page}"
                  >
                    <a class="page-link" href="#" @click.prevent="getProducts(page)">{{page}}</a>
                  </li>
                  <li class="page-item" :class="{'disabled':!pagination.has_next}">
                    <a
                      class="page-link"
                      href="#"
                      aria-label="Next"
                      @click.prevent="getProducts(pagination.current_page + 1)"
                    >
                      <span aria-hidden="true">&raquo;</span>
                    </a>
                  </li>
                </ul>
              </nav>
            </div>
            <div class="tab-pane" id="RPG" role="tabpanel">
              <div class="row">
                <div
                  class="col-sm-6 mb-4"
                  v-for="item in searchTitle"
                  :key="item.id"
                  v-if="item.category=='角色扮演'"
                >
                  <div
                    class="bg-cover item-img img-hover"
                    :style="{backgroundImage:`url(${item.imageUrl})`}"
                  ></div>
                  <div class="row">
                    <div class="col-sm-6">
                      <div
                        class="item-name text-secondary py-2 text-center border-right-0"
                      >{{item.title}}</div>
                    </div>
                    <div class="col-sm-6">
                      <div
                        class="item-price text-secondary py-2 text-center"
                      >{{item.price | currency}}</div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-12">
                      <button
                        class="item-btn btn btn-block btn-primary text-info rounded-0 py-3"
                        @click="addToCart(item.id)"
                      >加入購物車</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="tab-pane" id="shooting" role="tabpanel">
              <div class="row">
                <div
                  class="col-sm-6 mb-4"
                  v-for="item in searchTitle"
                  :key="item.id"
                  v-if="item.category=='射擊'"
                >
                  <div
                    class="bg-cover item-img img-hover"
                    :style="{backgroundImage:`url(${item.imageUrl})`}"
                  ></div>
                  <div class="row">
                    <div class="col-sm-6">
                      <div
                        class="item-name text-secondary py-2 text-center border-right-0"
                      >{{item.title}}</div>
                    </div>
                    <div class="col-sm-6">
                      <div
                        class="item-price text-secondary py-2 text-center"
                      >{{item.price | currency}}</div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-12">
                      <button
                        class="item-btn btn btn-block btn-primary text-info rounded-0 py-3"
                        @click="addToCart(item.id)"
                      >加入購物車</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="tab-pane" id="action" role="tabpanel">
              <div class="row">
                <div
                  class="col-sm-6 mb-4"
                  v-for="item in searchTitle"
                  :key="item.id"
                  v-if="item.category=='動作'"
                >
                  <div
                    class="bg-cover item-img img-hover"
                    :style="{backgroundImage:`url(${item.imageUrl})`}"
                  ></div>
                  <div class="row">
                    <div class="col-sm-6">
                      <div
                        class="item-name text-secondary py-2 text-center border-right-0"
                      >{{item.title}}</div>
                    </div>
                    <div class="col-sm-6">
                      <div
                        class="item-price text-secondary py-2 text-center"
                      >{{item.price | currency}}</div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-12">
                      <button
                        class="item-btn btn btn-block btn-primary text-info rounded-0 py-3"
                        @click="addToCart(item.id)"
                      >加入購物車</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="tab-pane" id="adventure" role="tabpanel">
              <div class="row">
                <div
                  class="col-sm-6 mb-4"
                  v-for="item in searchTitle"
                  :key="item.id"
                  v-if="item.category=='冒險'"
                >
                  <div
                    class="bg-cover item-img img-hover"
                    :style="{backgroundImage:`url(${item.imageUrl})`}"
                  ></div>
                  <div class="row">
                    <div class="col-sm-6">
                      <div
                        class="item-name text-secondary py-2 text-center border-right-0"
                      >{{item.title}}</div>
                    </div>
                    <div class="col-sm-6">
                      <div
                        class="item-price text-secondary py-2 text-center"
                      >{{item.price | currency}}</div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-12">
                      <button
                        class="item-btn btn btn-block btn-primary text-info rounded-0 py-3"
                        @click="addToCart(item.id)"
                      >加入購物車</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery";
import SwiperSlider from "../SwiperSlider";

export default {
  data() {
    return {
      products: [],
      product: {},
      pagination: {},
      status: {
        loadingItem: ""
      },
      form: {
        user: {
          name: "",
          email: "",
          tel: "",
          address: ""
        },
        message: ""
      },
      cart: {},
      isLoading: false,
      coupon_code: "",
      showDismissibleAlert: false,
      search: ""
    };
  },
  components: {
    SwiperSlider
  },
  methods: {
    getProducts(page) {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/products?page=${page}`;
      vm.isLoading = true;
      this.$http.get(url).then(response => {
        vm.products = response.data.products;
        console.log(response);
        vm.isLoading = false;
        vm.pagination = response.data.pagination;
      });
    },
    getProduct(id) {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/product/${id}`;
      vm.status.loadingItem = id;
      this.$http.get(url).then(response => {
        vm.product = response.data.product;
        console.log(response);
        vm.$router.push(`/commodity/${id}`);
        vm.status.loadingItem = "";
      });
    },
    addToCart(id, qty = 1) {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/cart`;
      vm.status.loadingItem = id;
      const cart = {
        product_id: id,
        qty
      };
      vm.showDismissibleAlert = false;
      this.$http.post(url, { data: cart }).then(response => {
        console.log(response);
        vm.status.loadingItem = "";
        vm.getCart();
        if (response.data.success) {
          vm.showDismissibleAlert = true;
        }
      });
    },
    getCart() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/cart`;
      vm.isLoading = true;
      this.$http.get(url).then(response => {
        vm.cart = response.data.data;
        console.log(response);
        vm.isLoading = false;
      });
    }
  },
  computed: {
    searchTitle() {
      if (this.search) {
        return this.products.filter(item => {
          return (
            item.title.toLowerCase().indexOf(this.search.toLowerCase()) != -1
          );
        });
      } else {
        return this.products;
      }
      console.log(searchTitle.length);
    }
  },
  created() {
    this.getProducts();
    this.getCart();
  }
};
</script>