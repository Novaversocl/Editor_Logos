<template>
  <div id="app" class="container  fondo-transparente">



    
    <form class="form-container">
      <label for="id_colorInput">Color de Fondo:</label>
      <div class="salto"></div>

      <label for="gradientStart">Inicio:</label>
      <input type="color" id="gradientStart" v-model="v_model_gradientStartColor" />
      <div class="salto"></div>

      <label for="gradientEnd">Fin:</label>
      <input class="derecha" type="color" id="gradientEnd" v-model="v_model_gradientEndColor" />
      <div class="salto"></div>

      <div class="salto"></div>
      <label for="id_textInput">Color de Texto: </label>
      <input type="color" id="id_textInput" v-model="v_model_textColor" />
      <label>
        <div class="salto"></div>
        <input type="checkbox" v-model="v_model_showText" /> Mostrar Texto
      </label>
      <div class="salto"></div>

      <label for="borderInput">Borde:</label>
      <div class="salto"></div>
      <input type="range" id="borderInput" v-model="v_model_borderWidth" min="0" max="50" step="1" />
      <span>{{ v_model_borderWidth }}px</span>

      <div class="salto"></div>
      <textarea v-model="v_model_text" :style="{ resize: 'none' }"></textarea>
      <div class="salto"></div>



      <div class="salto"></div>

      <label for="id_fontInput">Tipo de Fuente:</label>
      <div class="salto"></div>
      <select v-model="v_model_font" @change="updateFont" class="input_fuente">
        <option v-for="font in googleFonts" :key="font.family" :value="font.family">
          {{ font.family }}
        </option>
      </select>

      <label>
        <div class="salto"></div>
        <!-- <input type="checkbox" v-model="v_model_opacity" /> Sombra Texto -->
        <input type="checkbox" v-model="v_model_sombra" /> Sombra Texto

      </label>

      <div class="salto"></div>
      <label>Tamaño de Letra:</label>
      <div>
        <label>
          <input type="radio" value="pequeño" v-model="v_model_fontSize" />
          Pequeño
        </label>
        <div class="salto"></div>
        <label>
          <input type="radio" value="mediano" v-model="v_model_fontSize" />
          Mediano
        </label>
        <div class="salto"></div>
        <label>
          <input type="radio" value="grande" v-model="v_model_fontSize" />
          Grande
        </label>
      </div>

      <br>
<div class="container boton">

 

</div>
<div class="container boton">
        <button @click="descargarImagen" type="button" class="btn btn-success">Descargar Gratis</button>

      </div>
   

    </form>

    <div  class="square-container">
      <div ref="descargarLogoContainer" class="square" id="movable-div" @mousedown="startDrag" :style="{
        ...div_style,
        background: v_model_gradientStyle,
        color: v_model_textColor,
        // opacity: v_model_opacity ? '0.8' : '1',
        textShadow: v_model_sombra ? '2px 2px 4px rgba(0, 0, 0, 0.5)' : 'none' ,
        // boxShadow: v_model_opacity ? '0 0 10px rgba(0, 0, 0, 0.5)' : 'none' ,/* Agregar resplandor solo si la opacidad está activada */
        borderRadius: v_model_borderWidth + 'px',
     
  borderColor: 'transparent',
  


      }">
        <span :style="{
          fontSize: fontSize,
          fontFamily: v_model_font,
        }" v-show="v_model_showText">
          {{ v_model_text }}
        </span>
      </div>
    </div>
  </div>

  <footer>
<p class="footer">Desarrollado por Novaverso
<a href="https://github.com/Novaversocl"> GitHub</a>
</p>
  </footer>
</template>

<script>
import axios from 'axios';
import html2canvas from 'html2canvas';
export default {
  name: "App",
  data() {
    return {

      v_model_backgroundColor: "",
      v_model_textColor: "white",
      v_model_showText: true,
      v_model_text: "Nombre empresa",
      v_model_font: "",
      v_model_bold: false,
      v_model_fontSize: "pequeño",
      // v_model_opacity: false,
      v_model_sombra: false,
      v_model_borderWidth: 0,
      v_model_gradientStartColor: "#251177", // Color de inicio predeterminado
      v_model_gradientEndColor: "#ff0000", // Color de fin predeterminado
      googleFonts: [],


      div_isDragging: false, // Variable para controlar si se está arrastrando el div
      div_startX: 0, // Posición inicial del mouse en el eje X al iniciar el arrastre
      div_startY: 0, // Posición inicial del mouse en el eje Y al iniciar el arrastre
      div_X: 0, // Posición actual del div en el eje X
      div_Y: 0, // Posición actual del div en el eje Y
      div_initialX: 0, // Posición inicial del div en el eje X al iniciar el arrastre
      div_initialY: 0, // Posición inicial del div en el eje Y al iniciar el arrastre
    };
  },
  computed: {
    v_model_gradientStyle() {
      return `linear-gradient(to bottom right, ${this.v_model_gradientStartColor}, ${this.v_model_gradientEndColor})`;
    },
    fontSize() {
      if (this.v_model_fontSize === "pequeño") {
        return "1rem";
      } else if (this.v_model_fontSize === "mediano") {
        return "2rem";
      } else if (this.v_model_fontSize === "grande") {
        return "3rem";
      } else {
        return "1rem"; // Tamaño predeterminado si no se selecciona nada
      }
    },
    div_style() {
      // Aquí se calculan los estilos dinámicos del div
      return {
        cursor: this.div_isDragging ? "grabbing" : "grab", // Cambia el cursor según si se está arrastrando o no
        left: this.div_X + "px", // Posición izquierda del div
        top: this.div_Y + "px", // Posición superior del div
        position: "absolute", // Posición absoluta del div
        // width: "180px", // Ancho del div
        // height: "180px", // Alto del div
        // backgroundColor: "lightblue", // Color de fondo del div
      };
    },
  },

  methods: {

    descargarImagen() {
  const div = this.$refs.descargarLogoContainer;
  if (!div) {
    console.error("Elemento ref 'descargarLogoContainer' no encontrado");
    return;
  }

  html2canvas(div, {
    backgroundColor: null, // Establecer el fondo transparente
    scale: 3, // Escala mayor para mejorar la calidad de la imagen
    logging: false, // Deshabilitar el registro de consola para mejorar el rendimiento
    dpi: window.devicePixelRatio * 900, // DPI más alto para mejorar la calidad de impresión
  }).then((canvas) => {
    const imgData = canvas.toDataURL('image/png', 1.0); // Calidad máxima
    const link = document.createElement('a');
    link.href = imgData;
    link.download = 'logo.jpg'; // Cambiar la extensión a .jpg
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }).catch(error => {
    console.error("Error al generar la imagen con html2canvas:", error);
  });
},

    startDrag(event) {
      // Método para iniciar el arrastre del div
      this.div_isDragging = true; // Se establece que se está arrastrando el div
      this.div_startX = event.clientX; // Se guarda la posición inicial del mouse en el eje X
      this.div_startY = event.clientY; // Se guarda la posición inicial del mouse en el eje Y
      this.div_initialX = this.div_X; // Se guarda la posición inicial del div en el eje X
      this.div_initialY = this.div_Y; // Se guarda la posición inicial del div en el eje Y
      document.addEventListener("mousemove", this.onDrag); // Se agrega el evento de arrastre del mouse
      document.addEventListener("mouseup", this.endDrag); // Se agrega el evento de soltar el mouse
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
      document.removeEventListener("mousemove", this.onDrag); // Se remueve el evento de arrastre del mouse
      document.removeEventListener("mouseup", this.endDrag); // Se remueve el evento de soltar el mouse
    },
    async fetchGoogleFonts() {
      try {
        const response = await axios.get('https://www.googleapis.com/webfonts/v1/webfonts?key=AIzaSyD_HpYZvJULJ1Tai7jWA-sb3aFJ9MBRcd0');
        this.googleFonts = response.data.items;
      } catch (error) {
        console.error('Error fetching Google Fonts:', error);
      }
    },
    updateFont() {
      const selectedFont = this.v_model_font;
      const link = document.createElement('link');
      link.href = `https://fonts.googleapis.com/css2?family=${selectedFont.replace(/ /g, '+')}&display=swap`;
      link.rel = 'stylesheet';
      document.head.appendChild(link);
    },
  },
  mounted() {
    this.fetchGoogleFonts();
  },



};
</script>

<style>
#movable-div {
  user-drag: none;
  /* Previene el arrastre predeterminado */
  -webkit-user-drag: none;
  /* Para navegadores WebKit */
}

.container {
  display: flex;
}

.fondo {
  background-color: rgb(0, 11, 26);
}
/* .fondo-transparente {
  background-color: transparent;
} */

.form-container {
  margin-left: 5px;
  margin-right: 1px;
  margin-top: 1rem;
  padding-left: 10px;
  padding-right: 5px;
  padding-top: 1rem;
  padding-bottom: 8rem;
  width: 180px;
  height: 350px;
  color: aliceblue;
  background: linear-gradient(to bottom right,
      rgb(37, 17, 119),
      rgb(255, 0, 0));
  /* Degradado de fondo */

  border-radius: 0rem 2rem;
}

.square-container {
  
  /* width: 550px; */
  /* padding:3rem; */
  /* Ancho fijo del contenedor del cuadrado */

  display: flex;
  justify-content: center;
  align-items: center;


  margin: 1rem;
  position: relative;
  user-select: none;
}

.square {

  display: flex;
  justify-content: center;
  align-items: center;
  width: 10rem;
  height: 10rem;

  padding: 3rem;


}

/* Estilos para dispositivos móviles */
@media screen and (max-width: 600px) {
  .square {
 
padding: 0.5rem;
 margin: 0;
 width: 8rem;
  height: 8rem;
 
  /* padding-left:0 ;
  padding-right: 0.5rem;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem; */
 


    /* Otros estilos específicos para dispositivos móviles */
  }

  .form-container {
   /* margin-left: 1px;
  margin-right: 1px;
  margin-top: 1rem;
  padding-left: 1px;
  padding-right: 1px;
  padding-top: 1rem; */
  /* padding-bottom: 8rem; */
margin: 1px;
padding: 5px;

  width: 11rem;
  height: 30rem;

  color: aliceblue;
  background: linear-gradient(to bottom right,
      rgb(137, 46, 158),
      rgb(13, 5, 255));
  /* Degradado de fondo */

  border-radius: 0rem 2rem;
}

}







.red {
  background-color: red !important;
}

salto {
  padding-bottom: 1rem;
}

.derecha {
  margin-left: 1rem;
}

.input_fuente {

  width: 150px;

}

/* Agrega un estilo para los iconos en el dropdown */
.icon-option {
  display: flex;
  align-items: center;
}

.icon-option i {
  margin-right: 5px;
}


#app {
}

footer {
  margin-top: auto; /* Push the footer to the bottom of the page */
}

.footer {
  text-align: center;
  color: #ffffff; /* Adjust color as needed */
padding-top: 5rem;
}

.boton{


justify-content: center;

}


.btn {
  display: inline-block;
  font-weight: 400;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  user-select: none;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  cursor: pointer;
}

.btn-success {
  color: #fff;
  background-color: #28a745;
  border-color: #28a745;
}

.btn-success:hover {
  color: #fff;
  background-color: #218838;
  border-color: #1e7e34;
}

.btn-success:focus, .btn-success.focus {
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgba(40, 167, 69, 0.5);
}

.btn-success:active, .btn-success.active, .show > .btn-success.dropdown-toggle {
  color: #fff;
  background-color: #1e7e34;
  border-color: #1c7430;
}

.btn-success:active:focus, .btn-success.active:focus, .show > .btn-success.dropdown-toggle:focus {
  box-shadow: 0 0 0 0.2rem rgba(40, 167, 69, 0.5);
}

.square-container{
}

.transparent-border {
  /* border: none !important; */
}
.square {
  /* border: 50px solid rgba(0, 0, 0, 0); */
/* background: #28a745 */
}



</style>
