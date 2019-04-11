<template>
  <div>
    <Navbar
      v-on:list="getNavbarList($event)"
    ></Navbar>
    <a name="example"></a>
    <Form 
      :word="singleWord"
      :smit="enabled"
      :edt="!enabled"
      v-on:formSubmit="add($event)"
      v-on:formEdit="edit($event)"
       v-on:search="searchWord($event)" />
  <div  v-if="!error" class="container mt-3 grid-striped">
    <ListHead />
     <ListElement
   v-on:editme="getItem($event)"
   v-on:deleteme="deleteItem($event)"
    v-for="aword in wordList"
      v-bind:word="aword"
      v-bind:key="aword.id"
      >
    </ListElement>
    </div>

  <p v-if="error" class="error">No data found!</p>
  
  </div>
</template>

<script>
import ListElement from "./ListElement";
import Navbar from "./Navbar";
import ListHead from "./ListHead";
import Form from "./Form";

export default {
  name: "App",
  data() {
    return {
      wordList: [],
      enabled: true,
      error: false,
      theId: "",
      singleWord: {
        english: "",
        german: "",
        description: ""
      },
      herokuUrl: "https://eng-ger-dictio.herokuapp.com/api/words",
      localUrl: "http://localhost:3000/api/words",
      URL: "https://eng-ger-dictio.herokuapp.com/api/words"
    };
  },
  methods: {
    getItems() {
      fetch(this.URL)
        .then(res => res.json())
        .then(data => (this.wordList = data))
        .catch(err => console.log(err));
    },
    getNavbarList(str) {
      fetch(this.URL + str)
        .then(res => res.json())
        .then(data => (this.wordList = data))
        .catch(err => console.log(err));
    },
    getItem(id) {
      this.theId = id;
      this.enabled = false;
      fetch(this.URL + "/" + id)
        .then(res => res.json())
        .then(data => {
          this.singleWord.english = data.english;
          this.singleWord.german = data.german;
          this.singleWord.description = data.description;
        })
        .catch(err => console.log(err));
    },
    edit(obj) {
      this.enabled = true;
      fetch(this.URL + "/" + this.theId, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },
        method: "PUT",
        body: JSON.stringify(obj)
      })
        .then(this.getItems)
        .catch(err => console.log(err));
    },
    add(obj) {
      fetch(this.URL, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },
        method: "POST",
        body: JSON.stringify(obj)
      })
        .then(this.getItems)
        .catch(err => console.log(err));
    },
    deleteItem(id) {
      fetch(this.URL + "/" + id, {
        method: "DELETE"
      })
        .then(this.getItems)
        .catch(err => console.log(err));
    },
    searchWord(o) {
      // console.log(o);
      let qstr = "/?";
      if (o.english) {
        qstr += `english=${o.english}`;
      } else if (o.german) {
        qstr += `german=${o.german}`;
      } else if (o.description) {
        qstr += `description=${o.description}`;
        // for descr other algo: we get an array back
        return (
          fetch(this.URL + qstr)
            .then(response => response.json())
            .then(data => {
              if (!data || data.length == 0) {
                console.log("no data found!");
                this.error = true;
                setTimeout(() => (this.error = false), 2000);
              } else {
                this.wordList = data;
              }
            })
            .catch(err => console.log(err))
        );
      }
      // console.log(qstr);
      fetch(this.URL + qstr)
        .then(response => response.json())
        .then(data => {
          if (!data) {
            console.log("no data found!");
          } else {
            this.wordList = [data];
          }
        })
        .catch(err => console.log(err));
    }
  },
  created() {
    this.getItems();
  },
  components: {
    ListElement,
    Navbar,
    Form,
    ListHead
  }
};
</script>

<style>
.error {
  color: #b46868;
  margin-top: 30px;
}
 .grid-striped .row:nth-of-type(odd) {
    background-color: rgba(0, 0, 0, 0.05);
  }
@media screen and (max-width: 350px) {}
</style>
