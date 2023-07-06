<template>
  <CRow class="mb-4">
    <CCol md v-if="hasVersionCollection">
      <CFormInput v-model="search" type="text" placeholder="Faça sua busca" />
    </CCol>
    <CCol md>
      <CButton
        component="a"
        class="float-end"
        color="primary"
        href="/#/version"
        role="button"
        >Nova versão</CButton
      >
    </CCol>
  </CRow>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Versões</strong>
        </CCardHeader>
        <CCardBody>
          <CTable striped hover>
            <CTableHead>
              <CTableRow>
                <CTableHeaderCell scope="col">Nome</CTableHeaderCell>
                <CTableHeaderCell scope="col">Data de início</CTableHeaderCell>
                <CTableHeaderCell scope="col"
                  >Data de lançamento</CTableHeaderCell
                >
                <CTableHeaderCell scope="col">Ações</CTableHeaderCell>
              </CTableRow>
            </CTableHead>
            <CTableBody>
              <CTableRow v-for="(version, key) in versionCollection" :key="key">
                <CTableDataCell>{{ version.name }}</CTableDataCell>
                <CTableDataCell>{{ version.startDate }}</CTableDataCell>
                <CTableDataCell>{{ version.releaseDate }}</CTableDataCell>
                <CTableDataCell>
                  <CButton color="dark" variant="outline" size="sm">
                    Editar
                  </CButton>
                  <CButton
                    color="danger"
                    variant="outline"
                    size="sm"
                    class="ms-2"
                    @click="handleRemove(key)"
                  >
                    Excluir
                  </CButton>
                </CTableDataCell>
              </CTableRow>
            </CTableBody>
          </CTable>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import { copy } from '@/core/helpers/format'

export default {
  name: 'Versions',
  data() {
    return {
      versionCollection: [],
      search: '',
    }
  },
  mounted() {
    this.getVersions()
  },
  computed: {
    hasVersionCollection() {
      return this.versionCollection && this.versionCollection.length
        ? true
        : false
    },
  },
  methods: {
    getVersions() {
      const versionCollection = localStorage.getItem('versionCollection')
      if (versionCollection)
        this.versionCollection = JSON.parse(versionCollection)
    },
    handleRemove(id) {
      const versionCollection = copy(this.versionCollection)
      versionCollection.splice(id, 1)
      localStorage.setItem(
        'versionCollection',
        JSON.stringify(versionCollection),
      )
      this.versionCollection = versionCollection
    },
  },
}
</script>
