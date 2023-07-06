<template>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Versão</strong>
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
                v-model="version.name"
                type="text"
                placeholder="Nome da versão"
                required
              />
              <CFormFeedback invalid> Preencha um título válido </CFormFeedback>
            </CCol>
            <CCol :md="3">
              <CFormLabel for="startDate">Data de início</CFormLabel>
              <CFormInput
                id="startDate"
                v-model="version.startDate"
                type="text"
                placeholder="Data de entrega"
              />
            </CCol>
            <CCol :md="3">
              <CFormLabel for="releaseDate">Data de lançamento</CFormLabel>
              <CFormInput
                id="releaseDate"
                v-model="version.releaseDate"
                type="text"
                placeholder="Data de lançamento"
              />
            </CCol>
            <CCol :md="6">
              <CFormLabel for="responsibles">Responsáveis</CFormLabel>
              <CFormSelect id="responsibles" v-model="version.responsibles">
                <option disabled>Choose...</option>
                <option>...</option>
              </CFormSelect>
              <CFormFeedback invalid>
                Selecione um ou mais responsáveis
              </CFormFeedback>
            </CCol>
            <CCol :md="6">
              <CFormLabel for="description">Descrição (opcional)</CFormLabel>
              <CFormInput
                v-model="version.description"
                type="text"
                placeholder="Descrição da versão"
              />
            </CCol>
            <CCol :xs="12">
              <CButton color="primary" type="submit">Criar versão</CButton>
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
  name: 'Version',
  data() {
    return {
      versionCollection: [],
      version: {
        name: '',
        startDate: '',
        releaseDate: '',
        responsibles: [],
        description: '',
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
    this.getVersions()
  },
  methods: {
    getVersions() {
      const versionCollection = localStorage.getItem('versionCollection')
      if (versionCollection)
        this.versionCollection = JSON.parse(versionCollection)
    },
    async handleSubmit(event) {
      const form = event.currentTarget
      event.preventDefault()
      event.stopPropagation()

      if (!form.checkValidity()) return (this.validatedForm = true)
      this.validatedForm = null

      let versionCollection = this.versionCollection
      const version = copy(this.version)
      versionCollection.push(version)
      localStorage.setItem(
        'versionCollection',
        JSON.stringify(versionCollection),
      )

      event.currentTarget.reset()
      this.$router.push('versions')
    },
  },
}
</script>
