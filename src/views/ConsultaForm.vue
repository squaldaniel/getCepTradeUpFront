<template>
  <div class="card bg-base-100 w-96 shadow-xl">
    <div class="card-body">
      <h2 class="card-title">Pesquise aqui!</h2>
      <form @submit.prevent="realizarConsulta">
        <label for="numero">Número (Formato: 99999-999): </label>
        <input style="margin-bottom: 20px" placeholder="Digite aqui" class="input input-bordered input-accent w-full max-w-xs"
        v-model="numero" type="text" @input="formatarNumero" pattern="[0-9]{5}-[0-9]{3}" required />
        <button class="btn btn-primary w-full p-2" type="submit">Realizar Consulta</button>
      </form>
      <div class="divider divider-info">Cep´s</div>
      <p v-if="consultaEmAndamento">Realizando consulta...</p>
      <p v-if="consultaErro">Erro ao realizar a consulta.</p>
      <div class="card-actions justify-end">
        <ul>
          <li v-for="consulta in consultas" :key="consulta.id">
            {{ consulta.numero }} {{ consulta.resultado }}
          </li>
        </ul>
      </div>
    </div>
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
  