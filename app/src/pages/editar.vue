<template>
  <q-page class="row flex-center fit bg-grey-4">
    <form-cadastro />
  </q-page>
</template>

<script>
import FormCadastro from '../components/form'

export default {
  name: 'PaginaCadastroClientes',
  components: { FormCadastro }
}
</script>
