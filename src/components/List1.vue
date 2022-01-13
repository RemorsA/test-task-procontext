<template>
  <div class="container">

    <div class="container-list">
      <div class="icon-right" @click="openCloseList = !openCloseList" v-if="!openCloseList"></div>
      <div class="icon-down" v-else @click="openCloseList = !openCloseList"></div>
      <input class='list-checkbox' type="checkbox" v-model="checkAllList" :indeterminate="indeterminate" @click="deleteAllCheck">
    </div>

    <div class="container-item" v-if="openCloseList">
      <div class="h4-items-info" v-for="item in items" :key='item.value'>
          <input type="checkbox" class="item-checkbox" v-model="checkedList" :value="item.value" @change="toggleItem(item.value)">
          {{item.item}}
          <h4 class="h4-count-info">{{item.count}}</h4>
      </div>
    </div>

    <div class="container-color-count" v-if="arrayWithItems.length > 0 && openCloseList">

      <div class="btn-input-color-count" v-for="item in arrayWithItems" :key="item.value" @change="addCurrentColor(item)">
        <button class="btn-add-del" @click='plus(item)'>+</button>
        <input class="input-color" type="color" v-model="currentColor" :style="{ 'background-color': item.color }">
        <button class="btn-add-del" @click="minus(item)">-</button>

          <div class="container-color-cubes"  v-for="colorCube in colorCubes" :key="colorCube.value">
            <div class="color" v-if="item.value === colorCube.value" :style="{ 'background-color': colorCube.color }"></div>
          </div>

      </div>

    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {item: 'item-1', value: 1, color: '#B0FFDB', count: 0},
        {item: 'item-2', value: 2, color: '#FF70BC', count: 0},
        {item: 'item-3', value: 3, color: '#FF5319', count: 0},
        {item: 'item-4', value: 4, color: '#FF5322', count: 0}
      ],
      arrayWithItems: [],
      checkedList: [],
      colorCubes: [],
      currentColor: '',
      openCloseList: false,
      indeterminate: false,

    }
  },
  methods: {
    plus(item) {
      item.count++
      this.colorCubes.push(item)
    },
    minus(item) {
      const ind = this.colorCubes.findIndex(t => t.value === item.value)
      if (ind > -1 ) {
        item.count--
        this.colorCubes.splice(ind, 1)
      }
    },
    addCurrentColor(item) {
      item.color = this.currentColor
    },
    deleteAllCheck() {
      if (this.checkedList.length=== 3 ||
        this.checkedList.length === 2 ||
        this.checkedList.length === 1) {
        this.arrayWithItems = []
      }
      if (this.checkedList.length === 0) {
        this.arrayWithItems = this.items
      }
    },
    additems(value) {
      this.arrayWithItems.push(this.items.find(t => t.value === value))
    },
    deleteItems(value) {
      this.arrayWithItems = this.arrayWithItems.filter(t => t.value !== value)
    },
    toggleItem(value) {
      if (this.arrayWithItems.find(t => t.value === value)) {
        this.deleteItems(value)
      } else {
        this.additems(value)
      }
    }
  },
  computed: {
    checkAllList: {
      get: function () {
        return this.items ? this.checkedList.length == this.items.length: false
      },
      set: function (value) {
        let checked = []
        if (value) {
          this.items.forEach(function (item) {
            checked.push(item.value)
          })
        }
        this.checkedList = checked
      }
    }
  },
  watch: {
    checkedList() {
      if (this.checkedList.length === 1 || this.checkedList.length === 2) {
        this.indeterminate = true
      }
      if (this.checkedList.length === 3 || this.checkedList.length === 0) {
        this.indeterminate = false
      }
    }
  }
}
</script>

<style lang='scss'>
  @import url('https://fonts.googleapis.com/css2?family=Staatliches&display=swap');

  * {
    box-sizing: border-box;
    margin: 0%;
    padding: 0%;
  }
  
  .container {
    padding: 50px;

    .container-list {
      display: flex;
      width: 100%;
      .icon-right{
        width: 28px;
        height: 28px;
        background-image: url('arrowRight.svg');
        cursor: pointer;
      }
      .icon-down {
        width: 28px;
        height: 28px;
        background-image: url('arrowDown.svg');
        cursor: pointer;
      }
      .list-checkbox {
        width: 26px;
        height: 26px;
      }
    }

    .container-item {
      margin-left: 50px;
      .h4-items-info {
        padding: 10px;
        font-family: 'Staatliches', cursive;
        display: flex;
        .h4-count-info {
          margin-left: 50px;
        }
      }
      .item-checkbox {
        width: 26px;
        height: 26px;
      }
    }

    .container-color-count {
      margin-left: 200px;
      .btn-input-color-count {
        display: flex;
        padding: 10px;
        .input-color {
          width: 50px;
          height: 30px;
        }
        .btn-add-del {
          width: 20px;
          height: 20px;
          outline: 0;
          border: 0;
          background:#695252;
          border-radius: 50%;
          color: white;
          cursor: pointer;
        }
        .color {
          margin-left: 5px;
          width: 25px;
          height: 25px;
          border: 1px solid black;
        }
      }
    }

  }
</style>
