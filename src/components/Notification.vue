<template>
  <div>

  </div>
</template>

<script>
import { mapGetters } from 'vuex';
export default {
  name: 'Notification',
  methods: {
    showNotification({ variant, msg, title }) {
      this.$bvToast.toast(msg, {
        title: title,
        variant: variant,
        solid: true
      });
    }
  },
  watch: {
    messagePool: 'showNotification'
  },
  computed: {
    ...mapGetters(['messagePool']),
  }
}
</script>


<style scoped>

</style>