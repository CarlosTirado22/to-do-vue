<script setup>
import { ref, onMounted  } from 'vue';
import { format } from 'date-fns';

// Cree una referencia reactiva para almacenar la hora actual
const horaActual = ref('');

// Función para obtener la hora actual y actualizar la referencia reactiva
const obtenerHoraActual = () => {
  const ahora = new Date();
  let hora = ahora.getHours();
  const minutos = ahora.getMinutes();
  const segundos = ahora.getSeconds();
  
  // Convertir a formato de 12 horas
  const amPm = hora >= 12 ? 'PM' : 'AM';
  hora = hora % 12 || 12; // Convertir las 0 horas a 12 en formato de 12 horas

  // Formatear la hora
  horaActual.value = `${hora}:${minutos}:${segundos} ${amPm}`;
};

// Llamar a la función obtenerHoraActual cuando el componente se monte
onMounted(() => {
  obtenerHoraActual();
  // Actualizar la hora cada segundo
  setInterval(obtenerHoraActual, 1000);
});

// Crear una referencia reactiva para almacenar la fecha actual
const fechaActual = ref(format(new Date(), 'dd/MM/yyyy'));

//funcion del TO-DO

const textoInput = ref(""); // Variable para almacenar el texto que ingrese
const arrayFavoritos = ref([]); // Array para almacenar los textos favoritos

const add = () => {
  if (textoInput.value.trim() !== "") { // Esta condicion verifica si el texto no está vacío
    arrayFavoritos.value.push({ texto: textoInput.value, clickeado: false }); // Agrega el texto al array, y coloco clickeado en false para que nose subraye
    textoInput.value = ""; // Limpia el input después de agregar el texto
  }
};

const resetear = (index) =>{
  arrayFavoritos.value.splice(index, 1); // Elimina el elemento en la posición indicada
};

const toggleUnderline = (index) => {//esta funcion toma como parametro INDEX, que indica el índice del elemento en el array arrayFavoritos que se ha clicado.
  arrayFavoritos.value[index].clickeado = !arrayFavoritos.value[index].clickeado;//arrayFavoritos.value[index]: Accedemos al elemento correspondiente en el array utilizando el índice proporcionado.
};//.clickeado: Accede a la propiedad clickeado del elemento para cambiar su valor.

</script>

<template>
  <div class="contenedor">
    <h1>Practica TO-DO</h1>
    <div class="cardO">
      <div class="card1">
        <div class="fecha">
           {{ fechaActual }}
        </div>
        <div class="hora">
          {{ horaActual }}
        </div>

        <div class="input-group mb-3">
          <input v-model="textoInput" type="text" class="form-control input" placeholder="Nueva tarea">
          <button v-on:click="add" class="btn btn-outline-secondary" type="button" id="button-addon2"><svg xmlns="http://www.w3.org/2000/svg" width="25" height="25" viewBox="0 0 24 24" fill="none" stroke="#000000" stroke-width="1" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg></button>
        </div>
        <!-- recorro el array y lo muestro, tambien ingreso la funcion para cambiar de valor la propiedad clickeado -->
        <div v-for="(texto, index) in arrayFavoritos" :key="index" class="resul" v-on:click="toggleUnderline(index)" :class="{ subrayado: texto.clickeado }">
          <span>{{ texto.texto }}</span>
          <button v-on:click="resetear(index)" class="borrar"><svg  width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#000000"  ><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path><line x1="10" y1="11" x2="10" y2="17"></line><line x1="14" y1="11" x2="14" y2="17"></line></svg></button>
        </div>
        
      </div>
    </div>
  </div>
  <footer class="footer pt-3  ">
    <div class="container-fluid">
      <div class="row align-items-center justify-content-lg-between">
        <div class="col-lg-6 mb-lg-0 mb-4">
          <div class="copyright text-center text-sm text-muted text-lg-start">
            © 
            creado por Carlos Tirado
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<style>

.contenedor{
  background: #F8F9FA;
  margin: 20px;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 3px 4px 9px rgba(0,0,0,.6);
}

h1{
  margin: 10px;
  margin-top: -5px;
  padding: 5px;
}

.cardO{
  margin: 10px;
  padding: 15px;
  display: flex;
  justify-content: center;
}

.card1{
  padding: 15px;
  margin: 20px;
  max-width: 26%;
  word-break: break-all;
  background-color: #F5F3F4;
  border: 1px solid #ccc;
  box-shadow: 1px 1px 6px rgba(0,0,0);
}

.resul{
  margin: 10px;
  background: #EDEDE9;
  justify-content: space-between;
  display: flex;
  border: 1px solid #ccc;
  box-shadow: 1px 1px 6px rgb(142, 165, 165);
}

span{
  margin: 10px;
}

.borrar{
  margin: 5px;
  border: none;
}

.fecha{
  padding: 5px;
  margin-bottom: 10px;
  font-size: 2em;
}

.subrayado {
  text-decoration: line-through;
}

/*PARA DISPOSITIVOS MOVILES*/
@media (max-width: 768px){
.contenedor{
  margin: 25px;
  height: 100%;
  margin-top: 20%;
}

.card1{
  max-width: 100%;
}

.footer{
  margin-right: 40%;
}

}
</style>
