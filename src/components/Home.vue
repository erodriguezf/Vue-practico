<template>
  <layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
         :total-label="'Ahorro total'"
         :label="label"
         :amount="amunt"
         :totalamunt="totalamunt"
       >
        <template #graphic>
            <Graphic :amounts="amounts" @pointer="pointer"/>
        </template>
        <template #action>
            <Action @submit="create"></Action>
        </template>
       </Resume>
    </template>
    <template #movements>
      <Movements
       :movements="movements"
       @remove="remove"
      >

      </Movements>
    </template>
  </layout>
</template>

<script>
import Layout from './Layout.vue';
import Header from './Header.vue';
import Resume from './Resume/Index.vue';
import Movements from './Movements/Index.vue';
import Action from './Action.vue';
import Graphic from './Resume/Graphic.vue';
export default {
  name: "HomeVue",
  components:{
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic
  },
  data() {
    return {
      amunt:null,
      label:null,
      movements:[],
    }
  },
  computed:{
     amounts(){
        const lastDate = this.movements
               .filter(m=>{
                  const today = new Date();
                  const oldate = today.setDate(today.getDate()-30);
                  return m.time > oldate;
               })
               .map(m=>m.amount)
        return lastDate.map((m,i)=>{
           const lastmovements = lastDate.slice(0,i+1);
           return lastmovements.reduce((suma,movement)=>{
               return suma+movement;
           },0)
        });
     },
     totalamunt(){
      return this.movements.reduce((suma,m)=>{
         return suma + m.amount;
      },0);
     }
  },
  mounted(){
    const movements = JSON.parse(localStorage.getItem("movements"));
     if(Array.isArray(movements)){
          this.movements = movements.map(m=>{
        return {...m, time:new Date(m.time)};
        });
     }
  },
  methods:{
    create(movement){
     this.movements.push(movement)
     this.save();
    },
    remove(id){
    const Index = this.movements.findIndex(m=>m.id === id);
    this.movements.splice(Index,1);
    this.save();
    },
    save(){
      localStorage.setItem("movements",JSON.stringify(this.movements));
    },
    pointer(e){
       console.log(e);
       this.amunt=e;
    }
  }
};
</script>
