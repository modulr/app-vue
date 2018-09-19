<template>
  <div>
    <Header></Header>
    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <h1>This is an dashboard page</h1>
        </v-layout>
      </v-container>
    </v-content>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/partials/Header.vue'

export default {
  name: 'dashboard',
  components: {
    Header
  }
}
</script>
