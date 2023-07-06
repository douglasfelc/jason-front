<template>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Épico</strong>
        </CCardHeader>
        <CCardBody>
          <CForm
            class="row g-3 needs-validation"
            novalidate
            :validated="validatedForm"
            @submit="handleSubmit"
          >
            <CCol :md="6">
              <CFormLabel for="title">Título</CFormLabel>
              <CFormInput
                id="title"
                v-model="epic.title"
                type="text"
                placeholder="Título do épico"
                required
              />
              <CFormFeedback invalid> Preencha um título válido </CFormFeedback>
            </CCol>
            <CCol :md="3">
              <CFormLabel for="dueData">Data de entrega</CFormLabel>
              <CFormInput
                id="dueData"
                v-model="epic.dueData"
                type="text"
                placeholder="Data de entrega"
              />
            </CCol>
            <CCol :md="3">
              <CFormLabel for="priority">Prioridade</CFormLabel>
              <CFormSelect id="priority" v-model="epic.priority">
                <option disabled>Choose...</option>
                <option>...</option>
              </CFormSelect>
              <CFormFeedback invalid> Selecione a prioridade </CFormFeedback>
            </CCol>
            <CCol :md="3">
              <CFormLabel for="responsibles">Responsáveis</CFormLabel>
              <CFormSelect id="responsibles" v-model="epic.responsibles">
                <option disabled>Choose...</option>
                <option>...</option>
              </CFormSelect>
              <CFormFeedback invalid>
                Selecione um ou mais responsáveis
              </CFormFeedback>
            </CCol>
            <CCol :md="3">
              <CFormLabel for="affectedVersions">Versões afetadas</CFormLabel>
              <CFormSelect
                id="affectedVersions"
                v-model="epic.affectedVersions"
              >
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
                v-model="epic.description"
                type="text"
                placeholder="Descrição do épico"
              />
            </CCol>
            <CCol :xs="12">
              <CButton color="primary" type="submit">Criar épico</CButton>
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
  name: 'Epic',
  data() {
    return {
      epicCollection: [],
      epic: {
        title: '',
        dueData: '',
        priority: null,
        responsibles: [],
        affectedVersions: [],
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
    this.getEpics()
  },
  methods: {
    getEpics() {
      const epicCollection = localStorage.getItem('epicCollection')
      if (epicCollection) this.epicCollection = JSON.parse(epicCollection)
    },
    async handleSubmit(event) {
      const form = event.currentTarget
      event.preventDefault()
      event.stopPropagation()

      if (!form.checkValidity()) return (this.validatedForm = true)
      this.validatedForm = null

      let epicCollection = this.epicCollection
      const epic = copy(this.epic)
      epicCollection.push(epic)
      localStorage.setItem('epicCollection', JSON.stringify(epicCollection))

      event.currentTarget.reset()
      this.$router.push('epics')
    },
  },
}
</script>
