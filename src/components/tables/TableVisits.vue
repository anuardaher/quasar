<template>
  <div class="row q-col-gutter-sm q-py-sm">
    <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
      <q-card class="text-grey-8">
        <q-card-section class="q-pa-none">
          <q-table
            class="no-shadow"
            :data="data"
            :hide-header="mode === 'grid'"
            :columns="columns"
            row-key="name"
            :filter="filter"
            :pagination.sync="pagination"
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
          field: 'id',
          sortable: true
        },
        {
          name: 'produto',
          align: 'left',
          label: 'Produto',
          field: 'user_name',
          sortable: true
        },
        {
          name: 'desc',
          required: true,
          label: 'Criação',
          align: 'left',
          field: 'date',
          sortable: true
        },
        {
          name: 'date',
          align: 'right',
          label: 'Enc.',
          field: 'date',
          sortable: true
        },
        {
          name: 'date',
          align: 'right',
          label: 'Quant.',
          field: '',
          sortable: true
        },
        {
          name: 'boolean',
          align: 'right',
          label: 'Status',
          field: 'boolean',
          sortable: true
        }
      ],
      data: [
        {
          id: 'U0001',
          name: '/login',
          date: '12-10-2019',
          user_name: 'Pratik Patel'
        },
        {
          id: 'U0002',
          name: '/Dashboard',
          date: '11-02-2019',
          user_name: 'Razvan Stoenescu'
        },
        {
          id: 'U0003',
          name: '/Map',
          date: '03-25-2019',
          user_name: 'Pratik Patel'
        },
        {
          id: 'U0004',
          name: '/Mail',
          date: '03-18-2019',
          user_name: 'Jeff Galbraith'
        },
        {
          id: 'U0005',
          name: '/Profile',
          date: '04-09-2019',
          user_name: 'Pratik Patel'
        }
      ],
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
    }
  }
}
</script>
