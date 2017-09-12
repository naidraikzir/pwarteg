<template>
  <div>
    <transition-group name="fade" mode="out-in">
      <div class="loading-wrapper" v-if="isLoading" key="loading">
        <div class="loading"></div>
      </div>
      <div key="list" v-if="results.length">
        <h2>Warteg-warteg terdekat:</h2>
        <div v-for="(venue, i) of results" :key="i" class="venue">
          <div>
            <strong>
              {{ venue.name }}
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill="none"
                stroke="#41b883"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round">
                  <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                  <circle cx="12" cy="10" r="3"/>
              </svg>
              {{ venue.location.distance }}
            </strong>
          </div>
          <div>{{ venue.location.formattedAddress.join(' ') }}</div>
        </div>
      </div>
    </transition-group>
<!-- 
    <pre>Location: {{ results }}</pre>
    <div>Alpha: {{ alpha }}</div>
    <div>Beta: {{ beta }}</div>
    <div>Gamma: {{ gamma }}</div>
    <div>Compass Heading: {{ compassHeading }}</div>
-->
  </div>
</template>

<script>
import fq from '../fq'

export default {
  data () {
    return {
      isLoading: false,
      results: [],
      alpha: '',
      beta: '',
      gamma: ''
    }
  },

  computed: {
    compassHeading () {
      // Convert degrees to radians
      var alphaRad = this.alpha * (Math.PI / 180);
      var betaRad = this.beta * (Math.PI / 180);
      var gammaRad = this.gamma * (Math.PI / 180);

      // Calculate equation components
      var cA = Math.cos(alphaRad);
      var sA = Math.sin(alphaRad);
      var cB = Math.cos(betaRad);
      var sB = Math.sin(betaRad);
      var cG = Math.cos(gammaRad);
      var sG = Math.sin(gammaRad);

      // Calculate A, B, C rotation components
      var rA = - cA * sG - sA * sB * cG;
      var rB = - sA * sG + cA * sB * cG;
      var rC = - cB * cG;

      // Calculate compass heading
      var compassHeading = Math.atan(rA / rB);

      // Convert from half unit circle to whole unit circle
      if (rB < 0) {
        compassHeading += Math.PI;
      } else if (rA < 0) {
        compassHeading += 2 * Math.PI;
      }

      // Convert radians to degrees
      compassHeading *= 180 / Math.PI;

      return compassHeading;
    }
  },

  mounted () {
    if ('geolocation' in navigator) {
      this.getPos()
        .then(this.search)
    } else {
      alert('Geolocation is not supported in your browser');
    }

    if (window.DeviceOrientationEvent) {
      window.addEventListener('deviceorientation', ({ alpha, beta, gamma }) => {
        this.alpha = alpha
        this.beta = beta
        this.gamma = gamma
      })
    }
  },

  methods: {
    getPos () {
      return new Promise((resolve, reject) => {
        this.isLoading = true
        navigator.geolocation.watchPosition(({ coords }) => {
          resolve(coords)
          this.isLoading = false
        })
      })
    },
    search (coords) {
      this.isLoading = true
      fetch(
        'https://api.foursquare.com/v2/venues/search?' +
        'v=20170911&ll=' + coords.latitude + '%2C%20' + coords.longitude + '&' +
        'query=warteg&' +
        'intent=checkin&' +
        'client_id=' + fq.client_id + '&' +
        'client_secret=' + fq.client_secret
      ).then((response) => {
        response.json()
          .then(({ response }) => {
            this.results = response.venues.map(
              ({ id, name, location }) => ({ id, name, location })
            ).sort((a, b) => a.location.distance < b.location.distance ? -1 : 1)
              .map((venue) => {
                let distance = venue.location.distance
                venue.location.distance = distance > 1000 ?
                  (distance / 1000).toFixed(2) + 'km' :
                  distance + 'm'
                return venue
              })
            this.isLoading = false
          })
      })
    },
  }
}
</script>

<style scoped>
.venue {
  margin-bottom: 1em;
}

.loading {
  animation: rotate 1s infinite;
  border: solid #444;
  border-width: 2px 2px 2px 0;
  border-radius: 50%;
  width: 3em;
  height: 3em;
  margin: auto;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0);
}

.fade-enter-to,
.fade-leave {
  opacity: 1;
  transform: scaleY(1);
}

.fade-enter-active,
.fade-leave-active {
  transition: 0.3s;
}

@keyframes rotate {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
