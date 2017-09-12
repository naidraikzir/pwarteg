<template>
  <section>
    <div ref="map" style="display: none;"></div>
    <transition-group name="fade" mode="out-in" tag="div">
      <loading v-if="isLoading" key="loading"></loading>
      <div key="list" v-if="results.length">
        <h2>Warteg-warteg terdekat:</h2>
        <div v-for="(place, i) of results" :key="i" class="place">
          <div>
            <strong>
              <span>{{ place.name }}</span>
              <a :href="place.dirUrl" target="_blank">
                <navigation-icon></navigation-icon>
              </a>
              <span>{{ place.distanceText }}</span>
            </strong>
          </div>
          <div>{{ place.vicinity }}</div>
        </div>
      </div>
    </transition-group>
  </section>
</template>

<script>
import Loading from './Loading'
import NavigationIcon from './NavigationIcon'

export default {
  components: {
    Loading,
    NavigationIcon
  },

  data () {
    return {
      isLoading: false,
      placesClient: {},
      distanceClient: {},
      currentPos: {},
      results: []
    }
  },

  mounted () {
    if ('geolocation' in navigator) {
      this.getPos()
        .then(this.search)
    } else {
      alert('Geolocation is not supported in your browser');
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
    search ({ latitude, longitude }) {
      let map = new google.maps.Map(this.$refs.map)
      this.currentPos = new google.maps.LatLng(latitude, longitude)
      this.placesClient = new google.maps.places.PlacesService(map)
      this.distanceClient = new google.maps.DistanceMatrixService()

      this.isLoading = true
      this.placesClient.nearbySearch({
        location: {
          lat: latitude,
          lng: longitude
        },
        keyword: 'warteg',
        radius: 5000
      }, (results, status) => {
        this.results = results.map((place) => {
          place.distanceText = ''
          place.distanceValue = 0
          place.dirUrl = 'https://www.google.com/maps/dir/' +
            latitude + '+' + longitude + '/' +
            place.geometry.location.lat() + '+' + place.geometry.location.lng()
          return place
        })
        this.setDistances()
        this.isLoading = false
      })
    },
    setDistances () {
      this.results.map((place) => {
        let destination = new google.maps.LatLng(
          place.geometry.location.lat(),
          place.geometry.location.lng()
        )

        this.distanceClient.getDistanceMatrix({
          origins: [ this.currentPos ],
          destinations: [ destination ],
          travelMode: 'WALKING',
          avoidTolls: true,
        }, (response, status) => {
          let distance = response.rows[0].elements
            .sort((a, b) => {
              return a.distance.value < b.distance.value ? -1 : 1
            })[0].distance
          place.distanceText = distance.text
          place.distanceValue = distance.value
          this.sort()
        })
      })
    },
    sort () {
      this.results.sort((a, b) => a.distanceValue < b.distanceValue ? -1 : 1)
    }
  }
}
</script>

<style scoped>
.place {
  margin-bottom: 1em;
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
</style>
