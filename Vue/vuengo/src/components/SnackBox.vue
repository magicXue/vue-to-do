<template>
  <div class="text-center">
    <v-snackbar
      v-model="bar"
      color= "info"
      timeout="2000"
    >
      {{ text }}
    </v-snackbar>
  </div>
</template>

<script>
export default {
    name:'SnackBox',
    data() {
        return {
          bar: this.snackbar,
        }
    },
    props: {
        text: String,
        snackbar: Boolean,
        snackcolor: String
    },
    watch: {
      snackbar() {
        this.bar = this.snackbar
      }
    }
}
</script>