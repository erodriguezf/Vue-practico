<template>
 <div class="movement">
    <div class="content">
        <h4>{{title}}</h4>
        <p>{{description}}</p>
    </div> 
    <div class="action">
        <img src="@/assets/trash-icon.svg" alt="borar" @click="remove"/>
        <p v-bind:class="{'red':  isNegative, 'green': isPositivetive}">{{amountCurrency}}</p>
    </div>
 </div>
</template>

<script>
   const currencyFormater = new Intl.NumberFormat(("es-CO"), {
  style: "currency", currency: "COP"
  });
  export default{
    name: "MovementVue",
    props:{
       title: String, 
       id:Number,
       description:String,
       amount:Number
    },
    methods:{
       remove(){
         // console.log(this.id);
         this.$emit("remove", this.id);
       }
    },
    computed:{
      amountCurrency(){
        return currencyFormater.format(this.amount);
      },
      isNegative(){
        return this.amount<0?true:false;
      },
      isPositivetive(){
        return this.amount>0?true:false;
      }
    }

  }
</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
    color: red;
}
.green {
    color: green;
}
</style>