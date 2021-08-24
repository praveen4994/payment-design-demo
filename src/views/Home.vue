<template>
  <div class="home">
    <header>
      <img src="@/assets/img/arrow.png" alt="Arrow" class="back" />
      <p class="title">Make Payment</p>
    </header>
    <div class="payment-selection">
      <div
        class="option"
        :class="{ selected: selectedPayment === 1 }"
        @click="selectPayment(1)"
      >
        <div class="details">
          <p class="title">Pay total due amount</p>
          <p class="amount">Rs. 10,000</p>
        </div>
        <div class="radio">
          <div class="round"></div>
        </div>
      </div>
      <div
        class="option"
        :class="{
          selected: selectedPayment === 2,
          'amount-unselected': customAmount <= 0,
        }"
        @click="selectPayment(2)"
      >
        <div class="details">
          <p class="hint">Pay custom amount</p>
          <p class="title">Paying custom amount</p>
          <p class="amount">Rs. {{ this.customAmount | toCurrency }}</p>
        </div>
        <p class="edit-btn" v-if="customAmount > 0">Edit</p>
        <div class="radio" v-else>
          <div class="round"></div>
        </div>
      </div>
    </div>
    <div class="payment-details" v-if="showPaymentSelection">
      <p class="heading">Choose a payment method</p>
      <p class="title">UPI</p>
      <div class="upi-options">
        <div class="item">
          <div class="icon">
            <img src="@/assets/img/pp.png" class="pp" />
          </div>
          <p class="title">PhonePe</p>
        </div>
        <div class="item">
          <div class="icon">
            <img src="@/assets/img/bhim.png" class="bhim" />
          </div>
          <p class="title">Bhim</p>
        </div>
        <div class="item">
          <div class="icon">
            <img src="@/assets/img/paytm.png" class="paytm" />
          </div>
          <p class="title">Paytm</p>
        </div>
        <div class="item">
          <div class="icon">
            <img src="@/assets/img/gpay.png" class="gpay" />
          </div>
          <p class="title">GooglePay</p>
        </div>
      </div>
      <p class="title">Deposit money in account</p>
      <div class="account-details">
        <div class="head">
          <p class="name">John Doe</p>
          <p class="bank">Axis Bank</p>
        </div>
        <div class="data">
          <div class="key">Account Number</div>
          <div class="value">123464859908XFGUJ</div>
        </div>
        <div class="data">
          <div class="key">IFSC Code</div>
          <div class="value">UTIXFRD</div>
        </div>
      </div>
    </div>
    <bottom-sheet :isOpen="showBottomSheet" @close="closeBottomSheet">
      <p class="title">Pay custom amount</p>
      <input
        type="tel"
        class="amount"
        v-model="amount"
        placeholder="Enter amount"
      />
      <button class="btn btn-primary" @click="amountSelected">Proceed</button>
    </bottom-sheet>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import BottomSheet from "@/components/BottomSheet.vue";

@Component({
  components: { BottomSheet },
  filters: {
    toCurrency: (amount: number) => {
      return amount.toLocaleString();
    },
  },
})
export default class Home extends Vue {
  private showPaymentSelection = false;
  private showBottomSheet = false;
  private selectedPayment = 0;
  private customAmount = 0;
  private amount = "";

  private selectPayment(val: number) {
    this.selectedPayment = val;
    if (val === 2) {
      this.showBottomSheet = true;
    } else {
      this.showPaymentSelection = true;
      this.customAmount = 0;
      this.amount = "";
    }
  }

  private closeBottomSheet() {
    this.showBottomSheet = false;
    this.amount = this.customAmount.toString();
  }

  private amountSelected() {
    this.customAmount = parseInt(this.amount, 10);
    this.showPaymentSelection = true;
    this.closeBottomSheet();
  }
}
</script>

<style lang="scss" src="@/styles/home.scss"></style>
