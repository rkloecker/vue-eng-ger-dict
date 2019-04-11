<template>
  
  <div class="container mt-3">
     <a name="example"></a>
    <div class="row ">
      <div class="col-md-12">

        <div class="panel panel-success">
          <div class="panel-heading">
            <h3 class="panel-title">Add Word to Dictionary</h3>
          </div>
          <div class="panel-body">
            <div class="row">
              <div class="col-md-12">
                <form id="sub">

                  <div class="form-group">
                    <label>English</label>
                    <!-- <input id="eng" type="text" :value="word.english" v-on:change="word.english = $event.target.value" class="form-control" > -->
                    <input id="eng" type="text" v-model="word.english" class="form-control" >
                  </div>
                  <div class="form-group">
                    <label>German</label>
                    <input id="deu" type="text"  v-model="word.german"  class="form-control" >
                  </div>
                  <div class="form-group">
                    <label>Description</label>
                    <input id="desc" type="text"  v-model="word.description"  class="form-control" >
                  </div>

                  <div id="buttons">
                    <button v-if="smit" id="btnSubmit" v-on:click.prevent="formSubmit" class="btn btn-success">Submit</button>
                    <button v-if="edt" v-on:click.prevent="formEdit" id="btnEdit" class="btn btn-danger">Edit</button>
                    <button id="btnQuery" class="btn btn-primary" v-on:click.prevent="search">Search</button>
                    <button id="btnCancel" class="btn btn-info" v-on:click.prevent="clearfields">Cancel</button>
                  </div>
                  <p v-if="!isValid()  && touchedSub" class="error">{{errmsg}}</p>
                  <p v-if="!isValidSearch()  && touchedSearch " class="error">{{errmsg}}</p>
                  <p v-if="!validateInput()" class="error">only letters</p>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- /.container -->
</template>

<script>
export default {
  props: ["word", "smit", "edt"],
  data() {
    return {
      add: true,
      edit: false,
      valid: false,
      searchValid: false,
      touchedSub: false,
      touchedSearch: false,
      errmsg: ""
    };
  },
  methods: {
    formSubmit() {
      if (this.isValid() && this.validateInput()) {
        this.$emit("formSubmit", this.setWord());
        this.clearfields();
        this.touchedSub = false;
      } else {
        this.touchedSub = true;
        this.errmsg = "Please fill out the complete form, use only letters and -";
        setTimeout(() => (this.touchedSub = false), 2000);
      }
    },
    formEdit() {
      if (this.isValid() && this.validateInput()) {
        this.$emit("formEdit", this.setWord());
        this.clearfields();
        this.touchedSub = false;
      } else {
        this.touchedSub = true;
        this.errmsg = "Please fill out the complete form, use only letters and -";
        setTimeout(() => (this.touchedSub = false), 2000);
      }
    },
    search() {
      if (this.isValidSearch()) {
        this.$emit("search", this.setWord());
        this.clearfields();
        this.touchedSearch = false;
      } else {
        this.touchedSearch = true;
        this.errmsg = "Please fill out one field";
        setTimeout(() => (this.touchedSearch = false), 2000);
      }
    },
    setWord() {
      return {
        english: this.word.english,
        german: this.word.german,
        description: this.word.description
      };
    },
    clearfields() {
      this.word.english = "";
      this.word.german = "";
      this.word.description = "";
    },
    isValidSearch() {
      return this.word.english || this.word.german || this.word.description;
    },
    isValid() {
      return this.word.english && this.word.german && this.word.description;
    },
    isValidWord(str) {
      return !/[^a-zäöüß,-\s]/i.test(str);
    },
    validateInput(){
      return this.isValidWord(this.word.english)  && this.isValidWord(this.word.german) && this.isValidWord(this.word.description);
    }
  }
};
</script>
