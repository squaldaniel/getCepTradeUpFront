<template>
  <div>
    <form @submit.prevent="realizarConsulta">
      <label for="numero">Número (Formato: 99999-999): </label>
      <input v-model="numero" type="text" @input="formatarNumero" pattern="[0-9]{5}-[0-9]{3}" required />
      <button type="submit">Realizar Consulta</button>
      <p v-if="consultaEmAndamento">Realizando consulta...</p>
      <p v-if="consultaErro">Erro ao realizar a consulta.</p>
    </form>

    <h2>Consultas Realizadas:</h2>
    <ul>
      <li v-for="consulta in consultas" :key="consulta.id">
        {{ consulta.numero }} {{ consulta.resultado }}
      </li>
    </ul>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      numero: '',
      consultaEmAndamento: false,
      consultaErro: false,
    }
  },
  computed: {
    consultas() {
      return this.$store.getters.obterConsultas
    }
  },
  methods: {
    formatarNumero() {
      // Remove qualquer caracter que não seja número
      const numeroLimpo = this.numero.replace(/[^\d]/g, '').replace('-', '')
      // Adiciona o caractere '-' automaticamente
      if (numeroLimpo.length >= 5) {
        this.numero = `${numeroLimpo.slice(0, 5)}-${numeroLimpo.slice(5, 8)}`
      } else {
        this.numero = numeroLimpo
      }
    },
    async realizarConsulta() {
      try {
        this.consultaEmAndamento = true;
        this.consultaErro = false;
        const numeroSemHifen = this.numero.replace('-', '');
        // url do backend
        const response = await fetch(`http://127.0.0.1:8000/api/getcep/${this.numero}`);
        const data = await response.json()
        if (data) {
          const consulta = {
            id: Date.now(),
            numero: numeroSemHifen,
            resultado: data.resultado,
          };
          this.$store.dispatch('adicionarConsulta', consulta);
          this.numero = ''; // Limpa o campo após a consulta
        } else {
          console.error('Dados retornados vazios ou em formato não esperado.');
        }
        const consulta = {
          id: Date.now(),
          numero: this.numero.replace('-', ''),
          resultado: ('Rua: ' + data.logradouro + ', Bairro: '+ data.bairro 
          + ' Localidade: '+data.localidade + ' - '+ data.uf)
        }
        this.$store.dispatch('adicionarConsulta', consulta)
        this.numero = '' // Limpa o campo após a consulta
      } catch (error) {
        console.error('Erro ao realizar a consulta:', error)
        this.consultaErro = true;
      } finally {
        this.consultaEmAndamento = false;
      }
    }
  }
}
</script>
  