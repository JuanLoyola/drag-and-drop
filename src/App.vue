<template>
  <div>
    <input placeholder="New task" v-model="newTask" @keyup.enter="add" />
    <section class="row">
      <div
      id="log"
      class="drop-zone"
      @drop="onDrop($event,1)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h1>Back Log</h1>
      <div
      v-for="item in getList(1)"
      :key="item.id"
      class="drag-el" 
      draggable="true"
      @dragstart="startDrag($event,item)"
      >
        {{ item.title }}
      </div>
    </div>

    <div
      id="todo"
      class="drop-zone"
      @drop="onDrop($event,2)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h1>To do</h1>
      <div
      v-for="item in getList(2)"
      :key="item.id"
      class="drag-el"
      draggable="true"
      @dragstart="startDrag($event,item)"
      >
        {{ item.title }}
      </div>
    </div>

    <div
      id="testing"
      class="drop-zone"
      @drop="onDrop($event,3)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h1>Testing</h1>
      <div
      v-for="item in getList(3)"
      :key="item.id"
      class="drag-el"
      draggable="true"
      @dragstart="startDrag($event,item)"
      >
        {{ item.title }}
      </div>
    </div>

    <div
      id="done"
      class="drop-zone"
      @drop="onDrop($event,4)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h1>Done</h1>
      <div
      v-for="item in getList(4)"
      :key="item.id"
      class="drag-el"
      draggable="true"
      @dragstart="startDrag($event,item)"
      >
        {{ item.title }}
      </div>
    </div>
    </section>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const items = ref([
      { id: 0, title: 'Forms', list: 1 },
      { id: 1, title: 'Footer', list: 1 },
      { id: 2, title: 'Product view', list: 2 },
      { id: 3, title: 'Banner 2', list: 3 },
      { id: 4, title: 'Banner 1', list: 3 },
      { id: 5, title: 'Navbar', list: 4 }
    ])

    const getList = (list) => {
      return items.value.filter((item) => item.list == list)
    }

    const startDrag = (event, item) => {
      console.log(item)
      event.dataTransfer.dropEffect = 'move'
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setData('itemID', item.id)
    }

    const onDrop = (event, list) => {
      const itemID = event.dataTransfer.getData('itemID')
      const item = items.value.find((item) => item.id == itemID)
      item.list = list
    }

    const add = () => {
      if (this.newTask) {
        this.items.push({ title: this.newTask })
        this.newTask = ""
      }
    }

    return {
      getList,
      onDrop,
      startDrag,
      newTask: '',
      add
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.row {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: row nowrap;
}
.drop-zone {
  width: 25%;
  margin: 0 20px;
  padding: 10px 10px;
  min-height: 350px;
  box-shadow: 0px 3px 3px #918f8f;
}

.drag-el {
  background-color: #f1f1f1;
  color: #535353;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  text-transform: uppercase;
  padding: 5px;
  margin-bottom: 10px;
  cursor: pointer;
  box-shadow: 0px 3px 3px #918f8f;
}

.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}

#log {
  background-color:#e1e2e5;
}

#todo {
  background-color: #c1e2ff;
  color: #3f93c4;
}

#testing {
  background-color: #fff1c9;
  color: #c4a03f;
}

#done {
  background-color: #cfe8da;
  color: #666561;
}
</style>
