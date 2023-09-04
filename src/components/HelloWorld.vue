<template>
  <div id="app">
    <h1>Juego del ahorcado</h1>
    <h2>Adivina la palabra</h2>
    <div id="palabra">
      <span v-for="letra in palabraOculta">{{ letra }}</span>
    </div>

    <div class="teclado">
      <button id="btn" v-for="letra in letras" :key="letra" @click="adivinar(letra)" :data-letra="letra" v-if="!enabled" disabled>{{ letra }}</button>
      <button id="btn" v-for="letra in letras"  @click="adivinar(letra)" :data-letra="letra" v-else>{{ letra }}</button>

    </div>
  </div>
</template>

<script>
import palabras from '@/assets/game/ahorcado/palabras.json';

export default {

  data() {
    return {
      palabraOculta: [],
      palabrasJuego: palabras,
      letra: "",
      palabra: "",
      fallos: 0,
      enabled:true,
      letras: ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"],

    };
  },
  mounted() {
    this.palabra = this.obtenerPalabraAleatoria();
    for (const l of this.palabra) {
      this.palabraOculta.push("_");
    }
  },
  methods: {
    obtenerPalabraAleatoria() {
      // Obtener un índice aleatorio
      const indiceAleatorio = Math.floor(Math.random() * this.palabrasJuego.palabras.length);

      // Obtener la palabra en la posición aleatoria
      const palabraAleatoria = this.palabrasJuego.palabras[indiceAleatorio];
      return palabraAleatoria;
    },
    adivinar(le) {


      const letraEnPalabra = this.palabra.includes(le);
      const botones = document.querySelectorAll(".teclado button");


      botones.forEach((boton) => {
        const letraBoton = boton.getAttribute("data-letra");
        if (le === letraBoton) {
          if (letraEnPalabra) {
            for (var x = 0; x < this.palabra.length; x++) {
              if (le == this.palabra.charAt(x)) {
                this.palabraOculta[x] = le

              }

            }
            boton.classList.add("correcta");
          } else {
            boton.classList.add("incorrecta");
            this.fallos++;

          }
        }
      });



      if (this.fallos === 10) {
        this.perder();
        this.enabled = false;
      }

      if (this.palabraOculta.join("") === this.palabra) {
        this.ganar();
      }
    },
    perder() {
      alert("¡Has perdido!");
    },
    ganar() {
      alert("¡Has ganado!");
    },
  },
};

</script>

<style scoped>
#palabra {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
  font-size: 50px;
}

.letra {
  font-size: 24px;
}

#cuerpo {
  display: flex;
  justify-content: center;
}

img {
  width: 100px;
}

.teclado {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  margin-bottom: 10px;
}

.teclado>button {
  
  height: 40px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 24px;
  font-family: sans-serif;
  margin: 10px;
}

.teclado>button:hover {
  background-color: #eee;
}

.teclado>button.correcta {
  background-color: #00ff00;
}

.teclado>button.incorrecta {
  background-color: #ff0000;
}
</style>