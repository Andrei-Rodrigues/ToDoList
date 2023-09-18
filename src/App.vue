<script>
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
      todos: [],
      text: ""
    }
  },

  // Methods are functions that mutate state and trigger updates.
  // They can be bound as event handlers in templates.
  methods: {
    add() {
      if (this.text == '') {
        return
      }
      this.todos.push(
        {
          "id": this.todos.length,
          "title": this.text,
          "isDone": false,
          "isEditing": false
        }
      )
      this.text = ''
    },
    apagar(item) {
      const index = this.todos.indexOf(item)
      this.todos.splice(index, 1)
    },
    editar(item) {
      item.isEditing = !item.isEditing
    },
    ordenar(itens) {
      return itens.sort((a, b) => a.isDone - b.isDone)
    }
  },
}

</script>

<template>
<div class="background-container">
  <div class="content-overlay">
    <div class="input-container">
  <b-container style="margin-top: 10px;">
  <b-row>
    <b-col cols="10">
      <input class="form-control" v-model="text">
    </b-col>
    <b-col cols="2">
      <button class="btn btn-primary" @click="add">adicionar</button>
    </b-col>
  </b-row>
  </b-container>

  <b-container>
    <b-card
    v-for="item in ordenar(todos)"
    :key="item"
    tag="article"
    :class="{ 'done': item.isDone } "
    :style="{
      'border-color': item.isDone ? 'green' : '',
      'box-shadow': item.isDone ? '0 0 10px green' : '',
      'margin-top': '10px'
    }">
      <b-row>
        <b-col sm="2">
          <b-form-checkbox v-model="item.isDone"></b-form-checkbox>
        </b-col>
        <b-col cols="8">
          <div v-if="item.isEditing">
            <input class="form-control" v-model="item.title">
          </div>
          <div v-else>
            {{ item.title }}
          </div>
        </b-col>
        <b-col sm="2">
          <div v-if="item.isEditing">
            <button class="btn btn-success" @click="editar(item)">Salvar</button>
          </div>

          <div v-else>
            <button class="btn btn-light" :disabled="item.isDone" @click="editar(item)">Editar</button>
            <button class="btn btn-danger" :disabled="item.isDone" @click="apagar(item)" style="margin-left:5px;">Remover</button>
          </div>
        </b-col>
      </b-row>
    </b-card>
  </b-container>
    </div>
  </div>
</div>
</template>

<style>
.done {
  border-color: #1aaa00;
}

.background-container {
  background-image: url('https://wallpapercrafter.com/sizes/3840x2160/87387-winter-snow-landscape-minimalism-minimalist-hd-4k-artist-artwork-digital-art-deviantart-5k-8k.jpg');
  background-size: cover;
  background-position: center;
  width: 100%;
  height: 100vh;
  overflow: hidden;
  position: relative;
}

.content-overlay {
  position: absolute;
  top: 1;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #000;
}

.input-container {
  text-align: center;
  width: 2000px;
}

b-card {
  margin-bottom: 10px;
}

</style>