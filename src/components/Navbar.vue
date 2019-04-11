<template>
     <nav class="navbar navbar-expand-sm navbar-dark bg-dark">
      <a class="navbar-brand" href="#">Dictionary</a>
     
      <div class="" id="navbarsExampleDefault">
        <ul class="navbar-nav">
         
          <li class="nav-item">
            <a class="nav-link" href="#" @click="list('description')">Description</a>
          </li>
           <li class="nav-item">
            <a class="nav-link" href="#" @click="list('english')">English</a>
          </li>
           <li class="nav-item">
            <a class="nav-link" href="#" @click="list('german')">Deutsch</a>
          </li>
           <li class="nav-item">
           <a   class="nav-link" href="#">
              <span @click="changeDirection('asc')" v-bind:class="{ active: isActive }">ASC</span>&nbsp;
              <span @click="changeDirection('desc')" v-bind:class="{ active: !isActive }">DESC</span>
            </a>
          </li>
          
        </ul>
      </div>
    </nav>
</template>

<script>
export default {
  data() {
    return {
     direction: 'asc',
     isActive: true // or desc
    };
  },
  methods: {
    list(type) {
      const querystr = `/?sort=${type}:${this.direction}`;
      this.$emit("list",querystr);
    },
    changeDirection(dir){
      this.direction = dir;
      this.isActive = !this.isActive;
    }
  }
};
</script>

<style>
a span{
  color: #677;
}
.active{
 color: white;
}
</style>
