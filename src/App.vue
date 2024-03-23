<template>
  <div id="app">
    <div class="alert" v-if="message">
      {{ message }}
    </div>

    <h1>Ürün Listesi</h1>
    <div class="apple">
      <img src="./assets/apple_m02x3ll_a_watch_series_6_gps_1595000-2514716391.jpg" alt="" width="60px">
      <div></div>
      <img src="./assets/MacBook-Air-featured-2648240706.jpg" alt="" width="70px">
      <div></div>
      <img src="./assets/710rzW2RGcL._SL1500_-2-1489044999.jpg" alt="" width="60px">
      <div></div>
      <img src="./assets/806667_01-815320266.jpg" alt="" width="60px">
    </div>
    <ul class="ul1">
      <li v-for="product in products" :key="product.id" class="li1">
        <img src="./assets/shopping-basket.png" alt="" width="10px">
        {{ product.name }} - Stok: {{ product.stock }} - Fiyat: {{ product.price }}TL
        <button class="btn1" @click="addToCart(product)">Sepete Ekle</button>
      </li>
    </ul>
    <hr class="hr1">
    <h1>Sepet</h1>

    <ul class="ul2">
      <li v-for="item in cart" :key="item.id">
        <div class="apple2">
          <img v-if="item.id === 1" src="./assets/apple_m02x3ll_a_watch_series_6_gps_1595000-2514716391.jpg" alt=""
               width="60px">
          <div></div>
          <img v-if="item.id === 2" src="./assets/MacBook-Air-featured-2648240706.jpg" alt="" width="70px">
          <div></div>
          <img v-if="item.id === 3" src="./assets/710rzW2RGcL._SL1500_-2-1489044999.jpg" alt="" width="60px">
          <div></div>
          <img v-if="item.id === 4" src="./assets/806667_01-815320266.jpg" alt="" width="60px">
        </div>
        {{ item.name }} - Miktar: {{ item.quantity }} - Fiyat : {{ calculatePrice(item) }}TL <br>
        <button class="btn1" @click="removeFromCart(item)">Sepeten Cıkar</button>
      </li>
    </ul>

    <div class="div1">
      <div class="toplam" v-if="calculateTotalPrice() > 0">
        Total: {{ calculateTotalPrice() }} TL
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        {
          id: 1,
          name: 'Apple Watch',
          stock: 5,
          price: 1000,
          img: "./assets/apple_m02x3ll_a_watch_series_6_gps_1595000-2514716391.jpg"
        },
        {id: 2, name: 'Macbook air M1', stock: 3, price: 3000, img: "./assets/MacBook-Air-featured-2648240706.jpg"},
        {id: 3, name: 'Airpods Kulaklık', stock: 7, price: 3000, img: "./assets/710rzW2RGcL._SL1500_-2-1489044999.jpg"},
        {id: 4, name: 'Ayakkabı', stock: 10, price: 2500, img: "./assets/806667_01-815320266.jpg"}
      ],
      cart: [],
      message: null,
      timer: null
    };
  },
  methods: {
    calculateTotalPrice() {
      return this.products.reduce((acc, product) => {
        const cart = this.cart.find(cart => cart.id === product.id);
        if (!cart) return acc;
        return acc + cart.quantity * product.price;
      }, 0)
    },
    calculatePrice(item) {
      const product = this.products.find((product) => product.id === item.id)
      return product.price * item.quantity
    },
    addToCart(product) {
      if (product.stock > 0) {
        const cartItem = this.cart.find(item => item.id === product.id);
        if (cartItem) {

          cartItem.quantity++;
        } else {
          this.cart.push({id: product.id, name: product.name, quantity: 1});
        }
        product.stock--;
      } else {
        if (this.timer) clearTimeout(this.timer)
        this.message = "Bu ürün stokta bulunmamaktadır!";
        this.timer = setTimeout(() => {
          this.message = null
        }, 3 * 1000)
      }
    },

    removeFromCart(item) {
      const cartItemIndex = this.cart.findIndex(cartItem => cartItem.id === item.id);
      if (cartItemIndex !== -1) {
        const cartItem = this.cart[cartItemIndex];
        if (cartItem.quantity > 1) { // Sepetten çıkarılan ürünün miktarı 1'den büyükse
          cartItem.quantity--; // Sepet miktarından bir azaltılır
          const product = this.products.find(product => product.id === item.id);
          if (product) {
            product.stock++; // Stoğa bir ürün eklenir
          }
        } else { // Sepetten çıkarılan ürün miktarı 1 ise
          this.cart.splice(cartItemIndex, 1); // Sepetten ürün tamamen çıkarılır
          const product = this.products.find(product => product.id === item.id);
          if (product) {
            product.stock++; // Stoğa bir ürün eklenir
          }
        }
      }
    }
  }
};
</script>


<style>
h1 {
  text-align: center;
}

li {
  text-align: center;
  list-style-type: none;
}

@keyframes slide {
  0% {
    right: -100%;
  }
  100% {
    right: 10px
  }
}

.alert {
  position: absolute;
  top: 10px;
  right: 10px;
  padding: 20px;
  background-color: red;
  animation: slide 1s;
}

.div1 {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}

.toplam {
  display: flex;
  text-align: center;
  align-items: center;
  justify-content: center;
  border: solid 1px black;
  width: 10%;
}

.ul1 {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 100px;
  width: 100%;
  padding: 0;
  margin: 0
}

.ul2 {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 100px;
  width: 100%;
  padding: 0;
  margin: 0
}

.btn1 {
  margin: 5px;
  background-color: white;
  border: solid 1px black;
  width: 50%;
  height: 30px;
  border-radius: 5px;
}

.apple {
  display: flex;
  width: 100%;
  gap: 160px;
  justify-content: center;
  align-items: center;
}

.apple2 {
  display: flex;
  justify-content: center;
  align-items: center;
}

.hr1 {
  width: 50%;
  margin-top: 10px;
}

</style>