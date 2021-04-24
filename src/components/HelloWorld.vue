<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <span>Emails: {{emails}}</span>
    <MultiEmail v-model="emails"/>
  </div>
</template>

<script>
import MultiEmail from "./MultiEmail";
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: () => ({
    emails: "uno@casa.cu"
  }),
  components: {
    MultiEmail
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
