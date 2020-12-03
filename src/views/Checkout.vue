<template>
  <div class="checkout-body">
    <div class="forms">
      <div class="first-form">
        <label for="cardnum">Karta raqami:</label>
        <input type="text" id="cardnum" v-model="card.number" />
        <label for="cardexpire">Karta muddati:</label>
        <input type="text" id="cardexpire" v-model="card.expire" />
        <button @click="sendCardData()">send card data</button>
      </div>
      <hr />
      <div class="second-form">
        <label for="smscode">SMS code:</label>
        <input type="text" id="smscode" v-model="smsCode" />
        <button @click="verify()">verify</button>
      </div>

      <hr />
      <div>
        <h3>create receipt:</h3>
        <div class="first-form">
          <label for="amount">Karta raqami:</label>
          <input type="number" id="amount" v-model="rec.amount" />
          <label for="description">Karta muddati:</label>
          <input type="text" id="description" v-model="rec.description" />
          <button @click="createReceipt()">create receipt</button>
        </div>
      </div>
    </div>

    <div class="results">
      <button @click="pay()">pay()</button>
      <button @click="send()">send()</button>
      <button @click="checkReceipt()">checkReceipt()</button>
      <button @click="getReceipt()">getReceipt()</button>
      <button @click="check()">check</button>
      <button @click="remove()">remove</button>
      <h3>result:</h3>
      <div style="max-width: 500px; height: 50vh; overflow-x: auto">
        {{ result }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      isFirstDone: false,
      result: null,
      card: {
        number: "",
        expire: "",
      },
      rec: {
        amount: 0,
        account: {
          order_id: 106,
        },
        description: "",
      },
      smsCode: "",
    };
  },
  methods: {
    getReceipt() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth":
            "5fbe275a967998274f7a7299:9&&vk6W%6QWoh3#OkdAOexCw4ZAEBYcg2mrv",
        },
        data: {
          id: 125,
          method: "receipts.get",
          params: {
            id: localStorage.getItem("id"),
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    checkReceipt() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth":
            "5fbe275a967998274f7a7299:9&&vk6W%6QWoh3#OkdAOexCw4ZAEBYcg2mrv",
        },
        data: {
          id: 125,
          method: "receipts.check",
          params: {
            id: localStorage.getItem("id"),
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    send() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth":
            "5fbe275a967998274f7a7299:9&&vk6W%6QWoh3#OkdAOexCw4ZAEBYcg2mrv",
        },
        data: {
          id: 125,
          method: "receipts.send",
          params: {
            id: localStorage.getItem("id"),
            phone: "998990785730",
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    pay() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth":
            "5fbe275a967998274f7a7299:9&&vk6W%6QWoh3#OkdAOexCw4ZAEBYcg2mrv",
        },
        data: {
          id: 125,
          method: "receipts.pay",
          params: {
            id: localStorage.getItem("id"),
            token: localStorage.getItem("token"),
            payer: {
              phone: "+998 33 099 00 01",
            },
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    createReceipt() {
      axios({
        url: "https://checkout.test.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth":"5fbe275a967998274f7a7299:9&&vk6W%6QWoh3#OkdAOexCw4ZAEBYcg2mrv",
        },
        data: {
          id: 125,
          method: "receipts.create",
          params: {
            amount: 1000,
            account: {
              order_id: 106,
            },
            description: "this is test",
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          localStorage.setItem("id", res.data.result.receipt._id);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },

    remove() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth": "5fbe275a967998274f7a7299",
        },
        data: {
          id: 125,
          method: "cards.remove",
          params: {
            token: localStorage.getItem("token"),
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    check() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth": "5fbe275a967998274f7a7299",
        },
        data: {
          id: 125,
          method: "cards.check",
          params: {
            token: localStorage.getItem("token"),
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    verify() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth": "5fbe275a967998274f7a7299",
        },
        data: {
          id: 125,
          method: "cards.verify",
          params: {
            token: localStorage.getItem("token"),
            code: this.smsCode,
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          this.result = res.data;
          this.check();
        })
        .catch((err) => {
          this.result = err;
        });
    },
    sendVerification() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth": "5fbe275a967998274f7a7299",
        },
        data: {
          id: 125,
          method: "cards.get_verify_code",
          params: {
            token: localStorage.getItem("token"),
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          localStorage.setItem("phone", res.data.result.phone);
          this.result = res.data;
        })
        .catch((err) => {
          this.result = err;
        });
    },
    sendCardData() {
      axios({
        url: "https://checkout.paycom.uz/api",
        method: "POST",
        headers: {
          "X-Auth": "5fbe275a967998274f7a7299",
        },
        data: {
          id: 125,
          method: "cards.create",
          params: {
            card: {
              number: this.card.number,
              expire: this.card.expire,
            },
            save: false,
          },
        },
      })
        .then((res) => {
          console.log("res", res);
          localStorage.setItem("token", res.data.result.card.token);
          this.result = res.data;
          this.sendVerification();
          this.isFirstDone = true;
        })
        .catch((err) => {
          this.result = err;
        });
    },
  },
};
</script>

<style scoped>
.checkout-body {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.first-form {
  display: flex;
  flex-direction: column;
}
</style>