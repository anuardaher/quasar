<template>
  <div class="row q-col-gutter-sm q-py-sm">
    <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
      <q-card class="text-grey-8">
        <q-card-section class="q-pa-none">
          <q-table
            :data="data"
            :hide-header="mode === 'grid'"
            :columns="columns"
            row-key="numero"
            :filter="filter"
            :pagination.sync="pagination"
            @row-click="(event, row) => $emit('row-click', row)"
            class="table-orcamento"
          >
            <template v-slot:top-right="props">
              <q-btn
                flat
                dense
                :icon="props.inFullscreen ? 'fullscreen_exit' : 'fullscreen'"
                @click="props.toggleFullscreen"
                v-if="mode === 'list'"
                class="q-px-sm offset-lg-6 offset-md-3"
              >
                <q-tooltip
                  :disable="$q.platform.is.mobile"
                  anchor="center left"
                  self="center end"
                  >{{
                    props.inFullscreen ? "Sair da tela cheia" : "Tela Cheia"
                  }}
                </q-tooltip>
              </q-btn>

              <q-btn
                flat
                dense
                icon="description"
                :label="!$q.platform.is.mobile ? 'Exportar para .csv' : null"
                no-caps
                @click="exportTable"
              />
            </template>
            <template v-slot:top-left>
              <q-input
                :class="$q.platform.is.mobile ? 'full-width q-mb-md' : null"
                dense
                debounce="300"
                v-model="filter"
                placeholder="Search"
              >
                <template v-slot:prepend>
                  <q-icon name="search" />
                </template>
              </q-input>
            </template>
            <template v-slot:body-cell-Action="props">
              <q-td :props="props">
                <q-btn
                  icon="edit"
                  size="sm"
                  flat
                  dense
                  @click.stop="edit(props.row)"
                />
                <q-btn
                  icon="delete"
                  size="sm"
                  class="q-ml-sm"
                  flat
                  dense
                  @click.stop="remove()"
                />
              </q-td>
            </template>
          </q-table>
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<script>
import { exportFile } from 'quasar'

function wrapCsvValue (val, formatFn) {
  let formatted = formatFn !== 0 ? formatFn(val) : val

  formatted = formatted === 0 || formatted === null ? '' : String(formatted)

  formatted = formatted.split('"').join('""')
  /**
   * Excel accepts \n and \r in strings, but some other CSV parsers do not
   * Uncomment the next two lines to escape new lines
   */
  // .split('\n').join('\\n')
  // .split('\r').join('\\r')

  return `"${formatted}"`
}
export default {
  name: 'TableVisits',
  data () {
    return {
      filter: '',
      mode: 'list',
      columns: [
        {
          name: 'id',
          align: 'left',
          label: 'Número',
          field: 'numero',
          sortable: true
        },
        {
          name: 'abertura',
          align: 'left',
          required: true,
          label: 'Abertura',
          field: 'abertura',
          sortable: true
        },
        {
          name: 'vencimento',
          align: 'left',
          label: 'Vencimento.',
          field: 'vencimento',
          sortable: true
        },
        {
          name: 'cidade',
          align: 'left',
          label: 'Cidade',
          field: 'cidade',
          sortable: true
        },
        {
          name: 'status',
          align: 'left',
          label: 'Status',
          field: row => (row.status === true ? 'Ativo' : 'Inativo'),
          sortable: true
        },
        {
          name: 'Action',
          label: 'Ações',
          field: 'Action',
          sortable: false,
          align: 'center'
        }
      ],
      data: [],
      pagination: {
        rowsPerPage: 10
      }
    }
  },
  methods: {
    exportTable () {
      // naive encoding to csv format
      const content = [this.columns.map(col => wrapCsvValue(col.label))]
        .concat(
          this.data.map(row =>
            this.columns
              .map(col =>
                wrapCsvValue(
                  typeof col.field === 'function'
                    ? col.field(row)
                    : row[col.field === 0 ? col.name : col.field],
                  col.format
                )
              )
              .join(',')
          )
        )
        .join('\r\n')

      const status = exportFile('table-export.csv', content, 'text/csv')

      if (status !== true) {
        this.$q.notify({
          message: 'Browser denied file download...',
          color: 'negative',
          icon: 'warning'
        })
      }
    },
    getOrcamentos () {
      const parsedOrcamentos = JSON.parse(
        window.localStorage.getItem('orcamento')
      )
      this.data = parsedOrcamentos ? [parsedOrcamentos] : []
    },
    remove () {
      this.$q.dialog({
        title: 'Alerta',
        icon: 'warning',
        cancel: true,
        color: 'negative',
        message: 'Tem certeza que deseja excluir?'
      })
        .onOk(() => {
          window.localStorage.removeItem('orcamento')
          this.getOrcamentos()
        })
    },
    edit (orcamento) {
      this.$router.push(`/orcamentos/editar/${orcamento.numero}`)
    }
  },
  created () {
    this.getOrcamentos()
  }
}
</script>

<style scoped>
table tbody tr:hover {
  background-color: primary;
}
</style>
