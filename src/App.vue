<template>
  <v-app>
    <v-main>
      <Mandadito />
    </v-main>
    <v-footer dark padless>
      <v-card class="flex" flat tile>
        <v-card-text
          class="py-2 white--text text-center d-flex flex-column justify-center"
        >
          <span>
            {{ new Date().getFullYear() }} — <strong>Ridery</strong>
          </span>
          <span class="d-flex justify-center" >
            <v-img src="loguito.png" alt="loguito" max-height="25" max-width="25" />
          </span>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>
import Mandadito from "./components/Mandadito";

export default {
  name: "App",

  components: {
    Mandadito,
  },

  data: () => ({
    //
  }),
};
</script>
