<template>
  <div>
    <div id="shirt-and-description">
      <!-- selected image shirt -->
      <div class="shirt-and-buttons">
        <img
          :src="image"
          :alt="clubs[selectedClub].team"
          :title="clubs[selectedClub].team"
          :style="{ backgroundColor: clubs[selectedClub].color }"
          :class="{ added: addedToCart }"
          class="selected-shirt"
        />
        <div class="cart-buttons">
          <button @click="addToCart" :class="shirtInStock">
            {{ addButton }}
          </button>
          <button @click="removeFromCart" :class="shirtInCart">
            Remove from Basket
          </button>
        </div>
      </div>
      <div class="club-icons">
        <div v-for="(club, index) in clubs" :key="index" class="club-icon">
          <img
            :src="club.icon"
            :alt="club.team"
            :title="club.team"
            @mouseover="selectAClub(index)"
          />
        </div>
      </div>

      <div class="product-info">
        <div class="product-description">
          <h1>Premier League 2020 Shirt Sale</h1>
          <h2>{{ clubs[selectedClub].team }}</h2>
          <p v-html="clubs[selectedClub].description"></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    clubs: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      selectedClub: 0, // changes on mouseover
      addedToCart: false
    }
  },
  methods: {
    addToCart() {
      let selectedShirt = this.clubs[this.selectedClub]
      console.log('emit adding =>', selectedShirt.team)
      if (selectedShirt.quantity > 0) {
        this.$emit('addToCart', selectedShirt)
        this.flashImage()
      } else {
        console.log(' no stock')
      }
    },
    removeFromCart() {
      let selectedShirt = this.clubs[this.selectedClub]
      console.log('emit removing =>', selectedShirt.team)
      if (selectedShirt.quantity <= 2) {
        this.$emit('removeFromCart', selectedShirt)
      } else {
        console.log(' full stock')
      }
    },
    selectAClub(index) {
      console.log(index)
      this.selectedClub = index // sets club on mouseover
    },
    flashImage() {
      setTimeout(() => {
        this.addedToCart = false
      }, 100)
      this.addedToCart = true
    }
  },
  computed: {
    addButton() {
      if (this.clubs[this.selectedClub].quantity === 0) {
        return 'Sold Out!'
      } else {
        return 'Add to Basket'
      }
    },
    image() {
      return this.clubs[this.selectedClub].shirt // get shirt from clubs[index].shirt
    },
    shirtInStock() {
      return {
        disabledButton: this.clubs[this.selectedClub].quantity === 0
      }
    },
    shirtInCart() {
      return {
        disabledButton: this.clubs[this.selectedClub].quantity === 3
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.added {
  box-shadow: 0 0 10px 10px #37003c;
}
.disabledButton {
  color: black !important;
  background: lightgray !important;
  &:hover {
    background: lightgray !important;
    cursor: default !important;
  }
}

#shirt-and-description {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: flex-start;
  margin-top: 1rem;
  width: 100%;
}
.shirt-and-buttons {
  margin-left: 1rem;
  flex: 0 1 1;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
}
.selected-shirt {
  height: 100%;
  padding: 5px;
  border: 2px solid #37003c;
  width: auto;
}
.cart-buttons {
  width: 100%;
  display: flex;
  button {
    border: 2px solid #37003c;
    background: lighten($color: #37003c, $amount: 10%);
    color: white;
    width: 100%;
    margin: 5px;
    padding: 5px;
    &:hover {
      cursor: pointer;
      background: lighten($color: #37003c, $amount: 20%);
    }
  }
}
.product-info {
  align-self: flex-start;
  flex: 1 1 300px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  .product-description h1 {
    color: white;
    background: #37003c;
    padding: 0.5rem;
  }
  .product-description h2 {
    margin: 1rem 0;
    padding: 0.5rem;
  }
  p {
    //  margin: 1rem 0;
    padding: 0 0.5rem;
    font-size: 1.1rem;
  }
}
.club-icons {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  .club-icon {
    margin: 5px 10px;
    img {
      height: 64px;
      box-shadow: 0 0 2px 2px #37003c;
    }
  }
}
@media only screen and (max-width: 400px) {
  #shirt-and-description {
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }
  .club-icons {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }
  .club-icon {
    text-align: center;
  }
}
</style>
