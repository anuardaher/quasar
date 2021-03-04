<template>
  <form-dialog title="Detalhes" ref="dialog">
    <template v-slot:body>
      <div class="row q-gutter-y-lg">
        <div class="col-12 row q-gutter-x-lg">
          <div class="col-12 text-subtitle1 text-primary">Dados</div>
          <div class="col-md-2 col-5">
            <q-input v-model="orcamento.abertura" label="Abertura" readonly>
              <template v-slot:prepend>
                <q-icon name="event" />
              </template>
            </q-input>
          </div>
          <div class="col-md-3 col-5">
            <q-input v-model="orcamento.vencimento" label="Vencimento" readonly>
              <template v-slot:prepend>
                <q-icon name="event" />
              </template>
            </q-input>
          </div>
          <div class="col-md-3 col-6">
            <q-input v-model="orcamento.faturamento" label="Faturamento" readonly>
              <template v-slot:prepend>
                <q-icon name="money" />
              </template>
            </q-input>
          </div>
          <div class="col-md-2 col-4">
            <q-input v-model="orcamentoStatus" label="Status" readonly>
              <template v-slot:prepend>
                <q-icon name="rule" />
              </template>
            </q-input>
          </div>
          <div class="col-md-5 col-12">
            <q-input v-model="orcamento.cidade" label="Cidade" readonly>
              <template v-slot:prepend>
                <q-icon name="location_on" />
              </template>
            </q-input>
          </div>
          <div class="col-md-5 col-12">
            <q-input v-model="orcamento.entrega" label="Entrega" readonly>
              <template v-slot:prepend>
                <q-icon name="location_on" />
              </template>
            </q-input>
          </div>
          <div class="col-12 q-mt-md">
              <q-input
                type="textarea"
                filled
                label="Descrição"
                v-model="orcamento.descricao"
                readonly
              />
            </div>
        </div>
        <div class="col-12 row q-gutter-x-lg" v-if="orcamento.veiculo">
          <div class="col-12 text-subtitle1 text-primary">Veículo</div>
          <div class="col-md-8 col-12">
            <q-input v-model="orcamento.veiculo.descricao" label="Descrição" readonly>
              <template v-slot:prepend>
                <q-icon name="directions_car" />
              </template>
            </q-input>
          </div>
          <div class="col-md-2 col-4">
            <q-input v-model="orcamento.veiculo.placa" label="Placa" readonly />
          </div>
          <div class="col-md-3 col-6">
            <q-input v-model="orcamento.veiculo.cor" label="Cor" readonly />
          </div>
          <div class="col-md-3 col-6">
            <q-input v-model="orcamento.veiculo.chassi" label="Chassi" readonly />
          </div>
          <div class="col-md-2 col-4">
            <q-input v-model="orcamento.veiculo.kilometragem" label="KM's" readonly />
          </div>
        </div>
        <div class="col-12 row">
          <div class="col-12 text-subtitle1 text-primary">Produtos</div>
          <div class="col-12 q-mt-md">
            <produtos-table
              :data="orcamento.produtos"
            />
          </div>
        </div>
      </div>
    </template>
    <template v-slot:actions>
      <q-btn v-close-popup label="Ok" color="primary" flat />
    </template>
  </form-dialog>
</template>

<script>
import FormDialog from 'src/components/dialogs/FormDialog'
import ProdutosTable from './ProdutosTable'

export default {
  components: {
    FormDialog,
    ProdutosTable
  },
  props: {
    orcamentoId: {
      type: [String, Number]
    }
  },
  data () {
    return {
      orcamento: {},
      show: false
    }
  },
  computed: {
    orcamentoStatus () {
      return this.orcamento.status ? 'Ativo' : 'Inativo'
    }
  },
  methods: {
    showDialog () {
      this.$refs.dialog.show = true
    }
  },
  created () {
    const parsedOrcamentos = JSON.parse(
      window.localStorage.getItem('orcamento')
    )
    this.orcamento = parsedOrcamentos ?? {}
  }
}
</script>

<style></style>
