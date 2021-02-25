<template>
  <q-page class="q-pa-sm">
    <page-header
      title="Orçamentos"
      subtitle="Liste e encontre novos orçamentos"
    >
      <template v-slot:action>
        <q-btn
          to="/orcamento/novo"
          icon="post_add"
          color="indigo-8"
          round
          glossy
          class="text-capitalize text-white"
        >
          <q-tooltip
            :disable="$q.platform.is.mobile"
            anchor="center left"
            self="center end"
            >Novo Orçamento
          </q-tooltip>
        </q-btn>
      </template>
    </page-header>
    <q-card>
      <q-item>
        <q-item-section>
          <div class="text-h6 text-indigo-8">
            Filtros
          </div>
        </q-item-section>
        <q-item-section side>
          <div class="text-h6 text-indigo-8">
            <q-btn
              glossy
              :round="$q.screen.lt.sm"
              :label="$q.screen.lt.sm ? '' : 'Buscar'"
              icon="search"
              color="secondary"
              class="text-capitalize text-white"
            ></q-btn>
          </div>
        </q-item-section>
      </q-item>
      <q-separator />
      <q-card-section>
        <div class="q-gutter-y-md q-gutter-x-md row">
          <q-select
            class="col-md-5 col-sm-6 col-xs-12"
            color="indigo-8"
            v-model="orcamento.produto"
            :options="produtos"
            label="Produtos"
            dense
          >
            <template v-slot:prepend>
              <q-icon name="build" />
            </template>
          </q-select>
          <q-input
            class="col-md-2 col-sm-6 col-xs-5"
            v-model="orcamento.criacao"
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
                  v-model="orcamento.criacao"
                  color="indigo-8"
                  mask="DD/MM/YYYY"
                  dense
                >
                  <div class="row items-center justify-end">
                    <q-btn v-close-popup label="Ok" color="indigo-8" flat />
                  </div>
                </q-date>
              </q-popup-proxy>
            </template>
          </q-input>
          <q-input
            class="col-md-2 col-sm-6 col-xs-5"
            v-model="orcamento.encerramento"
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
                  v-model="orcamento.encerramento"
                  color="indigo-8"
                  mask="DD/MM/YYYY"
                  dense
                >
                  <div class="row items-center justify-end">
                    <q-btn v-close-popup label="Ok" color="indigo-8" flat />
                  </div>
                </q-date>
              </q-popup-proxy>
            </template>
          </q-input>
          <q-select
            class="col-lg-1 col-md-2 col-sm-6 col-xs-5"
            color="indigo-8"
            v-model="orcamento.produto"
            :options="produtos"
            label="Status"
            dense
          >
            <template v-slot:prepend>
              <q-icon name="rule" />
            </template>
          </q-select>
          <q-input
            type="number"
            class="col-lg-1 col-md-2 col-sm-2 col-xs-5"
            color="indigo-8"
            v-model="orcamento.quantidade"
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
    <my-table />
  </q-page>
</template>

<script>
import PageHeader from 'src/components/PageHeader.vue'
import MyTable from 'src/components/tables/TableVisits'

export default {
  components: {
    PageHeader,
    MyTable
  },
  data () {
    return {
      orcamento: {},
      produtos: ['Chave']
    }
  }
}
</script>

<style></style>
