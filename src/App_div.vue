<template>
  <!-- Contenedor principal -->
  <div id="container">
    <!-- Div movible -->
    <div id="movable-div" @mousedown="startDrag" :style="div_style">
      Contenido del div
    </div>
  </div>
</template>


<script>
export default {
  data() {
    // Aquí se definen los datos del componente
    return {
      div_isDragging: false, // Variable para controlar si se está arrastrando el div
      div_startX: 0, // Posición inicial del mouse en el eje X al iniciar el arrastre
      div_startY: 0, // Posición inicial del mouse en el eje Y al iniciar el arrastre
      div_X: 0, // Posición actual del div en el eje X
      div_Y: 0, // Posición actual del div en el eje Y
      div_initialX: 0, // Posición inicial del div en el eje X al iniciar el arrastre
      div_initialY: 0 // Posición inicial del div en el eje Y al iniciar el arrastre
    };
  },
  computed: {
    div_style() {
      // Aquí se calculan los estilos dinámicos del div
      return {
        cursor: this.div_isDragging ? 'grabbing' : 'grab', // Cambia el cursor según si se está arrastrando o no
        left: this.div_X + 'px', // Posición izquierda del div
        top: this.div_Y + 'px', // Posición superior del div
        position: 'absolute', // Posición absoluta del div
        width: '200px', // Ancho del div
        height: '200px', // Alto del div
        backgroundColor: 'lightblue' // Color de fondo del div
      };
    }
  },
  methods: {
    startDrag(event) {
      // Método para iniciar el arrastre del div
      this.div_isDragging = true; // Se establece que se está arrastrando el div
      this.div_startX = event.clientX; // Se guarda la posición inicial del mouse en el eje X
      this.div_startY = event.clientY; // Se guarda la posición inicial del mouse en el eje Y
      this.div_initialX = this.div_X; // Se guarda la posición inicial del div en el eje X
      this.div_initialY = this.div_Y; // Se guarda la posición inicial del div en el eje Y
      document.addEventListener('mousemove', this.onDrag); // Se agrega el evento de arrastre del mouse
      document.addEventListener('mouseup', this.endDrag); // Se agrega el evento de soltar el mouse
      event.preventDefault(); // Se previene el comportamiento predeterminado
    },
    onDrag(event) {
      // Método para arrastrar el div
      if (this.div_isDragging) {
        const dx = event.clientX - this.div_startX; // Diferencia de movimiento en el eje X
        const dy = event.clientY - this.div_startY; // Diferencia de movimiento en el eje Y
        this.div_X = this.div_initialX + dx; // Se actualiza la posición del div en el eje X
        this.div_Y = this.div_initialY + dy; // Se actualiza la posición del div en el eje Y
        event.preventDefault(); // Se previene el comportamiento predeterminado
      }
    },
    endDrag() {
      // Método para finalizar el arrastre del div
      this.div_isDragging = false; // Se establece que no se está arrastrando el div
      document.removeEventListener('mousemove', this.onDrag); // Se remueve el evento de arrastre del mouse
      document.removeEventListener('mouseup', this.endDrag); // Se remueve el evento de soltar el mouse
    }
  }
};
</script>


Espero que esto sea lo que necesitas. Si tienes más preguntas o necesitas más ayuda, ¡no dudes en decírmelo!
<style scoped>
#container {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  user-select: none;
  /* Previene la selección de texto no deseada */
}

#movable-div {
  user-drag: none;
  /* Previene el arrastre predeterminado */
  -webkit-user-drag: none;
  /* Para navegadores WebKit */
}
</style>
