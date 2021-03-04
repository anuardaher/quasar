<template>
  <div>
    <page-header
      title="Orçamentos"
      subtitle="Liste e encontre novos orçamentos"
    >
      <template v-slot:action>
        <q-btn
          to="/orcamentos/novo"
          icon="post_add"
          color="primary"
          :round="$q.screen.lt.sm"
          :label="$q.screen.lt.sm ? '' : 'Novo'"
          glossy
          class="text-capitalize"
        />
      </template>
    </page-header>
    <q-card>
      <q-item>
        <q-item-section>
          <div class="text-h6 text-primary">
            Filtros
          </div>
        </q-item-section>
        <q-item-section side>
          <div class="text-h6 text-primary">
            <q-btn
              glossy
              :round="$q.screen.lt.sm"
              :label="$q.screen.lt.sm ? '' : 'Buscar'"
              icon="search"
              color="secondary"
              class="text-capitalize"
            ></q-btn>
          </div>
        </q-item-section>
      </q-item>
      <q-separator />
      <q-card-section>
        <div
          :class="
            `q-gutter-y-md ${
              $q.screen.gt.sm ? 'q-gutter-x-xl' : 'q-gutter-x-md'
            } row`
          "
        >
          <q-select
            class="col-md-5 col-sm-6 col-xs-12"
            color="primary"
            v-model="filtro.produto"
            :options="produtos"
            label="Produto"
            use-input
            @filter="filterProducts"
            input-debounce="0"
            dense
            clearable
          >
            <template v-slot:prepend>
              <q-icon name="build" />
            </template>
          </q-select>
          <q-input
            class="col-xl-1 col-md-2 col-sm-6 col-xs-5"
            v-model="filtro.criacao"
            label="Criação"
            mask="##/##/####"
            dense
          >
            <template v-slot:prepend>
              <q-icon name="event" class="cursor-pointer"> </q-icon>
              <q-popup-proxy
                ref="qDateProxy"
                transition-show="scale"
                transition-hide="scale"
              >
                <q-date
                  v-model="filtro.criacao"
                  color="primary"
                  mask="DD/MM/YYYY"
                  dense
                >
                  <div class="row items-center justify-end">
                    <q-btn v-close-popup label="Ok" color="primary" flat />
                  </div>
                </q-date>
              </q-popup-proxy>
            </template>
          </q-input>
          <q-input
            class="col-xl-1 col-md-2 col-sm-6 col-xs-5"
            v-model="filtro.encerramento"
            label="Encerramento"
            mask="##/##/####"
            dense
          >
            <template v-slot:prepend>
              <q-icon name="event" class="cursor-pointer"> </q-icon>
              <q-popup-proxy
                ref="qDateProxy"
                transition-show="scale"
                transition-hide="scale"
              >
                <q-date
                  v-model="filtro.encerramento"
                  color="primary"
                  mask="DD/MM/YYYY"
                  dense
                >
                  <div class="row items-center justify-end">
                    <q-btn v-close-popup label="Ok" color="primary" flat />
                  </div>
                </q-date>
              </q-popup-proxy>
            </template>
          </q-input>
          <q-select
            class="col-lg-1 col-md-2 col-sm-6 col-xs-5"
            color="primary"
            v-model="filtro.status"
            :options="status"
            label="Status"
            dense
            clearable
          >
            <template v-slot:prepend>
              <q-icon name="rule" />
            </template>
          </q-select>
          <q-input
            type="number"
            class="col-lg-1 col-md-2 col-sm-2 col-xs-5"
            color="primary"
            v-model="filtro.quantidade"
            label="Quantidade"
            dense
          >
            <template v-slot:prepend>
              <q-icon name="dialpad" />
            </template>
          </q-input>
        </div>
      </q-card-section>
    </q-card>
    <my-table @row-click="showDetailsDialog" />
    <details-dialog ref="details" :orcamento-id="orcamentoId"/>
  </div>
</template>

<script>
import PageHeader from 'src/components/PageHeader.vue'
import MyTable from 'src/components/tables/TableVisits'
import DetailsDialog from 'src/components/pages/orcamentos/OrcamentoDetailsDialog'

export default {
  components: {
    PageHeader,
    MyTable,
    DetailsDialog
  },
  data () {
    return {
      orcamento: {},
      orcamentoId: null,
      filtro: {},
      produtos: [],
      status: ['Ativo', 'Inativo']
    }
  },
  methods: {
    filterProducts (val, update, abort) {
      if (val.length < 3) return abort()
      setTimeout(() => {
        update(() => {
          if (val === '') return
          const needle = val.toLowerCase()
          this.produtos = [
            'Amortecedor',
            'Anel de pistão',
            'Bomba elétrica de combustível'
          ].filter(v => v.toLowerCase().indexOf(needle) > -1)
        })
      }, 1500)
    },
    showDetailsDialog (orcamento) {
      this.orcamentoId = orcamento.numero
      this.$refs.details.showDialog()
    }
  }
}
</script>

<style></style>
