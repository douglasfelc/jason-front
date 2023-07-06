<template>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Projeto</strong>
        </CCardHeader>
        <CCardBody>
          <CForm
            class="row g-3 needs-validation"
            novalidate
            :validated="validatedForm"
            @submit="handleSubmit"
          >
            <CCol :md="6">
              <CFormLabel for="name">Nome</CFormLabel>
              <CFormInput
                id="name"
                v-model="project.name"
                type="text"
                placeholder="Nome do projeto"
                required
              />
              <CFormFeedback invalid> Preencha um título válido </CFormFeedback>
            </CCol>
            <CCol :md="3">
              <CFormLabel for="shortName">Nome curto (wordpress)</CFormLabel>
              <CFormInput
                id="shortName"
                v-model="project.shortName"
                type="text"
                placeholder="Nome curto"
              />
            </CCol>
            <CCol :md="3">
              <CFormLabel for="template">Template</CFormLabel>
              <CFormSelect id="template" v-model="project.template">
                <option disabled>Choose...</option>
                <option>...</option>
              </CFormSelect>
              <CFormFeedback invalid>
                Selecione um ou mais responsáveis
              </CFormFeedback>
            </CCol>
            <CCol :md="12">
              <CFormLabel for="description">Descrição</CFormLabel>
              <CFormTextarea
                id="description"
                v-model="project.description"
                rows="3"
              ></CFormTextarea>
            </CCol>
            <CCol :xs="12">
              <CButton color="primary" type="submit">Criar projeto</CButton>
            </CCol>
          </CForm>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import { copy } from '@/core/helpers/format'

export default {
  name: 'Project',
  data() {
    return {
      projectCollection: [],
      project: {
        name: '',
        shortName: '',
        template: null,
      },
      validatedForm: null,
    }
  },
  props: {
    title: {
      type: String,
      default: '',
    },
  },
  mounted() {
    this.getProjects()
  },
  methods: {
    getProjects() {
      const projectCollection = localStorage.getItem('projectCollection')
      if (projectCollection)
        this.projectCollection = JSON.parse(projectCollection)
    },
    async handleSubmit(event) {
      const form = event.currentTarget
      event.preventDefault()
      event.stopPropagation()

      if (!form.checkValidity()) return (this.validatedForm = true)
      this.validatedForm = null

      let projectCollection = this.projectCollection
      const project = copy(this.project)
      projectCollection.push(project)
      localStorage.setItem(
        'projectCollection',
        JSON.stringify(projectCollection),
      )

      event.currentTarget.reset()
      this.$router.push('projects')
    },
  },
}
</script>
