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
            <CFormFloating class="mb-3">
              <CFormInput
                v-model="epic.title"
                type="text"
                placeholder="Título do épico"
                required
              />
              <CFormLabel for="title">Título</CFormLabel>
            </CFormFloating>
            <CFormFloating class="mb-3">
              <CFormInput v-model="epic.dueData" type="text" />
              <CFormLabel for="title">Data de entrega</CFormLabel>
            </CFormFloating>
            <CFormFloating>
              <CFormTextarea
                v-model="epic.description"
                placeholder="Descreva sua tarefa"
                style="height: 100px"
              ></CFormTextarea>
              <CFormLabel for="description">Descrição</CFormLabel>
            </CFormFloating>
            <CButton color="primary" type="submit">Criar épico</CButton>
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

      this.epic = copy(this.defaultEpic)
    },
  },
}
</script>
