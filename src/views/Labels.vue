<template>
  <CRow class="mb-4">
    <CCol md v-if="hasLabelCollection">
      <CFormInput v-model="search" type="text" placeholder="Faça sua busca" />
    </CCol>
    <CCol md>
      <CButton color="primary float-end" @click="showForm = !showForm"
        >Nova etiqueta</CButton
      >
    </CCol>
  </CRow>
  <CRow v-show="showForm | !hasLabelCollection">
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Nova etiqueta</strong>
          <span
            class="badge rounded-pill float-end"
            :style="{ backgroundColor: taskLabel.color }"
          >
            {{ labelPreview }}
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
                v-model="taskLabel.name"
                type="text"
                placeholder="Nome da etiqueta"
                required
              />
              <CFormFeedback invalid> Preencha um nome válido </CFormFeedback>
            </CCol>
            <CCol :md="4">
              <CFormLabel for="description">Descrição (opcional)</CFormLabel>
              <CFormInput
                v-model="taskLabel.description"
                type="text"
                placeholder="Descrição da etiqueta"
              />
            </CCol>
            <CCol :md="1">
              <CFormLabel for="color">Cor</CFormLabel>
              <CFormInput
                v-model="taskLabel.color"
                type="color"
                placeholder="Cor da etiqueta"
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
                value="Criar etiqueta"
              />
            </CCol>
          </CForm>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
  <CRow v-if="hasLabelCollection">
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Etiquetas</strong>
        </CCardHeader>
        <CCardBody>
          <CTable striped hover>
            <CTableHead>
              <CTableRow>
                <CTableHeaderCell scope="col">Etiqueta</CTableHeaderCell>
                <CTableHeaderCell scope="col">Descrição</CTableHeaderCell>
                <CTableHeaderCell scope="col">Ações</CTableHeaderCell>
              </CTableRow>
            </CTableHead>
            <CTableBody>
              <CTableRow v-for="(taskLabel, key) in labelCollection" :key="key">
                <CTableDataCell>
                  <span
                    class="badge rounded-pill"
                    :style="{ backgroundColor: taskLabel.color }"
                  >
                    {{ taskLabel.name }}
                  </span>
                </CTableDataCell>
                <CTableDataCell>{{ taskLabel.description }}</CTableDataCell>
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
  name: 'Labels',
  beforeCreate() {
    this.defaultTaskLabel = {
      name: '',
      description: '',
      color: '#321fdb',
    }
  },
  data() {
    return {
      labelCollection: [],
      search: '',
      taskLabel: copy(this.defaultTaskLabel),
      showForm: false,
      formSubmit: 'disabled',
      validatedForm: null,
    }
  },
  mounted() {
    this.getLabels()
  },
  computed: {
    labelPreview() {
      if (!this.taskLabel.name) return 'Pré-visualização da etiqueta'
      return this.taskLabel.name
    },
    hasLabelCollection() {
      return this.labelCollection && this.labelCollection.length ? true : false
    },
  },
  methods: {
    getLabels() {
      const labelCollection = localStorage.getItem('labelCollection')
      if (labelCollection) this.labelCollection = JSON.parse(labelCollection)
    },
    handleSubmit(event) {
      const form = event.currentTarget
      event.preventDefault()
      event.stopPropagation()

      if (!form.checkValidity()) return (this.validatedForm = true)
      this.validatedForm = null

      let labelCollection = this.labelCollection
      const taskLabel = copy(this.taskLabel)
      labelCollection.push(taskLabel)
      localStorage.setItem('labelCollection', JSON.stringify(labelCollection))

      this.taskLabel = copy(this.defaultTaskLabel)
      this.showForm = false
    },
    handleRemove(id) {
      const labelCollection = copy(this.labelCollection)
      labelCollection.splice(id, 1)
      localStorage.setItem('labelCollection', JSON.stringify(labelCollection))
      this.labelCollection = labelCollection
    },
    makeCopy(obj) {
      return JSON.parse(JSON.stringify(obj))
    },
  },
}
</script>
