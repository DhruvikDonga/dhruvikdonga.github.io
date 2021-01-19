<template>
  <v-app>
    <v-app-bar
      app
      color="dark"
      dark
    >
      <v-row class="text-center"><v-col class="mb-1"><h1>Dhruvik Donga</h1></v-col></v-row>
      
     </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
    <v-footer 
    
    fixed
    >
      
      <h3>
        Connect on:-
        <v-btn
        
        text
        icon
        color="blue lighten-2"
        href="https://www.linkedin.com/in/dhruvik-donga-9363a0172/"
      >
        <v-icon color="primary" dark>mdi-linkedin</v-icon>
        </v-btn>
        <v-btn
        
        text
        icon
        color="blue lighten-2"
        href="https://www.facebook.com/dhruvik.donga/"
      >
        <v-icon color="blue" dark>mdi-facebook</v-icon>
        </v-btn>
        <v-btn
        
        text
        icon
        color="blue lighten-2"
        href="https://github.com/DhruvikDonga"
      >
        <v-icon dark>mdi-github</v-icon>
        </v-btn>
        <v-btn
       
        text
        icon
        color="blue lighten-2"
        href="https://www.reddit.com/user/dhruvik_d"
      >
        <v-icon color="red" dark>mdi-reddit</v-icon>
        </v-btn>
        <v-btn
        
        text
        icon
        color="blue lighten-2"
        href="https://dhruvikdonga.github.io/"
      >
        <v-icon color="primary" dark>mdi-web</v-icon>
        </v-btn>
        
      </h3>
    </v-footer>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
