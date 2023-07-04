<template>
  <CModal
    alignment="center"
    :visible="showModalX"
    @close="
      () => {
        showModal = false
      }
    "
  >
    <CModalHeader v-if="this.title">
      <CModalTitle>{{ this.title }}</CModalTitle>
    </CModalHeader>
    <CModalBody>
      <slot></slot>
    </CModalBody>
    <CModalFooter>
      <CButton color="secondary" @click="closeModal()">Close</CButton>
      <CButton color="primary">Save changes</CButton>
    </CModalFooter>
  </CModal>
</template>
<script>
export default {
  data() {
    return {
      showModal: false,
    }
  },
  props: {
    title: {
      type: String,
      default: '',
      required: false,
    },
    showModalX: {
      type: Boolean,
      default: false,
      required: false,
    },
  },
  methods: {
    closeModal() {
      this.showModal = false
    },
  },
}
</script>
