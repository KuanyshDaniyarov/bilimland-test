<template>
  <div class="wrapper">
    <h4>Форма создания</h4>
    <form class="form">
      <div class="form-group">
        <label>Название задачи</label>
        <input type="text" class="form-control" v-model="form.title">
      </div>
      <div class="form-group">
        <label>Описание задачи</label>
        <textarea type="text" rows="4" class="form-control" v-model="form.description"></textarea>
      </div>
      <button type="button" class="btn" @click="handleSave" :class="{'disabled': !this.form.title}">
        Создать
      </button>
    </form>
    <p v-if="isLoading">Loading...</p>
    <div v-else>
      <h4>Список задач</h4>
      <div class="filter">
        <input type="text" class="form-control" v-model="search">
        <button class="btn" @click="sortByTitle">Фильтр</button>
      </div>
      <div class="table" role="table">
        <div class="tr th">
          <div class="td"></div>
          <div class="td">Порядковый номер</div>
          <div class="td">Название задачи</div>
          <div class="td">Описание задачи</div>
        </div>
        <div class="tbody">
          <div class="tr" v-for="(task, index) of filteredTaskList" :key="index">
            <div class="td">
              <input type="checkbox" v-model="task.checked">
            </div>
            <div class="td">{{index + 1}}</div>
            <div class="td">{{ task.title }}</div>
            <div class="td">{{ task.description }}</div>
          </div>
        </div>
      </div>
      <button type="button" class="btn" @click="handleRemove" :class="{'disabled': !isChecked}">
        Удалить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Test',
  mounted() {
    const promise = new Promise((resolve) => {
      setTimeout(() => {
        resolve();
      }, 3000);
    });
    promise.then(() => {
      this.isLoading = false;
    });
  },
  data() {
    return {
      form: {
        title: '',
        description: '',
      },
      search: '',
      isLoading: true,
      isSort: false,
      taskList: [
        {
          title: 'Тестовое название',
          description: 'Тестовое описание',
        },
      ],
      sortBy: 'desc',
    };
  },
  computed: {
    filteredTaskList() {
      return this.taskList.filter((el) => {
        return !this.search || el.title.toLowerCase().includes(this.search.toLowerCase())
          || el.description.toLowerCase().includes(this.search.toLowerCase());
      }).sort((a, b) => {
        if (this.sortBy === 'asc') {
          return a.title.localeCompare(b.title);
        }
        return b.title.localeCompare(a.title);
      });
    },
    isChecked() {
      return this.taskList.some((el) => el.checked);
    },
  },
  methods: {
    handleSave() {
      this.taskList.push({ ...this.form });
      Object.keys(this.form).forEach((key) => this.form[key] = '');
    },
    handleRemove() {
      this.taskList = this.taskList.filter((el) => !el.checked);
    },
    sortByTitle() {
      this.sortBy = this.sortBy === 'asc' ? 'desc' : 'asc';
    },
  },
};
</script>

<style lang="scss">
  .wrapper {
    max-width: 768px;
    margin: 0 auto;
  }
  h4 {
    margin: 8px 0;
  }
  p {
    text-align: center;
  }
  .form {
    margin-bottom: 16px;
    overflow: auto;
    &-group {
      margin-bottom: 8px;
    }
    label {
      line-height: 28px;
    }
    &-control {
      display: block;
      padding: 5px 15px;
      line-height: 1.5;
      box-sizing: border-box;
      width: 100%;
      border: 1px solid #dcdfe6;
      border-radius: 4px;
      transition: border-color .2s cubic-bezier(.645,.045,.355,1);
    }
  }
  .btn {
    cursor: pointer;
    outline: none;
    transition: 0.1s;
    padding: 12px 20px;
    border-radius: 4px;
    color: #fff;
    background-color: #409eff;
    border: none;
    float: right;
    &:hover, &:focus {
      background: #66b1ff;
      border-color: #66b1ff;
    }
    &:active {
      background: #3a8ee6;
      border-color: #3a8ee6;
    }
    &.disabled {
      pointer-events: none;
      opacity: .5;
    }
  }
  .table {
    display: flex;
    flex-flow: column nowrap;
    font-size: .8rem;
    line-height: 1.5;
    border-bottom: 1px solid #d0d0d0;
    flex: 1 1 auto;
    margin-bottom: 16px;
    .th {
      display: none;
      font-weight: 700;
      background-color: #f2f2f2;
    }
    .tr {
      width: 100%;
      display: flex;
      flex-flow: row nowrap;
      &:nth-of-type(even) {
        background-color: #f2f2f2;
      }
      &:nth-of-type(odd) {
        background-color: #ffffff;
      }
    }
    .td {
      display: flex;
      flex-flow: row nowrap;
      flex-grow: 1;
      flex-basis: 0;
      padding: 0.5em;
      word-break: break-word;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      border-bottom: 1px solid #d0d0d0;
      justify-content: center;
      &:first-child {
        flex-grow: 0.2;
      }
    }
  }
  .filter {
    display: flex;
    align-items: center;
    width: 50%;
    .btn {
      padding: 8px 20px;
      margin-left: 16px;
    }
  }
</style>
