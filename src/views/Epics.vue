<template>
  <CRow class="mb-4">
    <CCol md v-if="hasEpicCollection">
      <CFormInput v-model="search" type="text" placeholder="Faça sua busca" />
    </CCol>
    <CCol md>
      <CButton
        component="a"
        class="float-end"
        color="primary"
        href="/#/epic"
        role="button"
        >Novo épico</CButton
      >
    </CCol>
  </CRow>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Épicos</strong>
        </CCardHeader>
        <CCardBody>
          <CTable striped hover>
            <CTableHead>
              <CTableRow>
                <CTableHeaderCell scope="col">Título</CTableHeaderCell>
                <CTableHeaderCell scope="col">Data de entrega</CTableHeaderCell>
                <CTableHeaderCell scope="col">Prioridade</CTableHeaderCell>
                <CTableHeaderCell scope="col">Ações</CTableHeaderCell>
              </CTableRow>
            </CTableHead>
            <CTableBody>
              <CTableRow v-for="(epic, key) in epicCollection" :key="key">
                <CTableDataCell>{{ epic.title }}</CTableDataCell>
                <CTableDataCell>{{ epic.dueData }}</CTableDataCell>
                <CTableDataCell>{{ epic.priority }}</CTableDataCell>
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
  name: 'Epics',
  data() {
    return {
      epicCollection: [],
      search: '',
    }
  },
  mounted() {
    this.getEpics()
  },
  computed: {
    hasEpicCollection() {
      return this.epicCollection && this.epicCollection.length ? true : false
    },
  },
  methods: {
    getEpics() {
      const epicCollection = localStorage.getItem('epicCollection')
      if (epicCollection) this.epicCollection = JSON.parse(epicCollection)
    },
    handleRemove(id) {
      const epicCollection = copy(this.epicCollection)
      epicCollection.splice(id, 1)
      localStorage.setItem('epicCollection', JSON.stringify(epicCollection))
      this.epicCollection = epicCollection
    },
  },
}
</script>
