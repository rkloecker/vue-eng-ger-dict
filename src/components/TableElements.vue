<template>
   <tr>
			<td>{{word.english | uppercase}}</td>
			<td>{{word.german | foo}}</td>
			<td>{{bar(word.description)}}</td>
			<td><div class="edit_delete" :id="word._id">
        <button v-on:click="edit(word._id)" class="btn btn-success">
        <i class="far fa-sm">&#xf044;</i>
        </button>
			<button v-on:click="deleteMe(word._id)" class="btn btn-danger">
        <i class="far fa-sm">&#xf2ed;</i>
      </button></div></td>
		</tr>
</template>

<script>
export default {
  props: ["word"],
  data() {
    return {
      msg: "Welcome App"
    };
  },
  filters: {
    uppercase: function(value) {
      // return value.toUpperCase();
      return value;
    },
    foo: function(value) {
      return value.join(",");
    }
  },
  methods: {
    bar(x) {
      // return x.toUpperCase();
      return x;
    },
    edit: function(value) {
       this.$emit('editme', value);
       	window.location.hash = 'example';
       	window.location.hash = null;
    },
    deleteMe: function(value) {
       this.$emit('deleteme', value);
       	
    }
  }
};
</script>

<style scoped>
@media screen and (max-width: 560px) {
  /* button {
   width: 50% !important;
   
  }
   i {
   width: 30% !important;
   height: 30% !important;
  }
  .edit_delete{
     width: 50% !important;
  } */
  
}
</style>
