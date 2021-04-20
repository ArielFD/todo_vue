<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <a class="navbar-brand" href="#">JS Todo List</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">
              Home <span class="sr-only">(current)</span>
            </a>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0" id="filters">
          <label class="font-weight-bold text-info mr-3">Filters</label>
          <input
            type="radio"
            name="type"
            v-model.number="check"
            class="mx-1"
            value="1"
          />
          <label for="male" class="mb-0">All</label>
          <input
            type="radio"
            name="type"
            v-model.number="check"
            class="mx-1"
            value="2"
          />
          <label for="female" class="mb-0">Completed</label>
          <input
            type="radio"
            name="type"
            v-model.number="check"
            class="mx-1"
            value="3"
          />
          <label for="other" class="mr-2 mb-0">Uncompleted</label>

          <input
            class="form-control mr-sm-2"
            type="search"
            v-model="words"
            placeholder="Words"
            aria-label="Search"
          />
          <button
            class="btn btn-outline-info my-2 my-sm-0"
            type="submit"
            id="search"
            @click.prevent="filter()"
          >
            Search
          </button>
        </form>
      </div>
    </nav>

    <!-- Modal -->

    <b-modal id="modal-1" title="BootstrapVue">
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">
              Edit Todo
            </h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div
              class="alert alert-danger"
              v-bind:class="{ 'd-none': hasError }"
              role="alert"
              id="modal-alert"
            >
              A simple danger alert—check it out!
            </div>
            <form>
              <div class="form-group">
                <label>Title</label>
                <input
                  v-model="modal_title"
                  type="text"
                  class="form-control"
                  placeholder="Do Something"
                />
              </div>
              <div class="form-group">
                <label>Description</label>
                <textarea
                  class="form-control"
                  v-model="modal_description"
                  rows="3"
                >
                </textarea>
              </div>
              <div class="form-group d-inline-flex">
                <label>Completed</label>
                <div class="mt-1 ml-2">
                  <input v-model="modal_completed" type="checkbox" />
                </div>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-info"
              id="modal-btn"
              @click="saveModal()"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </b-modal>

    <main>
      <div class="container pt-3">
        <div
          class="alert alert-danger"
          v-bind:class="{ 'd-none': hasError }"
          role="alert"
          id="alert"
        >
          A simple danger alert—check it out!
        </div>

        <!-- Add Todo -->

        <div>
          <form>
            <div class="row">
              <div class="col-sm-3 d-sm-flex align-items-center">
                <label class="m-sm-0">Title</label>
                <input
                  type="text"
                  v-model="title"
                  class="form-control ml-sm-2"
                  placeholder="Learn JS"
                  v-on:keyup.enter="add"
                />
              </div>
              <div
                class="px-sm-2 col-sm-7 d-sm-flex align-items-center mt-2 mt-sm-0"
              >
                <label class="m-sm-0">Description</label>
                <input
                  type="text"
                  v-model="description"
                  class="form-control ml-sm-2"
                  placeholder="Watch JS Tutorials"
                />
              </div>
              <div class="col-sm-2 d-sm-flex justify-content-end mt-4 mt-sm-0">
                <button
                  type="button"
                  class="btn btn-info btn-block"
                  @click="add"
                >
                  Add
                </button>
              </div>
            </div>
          </form>
        </div>

        <!-- Table -->

        <div class="mt-5">
          <table class="table table-striped" id="table">
            <thead>
              <tr>
                <th scope="col">Todo</th>
                <th scope="col">Description</th>
                <th scope="col">
                  <div class="d-flex justify-content-center">
                    Completed
                  </div>
                </th>
                <th scope="col"></th>
              </tr>
              <tr v-for="(item, index) of todos" :key="item.id">
                <th scope="col" v-bind:class="{ 'd-none': item.hasErrorRow }">
                  {{ item.title }}
                </th>
                <th scope="col" v-bind:class="{ 'd-none': item.hasErrorRow }">
                  {{ item.description }}
                </th>
                <th
                  scope="col"
                  class="text-center"
                  v-bind:class="{ 'd-none': item.hasErrorRow }"
                >
                  <input type="checkbox" id="checkbox" v-model="item.checked" />
                </th>
                <th
                  scope="col"
                  class="text-right"
                  v-bind:class="{ 'd-none': item.hasErrorRow }"
                >
                  <b-button
                    v-b-modal.modal-1
                    variant="primary"
                    class=" mb-1 ml-1"
                    @click="edit(index)"
                  >
                    <i class="fa fa-pencil"></i>
                  </b-button>
                  <button
                    class="btn btn-danger mb-1 ml-1"
                    @click="remove(index)"
                  >
                    <i class="fa fa-trash"></i>
                  </button>
                </th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "Inicio",
  data() {
    return {
      id: 1,
      title: "",
      description: "",
      checked: false,
      hasErrorRow: false,
      hasError: true,
      modal_title: "",
      modal_description: "",
      modal_completed: false,
      indexArray: 0,
      words: "",
      check: 1,
      todos: [
        {
          id: 0,
          title: "learn js",
          description: "watch js tutorials",
          checked: false,
          hasErrorRow: false,
        },
      ],
    };
  },
  computed: {},
  methods: {
    add: function() {
      if (this.title === "" || this.description === "") {
        this.hasError = false;
      } else {
        this.hasError = true;
        this.todos.push({
          id: this.id++,
          title: this.title,
          description: this.description,
          checked: this.checked,
          hasErrorRow: this.hasErrorRow,
        });
      }
      this.title = "";
      this.description = "";
    },
    remove: function(index) {
      this.todos.splice(index, 1);
    },
    edit: function(index) {
      this.modal_title = this.todos[index].title;
      this.modal_description = this.todos[index].description;
      this.modal_completed = this.todos[index].checked;
      this.indexArray = this.todos[index].id;
    },
    saveModal: function() {
      if (this.modal_title === "" || this.modal_description === "") {
        this.hasError = false;
      } else {
        this.hasError = true;
        this.todos[this.indexArray].id = this.indexArray;
        this.todos[this.indexArray].title = this.modal_title;
        this.todos[this.indexArray].description = this.modal_title;
        this.todos[this.indexArray].checked = this.modal_title;
      }
    },
    filter: function() {
      console.log("test"+ this.check)
      console.log(this.check == 1)
      if (this.words.length > 0 && this.check == 1) {
        console.log("test1 "+ this.check)
        this.todos.forEach((value, index) => {
          if (
            !value.title.includes(this.words) ||
            !value.description.includes(this.words)
          ) {
            value.hasErrorRow = true;
          }
        });
      } else if (this.check == 1) {
        this.todos.forEach((value, index) => {
          value.hasErrorRow = false;
        });
      }

      if (this.words.length > 0 && this.check == 2) {
        console.log("2do if")
        this.todos.forEach((value, index) => {
          if (
            !value.title.includes(this.words) ||
            !value.description.includes(this.words) && 
             value.checked
          ) {
            value.hasErrorRow = true;
          }
        });
      }

      if (this.words.length > 0 && this.check == 3) {
        console.log("2do if")
        this.todos.forEach((value, index) => {
          if (
            !value.title.includes(this.words) ||
            !value.description.includes(this.words) &&
            !value.checked
          ) {
            value.hasErrorRow = true;
          }
        });
      }
    },
  },
};
</script>

<style scoped></style>
