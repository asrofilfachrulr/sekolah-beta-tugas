<template>
  <div class="py-4">
    <div class="container">
      <div
        class="title border-bottom d-flex align-items-center justify-content-between py-2"
      >
        <h5>Task</h5>
        <div class="d-flex align-items-center">
          <input
            type="text"
            class="form-control"
            placeholder="Search"
            v-model="searchQuery"
          />
          <div class="dropdown ms-3">
            <button
              class="btn btn-light dropdown-toggle"
              type="button"
              id="dropdownMenuButton1"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              {{ selectedCategory }}
            </button>
            <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
              <li v-for="(category, idx) in computedCategory" :key="idx">
                <a
                  class="dropdown-item"
                  style="cursor: pointer"
                  @click="
                    (function () {
                      selectedCategory = capitalize(category);
                    })()
                  "
                  >{{ capitalize(category) }}</a
                >
              </li>
              <li></li>
            </ul>
          </div>
          <button class="btn btn-danger ms-3" @click="resetFilter">
            Reset
          </button>
          <div class="d-flex align-item-center justify-content-end w-100">
            <span class="me-2 h-100 align-middle">View As</span>
            <button class="btn btn-light py-1 px-3" @click="isGrid = !isGrid">
              {{ isGrid ? "Grid" : "List" }}
            </button>
          </div>
        </div>
      </div>
      <div class="list-task row">
        <CardItem
          v-for="(task, i) in resultFilter"
          :key="i"
          :task="task"
          :isGrid="isGrid"
        />
      </div>
      <div class="action py-2">
        <button
          class="btn btn-success add-button"
          v-if="!isCreating"
          href="#"
          @click="isCreating = !isCreating"
        >
          <a>New Task</a>
        </button>
        <form v-else class="add-card" @submit.prevent="appendTask">
          <p>New Task</p>
          <div class="card mb-2 border-0">
            <div class="card-body d-flex flex-column p-0">
              <input
                class="form-control mb-2 w-50"
                placeholder="Title"
                type="text"
                v-model="newTitle"
              />
              <textarea
                class="form-control small w-50"
                placeholder="Description"
                rows="3"
                v-model="newDesc"
              ></textarea>
              <div class="radio-group-category mt-3">
                <p>Category:</p>
                <div class="form-check">
                  <div v-for="(category, i) in computedCategory" :key="i">
                    <input
                      class="form-check-input"
                      type="radio"
                      name="task-categories"
                      :id="`category-radio-${i}`"
                      v-model="newCategory"
                      :value="capitalize(category)"
                      v-if="i != 0"
                    />
                    <label
                      class="form-check-label"
                      :for="`category-radio-${i}`"
                      v-if="i != 0"
                    >
                      {{ capitalize(category) }}
                    </label>
                  </div>
                  <label for="category-radio-custom">
                    <input
                      class="form-check-input"
                      type="radio"
                      name="task-categories"
                      :id="'category-radio-custom'"
                      v-model="newCategory"
                      :value="capitalize(customCategory)"
                    />
                    Other:
                  </label>
                  <input
                    class="form-control w-25 d-inline-block"
                    placeholder="New Category"
                    type="text"
                    v-model="customCategory"
                  />
                </div>
              </div>
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2" type="submit">Save</button>
            <button class="btn btn-outline-secondary" @click="resetVModel">
              Cancel
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  layout(_) {
    return "custom";
  },
  data() {
    return {
      searchQuery: "",
      // Tipe layout daftar task
      isGrid: true,
      // Status saat menambahkan task
      isCreating: false,
      // Daftar task

      tasks: [
        {
          title: "Sweeping",
          description:
            "Sweep the floors of your home to remove dirt, dust, and debris.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Dusting",
          description:
            "Wipe away dust from surfaces in your home, such as furniture, appliances, and shelves.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Mopping",
          description:
            "Wipe and clean the floors of your home with a mop and cleaning solution.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Vacuuming",
          description:
            "Remove dirt, dust, and debris from your carpets and rugs with a vacuum cleaner.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Laundry",
          description: "Wash, dry, and fold clothes.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Dishes",
          description: "Wash and put away dishes.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Trash",
          description: "Take out the trash and recycling.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Making the bed",
          description:
            "Make your bed every morning to start your day off right.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Grocery shopping",
          description:
            "Go to the grocery store to buy food for your household.",
          isDone: false,
          category: "Food",
        },
        {
          title: "Cooking",
          description: "Prepare a meal for your family or yourself.",
          isDone: false,
          category: "Food",
        },
        {
          title: "Cleaning the bathroom",
          description:
            "Wipe down the surfaces in your bathroom, and clean the toilet, sink, and shower.",
          isDone: false,
          category: "Home",
        },
        {
          title: "Taking out the dog",
          description:
            "Walk your dog for at least 30 minutes to give them exercise and relieve them of waste.",
          isDone: false,
          category: "Pet Care",
        },
        {
          title: "Yard work",
          description: "Mow the lawn, trim the hedges, and weed the garden.",
          isDone: false,
          category: "Pet Care",
        },
        {
          title: "Car wash",
          description: "Wash and wax your car to keep it clean and protected.",
          isDone: false,
          category: "Transportation",
        },
        {
          title: "Doctor's appointment",
          description: "Go to the doctor for a checkup or other medical needs.",
          isDone: false,
          category: "Personal Development",
        },
        {
          title: "Banking",
          description:
            "Go to the bank to deposit checks, make a withdrawal, or pay bills.",
          isDone: false,
          category: "Personal Development",
        },
        {
          title: "Volunteering",
          description:
            "Give your time to a local charity or organization to help others.",
          isDone: false,
          category: "Personal Development",
        },
        {
          title: "Learning a new skill",
          description:
            "Take a class or read a book to learn a new skill that interests you.",
          isDone: false,
          category: "Personal Development",
        },
      ],
      selectedCategory: "All", //default
      newTitle: "",
      newDesc: "",
      newCategory: "",
      customCategory: "",
    };
  },
  methods: {
    capitalize(sentence) {
      return sentence.replace(/(^\w{1})|(\s+\w{1})/g, (letter) =>
        letter.toUpperCase()
      );
    },
    appendTask() {
      try {
        this.tasks.push({
          title: this.newTitle,
          description: this.newDesc,
          category: this.newCategory,
        });
      } catch (error) {
        alert(
          "kesalahan saat menambah, pastikan semua field terisi dan memilih kategori"
        );
      }
      this.resetVModel();
      this.resetFilter();
    },
    resetVModel() {
      this.isCreating = !this.isCreating;
      this.newTitle = "";
      this.newDesc = "";
      this.newCategory = "";
      this.customCategory = "";
    },
    resetFilter() {
      this.selectedCategory = "All";
      this.searchQuery = "";
    },
  },
  computed: {
    resultFilter() {
      return this.tasks.filter((task) => {
        if (
          task.category === this.selectedCategory ||
          this.selectedCategory === "All"
        )
          return this.searchQuery
            .toLowerCase()
            .split("")
            .every((v) => task.title.toLowerCase().includes(v));
        else return false;
      });
    },
    computedCategory() {
      const categories = ["all"];
      this.tasks.forEach((task) => {
        const category = task.category.toLowerCase();
        if (categories.indexOf(category) == -1) categories.push(category);
      });

      return categories;
    },
  },
};
</script>
<style>
</style>