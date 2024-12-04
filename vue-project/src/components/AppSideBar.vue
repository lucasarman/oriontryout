<template>
  <div id="sidebar">
    <div>
      <h1>Gerenciar Formas</h1>
      <p>Aqui você poderá gerenciar formas desenhadas no mapa.</p>
    </div>

    <div id="buttons">
      <button @click="toggleDrawing('rectangle')">Desenhar Retângulo</button>
      <button @click="toggleDrawing('circle')">Desenhar Círculo</button>
      <button @click="toggleDrawing('polygon')">Desenho Livre</button>
      <button @click="clearShapes">Limpar Formas</button>
    </div>

    <div>
      <h3>Formas Salvas:</h3>
      <ul>
        <li v-for="(shape, index) in shapes" :key="index">
          {{ shape.type }} - {{ shape.id }}
          <div>
            <button @click="loadShape(index)">Carregar</button>
            <button @click="removeShape(index)">Remover</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppSidebar',
  props: {
    shapes: {
      type: Array,
      required: true,
    },
  },
  emits: ['loadShape', 'removeShape', 'toggleDrawing', 'clearShapes'],
  methods: {
    loadShape(index) {
      this.$emit('loadShape', index) 
    },
    removeShape(index) {
      this.$emit('removeShape', index) 
    },
    toggleDrawing(type) {
      this.$emit('toggleDrawing', type) // Emite o evento para alterar o modo de desenho
    },
    clearShapes() {
      this.$emit('clearShapes') // Emite o evento para limpar as formas
    },
  },
}
</script>

<style>
#sidebar {
  max-width: 50vh;
  background: #333;
  color: white;
  padding: 20px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
}
#sidebar h1 {
  font-size: 24px;
  margin-bottom: 10px;
}
#sidebar p {
  font-size: 16px;
  margin-bottom: 20px;
}
#sidebar h3 {
  font-size: 20px;
  margin-bottom: 20px;
}
#sidebar ul {
  list-style: none;
  padding: 0;
}
#sidebar li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-radius: 4px;
  transition: background 0.3s;
}
#sidebar li:hover {
  background: #444;
}
#sidebar button {
  background: #555;
  border: none;
  color: white;
  padding: 15px;
  border-radius: 4px;
  cursor: pointer;
  margin-bottom: 10px;
  flex-direction: column;
  align-items: center;
  width: 100%;
}
#sidebar button:hover {
  background: #777;
}
#buttons {
  flex-direction: column;
  align-items: center;
  width: 100%;
}
</style>
