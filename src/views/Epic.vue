<template>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Épico</strong>
        </CCardHeader>
        <CCardBody>
          <CForm>
            <CFormFloating class="mb-3">
              <CFormInput
                v-model="epic.title"
                type="text"
                placeholder="Título do épico"
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
            <CButton color="primary" type="submit" @click="save()"
              >Enviar</CButton
            >
          </CForm>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
export default {
  name: 'FloatingLabels',
  data() {
    return {
      epic: {
        title: '',
        dueData: '',
        description: '',
      },
    }
  },
  props: {
    title: {
      type: String,
      default: '',
    },
  },
  methods: {
    async save() {
      localStorage.setItem('epic', JSON.stringify(this.epic))
      const epic = localStorage.getItem('epic')
      console.log('epic', epic)
    },
  },
  watch: {
    /*
    epic(newEpic) {
      localStorage.epic = newEpic
      console.log('localStorage', localStorage)
    },
    */
  },
}
</script>
