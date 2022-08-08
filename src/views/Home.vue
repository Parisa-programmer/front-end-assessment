<template>
  <div class="home">
   
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    // HelloWorld
  },
  data() {
    return {
      sort: {
        key: '',
        isAsc: false
      },
      tab: 0,
      allRows: 50,
      numberForShow: 5,
      rows: [],
      sortArray: '',
      lengthNumbersTab: 0,
      allRowsArray: [],
      showRowArray: [],
      sortedArray: []
    }
  },
  computed: {
    /*sortedItems() {
      const list = this.sortArray.slice();
      if (!!this.sort.key) {
        list.sort((a, b) => {
          a = a[this.sort.key]
          b = b[this.sort.key]
          return (a === b ? 0 : a > b ? 1 : -1) * (this.sort.isAsc ? 1 : -1)
        });
      }
      this.sortedArray = list
      this.allRowsArray = list
      this.tab = 0
    },*/
  },
  watch: {
    numberForShow() {
      // if ((this.numberForShow.length != 0) && (this.numberForShow != 0) && (this.numberForShow <= this.allRows)) {
      //   this.lengthNumbersTab = Math.floor(this.allRows / this.numberForShow)
      //   this.changeNumberForShow(this.numberForShow)
      // }
    }
  },
  methods: {
    sortBy(key) {
      this.sort.isAsc = this.sort.key === key ? !this.sort.isAsc : false;
      this.sort.key = key;
    },
    showThisRow(number) {
      let newRow = {}
      let rows = []
      let startNumber = (number - 1) * 10
      for (let i = startNumber + 1; i <= this.allRows; i++) {
        newRow = {
          id: i,
          companyName: 'Company name' + i,
          city: 'City' + i,
          region: 'Region' + i,
          country: 'Country' + i
        }
        rows.push(newRow)
      }
      this.sortArray = rows
      this.allRowsArray = rows
    },
    showId(id) {
      alert(id)
    },
    changeNumberForShow(number) {
      // let shows = []
      let startNumber = (number - 1) * this.numberForShow
      // let endNumber = (startNumber + this.numberForShow)
      console.log(startNumber, startNumber + this.numberForShow)
      // for (let i = startNumber; i < startNumber + this.numberForShow; i++) {
      //   shows.push(this.allRowsArray[i])
      // }
      // this.showRowArray = shows
    },
  },
  created() {
    this.showThisRow(1)
    if ((this.numberForShow.length != 0) && (this.numberForShow != 0) && (this.numberForShow <= this.allRows)) {
      this.lengthNumbersTab = Math.floor(this.allRows / this.numberForShow)
    }
  },
}
</script>
