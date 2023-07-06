<template>
  <CRow class="mb-4">
    <CCol md v-if="hasStatusCollection">
      <CFormInput v-model="search" type="text" placeholder="Faça sua busca" />
    </CCol>
    <CCol md>
      <CButton color="primary" class="float-end" @click="showForm = !showForm"
        >Nova situação</CButton
      >
    </CCol>
  </CRow>
  <CRow v-show="showForm | !hasStatusCollection">
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Nova situação</strong>
          <span
            class="badge rounded-pill float-end"
            :style="{ backgroundColor: taskStatus.color }"
          >
            {{ statusPreview }}
          </span>
        </CCardHeader>
        <CCardBody>
          <CForm
            class="row g-3 needs-validation"
            novalidate
            :validated="validatedForm"
            @submit="handleSubmit"
          >
            <CCol :md="3">
              <CFormLabel for="name">Nome</CFormLabel>
              <CFormInput
                id="name"
                v-model="taskStatus.name"
                type="text"
                placeholder="Nome da situação"
                required
              />
              <CFormFeedback invalid> Preencha um nome válido </CFormFeedback>
            </CCol>
            <CCol :md="4">
              <CFormLabel for="description">Descrição (opcional)</CFormLabel>
              <CFormInput
                v-model="taskStatus.description"
                type="text"
                placeholder="Descrição da situação"
              />
            </CCol>
            <CCol :md="1">
              <CFormLabel for="color">Cor</CFormLabel>
              <CFormInput
                v-model="taskStatus.color"
                type="color"
                placeholder="Cor da situação"
                required
              />
            </CCol>
            <CCol :md="2">
              <CFormLabel for="cancelButton">&nbsp;</CFormLabel>
              <CFormInput
                id="cancelButton"
                type="button"
                value="Cancelar"
                @click="showForm = false"
              />
            </CCol>
            <CCol :md="2">
              <CFormLabel for="submitButton">&nbsp;</CFormLabel>
              <CFormInput
                id="submitButton"
                type="submit"
                color="primary"
                value="Criar situação"
              />
            </CCol>
          </CForm>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
  <CRow v-if="hasStatusCollection">
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Situações</strong>
        </CCardHeader>
        <CCardBody>
          <CTable striped hover>
            <CTableHead>
              <CTableRow>
                <CTableHeaderCell scope="col">Situação</CTableHeaderCell>
                <CTableHeaderCell scope="col">Descrição</CTableHeaderCell>
                <CTableHeaderCell scope="col">Ações</CTableHeaderCell>
              </CTableRow>
            </CTableHead>
            <CTableBody>
              <CTableRow
                v-for="(taskStatus, key) in statusCollection"
                :key="key"
              >
                <CTableDataCell>
                  <span
                    class="badge rounded-pill"
                    :style="{ backgroundColor: taskStatus.color }"
                  >
                    {{ taskStatus.name }}
                  </span>
                </CTableDataCell>
                <CTableDataCell>{{ taskStatus.description }}</CTableDataCell>
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
  name: 'Status',
  beforeCreate() {
    this.defaultTaskStatus = {
      name: '',
      description: '',
      color: '#321fdb',
    }
  },
  data() {
    return {
      statusCollection: [],
      search: '',
      taskStatus: copy(this.defaultTaskStatus),
      showForm: false,
      formSubmit: 'disabled',
      validatedForm: null,
    }
  },
  mounted() {
    this.getStatus()
  },
  computed: {
    statusPreview() {
      if (!this.taskStatus.name) return 'Pré-visualização'
      return this.taskStatus.name
    },
    hasStatusCollection() {
      return this.statusCollection && this.statusCollection.length
        ? true
        : false
    },
  },
  methods: {
    getStatus() {
      const statusCollection = localStorage.getItem('statusCollection')
      if (statusCollection) this.statusCollection = JSON.parse(statusCollection)
    },
    handleSubmit(event) {
      const form = event.currentTarget
      event.preventDefault()
      event.stopPropagation()

      if (!form.checkValidity()) return (this.validatedForm = true)
      this.validatedForm = null

      let statusCollection = this.statusCollection
      const taskStatus = copy(this.taskStatus)
      statusCollection.push(taskStatus)
      localStorage.setItem('statusCollection', JSON.stringify(statusCollection))

      this.taskStatus = copy(this.defaultTaskStatus)
      this.showForm = false
    },
    handleRemove(id) {
      const statusCollection = copy(this.statusCollection)
      statusCollection.splice(id, 1)
      localStorage.setItem('statusCollection', JSON.stringify(statusCollection))
      this.statusCollection = statusCollection
    },
  },
}
</script>
