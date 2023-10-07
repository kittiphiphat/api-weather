<template>
  <header class="sticky top-0 shadow-lg">
    <nav
      class="container flex flex-col m:flex-row items-center gap-4 text-white py-6"
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-4">
          <i class="fa-solid fa-umbrella fa-xl"></i>
          <p class="text-2xl">Weather</p>
        </div>
      </RouterLink>

      <div class="flex gap-4 flex-1 justify-end">

      
      </div>
    </nav>

  </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import { ref } from "vue";

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );
  }

  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  savedCities.value.push(locationObj);
  localStorage.setItem(
    "savedCities",
    JSON.stringify(savedCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
};

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Nunito:ital,wght@1,200;1,700&display=swap');

.text-2xl{
  font-size: 30px;
  font-weight: 700;
  font-family: 'Nunito', sans-serif;
}
</style>
