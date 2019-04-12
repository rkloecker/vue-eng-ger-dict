<template>
  <div>
    <Navbar
      v-on:list="getNavbarList($event)"
    ></Navbar>
    
    <Form 
      :word="singleWord"
      :isSubmit="submitMode"
      v-on:formSubmit="add($event)"
      v-on:formEdit="edit($event)"
      v-on:handleCancel="enableSubmitMode()"
       v-on:search="searchWord($event)" />
       <div v-if="loading" class="container">app is loading
 <div class="trinity-rings-spinner"> <div class="circle"></div> <div class="circle"></div> <div class="circle"></div> </div>
       </div>
       
  <div  v-if="!error" class="container mt-3 grid-striped">
    <ListHead v-if="!loading"/>
     <ListElement
   v-on:edit="getItem($event)"
   v-on:remove="deleteItem($event)"
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
      loading: true,
      submitMode: true, // if true: edit disabled and vice versa
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
    // if cancel is pressed only toggle if in submitmode
    enableSubmitMode(){
      if(!this.submitMode){
        this.submitMode = true;
      }
    },
    getItems() {
      console.log(process.env.VUE_APP_MY_ENV_VARIABLE)
      console.log(process.env)
      this.loading = true;
      fetch(this.URL)
        .then(res => res.json())
        .then(data => {
            this.wordList = data,
            this.loading = false
        })
        .catch(err => console.log(err));
    },
    getNavbarList(str) {
      fetch(this.URL + str)
        .then(res => res.json())
        .then(data => (this.wordList = data))
        .catch(err => console.log(err));
    },
    getItem(id) {
       this.loading = true;
      this.theId = id;
      this.submitMode = false;
      fetch(this.URL + "/" + id)
        .then(res => res.json())
        .then(data => {
          this.singleWord.english = data.english;
          this.singleWord.german = data.german;
          this.singleWord.description = data.description;
           this.loading = false;
        })
        .catch(err => console.log(err));
    },
    edit(obj) {
      this.submitMode = true;
      // console.log(this.submitMode)
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
/* loading-spinner */
.trinity-rings-spinner, .trinity-rings-spinner * {
      box-sizing: border-box;
    }

    .trinity-rings-spinner {
      height: 66px;
      width: 66px;
      padding: 3px;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: row;
      overflow: hidden;
      box-sizing: border-box;
    }
    .trinity-rings-spinner .circle {
      position:absolute;
      display:block;
      border-radius:50%;
      border: 3px solid #033805;
      opacity: 1;
    }

    .trinity-rings-spinner .circle:nth-child(1) {
      height: 60px;
      width: 60px;
      animation : trinity-rings-spinner-circle1-animation 1.5s infinite linear;
      border-width: 3px;
    }
    .trinity-rings-spinner .circle:nth-child(2) {
      height: calc(60px * 0.65);
      width: calc(60px * 0.65);
      animation : trinity-rings-spinner-circle2-animation 1.5s infinite linear;
      border-width: 2px;
    }
    .trinity-rings-spinner .circle:nth-child(3) {
      height: calc(60px * 0.1);
      width: calc(60px * 0.1);
      animation:trinity-rings-spinner-circle3-animation 1.5s infinite linear;
      border-width: 1px;
    }

    @keyframes trinity-rings-spinner-circle1-animation{
      0% {
        transform: rotateZ(20deg) rotateY(0deg);
      }
      100% {
        transform: rotateZ(100deg) rotateY(360deg);
      }
    }
    @keyframes trinity-rings-spinner-circle2-animation{
      0% {
        transform: rotateZ(100deg) rotateX(0deg);
      }
      100% {
        transform: rotateZ(0deg) rotateX(360deg);
      }
    }
    @keyframes trinity-rings-spinner-circle3-animation{
      0% {
        transform: rotateZ(100deg) rotateX(-360deg);
      }
      100% {
        transform: rotateZ(-360deg) rotateX(360deg);
      }
    }


.error {
  color: #b46868;
  margin-top: 30px;
}
 .grid-striped .row:nth-of-type(odd) {
    background-color: rgba(0, 0, 0, 0.05);
  }
@media screen and (max-width: 350px) {}
</style>
