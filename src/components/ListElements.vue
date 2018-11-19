<template>
   <li class="list-group-item mb-2">{{ word.english }} - {{ word.german }} - {{ word.description }}</li>
</template>

<script>
export default {
  props: ['word'],
  data () {
    return {
      msg: 'Welcome App'
    }
  }
}
</script>

<style scoped>
/* .active{
  color:white;
} */
h1, h2 {
  font-weight: normal;
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
