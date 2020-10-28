<template>
  <div>
              <div id='stars'></div>
<div id='stars2'></div>
<div id='stars3'></div>
    <nav
      class="navbar is-black mb-6"
      role="navigation"
      aria-label="main navigation"
    >
      <div class="navbar-brand">
        <a class="navbar-item" href="*">
          <img
            src="https://upload.wikimedia.org/wikipedia/commons/0/04/MarvelLogo.svg"
            width="112"
            height="28"
            class="animate__animated animate__fadeInDown"
          />
        </a>

        <a
          role="button"
          class="navbar-burger burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbarBasicExample"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div id="navbarBasicExample" class="navbar-menu">
        <div class="navbar-end px-5 py-2">
          <div class="nav__cart">
            <button @click="showCart = !showCart">
              <i class="fas fa-shopping-bag fa-2x"></i>
            </button>
            <span class="total-quantity">{{ totalQuantity }}</span>
            <div v-if="showCart" class="cart-dropdown">
              <ul class="cart-dropdown__list">
                <li v-for="comic in cart" :key="comic.id">
                  {{ comic.name }} ({{ comic.quantity }})
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="columns columns--background">

      <div
        v-for="comic in comics"
        :key="comic.id"
        class="column is-2-desktop ml-5 animate__animated animate__slideInRight"
      >
        <div class="card">
          <div class="card-image">
            <figure class="image is-2by3">
              <img :src="comic.image" alt="Placeholder image" />
            </figure>
          </div>
          <div class="card-content">
            <div class="media">
              <div class="media-content">
                <p class="title is-4">{{ comic.name }}</p>
              </div>
            </div>

            <div class="content">
              {{ comic.description }}
            </div>
            <div class="buttons has-addons">
              <button
                class="button is-info is-selected"
                @click="updateCart(comic, 'add')"
              >
                +
              </button>
              <button
                class="button is-danger is-selected"
                @click="updateCart(comic, 'subtract')"
              >
                -
              </button>
              <span class="cart__quantity">{{ comic.quantity }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Comics",

  data() {
    return {
      comicName: [],
      comicImage: [],
      comics: [
        {
          id: 1,
          name: "Sentry, the (Trade Paperback)",
          description:"On the edge of alcoholism and a failed marriage, Bob Reynolds wakes up to discover his true nature. A forgotten hero, he must unravel the conspiracy to erase his memory from mankind before an evil entity returns.",
          quantity: 0,
          image:"http://i.annihil.us/u/prod/marvel/i/mg/f/c0/4bc66d78f1bee/portrait_uncanny.jpg",
        },
        {
          id: 2,
          name: "The Marvel Universe (2004)",
          description:"Heavy-hitting heroes unite! This Official Handbook contains in-depth bios on more than 30 of the Marvel Universe's most awesome assemblages - including the Defenders, Power Pack and the New Thunderbolts! ",
          quantity: 0,
          image:"http://i.annihil.us/u/prod/marvel/i/mg/f/20/4bc63a47b8dcb/portrait_uncanny.jpg",
        },
        {
          id: 3,
          name: "Ant-Man (2003)",
          description:"Ant-Man digs deeper into finding out who is leaking those dirty little secrets that are threatening our national security. And who's better at uncovering dirty LITTLE secrets than him??",
          quantity: 0,
          image:"http://i.annihil.us/u/prod/marvel/i/mg/4/20/4bc697c680890/portrait_uncanny.jpg",
        },
      ],
      showCart: false,
    };
  },
  computed: {
    cart() {
      return this.comics.filter((comic) => comic.quantity > 0);
    },
    totalQuantity() {
      return this.comics.reduce((total, comic) => total + comic.quantity, 0);
    },
  },

  created() {
    fetch(
      "https://gateway.marvel.com:443/v1/public/comics?ts=1&apikey=ba06a51bdd14b79e7cce9be90ea1fdd3&hash=90e24cd3ddf942458632edb7f82bc42c"
    )
      .then((response) => response.json())
      .then((data) => {
        this.comicName = data.data.results["1"].title;
        this.comicImage =
          data.data.results["4"].thumbnail.path + "/portrait_fantastic.jpg";
   //si resulto llamado a la api, pero era muy complicada, tuve que sacar una apikey y generar una clave md5       
      });
  },
  methods: {
    updateCart(comic, updateType) {
      for (let i = 0; i < this.comics.length; i++) {
        if (this.comics[i].id === comic.id) {
          if (updateType === "subtract") {
            if (this.comics[i].quantity !== 0) {
              this.comics[i].quantity--;
            }
          } else {
            this.comics[i].quantity++;
          }

          break;
        }
      }
    },
  },
};
</script>


<style scoped>
.columns--background{
  background-image: url(https://i.postimg.cc/sxJFhWF2/galactus.png);
  background-repeat: no-repeat;
  background-position: right bottom;
}
</style>
