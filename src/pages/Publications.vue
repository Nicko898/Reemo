<script>
import { subscribeToNewPublication } from '../services/publication.js';
import { collection, getDocs, addDoc, serverTimestamp } from 'firebase/firestore';
import { db } from '../services/firebase.js';
import { subscribeToAuthState } from '../services/auth.js';

import Heading from '../components/Heading.vue';
import TarjetasVehiculos from '../components/TarjetasVehiculos.vue'; 
import AddIcon from '../icons/AddIcon.vue';

export default {
  name: 'Publications',
  components: { Heading, TarjetasVehiculos, AddIcon },
  data() {
    return {
      cars: [],
      loggedUser: {
        id: null,
        email: null,

      },
      loading: false,
    };
  },
  methods: {
    async fetchCars() {
      try {
        this.loading = true;
        const carsCollection = collection(db, 'cars');
        const carsSnapshot = await getDocs(carsCollection);
        this.cars = carsSnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}))
        .filter(car => car.user_id !== this.loggedUser.id);
      } catch (error) {
        console.error('Error al buscar autos:', error);
      }finally {
        this.loading = false; 
      }
    },
    async addCar(newCar) {
      // this.cars.push(newCar);
      const carsCollection = collection(db, 'cars');
      const docRef = await addDoc(carsCollection, {
        ...newCar,
        created_at: serverTimestamp()
      });
      this.cars.push({ id: docRef.id, ...newCar });
    },
    goToCarDetails(carId) {
      this.$router.push({ name: 'CarDetails', params: { id: carId } });
  }
  },
  // created() {
  //   this.fetchCars();
  // },
  mounted() {
  subscribeToAuthState(newUserData => {
    this.loggedUser = newUserData;

    if (this.loggedUser.id) {
      this.fetchCars();  
    }
  });

  subscribeToNewPublication((newCars) => {
    this.cars = newCars;
  });
},
};
</script>

<template>
  <Heading class="p-4 max-w-md mx-auto md:max-w-screen-xl">Publicaciones</Heading>

  <div v-if="loading" class="flex items-center justify-center w-fit mx-auto bg-gray-50">
    <div role="status">
      <svg aria-hidden="true" class="w-8 h-8 text-gray-200 animate-spin fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
        <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
      </svg>
      <span class="sr-only">Cargando...</span>
    </div>
  </div>

      <template v-if="loggedUser.id == null">
        <router-link 
          to="/Login" 
          class="fixed gap-4 md:flex items-center justify-center bottom-0 right-0 m-8 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-full md:rounded-lg text-md px-2 md:px-4 py-2 text-center"
          >
          <span class="hidden md:block">Publicar Vehículo</span>
          <AddIcon/>
        </router-link>
      </template>
      <template v-else>
        <router-link 
          to="/Publish" 
          class="fixed gap-4 md:flex items-center justify-center bottom-0 right-0 m-8 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-full md:rounded-lg text-md px-2 md:px-4 py-2 text-center"
          >
          <span class="hidden md:block">Publicar Vehículo</span>
          <AddIcon/>
        </router-link>
      </template>
    
    <!-- <div 
      v-if="cars.length === 0"
      class="flex flex-col justify-center items-center mb-4"
    >
      <h2 class="flex flex-col justify-center items-center text-lg font-bold mb-4">No hay autos publicados todavía.</h2>
      <router-link to="/Publish" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 text-center">Publicar Vehículo</router-link>
    </div> -->



    <div class="max-w-md mx-auto md:max-w-screen-xl p-4 mb-4 grid gap-4 md:grid-cols-2 md:mb-8 lg:grid-cols-3 xl:grid-cols-4">
      <div 
        v-for="(car, index) in cars" 
        :key="index" 
        class="border-4 rounded-lg shadow-md"
        >   
        <TarjetasVehiculos :car="car"></TarjetasVehiculos>
      </div>
    </div>
</template>
