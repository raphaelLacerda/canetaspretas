<template>
  <div>
    <h1>Ola Mundo CGU</h1>
    <span class="estilo">{{ msg }}</span>
    <div>
        <button v-on:click="incrementar">Increment msg</button>
        <button @click="incrementar">Increment 2</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: 'RAphael henrique',
      count: 0
    };
  },
  methods:{
      incrementar: function(){
          console.log('incrmentando');
          this.count++;
          this.msg+= this.count;
      }
  },
  beforeCreate() {
    console.log('antes de criar');
  },
  created() {
    console.log('foi criado');
  },
  beforeUpdate() {
    console.log('antes de atualizar');
  },
  updated() {
    console.log('foi atualizado');
  },
  beforeMount() {
    console.log('antes de montar');
  },
};
</script>

<style scoped>
.estilo {
  background-color: yellow;
}
</style>
