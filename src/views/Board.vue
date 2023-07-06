<template>
  <CRow>
    <CCol v-for="(status, key) in statusCollection" :key="key">
      <CCard>
        <CCardBody>
          <CCardTitle>{{ status.name }}</CCardTitle>
          <CCardText> Cards aqui dentro </CCardText>
        </CCardBody>
        <CCardFooter>
          <small class="text-medium-emphasis">Last updated 3 mins ago</small>
        </CCardFooter>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
export default {
  name: 'Board',
  data() {
    return {
      statusCollection: [],
    }
  },
  mounted() {
    this.getStatus()
  },
  methods: {
    getStatus() {
      const statusCollection = localStorage.getItem('statusCollection')
      if (statusCollection) this.statusCollection = JSON.parse(statusCollection)
    },
  },
}
</script>
