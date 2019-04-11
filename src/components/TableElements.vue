<template>
   <tr>
			<td>{{word.english | uppercase}}</td>
			<td>{{word.german | join}}</td>
			<td>{{word.description}}</td>
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
    join: function(value) {
      return value.join(",");
    }
  },
  methods: {
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
@media screen and (max-width: 560px) {}
</style>
