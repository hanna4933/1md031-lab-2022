<template>
  <div>
    
    
  
  
    <header class="header">
       
       <img src="https://snaped.fns.usda.gov/sites/default/files/styles/crop_ratio_7_5/public/2021-04/herbs.jpg?itok=g_UukhUg" class="header_img" id="headerpic">
      
       <h1 id="headertext">Välkommen till Green Burger </h1>
       
      
   </header>
    <section id="bm">
        <h3> Vår meny </h3>
        <p> Vår meny består av vegetariska och ekologiska produkter, för att du och vår planet ska må bra.</p>

    <div class="burgarmeny">
      
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"
              />

        
    </div>
        <p> * Alla ingridenser är ekologiska <br>
            ** Kan ej ätas</p>

    </section>
    <section id="info">
        <h3> Leveransadress </h3>
        <p> För tillfället leverar vi enbart med cykelbud och räckvidden är därmed begränsad.</p>
        <p>
            <label for="namn">Namn </label><br>
            <input type="text" id="namn" v-model="n" required="required" placeholder="För- och Efternamn">
        </p>
        

        <p>
            <label for="email">E-mail </label><br>
            <input type="email" id="email" v-model="em" required="required" placeholder="E-mail adress">
        </p>
       <p>Markera önskad leveransadress</p> 
        <div id="fitMap">
      <div id="map" v-on:click="setLocation" v-bind:style="{background: 'url(' + require('../../public/img/polacks.jpg')+')'}">
        <div v-bind:style="{left: location.x +'px', top: location.y +'px'}" v-bind:key="'dots'+key">
          T
        </div>
      </div>
    </div>


    
    </section>
    <section id="pf">
        <h3> Personlig information</h3>
        <input type="radio" id="kvinna" v-model="kön" value="Kvinna" checked="checked">
        <label for="kvinna">Kvinna</label>
        <input type="radio" id="inteKvinna" v-model="kön" value="Man">
        <label for="man">Man</label>
        <input type="radio" id="annat" v-model="kön" value="Annat">
        <label for="Annat">Vill inte ange</label>
    
        <p>
            <label for="betalsätt">Betalsätt</label>
            
                <select id="betalsätt" v-model="bts">
                    <option>Kort</option>
                    <option>Swish</option>
                    <option>Faktura</option>
                    <option>Kontant (endast vid upphämtning)</option>
                </select>
        </p>
    </section>
    <div>
    <button v-on:click = "markDone(key)" id="beställningsknapp">
        <img src="https://as1.ftcdn.net/v2/jpg/03/60/20/26/1000_F_360202679_aDj3vV839TyYiFfMuFRppEWEbVbWrOLH.jpg"
            style="width: 20px;">
        Skicka beställning
    </button>
    </div>

    
  
    
    <hr>
    <footer>

        Adress: Uppsalagatan 1 <br>
        Tel: 123456789 <br>
        &copy; 2022 Green Burger Paradise
    </footer>
    
    </div>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();
function MenuItem (nm, image, price, cont){
  this.name = nm;
  this.img = image;
  this.burgerPrice = price;
  this.contents = cont;
}

let burger1 = new MenuItem("kikärtsburgare", "https://resources.mynewsdesk.com/image/upload/f_auto,t_limit_1000/fe6yt94umayztvcruy9x.jpg", 149, "Nej");
let burger2 = new MenuItem("zucchiniburgare", "https://resources.mynewsdesk.com/image/upload/ar_16:9,c_fill,dpr_auto,f_auto,g_auto,q_auto:good,w_1782/fuld1fyerv4iqjpbrqsn", 125,"Ja" );
let burger3 = new MenuItem("quornburgare", "https://resources.mynewsdesk.com/image/upload/f_auto,t_limit_1000/usuqjsfqe2anhcdvjgqa.jpg", 179, "Nej");
let myBurgers = [burger1, burger2, burger3];

console.log(myBurgers);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      orderedBurger: {},
      location:  {x: 0,
                  y: 0
      }
    };
  },

  methods: {
    addToOrder: function(event){
      this.orderedBurger[event.name] = event.amount;
    },

    

    setLocation: function(event){
      var offset={
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
        };
        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;
       

    },
    
    markDone: function(){
      console.log("Beställningsinfo: " +this.n +" " + this.em + " " + " Beställning: ")
      console.log( this.orderedBurger)
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                            name: this.n,
                                          email: this.em,
                                        kön: this.kön,
                                      betalsätt: this.bts },
                                orderItems:   [this.orderedBurger]
                              }
                 );
      
    },
    
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
     
    }
  }
}
</script>

<style>
@import url('https://fonts.cdnfonts.com/css/le-sofia');
@import url('https://fonts.cdnfonts.com/css/major-mono-display-2');

  #map {
    width: 1920px;
    height: 1078px;
    background-color: red;
    position: relative;
    background: no-repeat;
    cursor: crosshair;
  }

  #fitMap {
  height: 400px;
  width: auto;
  margin: 20px;
  overflow:scroll;
  }
  
  #map div {
    position: absolute;
    background: black;
    background-image: url("https://cdn-icons-png.flaticon.com/512/684/684908.png");
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  body{
    font-family: arial;
    font-size: 20px;
    background-color: rgba(35, 70, 35, 0.932);
    
    
}

header{
    
    height: 100px;
    width: 1202px;
    overflow: hidden;
    margin-left: 20px;
    background-color: rgb(231, 232, 227);
    
    
}
#headerpic{
    opacity: 60%;
    width: auto;
    
    
}
#headertext{
    position: absolute;
    color: rgb(27, 86, 9);
    margin: 20px;
    margin-top: -950px;
    font-family: 'Major Mono Display', sans-serif;
    font-size: bold;
}

.allergener{
    font-weight: bold;
    color: red;
}
#bm{
    background-color: #3b3935;
    color: #dbd8ce;
    border: 10px Ridge rgb(83, 78, 36);
    
    font-family: 'Le Sofia', sans-serif;
    font-size: px;
    text-align: center;   
    
    
}
#info{
    border: 3px Double black;
    padding: 20px;
    background-color: rgba(235, 234, 216, 0.616);
}
#pf{
    border: 3px Dashed green;
    padding: 20px;
    background-color: rgba(235, 234, 216, 0.616);
}
#beställningsknapp:hover{
    background-color: green;
    cursor: pointer;
   }
   #beställningsknapp{
    margin: 50px 50px;
}

section{
    margin: 20px 20px;
}
/*div{
    padding: 1px 23px 4px;
}*/
.burgarmeny{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33% 33% 33%;
}

</style>