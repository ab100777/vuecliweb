<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-sm-2 text-center text-primary checkout-step py-3">
        <h3>step1</h3>確認訂單
        <br />填寫資料
      </div>
      <div class="col-sm-2 text-center text-info bg-warning checkout-step mx-3 py-3">
        <h3>step2</h3>確認付款
      </div>
    </div>
    <div class="my-5 row justify-content-center negative-margin">
      <form class="col-md-6" style="padding-bottom:50px" @submit.prevent="payOrder">
        <table class="table text-info">
          <thead class="cart-head">
            <th></th>
            <th>品名</th>
            <th>數量</th>
            <th>單價</th>
          </thead>
          <tbody>
            <tr v-for="item in order.products" :key="item.id">
              <td
                :style="{backgroundImage:`url(${item.product.imageUrl})`}"
                class="bg-cover cart-img"
              ></td>
              <td class="align-middle">{{ item.product.title }}</td>
              <td class="align-middle">{{ item.qty }}套</td>
              <td class="align-middle text-right">{{ item.final_total | currency}}</td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <td></td>
              <td colspan="2" class="text-right">總計</td>
              <td class="text-right">{{ order.total | currency}}</td>
            </tr>
          </tfoot>
        </table>

        <table class="table text-info">
          <tbody>
            <tr>
              <th width="100">Email</th>
              <td>{{ order.user.email }}</td>
            </tr>
            <tr>
              <th>姓名</th>
              <td>{{ order.user.name }}</td>
            </tr>
            <tr>
              <th>收件人電話</th>
              <td>{{ order.user.tel }}</td>
            </tr>
            <tr>
              <th>收件人地址</th>
              <td>{{ order.user.address }}</td>
            </tr>
            <tr>
              <th>付款狀態</th>
              <td>
                <span v-if="!order.is_paid">尚未付款</span>
                <span v-else class="text-success">付款完成</span>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="text-right" v-if="order.is_paid === false">
          <button class="btn btn-danger">確認付款去</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      order: {
        user: {}
      },
      orderId: ""
    };
  },
  methods: {
    getOrder() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/order/${vm.orderId}`;
      vm.isLoading = true;
      this.$http.get(url).then(response => {
        vm.order = response.data.order;
        console.log(response);
        vm.isLoading = false;
      });
    },
    payOrder() {
      const vm = this;
      const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/pay/${vm.orderId}`;
      vm.isLoading = true;
      this.$http.post(url).then(response => {
        console.log(response);
        if (response.data.success) {
          vm.getOrder();
        }
        vm.isLoading = false;
      });
    }
  },
  created() {
    this.orderId = this.$route.params.orderId;
    this.getOrder();
    console.log(this.orderId);
  }
};
</script>