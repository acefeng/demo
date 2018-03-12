<template>
  <div id="app">
    <div id="od1">{{msg}}</div>
    <div id="od2">{{msg1}}</div>
    <button @click="ineer()">出现代码</button>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      msg1: ''
    }
  },
  methods:{
    getJSON(url){
      var promise = new Promise((resolve, reject) => {
        var client = new XMLHttpRequest();
        client.open("GET", url,true);
        client.onreadystatechange = handler;
        client.send(null);

        function handler() {
          if(this.readyState==4){
            if (this.status === 200) {
              resolve(this.responseText);
            } else {
              reject(new Error(this.statusText));
            }
          }
          
        };
      });
      return promise;
    },
    ineer(){
      this.getJSON("src/1.txt").then((json) => {
        this.msg=json;
        return json
      }).then((json) => {
        this.getJSON("src/2.txt").then((json1) => {
          this.msg1 = json1+json;
        }).catch((error) => {
          console.error('出错了',error);
        })
      }).catch((error) => {
        console.error('出错了', error);
      });
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
