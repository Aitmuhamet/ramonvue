<script>
import { defineComponent, toRefs } from 'vue';

  export default defineComponent({
    props: {
      // getFilteredListCharacters: {
      //   type: Function,
      //   required: true
      // }
    },
    setup(props) {
      const applyFilters = () => {
        props.getFilteredListCharacters();
      }

      return {
        applyFilters
      };
    }
  })
</script>

<template>
  <div class="btn-wrapper d-flex justify-content-end w-100">
    <button class="btn filter-btn d-flex align-items-center" type="button" data-bs-toggle="offcanvas"
      data-bs-target="#offcanvasExample" aria-controls="offcanvasExample">
      Filters
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel"
        viewBox="0 0 16 16">
        <path
          d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2z" />
      </svg>
    </button>
  </div>

  <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasExample" aria-labelledby="offcanvasExampleLabel">
    <div class="offcanvas-header">
      <h2 class="offcanvas-title" id="offcanvasExampleLabel">Filters</h2>
      <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
    </div>
    <div class="offcanvas-body">
      <div>
        Some text as placeholder. In real life you can have the elements you
        have chosen. Like, text, images, lists, etc.
      </div>
      <select class="form-select" aria-label="Default select example">
        <option selected>Выберите статус</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <div class="input-group mb-3">
        <input type="search" class="form-control" placeholder="Введите имя персонажа" aria-label="Recipient's username"
          aria-describedby="button-addon2">
        <button @click="applyFilters" class="btn" type="button" id="button-addon2">Применить</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.form-select,
.input-group {
  margin: 20px 0;
}

.btn-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.filter-btn,
.filter-btn:focus-visible {
  margin: 0px;
  border-radius: 0.5rem;
  padding: 0.5rem 1rem;
  font-weight: 700;
  border: 1px solid rgb(255, 152, 0);
  background: white;
  color: rgb(51, 51, 51);
  transition: all 0.1s ease 0s;
}

.btn {
  border: 1px solid #dee2e6;
}
.btn:focus-visible {
  border: 1px solid rgb(255, 152, 0);
}

.filter-btn:hover,
.btn:hover {
  border: 1px solid transparent;
  background: rgb(255, 152, 0);
  color: rgb(255, 255, 255);
}

.filter-btn svg {
  margin-left: 5px;
}
</style>
