<template>
  <div>
    <form @submit.prevent="validateForm">
      <div class="form-control">
        <label for="a-point-latitude">A point latitude</label>
        <input
          id="a-point-latitude"
          name="a-point-latitude"
          type="text"
          v-model="Alat"
          required
        />
      </div>
      <div class="form-control">
        <label for="a-point-longtitude">A point longtitude</label>
        <input
          id="a-point-longtitude"
          name="a-point-longtitude"
          type="text"
          v-model="Alon"
          required
        />
      </div>
      <hr />
      <div class="form-control">
        <label for="b-point-latitude">B point latitude</label>
        <input
          id="b-point-latitude"
          name="b-point-latitude"
          type="text"
          v-model="Blat"
          required
        />
      </div>
      <div class="form-control">
        <label for="b-point-longtitude">B point longtitude</label>
        <input
          id="b-point-longtitude"
          name="b-point-longtitude"
          type="text"
          v-model="Blon"
          required
        />
      </div>
      <div>
        <button>Calculate distance</button>
      </div>
    </form>
    <the-result
      v-if="resultDistancekm != 0 && resultDistancekm != -1"
      :result-distancekm="resultDistancekm"
    ></the-result>
    <the-error v-else-if="resultDistancekm === -1"></the-error>
  </div>
</template>

<script>
import TheError from "./TheError.vue";
import TheResult from "./TheResult.vue";
export default {
  components: {
    TheResult,
    TheError,
  },
  data() {
    return {
      Alat: "",
      Alon: "",
      Blat: "",
      Blon: "",
      resultDistancekm: 0,
      APIkey: "",
    };
  },
  methods: {
    validateForm() {
      if (
        this.Alat >= -90 &&
        this.Alat <= 90 &&
        this.Blat >= -90 &&
        this.Blat <= 90
      ) {
        if (
          this.Alon >= -180 &&
          this.Alon <= 180 &&
          this.Blon >= -180 &&
          this.Blon <= 180
        ) {
          this.calculateDistance();
        } else {
          alert("Error in Longtitude");
        }
      } else {
        alert("Error in Latitude");
      }
    },
    calculateDistance() {
      const requestOptions = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Content-Length": "450",
        },
        body: JSON.stringify({
          origins: [
            {
              latitude: this.Alat,
              longitude: this.Alon,
            },
          ],
          destinations: [
            {
              latitude: this.Blat,
              longitude: this.Blon,
            },
          ],
          travelMode: "driving",
        }),
      };
      fetch(
        "https://dev.virtualearth.net/REST/v1/Routes/DistanceMatrix?key=" +
          this.APIkey,
        requestOptions
      )
        .then((response) => response.json())
        .then((data) => {
          this.resultDistancekm =
            data.resourceSets[0].resources[0].results[0].travelDistance;
        });
    },
  },
};
</script>

<style scoped>
form {
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 2rem;
  background-color: #ffffff;
}

.form-control {
  margin: 0.5rem 0;
}

.form-control.invalid input {
  border-color: red;
}

.form-control.invalid label {
  color: red;
}

label {
  font-weight: bold;
}

h2 {
  font-size: 1rem;
  margin: 0.5rem 0;
}

input,
select {
  display: block;
  width: 100%;
  font: inherit;
  margin-top: 0.5rem;
}

select {
  width: auto;
}

input[type="checkbox"],
input[type="radio"] {
  display: inline-block;
  width: auto;
  margin-right: 1rem;
}

input[type="checkbox"] + label,
input[type="radio"] + label {
  font-weight: normal;
}

button {
  font: inherit;
  border: 1px solid #0076bb;
  background-color: #0076bb;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  border-radius: 30px;
}

button:hover,
button:active {
  border-color: #002350;
  background-color: #002350;
}

hr {
  margin-top: 2rem;
  margin-bottom: 2rem;
}
</style>