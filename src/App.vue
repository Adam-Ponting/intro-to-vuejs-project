<template>
  <div id="app">
    <div id="top-nav">
      <a href="https://adp-cv.web.app/" target="_blank" title="Go to my website">
        <img src="./assets/ap-logo.png" alt />
      </a>
      <h1>Premier League 2020</h1>
    </div>
    <div id="product-and-basket">
      <app-product-info
        id="product"
        @addToCart="addToCart"
        @removeFromCart="removeFromCart"
        :clubs="clubs"
      />
      <div id="basket">
        <h3>Basket</h3>
        <p v-show="cart.length === 0" class="no-items">Basket is empty</p>
        <ul>
          <li v-for="(item, index) in cart" :key="index">
            1 x {{ item.team }} Shirt, £{{ item.price }}
            <button
              @click="removeFromBasket(index)"
              title="Remove Item"
            >&#10006; Remove</button>
          </li>
        </ul>
        <div class="totals">
          <div>
            <p>Basket Items: {{ cart.length }}</p>
            <p>Basket Total : £{{ cartTotal() }}</p>
          </div>
          <button @click="checkout" :disabled="cart.length === 0" :class="checkoutStyle">Checkout</button>
        </div>
      </div>
    </div>
    <div id="tabs">
      <span
        v-for="(tab, index) in tabs"
        :key="index"
        @click="selectedTab = tab"
        :class="{ activeTab: selectedTab === tab }"
        class="tab"
      >{{ tab }}</span>
    </div>

    <app-make-review v-show="selectedTab === 'Make a Review'" @review-submitted="reviewSubmitted" />
    <app-view-review v-show="selectedTab === 'View Reviews'" :reviews="reviews" />
    <div id="bottom-nav">
      <h1>Premier League 2020</h1>
      <div>
        <a href="https://adp-cv.web.app/" target="_blank" title="Go to my website">
          <img src="./assets/ap-logo.png" alt />
        </a>
        <a
          href="https://github.com/Mada75/intro-to-vuejs-project/"
          target="_blank"
          title="View on GitHub"
        >
          <img src="./assets/github.png" alt />
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import appMakeReview from '@/components/appMakeReview.vue'
import appProductInfo from '@/components/appProductInfo.vue'
import appViewReview from '@/components/appViewReview.vue'

export default {
  name: 'app',
  components: {
    appMakeReview,
    appProductInfo,
    appViewReview
  },
  data() {
    return {
      clubs: [
        {
          team: 'Manchester City',
          icon: require('@/assets/mancity.png'),
          shirt: require('@/assets/kitMCI.png'),
          description:
            'After a season that saw The Cityzens claim a domestic treble, cheer the team on to further success with the brand new kits for the 2019/2020 season by PUMA. <br/> Developed with dryCELL technology, the kit will ensure you remain dry and comfortable whether you are playing a full ninety minutes or cheering on from the stands.  <br/> The kit also features detailing to celebrate the clubs 125th anniversary, allowing you to reminisce about the clubs most iconic moments from the 1999 play-off heroics to the clubs first Premier League title, this kit will keep you singing Blue Moon with pride.',
          color: '#6CABDD',
          id: 'MCI',
          quantity: 3,
          price: 49.99
        },
        {
          team: 'Liverpool',
          icon: require('@/assets/lfc.png'),
          shirt: require('@/assets/kitLIV.png'),
          description:
            'Be matchday ready and cheer on The Reds for the 2019/2020 season with the brand new Liverpool kit brought to you by New Balance.  <br/> The kit pays homage to club legend Bob Paisley by featuring the icon’s signature in the inside of the neck, whilst the pinstripe design was first introduced during the 82/83 season when the Liverpool great was manager.  <br/> The shirt features NBdry technology to sweep moisture away from your skin, whilst the body mapping and mesh panelling enhances ventilation to ensure you will remain comfortable on the pitch or singing You’ll Never Walk Alone amongst the Anfield faithful.',
          color: ' #C8102E',
          id: 'LIV',
          quantity: 3,
          price: 59.99
        },
        {
          team: 'Chelsea',
          icon: require('@/assets/chelsea.png'),
          shirt: require('@/assets/kitCHE.png'),
          description:
            'Be ready for the 2019/2020 season with the new Chelsea home kit engineered by Nike.  <br/> The kit has been developed with the latest in performance technology including lightweight Nike Breathe fabric for superior breathability, whilst Dri-Fit wicks moisture away from your skin for complete comfort.  <br/> The shirts feature the iconic Chelsea blue with the stylish addition of red and white darts across the torso and the team crest shows exactly where your loyalty lies.',
          color: '#034694',
          id: 'CHE',
          quantity: 3,
          price: 39.99
        },
        {
          team: 'Tottenham Hotspur',
          icon: require('@/assets/spurs.png'),
          shirt: require('@/assets/kitTOT.png'),
          description:
            'Cheer on Tottenham Hotspur to success with the latest Spurs kit which is brought to you by Nike for the 2019/2020 season.  <br/> The shirts have been developed with the latest Nike technologies including Nike Breathe and Dri-Fit which supplies superior ventilation and moisture wicking capabilities to keep you feeling comfortable.  <br/> Look great at White Hart Lane or out and about in London as you support Tottenham to European and league success.',
          color: '#132257',
          id: 'TOT',
          quantity: 3,
          price: 19.99
        }
      ],
      cart: [],
      stock: '',
      removedItem: [],
      tabs: ['View Reviews', 'Make a Review'],
      selectedTab: 'View Reviews',
      reviews: []
    }
  },
  methods: {
    reviewSubmitted(review) {
      this.reviews.push(review) // push review object to reviews array
      this.selectedTab = 'View Reviews'
    },
    checkout() {
      alert(`${this.cartTotal()} to pay`)
    },
    removeFromBasket(i) {
      let self = this
      let removedItem = this.cart.splice(i, 1) // remove at position [i], 1 item
      console.log('removing item =>', removedItem[0].id)
      this.removedItem = removedItem[0].id
      var deletedClubShirt = this.clubs.find(function(club) {
        return club.id === self.removedItem
      })
      deletedClubShirt.quantity += 1
    },
    addToCart(selectedShirt) {
      console.log('adding item =>', selectedShirt.team)
      selectedShirt.quantity -= 1
      this.cart.push(selectedShirt)
    },
    removeFromCart(selectedShirt) {
      console.log('removing', selectedShirt.team)
      selectedShirt.quantity += 1

      this.cart.pop()
    },
    cartTotal() {
      /* https://stackoverflow.com/questions/50670204/sum-up-array-with-objects */
      /* https://medium.com/@tkwebdev/can-be-reduced-to-71c5c412ca50#--responses */
      let cart = this.cart
      const sum = cart.reduce((total, { price }) => total + price, 0)
      console.log(sum)
      return sum.toFixed(2)
    }
  },
  computed: {
    checkoutStyle() {
      return {
        disabledButton: this.cart.length === 0
      }
    }
  }
}
</script>

<style lang="scss">
#tabs {
  margin: 1rem;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: stretch;
}
.tab {
  cursor: pointer;
  margin: 0 0.5rem;
  border: 2px solid #f5f5f5;
  padding: 5px 10px;
  background: lighten($color: #37003c, $amount: 10%);
  color: white;
  &:hover {
    background: lighten($color: #37003c, $amount: 20%);
  }
}
.activeTab {
  background: lighten($color: #37003c, $amount: 20%);
}
.disabledButton {
  background: grey !important;
  &:hover {
    background: grey !important;
    cursor: default !important;
  }
}
#top-nav {
  background: linear-gradient(to right, #fff, 30%, #37003c);
  text-align: right;
  color: white;
  padding: 1rem;
  font-size: 1.3rem;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  a,
  img {
    height: 48px;
  }
}
#bottom-nav {
  background: linear-gradient(to left, #fff, 30%, #37003c);
  text-align: left;
  color: white;
  padding: 1rem;
  font-size: 1.3rem;
  width: 100%;
  margin-top: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  div {
    text-align: center;
  }
  img {
    margin: 0 0.25rem;
    height: 48px;
  }
}

#product-and-basket {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: stretch;
  background: #f5f5f5;
}
#product {
  flex: 1 1 800px;
}
.no-items {
  margin: 1rem;
  text-align: right;
}
#basket {
  flex: 1 1 400px;
  margin: 1rem;
  display: flex;
  flex-direction: column;
  border: 1px solid #37003c;
  background: #f5f5f5;
  h3 {
    background: #37003c;
    color: white;
    font-weight: 600;
    padding: 1rem;
  }
  .totals {
    margin-top: auto;
    display: flex;
    justify-content: space-between;
    p {
      font-weight: 600;
    }
    button {
      cursor: pointer;
      border: 2px solid #f5f5f5;
      padding: 5px 10px;
      background: lighten($color: #37003c, $amount: 10%);
      color: white;
      font-weight: bold;
      &:hover {
        background: lighten($color: #37003c, $amount: 20%);
      }
    }
    background: #37003c;
    color: white;
    padding: 1rem;
  }
}

#cart {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
.cart-items {
  margin-top: auto;
  margin-left: auto;
}
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  list-style-type: none;
  transition: 0.25s;
  font-family: PremierSans-Bold, Arial, Helvetica Neue, Helvetica, sans-serif;
}
#app {
  background: #f5f5f5;
}
#product-and-cart {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: stretch;
  flex-wrap: wrap;
  border: 10px solid darkorange;
  #app-product-info {
    border: 2px solid black;
    flex: 1 1 70%;
  }
  #cart {
    border: 2px solid lightblue;
    flex: 1 1 30%;
  }
}
ul {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-end;
  > li {
    margin: 0.5rem 1rem;
    &:first-of-type {
      margin-top: 1rem;
    }
    &:last-of-type {
      margin-bottom: 1rem;
    }

    > button {
      cursor: pointer;
      border: 2px solid #37003c;
      padding: 5px 10px;
      background: lighten($color: #37003c, $amount: 10%);
      color: white;
      &:hover {
        background: lighten($color: #37003c, $amount: 20%);
      }
    }
  }
}
@media only screen and (max-width: 400px) {
  #top-nav,
  #bottom-nav,
  body {
    font-size: 0.8rem;
    background-color: lightblue !important;
  }
  ul {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-end;
    > li {
      margin: 0.2rem 0.5rem;
      font-size: .8rem;
      &:first-of-type {
        margin-top: 1rem;
      }
      &:last-of-type {
        margin-bottom: 1rem;
      }

      > button {
        font-size: .7rem;
        cursor: pointer;
        border: 2px solid #37003c;
        padding: 1px 2px;
      }
    }
  }
}
</style>
