<template>
  <div class="page">
    <header class="page__header">
      <div class="stepper">
        <span
          v-for="(item, index) in steps"
          :key="index"
          :class="{
            ['stepper__item']: true, 
            ['stepper__item--active']: currentStep === index
          }"
        >
          {{ item.step }}
        </span>
      </div>
    </header>
    <section class="page__content">
      <hr class="divider" />

      <form>
        <!-- STEP 1-->
        <template v-if="currentStep === 0">
          <h2 class="form__title">Seja bem vindo(a)</h2>
          <div class="form__content">
            <div class="form__field">
              <label for="email">Endereço de e-mail</label>
              <input id="email" v-model="formData.email" type="text" />

              <div class="form__radio-button">
                <label>
                  <input type="radio" id="pf" v-model="tipoPessoa" value="pf" />
                  Pessoa física
                </label>

                <label>
                  <input type="radio" id="pj" v-model="tipoPessoa" value="pj" />
                  Pessoa jurídica
                </label>
              </div>
            </div>
          </div>
        </template>
        <!-- STEP 2-->
        <template v-if="currentStep === 1">
          <div v-if="tipoPessoa === 'pf'" class="form__content">
            <h2 class="form__title">Pessoa Física</h2>

            <div class="form__field">
              <label>Nome</label>
              <input type="text" v-model="formData.pessoaFisica.nome" />

              <label>CPF</label>
              <input type="text" v-model="formData.pessoaFisica.cpf" />

              <label>Data de nascimento</label>
              <input type="text" v-model="formData.pessoaFisica.dataNascimento" />

              <label>Telefone</label>
              <input type="text" v-model="formData.pessoaFisica.telefone" />
            </div>
          </div>

          <div v-if="tipoPessoa === 'pj'">
            <h2 class="form__title">Pessoa Jurídica</h2>
            <div class="form__field">
              <label>Razão social</label>
              <input type="text" v-model="formData.pessoaJuridica.razaoSocial" />

              <label>CNPJ</label>
              <input type="text" v-model="formData.pessoaJuridica.cnpj" />

              <label>Data de abertura</label>
              <input type="text" v-model="formData.pessoaJuridica.dataAbertura" />

              <label>Telefone</label>
              <input type="text" v-model="formData.pessoaJuridica.telefone" />
            </div>
          </div>
        </template>
        <!-- STEP 3 -->
        <template v-if="currentStep === 2">
          <h2 class="form__title">Senha de acesso</h2>

          <div class="form__field">
            <label>Sua senha</label>
            <input type="text" v-model="formData.senhaAcesso" />
          </div>
        </template>
        <!-- STEP 4 -->
        <template v-if="currentStep === 3">
          <h2 class="form__title">Revise suas informações</h2>

          <div class="form__field">
            <label>Endereço de e-mail</label>
            <input type="text" v-model="formData.email" />

            <template v-if="tipoPessoa === 'pf'">
              <div class="form__field">
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
            </template>

            <template v-if="tipoPessoa === 'pj'">
              <div class="form__field">
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
            </template>

            <label>Senha</label>
            <input type="text" v-model="formData.senhaAcesso" />
          </div>
        </template>
      </form>
      
      <div class="page__footer">
        <button v-if="currentStep !== 0" @click="prevStep">Voltar</button>
        <button @click="nextStep" class="btn-primary">{{`${currentStep === 3 ? 'Cadastrar' : 'Continuar'}`}}</button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentStep: 0,
      tipoPessoa: 'pf',
      steps: [ 
        { step: 1 }, 
        { step: 2 }, 
        { step: 3 }, 
        { step: 4 }
      ],
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
      return this.currentStep === 0
    },

    isLastIndex() {
      return this.currentStep === this.steps.length - 1
    },
  },

  methods: {
    nextStep() {
      if (!this.isLastIndex) {
        this.currentStep += 1
      } else if (this.currentStep === 3){
        this.submitForm()
      }
    },

    prevStep() {
      if (!this.isFirstIndex) {
        this.currentStep -= 1
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

.page__content {
  width: 100%;
  max-width: 500px;
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

.stepper__item {
  color: #333;
  font-size: 1rem;
  font-weight: bold;
}

.stepper__item--active {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  text-align: center;
  background-color: #EE9605;
  color: #fff;
  font-size: 2rem;
}

.divider {
  margin: 1rem 0;
}

.form__content {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.form__title {
  text-align: left;
  margin-bottom: 1rem;
}

.form__field {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 8px;
}

.form__radio-button {
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.form__radio-button label {
  display: flex;
  align-items: center;
  margin: 0 1rem 0 0;
}

.form__radio-button input {
  margin-right: .3rem;
}

.form__content label {
  font-size: 0.8rem;
  color: rgba(0, 0, 0, 0.75);
}

.form__field input {
  background: #fafafa;
  border: 1px solid #ededed;
  color: #333;
  border-radius: 6px;
  padding: 6px 10px;
  font-size: 1rem;
  outline: none;
}

.form__field input:focus {
  border-color: #EE9605;
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
}

.page__footer button {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: transparent;
  border: 1px solid #EE9605;
  font-size: 1rem;
  color: #EE9605;
  padding: 12px;
  max-height: 32px;
  border-radius: 4px;
  cursor: pointer;
}

.page__footer button.btn-primary {
  background-color: #EE9605;
  border-color: #EE9605;
  color: #fff;
  margin-left: 1rem;
}
</style>
