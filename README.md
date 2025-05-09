# Locate-Me

This project is a web application built using Vue 3 and Vite, designed to help users easily identify their current geographical location. Leveraging the power of Leaflet for interactive maps, Nominatim for reverse geocoding, and Bootstrap for a responsive and user-friendly interface, Locate-Me provides a seamless experience for discovering where you are.

## Key Features

* **Real-time Location Tracking:** Utilizes the browser's geolocation API to pinpoint the user's current position on a map.
* **Interactive Map:** Implements Leaflet, a popular open-source JavaScript library for mobile-friendly interactive maps. Users can zoom, pan, and explore the map.
* **Reverse Geocoding with Nominatim:** Integrates Nominatim, an open-source geocoding service based on OpenStreetMap data, to translate geographic coordinates (latitude and longitude) into human-readable addresses or place names. This allows the application to tell you your current location in a more understandable way (e.g., "You are approximately at the University of Burundi campus Kiriri").
* **User-Friendly Interface:** Styled with Bootstrap, a widely used CSS framework, ensuring a clean, responsive, and consistent design across various devices.
* **Text-to-Speech Feedback:** The application can audibly announce your current location using the browser's Web Speech API, providing an accessible way to know where you are without needing to look at the screen.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize Configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
