<template>
  <q-page class="q-pa-sm">
    <page-header
      title="Orçamentos"
      subtitle="Liste e encontre novos orçamentos"
    >
      <template v-slot:action>
        <q-btn
          to="/orcamento/novo"
          icon="add"
          color="indigo-8"
          round
          glossy
          class="text-capitalize text-white"
        />
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
      <q-card-section> </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import PageHeader from 'src/components/PageHeader.vue'

export default {
  components: {
    PageHeader
  }
}
</script>

<style></style>
