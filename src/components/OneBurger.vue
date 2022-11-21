<template>
    
    <div class="burgerMenu">
            <h4>{{ burger.name }} - {{ burger.burgerPrice }} kr</h4>
           
           <img class="burgerImage" v-bind:src="burger.img"/>

           <p> Antal beställda: {{amountOrdered}}</p>

           <button v-on:click="addBurger(key)" id="add"> Lägg till </button>
           <button v-on:click = "subBurger(key)" id="sub"> Ta bort</button>
    
           <p class="content"><span id = "innehåll">Innehåll*: </span> <br />
            {{burger.content1}}<br />
            {{burger.content2}}<br />
            {{burger.content3}}<br />
           <span id="allergenes"> {{burger.allergenes}}</span>
          </p>

            
        </div>
  </template>
  
  <script>
 
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
      return {
        amountOrdered: 0,
      }
    },
    methods: {
      addBurger: function(){
        this.amountOrdered++;
        this.$emit('orderedBurger', {name: this.burger.name,
                                amount: this.amountOrdered});
      },
      subBurger: function(){
        if(this.amountOrdered != 0){
          this.amountOrdered--;
          this.$emit('orderedBurger', {name: this.burger.name,
                                amount: this.amountOrdered});

          }        
      }
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .content{
    text-align: left;
    margin-left: 100px;
  }
  #allergenes{
    
    color: rgb(207, 75, 75);
}
#innehåll{
  font-size: 30px;
}
.burgerImage{
  width: 200px;
  border: 10px ridge rgb(83, 78, 36);
}
button{
  margin:10px;
}
#add{
  color: green;
}
#add:hover{
  cursor: pointer;
  background-color: green;
  color: white;
}
#sub{
  color: red;
}
#sub:hover{
  cursor: pointer;
  background-color: red;
  color: white;
}
  </style>
  