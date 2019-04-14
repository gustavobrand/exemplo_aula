<template>
  <div class="col-6">
    <q-card class="bg-white">
      <q-card-title class="bg-primary text-white q-py-sm">
        Cadastro de clientes
      </q-card-title>
      <q-card-main>
        <div class="row">
          <q-field class="col-6 q-pa-sm">
            <q-input
              v-model="form.nome"
              float-label="Nome"
            />
          </q-field>
          <q-field class="col-6 q-pa-sm">
            <q-input
              v-model="form.cpf"
              float-label="CPF"
            />
          </q-field>
          <q-field class="col-6 q-pa-sm">
            <q-select
              v-model="form.sexo"
              float-label="Sexo"
              :options="sexo"
            />
          </q-field>
          <q-field class="col-6 q-pa-sm">
            <q-input
              v-model="form.email"
              float-label="E-mail"
            />
          </q-field>
          <q-field class="col-6 q-pa-sm">
            <q-input
              v-model="form.telefone"
              float-label="Telefone"
            />
          </q-field>
          <q-field class="col-6 q-pa-sm">
            <q-input
              v-model="form.endereco"
              float-label="Endereço"
            />
          </q-field>
        </div>
      </q-card-main>
      <q-card-actions>
        <q-btn
          class="full-width"
          label="Salvar"
          color="primary"
          @click="Salvar(id)"
        />
      </q-card-actions>
    </q-card>
  </div>
</template>

<script>
export default {
  data () {
    return {
      id: Number(this.$route.params.id),
      form: {
        nome: '',
        cpf: '',
        sexo: '',
        email: '',
        telefone: '',
        endereco: ''
      },
      sexo: [
        { label: 'Masculino', value: 'Masculino' },
        { label: 'Feminino', value: 'Feminino' }
      ]
    }
  },
  mounted () {
    this.GetCliente(this.id)
  },
  methods: {
    Salvar (id) {
      const salvar = id
        ? this.$axios.patch(`api/clientes/${id}`, this.form)
        : this.$axios.post('api/clientes', this.form)

      salvar
        .then(() => {
          this.$q.notify({
            message: 'Cliente salvo com sucesso!',
            type: 'positive'
          })

          this.$router.push('/')
        })
        .catch(err => console.log(err))
    },
    GetCliente (id) {
      if (id) {
        this.$axios.get(`api/clientes/${id}`)
          .then(res => {
            this.form = res.data
          })
      }
    }
  }
}
</script>
