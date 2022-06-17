<template>
   <button @click="verinfo">Agregar movimiento</button>
   <teleport to="#app">
        <Modal 
        v-show="showmodal"
        @close="close"
        > 
        <template #body>
        <div>
          <form @submit.prevent="submit">
             <div class="field">
                <label>Titulo</label>
                <input type="text" v-model="title"/>
             </div>
             <div class="field">
                <label>Monto</label>
                <input type="number" v-model="amount"/>
             </div>
              <div class="field">
                <label>Descripcion</label>
                <textarea rows="4" v-model="description"></textarea>
             </div>
             <div class="field">
                <label class="radio-label">
                     <input type="radio" v-model="movementType" value="Ingreso">
                     <span>Ingreso</span>
                </label> 
                <label class="radio-label">
                     <input type="radio" v-model="movementType" value="Gasto">
                     <span>Gasto</span>
                </label> 
             </div>
             <div class="action">
                <button>Agregar Movimiento</button>
             </div>
          </form>
           </div>
         </template>
        </Modal>
   </teleport>
</template>

<script>
   import { ref} from 'vue';
   import Modal from './Modal.vue';
   export default{
     name: "ActionVue",
     data(){
        return{
            showmodal: ref(false),
            title: " ",
            amount: 0,
            description: " ",
            movementType: "Ingreso",
        }
     },
     methods:{
       verinfo(){
         this.showmodal=true;
       },
       close(){
         this.showmodal=false;
       }, 
       submit(){
         this.showmodal=false;
         this.$emit("submit",{
            title: this.title,
            amount: this.movementType ==="Ingreso"? this.amount:-this.amount,
            description: this.description,
            time: new Date(),
            id:new Date(),
         })
           this.title="",
           this.description="",
           this.amount=0,
           this.movementType="Ingreso"
       }
     },
     components:{
        Modal,
     }
   }
</script>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}
form {
  font-size: 1.24rem;
  width: 100%;
}
form .action {
  padding: 0 24px;
}
.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}
label {
  margin-bottom: 8px;
}
input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}
input[type="number"] {
  text-align: right;
}
.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}
.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}
input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}
input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>