<template>
 <div class="movements">
    <h2 class="title">Historial</h2>
    <div class="content">
            <Movement 
            v-for="{id,title,description, amount} in movements"
            :key="id"
            :id="id"
            :title="title"
            :description="description"
            :amount="amount"
            @remove="remove"
            >

            </Movement>
    </div>

 </div>
</template>

<script >
import Movement from './Movement.vue'
export default{
    name: "MovementsVue",
    props:{
       movements: []
    },
    components:{
        Movement
    },
    methods:{
     remove(id){
        this.$emit("remove",id)
      }
    }
}
</script>

<style scoped>
.movements {
  max-height: 100%;
  padding: 0 8px;
  margin-bottom: 14px;
}
.title {
  margin: 8px 16px 24px 16px;
  color: var(--brand-blue);
}
.content {
  max-height: 68vh;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow-y: scroll;
}
</style>