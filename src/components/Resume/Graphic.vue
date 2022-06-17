<template>
   <div>
       <svg
         @touchstart="tap"
         @touchmove="tap"
         @touchend="untap"
         viewBox ="0 0 300 200"
       >
        <line 
            stroke="#c4c4c4"
            stroke-width="2"
            x1="0"
            x2="300"
            :y1="zero"
            :y2="zero"
           />
        <polyline 
           fill="none"
           stroke="#0689B0"
           stroke-width="2"
           :points="points"
        />
        <line 
           stroke="#06b500"
           stroke-width="2"
           :x1="pointer"
           :x2="pointer"
           y1="0"
           y2="200"
           v-show="showpointer"
        />
       </svg>
       <p>Últimos 30 dias</p>
   </div>
</template>

<script>
  const pixels2=(amount,min,max)=>{
              const amunabs = amount + Math.abs(min);
              const minmax = Math.abs(min) + Math.abs(max);
              return 200 - ((amunabs*100)/minmax)*2;
  }
  export default{
     name: "GraphicVue",
     data(){
       return {
         showpointer:false,
         pointer:0,
       }
     },
     props:{
       amounts: {
        type: Array[Number]
       }
     },
     computed:{
        points(){
          const total = this.amounts.length;
            var max=-99999999999999999999999999999999;
              for(let i in this.amounts){
                   if(this.amounts[i]>max){
                      max=this.amounts[i];
                   }
              }
               var min=99999999999999999999999999999999;
              for(let i in this.amounts){
                   if(this.amounts[i]<min){
                      min=this.amounts[i];
                   }
              }
          return this.amounts.reduce((points,amount,i)=>{
             const x=(300/total)*(i+1);
              const y = pixels2(amount,min,max);
             //const z = pixels2(this.amounts.length? this.amounts[0]:0,min,max);
             return `${points} ${x},${y}`;
          }, `0, ${pixels2(this.amounts.length? this.amounts[0]:0,min,max)}`);
        },
        zero(){
            var max=-99999999999999999999999999999999;
              for(let i in this.amounts){
                   if(this.amounts[i]>max){
                      max=this.amounts[i];
                   }
              }
               var min=99999999999999999999999999999999;
              for(let i in this.amounts){
                   if(this.amounts[i]<min){
                      min=this.amounts[i];
                   }
              }

              return pixels2(0,min,max);
        }
     },
     watch:{
       pointer(value){
          const Index = Math.ceil(value/(300/this.amounts.length));
          if(Index <0 || Index>this.amounts.length) return;
          this.$emit("pointer",this.amounts[Index-1])
       }
     },
     methods:{
        tap({target,touches}){
          this.showpointer=true;
          const elementWidth = target.getBoundingClientRect().width;
          const elementX = target.getBoundingClientRect().x;
          const touchex=touches[0].clientX;
          this.pointer= ((touchex-elementX)*300)/elementWidth;
        //  this.$emit("tap",this.amounts);
        },
        untap(){
          this.showpointer=false;
        }
     }
  }
</script>

<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>