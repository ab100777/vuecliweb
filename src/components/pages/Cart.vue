<template>
  <div>
    <loading :active.sync="isLoading"></loading>
    <div class="row justify-content-center negative-margin">
      <div class="col-sm-2 text-center text-info bg-warning checkout-step py-3">
        <h3>step1</h3>確認訂單
        <br />填寫資料
      </div>
      <div class="col-sm-2 text-center text-primary checkout-step py-3">
        <h3>step2</h3>確認付款
      </div>
    </div>
    <div
      class="my-5 row justify-content-center negative-margin"
      style="margin-left: 0px;margin-right: 0px;"
    >
      <div class="row justify-content-center negative-margin">
        <table class="table text-info bg-warning mb-0">
          <thead class="cart-head">
            <th></th>
            <th>商品</th>
            <th>數量</th>
            <th class="text-right">單價</th>
            <th></th>
          </thead>
          <tbody v-for="item in cart.carts" :key="item.id">
            <tr v-if="cart.carts">
              <td
                :style="{backgroundImage:`url(${item.product.imageUrl})`}"
                class="bg-cover cart-img"
              ></td>
              <td class="align-middle cart-title">
                {{item.product.title}}
                <div class="text-success" v-if="item.coupon">已套用優惠券</div>
              </td>
              <td class="align-middle">{{item.qty}}套</td>
              <td class="align-middle text-right">{{item.final_total | currency}}</td>
              <td class="align-middle" style="width:31px">
                <button
                  type="button"
                  class="btn btn-outline-danger btn-sm"
                  @click="removeCartItem(item.id)"
                >
                  <i class="far fa-trash-alt"></i>
                </button>
              </td>
            </tr>
          </tbody>
          <tfoot class="bg-primary">
            <tr>
              <td></td>
              <td colspan="3" class="text-right">總計</td>
              <td class="text-right">{{cart.total | currency}}</td>
            </tr>
            <tr v-if="cart.final_total!==cart.total">
              <td></td>
              <td colspan="3" class="text-right" style="font-size:20px;font-weight:bold">折扣價</td>
              <td class="text-right" style="font-size:20px">{{cart.final_total | currency}}</td>
            </tr>
          </tfoot>
        </table>
        <div class="text-info coupon-info mt-3 mb-1" style="font-size:20px;">輸入優惠券代碼：coupon 來享有折扣吧！</div>
        <div class="input-group pb-3 input-group-sm">
          <input
            type="text"
            class="form-cintrol coupon-input"
            v-model="coupon_code"
            placeholder="請輸入優惠碼"
          />
          <div class="input-group-append coupon-btn">
            <button
              class="btn btn-outline-secondary text-info"
              style="width:100%"
              type="button"
              @click="addCouponCode"
            >套用優惠碼</button>
          </div>
        </div>
      </div>
    </div>
    <div
      class="row justify-content-center negative-margin"
      style="padding-bottom:50px;margin-left: 0px;margin-right: 0px;"
    >
      <form class="col-md-6 text-info" @submit.prevent="createOrder">
        <div class="form-group">
          <label for="useremail">Email</label>
          <input
            type="email"
            class="form-control"
            name="email"
            id="useremail"
            v-validate="'required|email'"
            v-model="form.user.email"
            placeholder="請輸入電子信箱"
            required
          />
          <span class="text-danger" v-if="errors.has('email')">{{errors.first('email')}}</span>
        </div>

        <div class="form-group">
          <label for="username">收件人姓名</label>
          <input
            type="text"
            class="form-control"
            name="name"
            id="username"
            :class="{'is-invalid':errors.has('name')}"
            v-model="form.user.name"
            v-validate="'required'"
            placeholder="輸入姓名"
          />
          <span class="text-danger" v-if="errors.has('name')">必須輸入姓名</span>
        </div>

        <div class="form-group">
          <label for="usertel">收件人電話</label>
          <input
            type="tel"
            class="form-control"
            name="tel"
            id="usertel"
            :class="{'is-invalid':errors.has('tel')}"
            v-model="form.user.tel"
            v-validate="'required'"
            placeholder="請輸入電話"
          />
          <span class="text-danger" v-if="errors.has('tel')">必須輸入電話</span>
        </div>

        <div class="form-group">
          <label for="useraddress">收件人地址</label>
          <input
            type="text"
            class="form-control"
            name="address"
            id="useraddress"
            :class="{'is-invalid':errors.has('address')}"
            v-model="form.user.address"
            v-validate="'required'"
            placeholder="請輸入地址"
          />
          <span class="text-danger" v-if="errors.has('address')">請輸入地址</span>
        </div>

        <div class="form-group">
          <label for="comment">留言</label>
          <textarea
            name
            id="comment"
            class="form-control"
            cols="30"
            rows="10"
            v-model="form.message"
          ></textarea>
        </div>
        <div class="text-right">
          <button class="btn btn-danger">送出訂單</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import $ from "jquery";

export default {
  data() {
    return {
      products: [],
      // product: {},
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
      coupon_code: ""
    };
  },
  methods: {
    getProducts() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/products`;
      vm.isLoading = true;
      this.$http.get(url).then(response => {
        vm.products = response.data.products;
        console.log(response);
        vm.isLoading = false;
      });
    },
    // getProduct(id) {
    //   const vm = this;
    //   const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/product/${id}`;
    //   vm.status.loadingItem = id;
    //   this.$http.get(url).then(response => {
    //     vm.product = response.data.product;
    //     $("#productModal").modal("show");
    //     console.log(response);
    //     vm.status.loadingItem = "";
    //   });
    // },
    // addToCart(id, qty = 1) {
    //   const vm = this;
    //   const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/cart`;
    //   vm.status.loadingItem = id;
    //   const cart = {
    //     product_id: id,
    //     qty
    //   };
    //   this.$http.post(url, { data: cart }).then(response => {
    //     console.log(response);
    //     vm.status.loadingItem = "";
    //     vm.getCart();
    //     $("#productModal").modal("hide");
    //   });
    // },
    getCart() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/cart`;
      vm.isLoading = true;
      this.$http.get(url).then(response => {
        vm.cart = response.data.data;
        console.log(response);
        vm.isLoading = false;
      });
    },
    removeCartItem(id) {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/cart/${id}`;
      vm.isLoading = true;
      this.$http.delete(url).then(response => {
        vm.getCart();
        console.log(response);
        vm.isLoading = false;
      });
    },
    addCouponCode() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/coupon`;
      const coupon = {
        code: vm.coupon_code
      };
      vm.isLoading = true;
      this.$http.post(url, { data: coupon }).then(response => {
        vm.getCart();
        console.log(response);
        vm.isLoading = false;
      });
    },
    createOrder() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/order`;
      const order = vm.form;
      // vm.isLoading = true;
      this.$validator.validate().then(result => {
        if (result) {
          this.$http.post(url, { data: order }).then(response => {
            vm.getCart();
            console.log("訂單已建立", response);
            if (response.data.success) {
              vm.$router.push(`/customer_checkout/${response.data.orderId}`);
            }
            vm.isLoading = false;
          });
        } else {
          console.log("欄位不完整");
        }
      });
    }
  },
  created() {
    this.getProducts();
    this.getCart();
  }
};
</script>