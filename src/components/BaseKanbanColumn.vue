<template>
  <div class="column">
    <header class="header">
      <h2 class="heading">
        {{ column.title }}
      </h2>
    </header>
    <article
      v-for="(card, index) in cards"
      :key="index"
      :card="card"
      class="card"
    >
      <div class="details">
        <p>
          {{ card.text }}
        </p>
        <button v-on:click="editCard(card)">
          Edit
        </button>
        <button v-on:click="deleteCard(card)">
          Delete
        </button>
      </div>
      <button
        v-if="card.column !== 0"
        v-on:click="moveCard(card, -1)"
        class="btn previous"
      >
        &lt;
      </button>
      <button
        v-if="card.column !== lastColumn"
        v-on:click="moveCard(card, 1)"
        class="btn next"
      >
        &gt;
      </button>
    </article>
    <button v-on:click="$emit('card:create')">
      Add card
    </button>
  </div>
</template>

<script>
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      lastColumn: 3,
    };
  },
  props: {
    column: {
      type: Object,
      required: true,
    },
    cards: {
      type: Array,
      required: true,
    },
  },
  methods: {
    editCard(card) {
      this.$emit('card:edit', {
        from: card.column,
        id: card.id,
      });
    },
    deleteCard(card) {
      this.$emit('card:delete', {
        from: card.column,
        id: card.id,
      });
    },
    moveCard(card, amount) {
      this.$emit('card:move', {
        from: card.column,
        to: card.column + amount,
        id: card.id,
      });
    },
  },
});
</script>

<style>
  .column .header{
    display: flex;
    height: 30px;
  }
  .column .heading{
    color: white;
    font-size: 16px;
    text-align: center;
    margin: auto;
  }
  .column:first-child .header{
    background-color: #8e6e95;
  }
  .column:nth-child(2) .header{
    background-color: #29a59c;
  }
  .column:nth-child(3) .header{
    background-color: #344759;
  }
  .column:nth-child(4) .header{
    background-color: #e8741e;
  }
  .card{
    background-color: white;
    border-bottom: 1px solid #acaaaa;
    display: grid;
    grid-template-columns: 20px auto 20px;
  }
  .btn{
    text-align: center;
    background-color: white;
    border: 0;
    width: 20px;
    margin: 0;
    grid-row: 1;
  }
  .details{
    padding: 0 10px;
    grid-column-start: 2;
  }
  .previous{
    grid-column-start: 1;
  }
  .next{
    grid-column-start: 3;
  }
</style>
