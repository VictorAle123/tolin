<template>
  <div class="home">
    <Header />
    <!-- component -->
    <div class="bg-white m-5 shadow rounded-lg p-6">
      <center>
      <h2 class="text-xl mb-2 font-semibold">Elegir fecha</h2>
      </center>
      <br>
            <form @submit.prevent="update">

      <div class="grid lg:grid-cols-4 gap-3">
      
      <label for="start"></label>
<input type="date" id="start" name="trip-start"
       value="2022-04-04"
       min="2022-04-04" max="2022-12-31">
       
<span 
                >Seleccionar paciente:</span>
      <select v-model="selected">
       
        
  <option disabled value="">Seleccione un elemento</option>
  <option   v-for="us in Users" :key="us.id" :value="us.id"> {{ us.nombre }}</option>
  
</select>


<button
              type="submit"
              class="rounded text-gray-100 px-3 py-1 bg-blue-500 hover:shadow-inner hover:bg-blue-700 transition-all duration-300"
            >
              Actualizar
            </button>
    </div>
      </form>
</div>
    <AccInfo />
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import { onBeforeMount, reactive, toRefs } from "vue";
import firebaseUser from "../store/user.js";
import { createUser } from "../main.js";
import AccInfo from "../components/AccInfo.vue";
import Loading from "../components/Loading.vue";
import axios from "axios";

   



export default {

data(){
    return  { 
      Users: null,
    
    };
    
    
  },
  
  name: "dient",
  components: {
    Header,
    AccInfo,
    Loading,
  },
    
 mounted() {
  axios.get( 'http://127.0.0.1:8000/api/paciente/traert'  
    ).then((response) => (this.Users = response.data)
      
    )},

   methods: {
       update()
  {
    console.log();
 
},
 

  },
    
  setup() {
    const state = reactive({
      Nombre: null,
      Direccion: null,
      Telefono: null,
      Edad: null,
      isLoading: false,

    });
  


    const addInfo = async () => {
      // await createUser({ ...state }, (state.isLoading = true));
      await createUser({
      Nombre: "",
      Direccion: "",
      Telefono: "",
      Edad: ""
    });


      // clear fields once method is done
      state.isLoading = false;
      state.Nombre = "";
      state.Direccion = "";
      state.Telefono = "";
      state.Edad = "";
    };
    const { name, image } = firebaseUser();
    return { name, image, ...toRefs(state), addInfo };
  },
};
</script>


