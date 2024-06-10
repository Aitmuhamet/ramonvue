<script>
export default {
  props: {
    openNextPage: {
      type: Function,
      required: true
    },
    openPrevPage: {
      type: Function,
      required: true
    },
    prevPage: {
      type: String,
      required: true
    },
    nextPage: {
      type: String,
      required: true
    }
  },
  methods: {
    showValues() {
      console.log(this.prevPage);
      console.log(this.nextPage);
    }
  }
}

</script>

<template>
  <section>
    <nav 
      aria-label="Page navigation example"
      class="d-flex align-items-center justify-content-center"
    >
      <ul class="pagination" @mouseenter="showValues()">
        <li class="page-item">
          <button v-if="prevPage !== null" class="page-link" @click="openPrevPage()">Prev</button>
          <button v-else class="page-link bg-secondary" disabled>Prev</button>
        </li>
        <li class="page-item">
          <button v-if="nextPage !== null" class="page-link" @click="openNextPage()">Next</button>
          <button v-else class="page-link bg-secondary">Next</button>
        </li>
      </ul>
    </nav>
  </section>
</template>

<style scoped>
  .pagination {
    background: transparent;
    margin-top: 50px;
  }
  .page-link,
  .page-link:focus  {
    margin: 0px;
    border-radius: 0.5rem;
    padding: 0.5rem 1rem;
    font-weight: 700;
    border: 1px solid rgb(255, 152, 0);
    color: rgb(51, 51, 51);
    transition: all 0.1s ease 0s;
    box-shadow: none;
  }
  .page-link:hover {
    border: 1px solid transparent;
    background: rgb(255, 152, 0);
    color: rgb(255, 255, 255);
  }
  .page-item:not(:last-child) .page-link {
    margin-right: 5px;
  }
</style>