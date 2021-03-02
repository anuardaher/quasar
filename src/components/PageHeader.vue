<template>
  <q-card class="q-mb-sm">
    <q-item>
      <q-item-section>
        <div class="text-h6 text-primary">
          {{ title }}
        </div>
        <div class="text-subtitle">
          {{ subtitle }}
        </div>
      </q-item-section>
      <q-item-section side>
        <slot name="action"></slot>
      </q-item-section>
    </q-item>
  </q-card>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    subtitle: {
      type: String,
      defualt: () => ''
    }
  }
}
</script>

<style></style>
