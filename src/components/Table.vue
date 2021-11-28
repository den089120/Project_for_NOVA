<template>
  <div>
    <div class="nav__bar">
      <form class="nav__bar__form">
        <div class="nav__bar__form__text">Количество на странице</div>
        <input v-model="limitPage"
               class="nav__bar__form__input"
               style="margin-right: 50px;width: 35px;">
        <select v-model="setSortCell" class="nav__bar__form__select">
          <option value="name" selected>фильтр по названию</option>
          <option value="value">фильтр по количеству</option>
          <option value="distance">фильтр по расстоянию</option>
        </select>
        <select v-model="setSortValue" class="nav__bar__form__select">
          <option value="equals" selected>равно</option>
          <option value="contains">содержит</option>
          <option value="more">больше</option>
          <option value="smaller">меньше</option>
        </select>
        <input v-model="inputSortList" class="nav__bar__form__input">
      </form>
    </div>
    <div class="table__container">
      <div class="table__row" style="position: sticky;top: 50px;">
        <div class="table__cell" style="background-color: rgba(210,255,250, 0.9)">Дата</div>
        <div class="table__cell" style="background-color: rgba(210,255,250, 0.9)">Название</div>
        <div class="table__cell" style="background-color: rgba(210,255,250, 0.9)">Количество</div>
        <div class="table__cell" style="background-color: rgba(210,255,250, 0.9)">Расстояние</div>
      </div>
      <div v-for="(data, index) in sortTable(setSortCell, setSortValue)"
           :key="data.id"
           style="width: 100%;display: flex;justify-content: center;">
        <div v-if="showPage(index)" class="table__row">
          <div class="table__cell">{{ data.date }}</div>
          <div class="table__cell">{{ data.name }}</div>
          <div class="table__cell">{{ data.value }} шт</div>
          <div class="table__cell">{{ data.distance }} км</div>
        </div>
      </div>
    </div>
    <div class="footer">
      <span v-for="page in pagesList"
            :key="page"
            @click="pageNew(page)"
            class="footer__page"
            :class="{ footer__page__active: (page === newPage) }">
        {{ page }}
      </span>
    </div>
  </div>

</template>

<script>
export default {
  name: "Table",
  data() {
    return {
      inputSortList: '',
      setSortCell: 'name',
      setSortValue: 'contains',
      limitPage: 6,
      dataTable: [
        {
          id: 1,
          date: '15/05/21',
          name: 'Kirov',
          value: 123,
          distance: 1500,
        },
        {
          id: 2,
          date: '17/05/21',
          name: 'Moscow',
          value: 543,
          distance: 1000,
        },
        {
          id: 3,
          date: '11/06/21',
          name: 'Kemerovo',
          value: 34567,
          distance: 500,
        },
        {
          id: 4,
          date: '14/07/21',
          name: 'Abakan',
          value: 4324,
          distance: 1200,
        },
        {
          id: 5,
          date: '25/08/21',
          name: 'Volgograd',
          value: 12,
          distance: 700,
        },
        {
          id: 6,
          date: '14/05/21',
          name: 'Kirov',
          value: 2314,
          distance: 1500,
        },
        {
          id: 7,
          date: '17/09/21',
          name: 'Moscow',
          value: 34,
          distance: 1000,
        },
        {
          id: 8,
          date: '21/06/21',
          name: 'Kemerovo',
          value: 4566,
          distance: 500,
        },
        {
          id: 9,
          date: '14/09/21',
          name: 'Abakan',
          value: 432,
          distance: 1200,
        },
        {
          id: 10,
          date: '20/08/21',
          name: 'Volgograd',
          value: 21,
          distance: 700,
        },
        {
          id: 11,
          date: '15/11/21',
          name: 'Kirov',
          value: 534,
          distance: 1500,
        },
        {
          id: 12,
          date: '17/11/21',
          name: 'Moscow',
          value: 34,
          distance: 1000,
        },
        {
          id: 13,
          date: '11/12/21',
          name: 'Kemerovo',
          value: 2345,
          distance: 500,
        },
        {
          id: 14,
          date: '24/07/21',
          name: 'Abakan',
          value: 46555,
          distance: 1200,
        },
        {
          id: 15,
          date: '25/01/21',
          name: 'Volgograd',
          value: 23,
          distance: 700,
        },
      ],
      pagesList: 0,
      newPage: 1,
    }
  },
  mounted() {
    this.getPagesList(this.dataTable)
  },
  computed: {},
  methods: {
    getPagesList(arr) {
      if (arr.length % this.limitPage === 0) {
        this.pagesList = arr.length / this.limitPage
      } else {
        this.pagesList = Math.floor(arr.length / this.limitPage) + 1
      }
    },
    sortTable(sortCell, sortIf) {
      let newArray = []
      if (sortIf === 'equals') {

        // когда равно

        if (sortCell === 'name') {
          newArray = this.dataTable.filter(el => el[sortCell].toString() === this.inputSortList)
        } else {
          newArray = this.dataTable.filter(el => parseInt(el[sortCell]) === parseInt(this.inputSortList))
        }
      } else if (sortIf === 'contains') {

        // когда содержит

        newArray = this.dataTable.filter(el => el[sortCell].toString().toLowerCase().includes(this.inputSortList.toLowerCase()))

      } else if (sortIf === 'more') {

        // когда больше

        if (sortCell === 'name') {
          newArray = this.dataTable
        } else {
          if (this.inputSortList === '') {
            newArray = this.dataTable
          } else {
            newArray = this.dataTable.filter(el => parseInt(el[sortCell]) > parseInt(this.inputSortList))
          }
        }

      } else if (sortIf === 'smaller') {

        // когда меньше

        if (sortCell === 'name') {
          newArray = this.dataTable
        } else {
          if (this.inputSortList === '') {
            newArray = this.dataTable
          } else {
            newArray = this.dataTable.filter(el => parseInt(el[sortCell]) < parseInt(this.inputSortList))
          }
        }
      }
      this.getPagesList(newArray)
      return newArray
    },
    showPage(index) {
      return ((this.limitPage * (this.newPage - 1)) <= index && (this.limitPage * (this.newPage)) > index)
    },
    pageNew(page) {
      this.newPage = page
    },
  },
}
</script>

<style scoped>
.nav__bar {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(210,255,250, 1);
}

.nav__bar__form {
  display: flex;
  padding-right: 15px;
}
.nav__bar__form__text {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-right: 10px;
}
.nav__bar__form__input {
  margin-right: 10px;
  border-radius: 5px;
  width: 120px;
  height: 25px;
}
.nav__bar__form__select {
  border-radius: 5px;
  font-size: 14px;
  margin-right: 10px;
}

.table__container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.table__row {
  display: flex;
  margin: 4px;
  width: 80%;
}
.table__cell {
  padding: 10px 20px;
  border: 1px solid black;
  width: 25%;
  margin: 0 4px;
}
.footer {
  bottom: 0;
  left: 0;
  width: 100%;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 18px;
  margin-top: 10px;
}
.footer__page {
  padding: 10px 20px;
  cursor: pointer;
}
.footer__page:hover {
  font-weight: 700;
  font-size: 18px;
  color: blueviolet;
}
.footer__page__active {
  font-weight: 700;
  font-size: 18px;
  color: indigo;
  text-decoration: underline;
}
</style>