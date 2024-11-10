<template>
  <div class="container mt-5">
    <h1 class="text-center">Tạo mã QR</h1>
    <div class="form-group">
      <label for="textInput">Điền chữ hoặc URL để tạo mã QR:</label>
      <input
        type="text"
        id="textInput"
        v-model="text"
        class="form-control"
        placeholder="Điền vào đây..."
        required
      />
    </div>

    <!-- QR Code Size Selection -->
    <div class="form-group mt-3">
      <label for="sizeSelect">Chọn kích thước:</label>
      <select v-model="size" class="form-control" id="sizeSelect">
        <option value="100">100x100</option>
        <option value="150">150x150</option>
        <option value="200">200x200</option>
        <option value="250">250x250</option>
        <option value="300">300x300</option>
        <option value="350">350x350</option>
        <option value="400">400x400</option>
        <option value="450">450x450</option>
        <option value="500">500x500</option>
      </select>
    </div>

    <button
      class="btn btn-primary mt-3 mx-auto d-block"
      @click="generateQRCode"
    >
      Tạo mã QR
    </button>

    <div class="text-center mt-0" v-if="qrCodeDataUrl">
      <img
        :src="qrCodeDataUrl"
        :alt="`Mã QR cho ${text}`"
        :width="size"
        :height="size"
      />
      <br />
      <!-- Download button with dynamic file name -->
      <a
        :href="qrCodeDataUrl"
        :download="fileName"
        class="btn btn-success mt-0"
      >
        Tải về mã QR
      </a>
    </div>
  </div>
</template>

<script>
import QRCode from "qrcode";

export default {
  data() {
    return {
      text: "",
      size: 150, // default size
      qrCodeDataUrl: "",
    };
  },
  computed: {
    fileName() {
      return `qrcode_${this.size}x${this.size}.png`;
    },
  },
  methods: {
    async generateQRCode() {
      try {
        const options = {
          width: this.size, // set the width based on selected size
          height: this.size,
        };
        this.qrCodeDataUrl = await QRCode.toDataURL(this.text, options);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
}
img {
  max-width: 100%;
}
</style>
