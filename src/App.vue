<template>
  <main>

    <div class="heading">
      <h1>Ethernal Scroll</h1>
      <h2>A simple infinite scroll example using Vue.js</h2>
      <h4>Pages loaded: {{ page_count }}</h4>
    </div>

    <div class="container" id="app">
      <grid-container id="infinite-list">

        <transition name="fade">
          <div class="loading" v-show="loading">
            Loading next batch
          </div>
        </transition>

<!--        <grid-item style="scroll-snap-align:end" :class="item.size" v-for="item in items">-->
        <grid-item style="scroll-snap-align:center;text-align: center" :class="item.size" v-for="item in items">
          <p><em>Name</em>: {{ item.name }}<br>
            <em>Date of birth</em>: {{ item.dob }}<br>
            <em>Place of birth</em>: {{ item.location }}</p>
        </grid-item>
      </grid-container>
    </div>

  </main>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      loading: false,
      isBottom: false,
      items: [],
      page_count: 0,
      sizes: ["tall", "taller", "tallest"]
    };
  },
  methods: {
    getNextBatch(size) {
      this.loading = true;
      if(Math.floor(Math.random() * 5)%2==0){


        axios.get("https://random-data-api.com/api/v2/users?size=" + size + "&response_type=json").then((response) => {

          response.data.map((x) => {
            const new_item = {
              name: x.first_name + " " + x.last_name,
              dob: x.date_of_birth,
              location: x.address.state,
              // size: this.sizes[Math.floor(Math.random() * this.sizes.length)]
              size: this.sizes[2]
            };
            this.items.push(new_item)
          })
          this.loading = false;
          this.isBottom = false
          this.page_count++;
          return false
        });
      }else{

        setTimeout(()=>{
          this.loading = false;
          console.log("not found data")
        },500)
        return true


      }

    }
  },
  beforeMount() {
    this.getNextBatch(30);
  },
  mounted() {
    const masonry = document.querySelector('#infinite-list');

    document.title = "Ethrnal Scroll Demo";


    masonry.addEventListener('scroll', e => {
      if (masonry.scrollTop + masonry.clientHeight >= masonry.scrollHeight) {
        // this.getNextBatch(30);
        // this.getNextBatch(5);
        this.isBottom=true
      }
    })

    function debounce(func, timeout = 300){
      let timer;
      return (...args) => {
        clearTimeout(timer);
        timer = setTimeout(() => { func.apply(this, args); }, timeout);
      };
    }



    masonry.addEventListener('wheel', debounce((e)=>{

      // console.log(`${e.deltaY}   `)
      // let scrollDistance= Math.abs(e.wheelDelta || -e.deltaY * 40)
      if(this.isBottom){
        this.loading=true
        console.log("触发查询")

        setTimeout(()=>{
          let empty = this.getNextBatch(10);
          if(empty){
            alert("暂无最新数据！")
          }
          this.loading=false

        }, 100)
        // isBottom=false
      }

    }))



  },
};
</script>

<style>
body {
  background-color: #ffffff;
  padding: 50px;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif
}

.container {
  padding: 40px 80px 15px 80px;
  background-color: #fff;
  border-radius: 8px;
  max-width: 800px;
  margin: auto;
}

.heading {
  text-align: center;
}

.heading h1 {
  text-align: center;
  margin: 0 0 5px 0;
  font-weight: 900;
  font-size: 4rem;
  color: #333;
}

.heading h4 {
  color: #555;
  text-align: center;
  margin: 0 0 35px 0;
  font-weight: 400;
  font-size: 24px;
}

.loading {
  text-align: center;
  position: absolute;
  color: #fff;
  z-index: 100;
  background: #0dd85b;
  padding: 8px 18px;
  border-radius: 5px;
  left: calc(50% - 45px);
  top: calc(50% - 18px);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

grid-container {
  display: grid;
  /* 1 */
  grid-auto-rows: 50px;
  /* 2 */
  grid-gap: 10px;
  /* 3 */
  grid-template-columns: repeat(auto-fill, minmax(30%, 1fr));
  /* 4 */
  overflow: auto;
  height: 30vh;
  border: 2px solid;
  border-radius: 5px;
  text-align: center;
  margin: 0 0 35px 0;
  scroll-snap-type: both mandatory;
  overscroll-behavior-y: contain;
  padding-bottom: 20px;
}

.tall {
  grid-row: span 2;
  background-color: #7632ed;
}

.taller {
  grid-row: span 3;
  background-color: #925cef;
}

.tallest {
  grid-row: span 4;
  background-color: #b9a3de;
}
</style>


