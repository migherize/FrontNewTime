<template>
  <section class="hero is-dark is-fullheight">
    <!-- Hero head: will stick at the top -->
    <div class="hero-head">
      <header class="navbar">
        <div class="container">
          <div class="navbar-brand">
            <a class="navbar-item">
              <img
                src="../assets/bitcoin.png"
                alt="Logo"
              />
            </a>
            <span class="navbar-burger burger" data-target="navbarMenuHeroC">
              <span></span>
              <span></span>
              <span></span>
            </span>
          </div>
          <div id="navbarMenuHeroC" class="navbar-menu">
            <div class="navbar-end">
              <a class="navbar-item is-active"> Home </a>
              <a class="navbar-item"> Last Update: {{ history() }} </a>
              <span class="navbar-item">
                <a class="button is-success is-inverted" @click="onSubmit">
                  <span class="icon">
                    <i class="fab fa-github"></i>
                  </span>
                  <span>Update</span>
                </a>
              </span>
            </div>
          </div>
        </div>
      </header>
    </div>

    <!-- Hero content: will be in the middle -->
    <div class="hero-body">
      <div class="columns">
        <div class="column is-two-thirds-desktop is-four-fifths-mobile is-four-fifths-tablet">
          <div v-for="(item, index) in myJson" :key="index">
            <div class="card">
                <header class="card-header">
                <p class="subtitle is-6" id="hours">{{ change_date(item.date) }} hours</p>
                <p class="card-header-title"> {{ item.title }}</p>
                <button class="card-header-icon" aria-label="more options" @click="muestra_oculta('contenido'+index)">
                    <span class="icon">
                    <i class="fas fa-angle-down" aria-hidden="true"></i>
                    </span>
                </button>
                </header>
                <div class="card-content" :id="'contenido'+index">
                <div class="content">
                    {{ item.description }}
                    <a v-bind:href="item.link" class="link">{{ change_link(item.link)}}</a>

                </div>
                </div>
            </div>
          </div>
        </div>
        <div class="column" id="col2">
        </div>
        <div class="column" id="col2"></div>
      </div>
    </div>

    <!-- Hero footer: will stick at the bottom -->
  </section>
</template>

<script>
import { mapState } from "vuex";
import json from './../data/items.json'

export default {
  name: "NewCard",
  data() {
      return {
          timer: '',
          item: [],
          myJson: json
      }
  },
  computed: {
    ...mapState(["news"]),
    numero(){
        return this.$store.state.news
    }
  },
  methods: {
    muestra_oculta: function(id) {
        if (document.getElementById){ //se obtiene el id
            var el = document.getElementById(id); //se define la variable "el" igual a nuestro div
            el.style.display = (el.style.display == 'none') ? '' : 'none'; //damos un atributo display:none que oculta el div
        }
    },
    onSubmit () {
        location.reload()
    },
    update () {
        this.timer = setInterval(function(){
        location.reload()
    }, 3600000); // 10 seg
    },
    cover () {
        //const data = JSON.parse(JSON.stringify(this.$store.state.news));
        //console.log("tamaño:",data.length);
        for (let index = 0; index < this.myJson.length; index++) {
            this.muestra_oculta("contenido"+index);
        }
    },
    show () {
        console.log("Noticias:",this.myJson.length);
        //this.myJson.forEach(obj => {
        //Object.entries(obj).forEach(([key, value]) => {
        //    console.log(`${key} ${value}`);
        //});
        //console.log('-------------------');
        //});
    },
    change_date (f) {
        var separador = ' ';
        // fecha de noticia.
        var separar = f.split(separador);
        var d1 = new Date(separar[0]+', '+separar[1]);

        // fecha de hoy
        var d2 = new Date();

        // fecha diferencia
        var d3 = new Date(d2.getTime()-d1.getTime());
        var d4 = Math.round(d3 / 3600000);
        return d4
    },
    change_link(l){
        const regexp = 'https://[0-9A-Za-z.,]+';
        const array = [...l.matchAll(regexp)];
        var host = array[0].toString().replace('https://','')
        //console.log("array",array[0].toString().replace('https://',''));
        return host
    },
    history(){
      //console.log("this.myJson[0]",this.myJson[0]['history']);
      var f = this.myJson[0]['history']
      return f
    }
  },
  mounted() {
    //this.cover();
    this.update();
    this.show();
    (function(){
      console.log("hole");
      var burger = document.querySelector('.burger');
      var nav = document.querySelector('#'+burger.dataset.target);

      burger.addEventListener('click', function(){
      burger.classList.toggle('is-active');
      nav.classList.toggle('is-active');
      });
    })(); 
  },
};
</script>

<style>
.card {
  font-family: Helvetica, Arial, sans-serif;
  background: #202328;
  color: white;
  margin-top: 20px;
}
.card-header-title {
  color: white;
}
.content {
    text-align: justify;
}
.card-header-title {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}
#hours {
    padding-top: 20px;
    padding-bottom: 0px;
    color: yellowgreen;
    width: 100px !important;
    text-align: left;
}
.card-header-icon {
    padding: 0.25rem;
    height: 30px;
    margin: auto;
}
.link {
    padding-left: 10px;
    color: blue !important;
    
}
html {
  background-color: #363636;
  color: #f5f5f5;
}

@media screen and (min-width: 1000px) and (max-width: 1200px) {
  .card {
    width: 1500px;
  }
}

@media screen and (min-width: 780px) and (max-width: 1000px) {
  .hero {
    width: 1000px;
  }
  .navbar-burger {
    margin-left: 0px;
  }
  .card {
    width: 1200px;
  }
  .navbar-end .navbar-item {
    width: 500px;
  }
  #col2 {
    display: none;
  }
}
@media screen and (min-width: 660px) and (max-width: 780px) {
  .hero {
    width: 1000px;
  }
  .navbar-burger {
    margin-left: 0px;
  }
  .card {
    width: 800px;
  }
  .navbar-end .navbar-item {
    width: 300px;
  }
  #col2 {
    display: none;
  }
}

@media screen and (min-width: 400px) and (max-width: 660px) {
  .hero {
    width: 900px;
  }
  .navbar-burger {
    margin-left: 0px;
  }
  .card {
    width: 800px;
  }
  .navbar-end .navbar-item {
    width: 300px;
  }
  #col2 {
    display: none;
  }
}
@media screen and (min-width: 0px) and (max-width: 400px) {
  .hero {
    width: 900px;
  }
  .navbar-burger {
    margin-left: 0px;
  }
  .card {
    width: 600px;
  }
  #col2 {
    display: none;
  }
}

</style>