<template>
  <div id="view-review">
    <p v-if="!reviews.length">There are no reviews yet.</p>
    <ul>
      <li v-for="(review, index) in reviews" :key="index">
        <p>User: {{ review.name }}</p>
        <p>Rating: {{ review.rating }}</p>
        <p>Review: {{ review.review }}</p>
        <p>Recommended:{{ review.recommend }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    reviews: {
      type: Array,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>
#view-review {
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
  background: white;
  text-align: center;
  border: 1px solid black;
}
ul > li {
  width: 100%;
  text-align: left;
  padding-left: 2rem;
}
</style>
