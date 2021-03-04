<template>
  <div class="row q-col-gutter-sm q-py-sm">
    <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
      <q-card class="text-grey-8">
        <q-card-section class="q-pa-none">
          <q-table
            :data="data"
            :columns="columns"
            row-key="numero"
            :filter="filter"
            class="table-orcamento"
            hide-pagination
          >
            <template v-slot:body-cell-Action="props" v-if="showActions">
              <q-td :props="props">
                <q-btn icon="edit" size="sm" flat dense />
                <q-btn icon="delete" size="sm" class="q-ml-sm" flat dense />
              </q-td>
            </template>
          </q-table>
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<script>

export default {
  name: 'TableVisits',
  props: {
    showActions: {
      type: Boolean,
      default: false
    },
    data: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      filter: '',
      mode: 'list',
      columns: [
        {
          name: 'produto',
          required: true,
          label: 'Produto',
          field: 'descricao',
          sortable: true,
          align: 'left'
        },
        {
          name: 'quantidade',
          label: 'Quantidade',
          field: 'quantidade',
          sortable: true,
          align: 'left'
        },
        {
          name: 'valor',
          label: 'Valor Unitáiro',
          field: 'valor',
          sortable: true,
          align: 'left'
        },
        {
          name: 'outraMarca',
          label: 'Pode outra marca?',
          align: 'left',
          field: row => (row.podeOutraMarca ? 'Sim' : 'Não')
        },
        {
          name: 'dataFinal',
          label: 'Data Final',
          field: 'dataFinal',
          align: 'left'
        }
      ]
    }
  },
  methods: {
  },
  created () {
  }
}
</script>

<style scoped>
table tbody tr:hover {
  background-color: primary;
}
</style>
