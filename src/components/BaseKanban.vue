<template>
  <div>
    <h1>
      {{ count === 0 ? 'Add cards to get started': `${count} cards found` }}
    </h1>
    <div class="kanban">
      <base-kanban-column
        v-for="column in columns"
        :key="column.id"
        :column="column"
        :cards="sortedCards[column.id]"
        @card:create="createCard(column.id)"
        @card:edit="editCard($event)"
        @card:move="moveCard($event)"
        @card:delete="deleteCard($event)"
        class="column"
      />
    </div>
  </div>
</template>

<script>
/* eslint-disable arrow-body-style */
import Vue from 'vue';
import BaseKanbanColumn from './BaseKanbanColumn.vue';

export default Vue.extend({
  components: {
    BaseKanbanColumn,
  },
  data() {
    return {
      sortedCards: {},
      count: 0,
      nextId: 0,
      columns: [
        {
          id: 0,
          title: 'One',
        },
        {
          id: 1,
          title: 'Two',
        },
        {
          id: 2,
          title: 'Three',
        },
        {
          id: 3,
          title: 'Four',
        },
      ],
    };
  },
  beforeMount() {
    this.loadCards();
  },
  methods: {
    loadCards() {
      const cards = JSON.parse(localStorage.getItem('saved-cards') || '[]');
      this.count = cards.length;
      this.sortedCards = this.columns.map(
        column => cards.filter(card => card.column === column.id),
      );
    },
    saveCards() {
      // eslint-disable-next-line array-callback-return
      const allCards = this.sortedCards
        .reduce((accumulator, columnCards) => accumulator.concat(columnCards));
      localStorage.setItem('saved-cards', JSON.stringify(allCards));
    },
    createCard(columnId) {
      this.count = this.count + 1;
      this.nextId = this.nextId + 1;
      this.pushCard({
        to: columnId,
        card: {
          // eslint-disable-next-line no-alert
          text: window.prompt('Add card'),
          column: columnId,
          id: this.nextId,
        },
      });
    },
    deleteCard({ from, id }) {
      this.count = this.count - 1;
      this.pluckCard({ from, id });
      this.saveCards();
    },
    editCard({ from, id }) {
      const card = this.pluckCard({ from, id });
      this.pushCard({
        to: from,
        card: {
          ...this.card,
          // eslint-disable-next-line no-alert
          text: window.prompt('Edit card', card.text),
        },
      });
    },
    moveCard({ from, to, id }) {
      const card = this.pluckCard({ from, id });
      this.pushCard({ to, card });
    },
    pluckCard({ from, id }) {
      const cards = this.sortedCards[from];
      const [card] = cards.splice(cards.findIndex(item => item.id === id), 1);
      return card;
    },
    pushCard({ to, card }) {
      this.sortedCards[to].push({
        ...card,
        column: to,
      });
      this.saveCards();
    },
  },
});
</script>

<style>
  h1{
    text-align: center;
  }
  .kanban{
    padding: 25px;
    background-color: #eceeee;
    display:  grid;
    grid-template-columns: auto auto auto auto;
    grid-gap: 25px;
  }
</style>
