<template>
  <div class="page">
    <header class="page__header">
      <div class="stepper">
        <span
          v-for="(item, index) in steps"
          :key="index"
          :class="{ 'stepper__item--active': currentstep === index }"
        >
          {{ item.step }}
        </span>
      </div>
    </header>
    <section class="page__content">
      <hr class="divider" />

      <form>
        <!-- STEP 1-->
        <template v-if="currentstep === 0">
          <h2>Seja bem vindo(a)</h2>
          <div class="form__row">
            <div class="form__field">
              <label for="email">Endereço de e-mail</label>
              <input id="email" v-model="formData.email" type="text" />

              <div class="form__radio-row">
                <div>
                  <input type="radio" id="pf" v-model="tipoPessoa" value="pf" />
                  <label for="pf">Pessoa física</label>
                </div>

                <div>
                  <input type="radio" id="pj" v-model="tipoPessoa" value="pj" />
                  <label for="pj">Pessoa jurídica</label>
                </div>
              </div>
            </div>
          </div>
        </template>
        <!-- STEP 2-->
        <template v-if="currentstep === 1">
          <div v-if="tipoPessoa === 'pf'">
            <h2>Pessoa Física</h2>

            <label>Nome</label>
            <input type="text" v-model="formData.pessoaFisica.nome" />

            <label>CPF</label>
            <input type="text" v-model="formData.pessoaFisica.cpf" />

            <label>Data de nascimento</label>
            <input type="text" v-model="formData.pessoaFisica.dataNascimento" />

            <label>Telefone</label>
            <input type="text" v-model="formData.pessoaFisica.telefone" />
          </div>

          <div v-if="tipoPessoa === 'pj'">
            <h2>Pessoa Jurídica</h2>

            <label>Razão social</label>
            <input type="text" v-model="formData.pessoaJuridica.razaoSocial" />

            <label>CNPJ</label>
            <input type="text" v-model="formData.pessoaJuridica.cnpj" />

            <label>Data de abertura</label>
            <input type="text" v-model="formData.pessoaJuridica.dataAbertura" />

            <label>Telefone</label>
            <input type="text" v-model="formData.pessoaJuridica.telefone" />
          </div>
        </template>
        <!-- STEP 3 -->
        <template v-if="currentstep === 2">
          <h2>Senha de acesso</h2>

          <div>
            <label>Sua senha</label>
            <input type="text" v-model="formData.senhaAcesso" />
          </div>
        </template>
        <!-- STEP 4 -->
        <template v-if="currentstep === 3">
          <h2>Revise suas informações</h2>

          <div>
            <label>Endereço de e-mail</label>
            <input type="text" v-model="formData.email" />

            <div v-if="tipoPessoa === 'pf'">
              <label>Nome</label>
              <input type="text" v-model="formData.pessoaFisica.nome" />

              <label>CPF</label>
              <input type="text" v-model="formData.pessoaFisica.cpf" />

              <label>Data de nascimento</label>
              <input
                type="text"
                v-model="formData.pessoaFisica.dataNascimento"
              />

              <label>Telefone</label>
              <input type="text" v-model="formData.pessoaFisica.telefone" />
            </div>

            <div v-if="tipoPessoa === 'pj'">
              <label>Razão social</label>
              <input
                type="text"
                v-model="formData.pessoaJuridica.razaoSocial"
              />

              <label>CNPJ</label>
              <input type="text" v-model="formData.pessoaJuridica.cnpj" />

              <label>Data de abertura</label>
              <input
                type="text"
                v-model="formData.pessoaJuridica.dataAbertura"
              />

              <label>Telefone</label>
              <input type="text" v-model="formData.pessoaJuridica.telefone" />
            </div>

            <label>Senha</label>
            <input type="text" v-model="formData.senhaAcesso" />
          </div>
        </template>
      </form>
      <button @click="submitForm">Enviar dados</button>
      <div class="page__footer">
        <button v-if="currentstep !== 0" @click="prevStep">Voltar</button>
        <button @click="nextStep">Avancar</button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentstep: 0,
      tipoPessoa: 'pf',
      steps: [],
      formData: {
        email: '',
        pessoaFisica: {
          nome: '',
          cpf: '',
          dataNascimento: '',
          telefone: '',
        },
        pessoaJuridica: {
          razaoSocial: '',
          cnpj: '',
          dataAbertura: '',
          telefone: '',
        },
        senhaAcesso: '',
      },
    }
  },

  computed: {
    isFirstIndex() {
      return this.currentstep === 0
    },

    isLastIndex() {
      return this.currentstep === this.steps.length - 1
    },
  },

  created() {
    this.steps = [
      { step: 1 }, 
      { step: 2 }, 
      { step: 3 }, 
      { step: 4 }
    ]
  },

  methods: {
    nextStep() {
      if (!this.isLastIndex) {
        this.currentstep += 1
        console.log('DADOS', this.formData)
      }
    },

    prevStep() {
      if (!this.isFirstIndex) {
        this.currentstep -= 1
      }
    },

    submitForm() {
      if (this.tipoPessoa === 'pf') {
        delete this.formData.pessoaJuridica
        console.log('SUBMIT PF', this.formData)
      } else if (this.tipoPessoa === 'pj') {
        delete this.formData.pessoaFisica
        console.log('SUBMIT PJ', this.formData)
      }
    },
  },
}
</script>

<style scoped>
.page {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #333;
}

.page__header {
  width: 100%;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stepper {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 38px;
}
.stepper__item--active {
  color: #e9af0e;
}

.divider {
  margin: 1rem 0;
}

form {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.form__row {
  width: 100%;
  display: flex;
  gap: 18px;
}

.form__field {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 8px;
}

.form__radio-row {
  display: flex;
  align-items: center;
}

.form__radio-row div {
  display: flex;
  align-items: center;
}

.form__field label {
  font-size: 0.8rem;
  color: rgba(0, 0, 0, 0.75);
}
.form__field input,
.form__field select {
  width: 100%;
  background: #fafafa;
  border: 1px solid #ededed;
  color: #333;
  border-radius: 6px;
  padding: 6px 10px;
  font-size: 1rem;
  outline: none;
}
.form__field input:focus,
.form__field select:focus {
  border-color: #e9af0e;
}
.form__field-error {
  font-size: 0.7rem;
  color: red;
}

.page__footer {
  margin-top: 1rem;
  width: 100%;
  height: 48px;
  display: flex;
  justify-content: space-between;
  padding-left: 18px;
  padding-right: 18px;
}
.page__footer button {
  display: flex;
  justify-content: center;
  align-items: center;
  background: transparent;
  border: 1px solid #cecece;
  font-size: 1rem;
  color: #9e9e9e;
  padding: 12px;
  max-height: 32px;
  border-radius: 4px;
  cursor: pointer;
}
.page__footer button.btn-primary {
  border-color: #e9af0e;
  color: #fff;
}
</style>
