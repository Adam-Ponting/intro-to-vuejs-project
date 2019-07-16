<template>
  <form id="review-form" @submit.prevent="onSubmit">
    <p class="question">
      <label for="name" class="q">Name:</label>
      <input id="name" class="a" placeholder="Enter your Name" v-model="name" />
    </p>

    <p class="question">
      <label for="review" class="q">Review:</label>
      <textarea id="review" class="a" rows="5" v-model="review"></textarea>
    </p>

    <p class="question">
      <label for="rating" class="q">Rating:</label>
      <!-- .number converts string to number -->
      <select id="rating" class="a" v-model.number="rating">
        <option>5</option>
        <option>4</option>
        <option>3</option>
        <option>2</option>
        <option>1</option>
      </select>
    </p>
    <div class="question">
      <p class="q">Would you recommend this product?</p>
      <span class="a">
        <label>
          <input type="radio" value="Yes" v-model="recommend" />
          Yes
        </label>
        <br />
        <label>
          <input type="radio" value="No" v-model="recommend" />
          No
        </label>
      </span>
    </div>
    <div v-if="errors.length">
      <p class="error"></p>
      <b>Please correct the following error(s) and try again:</b>
      <ul>
        <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
      </ul>
    </div>

    <p class="submit">
      <input type="submit" value="Submit" class="button" />
    </p>
  </form>
</template>

<script>
//import { EventBus } from '../event-bus.js'
export default {
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      recommend: null,
      errors: []
    }
  },
  methods: {
    onSubmit() {
      this.errors = []
      if (this.name && this.review && this.rating && this.recommend) {
        let productReview = {
          name: this.name,
          review: this.review,
          rating: this.rating,
          recommend: this.recommend
        }
        this.$emit('review-submitted', productReview) // send review-submitted event with productReview as payload to parent
        // EventBus.$emit('review-submitted', productReview) // send review-submitted event with productReview as payload as communicaing to grandparent
        // resets values
        this.name = null
        this.review = null
        this.rating = null
        this.recommend = null
      } else {
        if (!this.name) this.errors.push('Name required.')
        if (!this.review) this.errors.push('Review required.')
        if (!this.rating) this.errors.push('Rating required.')
        if (!this.recommend) this.errors.push('Recommendation required.')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
input:focus {
  background: lighten($color: #37003c, $amount: 20%);
}
textarea:focus {
  background: lighten($color: #37003c, $amount: 20%);
}
select:focus {
  background: lighten($color: #37003c, $amount: 20%);
}

#review-form {
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
  background: white;
  border: 1px solid black;
}
.question {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin: 0.5rem 0;
}
.q,
.a {
  flex: 1 1 10px;
  color: white;
  padding: 0.25rem;
}
.a {
  background: lighten($color: #37003c, $amount: 10%);
  &::placeholder {
    color: white;
    opacity: 1; /* Firefox */
  }
}
.q {
  padding-right: 0.5rem;
  text-align: right;
  color: black;
}
.button {
  text-align: center;
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
.submit {
  text-align: center;
  margin: 0 auto;
}
</style>
