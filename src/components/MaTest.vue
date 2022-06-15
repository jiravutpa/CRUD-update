<template>
  <div class="vld-parent">
    <loading
      v-model:active="isLoading"
      :can-cancel="true"
      :on-cancel="onCancel"
      :is-full-page="false"
    />

    <button @click.prevent="doAjax">fetch Data</button>
  </div>
</template>

<script>
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";

export default {
  data() {
    return {
      isLoading: false,
    };
  },
  components: {
    Loading,
  },
  methods: {
    doAjax() {
      this.isLoading = true;
      // simulate AJAX
      setTimeout(() => {
        this.isLoading = false;
      }, 1000);
    },
    onCancel() {
      console.log("User cancelled the loader.");
    },
  },
};
</script>