<template>
  <div id="container">
    <img
      id="movable-image"
      src="data:image/jpeg;base64,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"      alt="Imagen Movible"
      @mousedown="startDrag"
      :style="{ left: imageX + 'px', top: imageY + 'px', position: 'absolute', cursor: isDragging ? 'grabbing' : 'grab' }"
      @dragstart.prevent
    />

  </div>



</template>



<script>
export default {
  data() {
    return {
      isDragging: false,
      startX: 0,
      startY: 0,
      imageX: 0,
      imageY: 0,
      initialX: 0,
      initialY: 0,



    };
  },
  methods: {
    startDrag(event) {
      this.isDragging = true;
      this.startX = event.clientX;
      this.startY = event.clientY;
      this.initialX = this.imageX;
      this.initialY = this.imageY;
      document.addEventListener('mousemove', this.onDrag);
      document.addEventListener('mouseup', this.endDrag);
      document.addEventListener('mouseleave', this.endDrag);
      event.preventDefault(); // Previene comportamiento predeterminado
    },
    onDrag(event) {
      if (this.isDragging) {
        const dx = event.clientX - this.startX;
        const dy = event.clientY - this.startY;
        this.imageX = this.initialX + dx;
        this.imageY = this.initialY + dy;
        event.preventDefault(); // Previene comportamiento predeterminado
      }
    },
    endDrag() {
      this.isDragging = false;
      document.removeEventListener('mousemove', this.onDrag);
      document.removeEventListener('mouseup', this.endDrag);
      document.removeEventListener('mouseleave', this.endDrag);
    }


    
  }
};
</script>




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



#movable-image {
  width: 500px;
  /* Ajusta el tamaño según tus necesidades */
  height: auto;
  cursor: grab;
  user-drag: none;
  /* Previene el arrastre predeterminado */
  -webkit-user-drag: none;
  /* Para navegadores WebKit */
}
</style>
