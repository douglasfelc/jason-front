<template>
  <CRow class="mb-4">
    <CCol md v-if="hasProjectCollection">
      <CFormInput v-model="search" type="text" placeholder="Faça sua busca" />
    </CCol>
    <CCol md>
      <CButton
        component="a"
        class="float-end"
        color="primary"
        href="/#/project"
        role="button"
        >Novo projeto</CButton
      >
    </CCol>
  </CRow>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Projetos</strong>
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
              <CTableRow v-for="(project, key) in projectCollection" :key="key">
                <CTableDataCell>{{ project.name }}</CTableDataCell>
                <CTableDataCell>{{ project.startDate }}</CTableDataCell>
                <CTableDataCell>{{ project.releaseDate }}</CTableDataCell>
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
  name: 'Projects',
  data() {
    return {
      projectCollection: [],
      search: '',
    }
  },
  mounted() {
    this.getProjects()
  },
  computed: {
    hasProjectCollection() {
      return this.projectCollection && this.projectCollection.length
        ? true
        : false
    },
  },
  methods: {
    getProjects() {
      const projectCollection = localStorage.getItem('projectCollection')
      if (projectCollection)
        this.projectCollection = JSON.parse(projectCollection)
    },
    handleRemove(id) {
      const projectCollection = copy(this.projectCollection)
      projectCollection.splice(id, 1)
      localStorage.setItem(
        'projectCollection',
        JSON.stringify(projectCollection),
      )
      this.projectCollection = projectCollection
    },
  },
}
</script>
