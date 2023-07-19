<template>
  <div class="card">
    <div class="image" v-if="loaded">
      <img :src="imageUrl" :alt="item.name" />
    </div>
    <div class="image" v-else>
      <img src="/placeholder.jpg" alt="placeholder" />
    </div>
    <div class="card-text">
      <h3>{{ item.name ? item.name : "Not Available" }}</h3>
      <div>
        <h4>{{ item.countryCode ? item.countryCode : "Not Available" }}</h4>
        <h4>{{ item.launchDate ? item.launchDate : "Not Available" }}</h4>
        <h4>{{ item.objectType ? item.objectType : "Not Available" }}</h4>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 100%;
  max-width: 500px;
  margin-bottom: 20px;
  background: #333333;
  position: relative;
  border: 2px solid #555;
}
.image img {
  width: 100%;
  aspect-ratio: 1/1;
  object-fit: cover;
}
.card-text {
  background: #00000030;
  backdrop-filter: blur(5px);
  position: absolute;
  bottom: 0px;
  left: 0px;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
}
.card-text h3 {
  color: #de4c6b;
  font-weight: 800;
}

@media only screen and (min-width: 425px) {
  .card-text h3 {
    color: #de4c6b;
    font-weight: 800;
    font-size: 24px;
  }
  .card-text {
    font-size: 20px;
  }
}

@media only screen and (min-width: 500px) {
  .card {
    width: 100%;
    margin: 0 auto 20px auto;
    display: flex;
    align-items: center;
  }
  .image {
    width: 60%;
    aspect-ratio: 1/1;
  }
  .card-text {
    background: none;
    backdrop-filter: blur(0px);
    position: static;
    bottom: 0px;
    left: 0px;
    height: 100%;
    width: 100%;
    display: block;
    padding: 10px;
    font-size: 14px;
  }
  .card-text h3 {
    font-size: 18px;
  }
}
@media only screen and (min-width: 600px) {
  .card-text h3 {
    font-size: 22px;
  }
  .card-text {
    font-size: 18px;
  }
}
@media only screen and (max-width: 500px) {
  .card-text {
    background: #00000070;
  }
}
</style>

<script>
import axios from "axios";

export default {
  name: "SatelliteCard",
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      loaded: false,
      imageUrl: "",
    };
  },
  async mounted() {
    await this.getSatelliteImage();
    this.loaded = true;
  },
  methods: {
    async getSatelliteImage() {
      const launchYear = new Date(this.item.launchDate).getFullYear();
      try {
        if (launchYear > 1998) {
          const response = await axios.get(
            "https://api.nasa.gov/planetary/apod",
            {
              params: {
                date: this.item.launchDate,
                hd: false,
                api_key: "tc9ACYKAYtbyFMzr5quhz0SY422hB3tpH7fldRqM", // Replace with your NASA API key
              },
            }
          );
          this.imageUrl = response.data.hdurl;
        } else {
          this.imageUrl = "/placeholder.jpg";
        }
      } catch (error) {
        this.imageUrl = "/placeholder.jpg";
        console.error(error);
      }
    },
  },
};
</script>
