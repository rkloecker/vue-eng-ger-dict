<template>
<div class="row mb-2">
   <div class="col col-xs-3">	{{word.english | uppercase}} </div>
   <div class="col col-xs-3">	{{word.german | foo}} </div>
   <div class="col col-xs-3">		{{word.description}} </div>
   <div class="col col-xs-3">
     <button v-on:click="edit(word._id)" class="btn btn-success">
        <i class="far fa-sm">&#xf044;</i>
        </button>
   
			<button v-on:click="deleteMe(word._id)" class="btn btn-danger addpadding">
        <i class="far fa-sm">&#xf2ed;</i>
      </button>
		</div>
    	</div>
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
      this.$emit("editme", value);
      window.location.hash = "example";
      window.location.hash = null;
    },
    deleteMe: function(value) {
      this.$emit("deleteme", value);
    }
  }
};
</script>

<style scoped>
@media screen and (max-width: 540px) {
 
  .addpadding {
    padding-right: 9px !important;
    padding-right: 15.5px !important;
    margin-top: 2px !important;
  }
}
</style>
