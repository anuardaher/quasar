<template>
  <q-dialog v-model="show" :maximized="$q.screen.lt.sm">
    <q-card
      :style="$q.screen.gt.sm ? 'max-width: 800px' : null"
    >
      <q-card-section class="row items-center">
        <div class="text-h6">{{ title }}</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>

      <q-card-section style="max-height: 70vh" class="scroll">
        <slot name="body"></slot>
      </q-card-section>

      <q-card-actions align="right" class="bg-white text-teal">
        <slot name="actions"></slot>
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      show: false
    }
  }
}
</script>
