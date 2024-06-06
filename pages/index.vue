<template>
  <div class="container">
    <button @click="getLocation">
      <span class="text-field-title">Get Location</span>
    </button>
    <div>
      <span class="text-field-title">Location Object:</span>
      <p>{{ location }}</p>
    </div>
    <div>
      <span class="text-field-title">Latitude Only:</span>
      <p>{{ latitude }}</p>
    </div>
    <div>
      <span class="text-field-title">Longitude Only:</span>
      <p>{{ longitude }}</p>
    </div>
    <div>
      <span class="text-error">{{ errorMessage }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: {},
      latitude: "",
      longitude: "",
      errorMessage: ""
    };
  },
  methods: {
    getLocation() {
      if (!navigator.geolocation) {
        this.errorMessage = "Geolocation is not supported by your browser";
        return;
      }

      navigator.geolocation.getCurrentPosition(
        position => {
          this.location = position;
          this.latitude = position.coords.latitude;
          this.longitude = position.coords.longitude;
          this.errorMessage = "";
        },
        error => {
          switch (error.code) {
            case error.PERMISSION_DENIED:
              this.errorMessage = "User denied the request for Geolocation.";
              break;
            case error.POSITION_UNAVAILABLE:
              this.errorMessage = "Location information is unavailable.";
              break;
            case error.TIMEOUT:
              this.errorMessage = "The request to get user location timed out.";
              break;
            case error.UNKNOWN_ERROR:
              this.errorMessage = "An unknown error occurred.";
              break;
          }
        }
      );
    }
  }
};
</script>

<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  display: flex;
  flex-direction: column;
  height: 100vh;
}
.text-field-title {
  font-weight: bold;
}
.text-error {
  color: red;
}
</style>
