<template>
  <div id="app">
    <!-- <h1>Vue Tree Browser</h1> -->
    <TreeBrowser :nodes="root" @onClick="nodeWasClicked" />
  </div>
</template>

<script>
import TreeBrowser from "./components/TreeBrowser.vue";
//import rootData from "./root.json";

export default {
  name: "app",
  created() {
    this.loadData();
  },
  data() {
    return {
      root: "",
    };
  },
  watch: {
    root: {
      deep: true,
      handler: (newRoot) => {
        console.log(newRoot);
      },
    },
  },
  methods: {
    nodeWasClicked(node) {
      //alert(node.name);
    },
    async loadData() {
      // to fetch the data from the api
      return await fetch("http://localhost:3000/data/")
        .then((res) => {
          // a non-200 response code
          if (!res.ok) {
            // create error instance with HTTP status text
            const error = new Error(res.statusText);
            error.json = res.json();
            throw error;
          }

          return res.json();
        })
        .then((json) => {
          // set the response data

          this.root = json;
        })
        .catch((err) => {
          this.error.value = err;
          // In case a custom JSON error response was provided
          if (err.json) {
            return err.json.then((json) => {
              // set the JSON response message
              this.error.value.message = json.message;
            });
          }
        });
    },
  },
  components: {
    TreeBrowser,
  },
};
</script>

<style>
body {
  background-color: #333;
  color: white;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
