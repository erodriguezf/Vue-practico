<template>
  <main>
    <p>{{labelVisual}}</p>
    <h1>{{amountCurrency}}</h1>
    <div class="graphic">
        <slot name="graphic"></slot>
    </div>
    <div class="action">
        <slot name="action"></slot>
    </div>
  </main>
</template>

<script>
  const currencyFormater = new Intl.NumberFormat(("es-CO"), {
  style: "currency", currency: "COP"
  })
  export default{
    name: "ResumeVue",
    props: {
      totalLabel: {
        type: String,
         default: null,
      },
      label:{
        type: String,

      },
      amount:{
        type: Number,
        default: null,
      },
      totalamunt:{
        type: Number,
      }
    },
    computed:{
      totalvisual(){
        return this.amount !== null ? this.amount : this.totalamunt
      },
      labelVisual() {
         return this.label !== null ? this.label : this.totalLabel; 
      },
      amountCurrency(){
         return currencyFormater.format(this.totalvisual);
      }
    }
  }
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>