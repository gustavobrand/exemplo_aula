<template>
  <q-page class="row flex-center fit bg-grey-4">
    <q-card
      v-if="clientes.length"
      class="bg-white"
    >
      <q-card-title class="bg-primary text-white q-py-xs">
        Clientes
      </q-card-title>
      <q-table
        :data="clientes"
        :columns="colunas"
        row-key="id"
        :pagination.sync="paginacao"
        color="primary"
        dense
        grid
      >
        <q-td
          slot="body-cell-acoes"
          slot-scope="props"
          :props="props"
        >
          <q-icon
            class="q-mx-xs"
            name="edit"
            color="primary"
            size="20px"
            @click.native="$router.push(`/editar/${props.row.id}`)"
          >
            <q-tooltip
              anchor="bottom middle"
              self="top middle"
              :offset="[0, 4]"
              :delay="500"
            >
              Editar cliente
            </q-tooltip>
          </q-icon>
          <q-icon
            class="q-mx-xs"
            name="delete"
            color="primary"
            size="20px"
            @click.native="DelCliente(props.row.id)"
          >
            <q-tooltip
              anchor="bottom middle"
              self="top middle"
              :offset="[0, 4]"
              :delay="500"
            >
              Excluir cliente
            </q-tooltip>
          </q-icon>
        </q-td>
      </q-table>
    </q-card>
    <q-icon
      v-else
      name="far fa-folder-open"
      color="light"
      size="300px"
    />
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      clientes: [],
      colunas: [
        { name: 'nome', label: 'Nome', align: 'left', field: 'nome' },
        { name: 'cpf', label: 'CPF', align: 'left', field: 'cpf' },
        { name: 'sexo', label: 'Sexo', align: 'left', field: 'sexo' },
        { name: 'email', label: 'E-mail', align: 'left', field: 'email' },
        { name: 'telefone', label: 'Telefone', align: 'left', field: 'telefone' },
        { name: 'endereco', label: 'Endereço', align: 'left', field: 'endereco' },
        { name: 'acoes', label: '', align: 'left', field: 'acoes' }
      ],
      paginacao: {
        rowsPerPage: 10,
        page: 1
      }
    }
  },
  mounted () {
    this.GetClientes()
  },
  methods: {
    GetClientes () {
      this.$q.loading.show()
      this.$axios.get('api/clientes')
        .then(res => {
          this.clientes = res.data
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => {
          this.$q.loading.hide()
        })
    },
    DelCliente (id) {
      this.$q.dialog({
        title: 'Confirmar exclusão',
        message: 'Tem certeza que deseja excluir este cliente?',
        ok: 'Sim',
        cancel: 'Cancelar'
      }).then(() => {
        this.$axios.delete(`api/clientes/${id}`)
          .then(Res => {
            this.$q.notify({
              message: 'Cliente excluído com sucesso!',
              type: 'positive'
            })
          }).catch(err => {
            console.log(err)
          })
          .finally(() => {
            this.GetClientes()
          })
      })
    }
  }
}
</script>
