<template>
  <div class="main">
    <div class="first-half">
      <ScannerComp ref="scanner" />
    </div>
    <div class="row">
      <button @click="scanQrCode">Scan !</button>
      <p>{{ res }}</p>
    </div>
  </div>
</template>

<script>
import ScannerComp from "./components/Scanner.vue";

export default {
  name: "App",
  components: {
    ScannerComp,
  },
  data() {
    return {
      res: "",
    };
  },
  methods: {
    scanQrCode() {
      this.res = this.$refs.scanner.scanQRCode();
    },
  },
};
</script>

<style>
body {
  margin: 0;
}

.main {
  height: 100dvh;
}

.first-half {
  height: 50%;
  padding: 10px;
}

.row {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

button {
  border-radius: 10px;
  padding: 1rem;
}
</style>
