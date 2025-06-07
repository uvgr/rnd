<template>
  <div>
    <button @click="openModal">Download File</button>

    <div v-if="showModal" class="modal-backdrop">
      <div class="modal">
        <h3>Select File Format</h3>
        <select v-model="selectedFormat">
          <option disabled value="">-- Choose Format --</option>
          <option v-for="format in formats" :key="format" :value="format">
            {{ format.toUpperCase() }}
          </option>
        </select>

        <div class="modal-actions">
          <button @click="downloadFile" :disabled="!selectedFormat">Download</button>
          <button @click="closeModal">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DownloadComponent',
  data() {
    return {
      showModal: false,
      selectedFormat: '',
      formats: ['pdf', 'csv', 'json']
    };
  },
  methods: {
    openModal() {
      this.selectedFormat = '';
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    downloadFile() {
      if (!this.selectedFormat) return;

      const content = 'Sample content for download.';
      const blob = new Blob([content], { type: 'text/plain' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = `sample-file.${this.selectedFormat}`;
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);

      this.closeModal();
    }
  }
};
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal {
  background-color: white;
  padding: 1em;
  border-radius: 8px;
  width: 300px;
  margin: 10% auto;
}

.modal-actions {
  margin-top: 1em;
  display: flex;
  justify-content: space-between;
}
</style>
