<script setup>
import { onMounted, ref } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css';

const map = ref(null)
const mapContainer = ref(null)
const isClicked = ref(false)
const lat = ref(null)
const long = ref(null)
const errorMessage = ref('')

const reverseGeocode = async (latitude, longitude) => {
  const apiUrl = `https://nominatim.openstreetmap.org/reverse?format=jsonv2&lat=${latitude}&lon=${longitude}`;
  try {
    const response = await fetch(apiUrl);
    if (!response.ok) {
      console.error('Reverse geocoding failed:', response.status);
      return null;
    }
    const data = await response.json();
    if (data && data.display_name) {
      return `you're aproximately at ${data.display_name}`;
    }
    return null;
  } catch (error) {
    console.error('Error during reverse geocoding:', error);
    return null;
  }
};

const speakLocation =   (latitudeOrText, longitude) => {
 if ('speechSynthesis' in window) {
     const speech = new SpeechSynthesisUtterance();
     speech.lang = 'en-US'; // You can adjust the language
     if (typeof latitudeOrText === 'string') {
     speech.text = latitudeOrText; // Speak the location name directly
   } else if (latitudeOrText && longitude) {
   // latitudeOrText is likely the lat ref, longitude is the long ref
   speech.text = `Your current latitude is ${latitudeOrText.value.toFixed(6)} and your longitude is ${longitude.value.toFixed(6)}.`;
   } else {
     console.warn('Invalid input for speakLocation.');
     return;
    }
       window.speechSynthesis.speak(speech); 
     } else {
         console.warn('Text-to-speech is not supported in this browser.');
         errorMessage.value = 'Text-to-speech is not supported in your browser.';
      }
}


const showMyLocation = async () => {
  if (!map.value) return;
  isClicked.value = true
  if(navigator.geolocation){
    navigator.geolocation.watchPosition(
     async(position) => {
        lat.value = position.coords.latitude
        long.value = position.coords.longitude

        map.value.setView([lat.value, long.value], 13)
        L.marker([lat.value, long.value]).addTo(map.value)
        .bindPopup("You are here!").openPopup();

        const locationName = await reverseGeocode(lat.value, long.value)

        if (locationName) {
            speakLocation(locationName);
          } else {
            speakLocation(lat, long);
          }

          map.value.on('locationerror', (err) => {
        alert(`Error getting location: ${err.message}`);

    
  });
})
  }
  
  
 

 
};

onMounted(() =>{
  map.value = L.map(mapContainer.value).setView([51.505, -0.09], 13);
  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
}).addTo(map.value);
})

</script>
<template>
  <div class="container">
      <div class="row">
          <span class="col w-100 d-flex justiy-content-center">
            <button @click="showMyLocation" class="btn btn-outline-primary mx-auto mt-3" >Locate Me</button>
          </span>
      </div>

      <div class="row container">
        <div  ref="mapContainer" class="col" style="height: 450px; width: 750px; margin: 5% auto;"></div>
      </div>
  </div>
</template>
<style>
   
</style>