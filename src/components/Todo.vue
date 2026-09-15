<template>
  <div class="wrapper">
    <h1 class="header">Todo List</h1>
    <div ref="completedRef" class="totalDiv">
      <p class="total">
        Total Tasks - {{ data.length }} // Completed Tasks - {{ getCompleted }}
      </p>
    </div>
    <div class="inputDiv">
      <input
        ref="myInput"
        type="text"
        class="todoInput"
        v-model="model"
        placeholder="Enter a new todo"
      />
      <button class="addTodoBtn" @click="addToDo">Add Todo</button>
    </div>
    <ul class="ul">
      <li v-for="(item, index) in data" :key="index">
        <span :class="{ strike: item.completed }">{{ item.text }}</span>
        <div class="btnGroup">
          <button class="doneBtn" @click="doneBtn(item)">Done</button>
          <button class="deleteBtn" @click="deleteBtn(index)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Todo",
  watch: {
    getCompleted(newVal) {
      if (newVal / this.data.length >= 0.5) {
        this.$refs.completedRef.style.background = "green";
      } else {
        this.$refs.completedRef.style.background = "red";
      }
    },
  },
  computed: {
    getCompleted() {
      return this.data.filter((item) => item.completed).length;
    },
  },
  methods: {
    addToDo() {
      if (this.model !== "") {
        this.data.push({
          id: Date.now(),
          text: this.model,
          completed: false,
        });
      }

      this.model = "";
      this.$refs.myInput.focus();
    },
    deleteBtn(index) {
      this.data.splice(index, 1);
    },

    doneBtn(item) {
      const idx = this.data.findIndex((x) => x.id === item.id);
      this.data[idx].completed = !item.completed;
    },
  },
  data() {
    return {
      model: "",
      data: [],
    };
  },
};
</script>

<style scoped>
.wrapper {
  width: 100%;
  max-width: 650px;
  min-height: 500px;
  margin: 40px auto;
  padding: 40px;
  box-sizing: border-box;

  display: flex;
  flex-direction: column;
  align-items: center;

  background: #f8fafc;
  border-radius: 24px;
  box-shadow: 0 20px 50px rgba(15, 23, 42, 0.08);
}

/* Header */
.header {
  margin: 0 0 25px;
  color: #0f172a;
  font-size: 36px;
  font-weight: 700;
  letter-spacing: -1px;
}

/* Progress / total */
.totalDiv {
  width: 100%;
  max-width: 500px;
  padding: 16px 20px;
  margin-bottom: 25px;

  background: #eef2ff;
  border: 1px solid #e0e7ff;
  border-radius: 14px;

  box-sizing: border-box;
  transition: 0.3s ease;
}

.total {
  margin: 0;
  color: #fff;
  font-size: 15px;
  font-weight: 600;
  text-align: center;
}

/* Input section */
.inputDiv {
  width: 100%;
  max-width: 500px;

  display: flex;
  align-items: center;
  gap: 10px;

  margin-bottom: 30px;
}

.todoInput {
  flex: 1;
  min-width: 0;

  padding: 13px 16px;

  border: 1px solid #dbe2ea;
  border-radius: 12px;

  background: white;
  color: #0f172a;

  font-size: 15px;
  outline: none;

  transition: all 0.2s ease;
  box-sizing: border-box;
}

.todoInput::placeholder {
  color: #94a3b8;
}

.todoInput:focus {
  border-color: #6366f1;
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.12);
}

/* Add button */
.addTodoBtn {
  padding: 13px 18px;

  border: none;
  border-radius: 12px;

  background: #6366f1;
  color: white;

  font-size: 14px;
  font-weight: 600;

  cursor: pointer;
  white-space: nowrap;

  transition: all 0.2s ease;
}

.addTodoBtn:hover {
  background: #4f46e5;
  transform: translateY(-1px);
  box-shadow: 0 6px 15px rgba(99, 102, 241, 0.25);
}

.addTodoBtn:active {
  transform: translateY(0);
}

/* Todo list */
.ul {
  width: 100%;
  max-width: 500px;

  display: flex;
  flex-direction: column;
  gap: 12px;

  margin: 0;
  padding: 0;

  list-style: none;
}

/* Todo item */
.ul li {
  width: 100%;
  min-height: 58px;
  padding: 12px 14px 12px 18px;

  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 15px;

  box-sizing: border-box;

  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 14px;

  color: #1e293b;

  box-shadow: 0 3px 10px rgba(15, 23, 42, 0.04);

  transition: all 0.2s ease;
}

.ul li:hover {
  border-color: #c7d2fe;
  box-shadow: 0 6px 18px rgba(15, 23, 42, 0.08);
  transform: translateY(-1px);
}

/* Todo text */
.ul li span {
  flex: 1;
  min-width: 0;

  font-size: 15px;
  font-weight: 500;

  word-break: break-word;
  text-align: left;
}

.strike {
  text-decoration: line-through;
  color: #94a3b8 !important;
}

/* Buttons container */
.btnGroup {
  display: flex;
  align-items: center;
  gap: 6px;
}

/* Done button */
.doneBtn {
  padding: 7px 11px;

  border: none;
  border-radius: 8px;

  background: #ecfdf5;
  color: #059669;

  font-size: 13px;
  font-weight: 600;

  cursor: pointer;

  transition: all 0.2s ease;
}

.doneBtn:hover {
  background: #059669;
  color: white;
}

/* Delete button */
.deleteBtn {
  padding: 7px 11px;

  border: none;
  border-radius: 8px;

  background: #fef2f2;
  color: #ef4444;

  font-size: 13px;
  font-weight: 600;

  cursor: pointer;

  transition: all 0.2s ease;
}

.deleteBtn:hover {
  background: #ef4444;
  color: white;
}

/* Mobile */
@media (max-width: 600px) {
  .wrapper {
    width: calc(100% - 30px);
    margin: 20px auto;
    padding: 25px 18px;
    border-radius: 18px;
  }

  .header {
    font-size: 30px;
  }

  .inputDiv {
    flex-direction: column;
    align-items: stretch;
  }

  .addTodoBtn {
    width: 100%;
  }

  .ul li {
    align-items: flex-start;
  }

  .btnGroup {
    flex-shrink: 0;
  }
}
</style>
