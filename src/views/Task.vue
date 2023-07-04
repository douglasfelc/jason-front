<template>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Tarefa</strong>
        </CCardHeader>
        <CCardBody>
          <CForm>
            <CRow :xs="{ gutter: 2 }">
              <CCol md>
                <CFormFloating class="mb-3">
                  <CFormInput
                    v-model="task.title"
                    type="text"
                    placeholder="Título da tarefa"
                  />
                  <CFormLabel for="title">Título</CFormLabel>
                </CFormFloating>
                <CFormFloating>
                  <CFormTextarea
                    v-model="task.description"
                    placeholder="Descreva sua tarefa"
                    style="height: 100px"
                  ></CFormTextarea>
                  <CFormLabel for="description">Descrição</CFormLabel>
                </CFormFloating>
                <CButton color="primary" type="submit" @click="save()"
                  >Enviar</CButton
                >
              </CCol>
              <CCol md>
                <CFormFloating>
                  <CFormSelect
                    v-model="task.responsibles"
                    aria-label="Floating label select example"
                    class="mb-3"
                  >
                    <option>Nenhum</option>
                    <option value="1">One</option>
                    <option value="2">Two</option>
                    <option value="3">Three</option>
                  </CFormSelect>
                  <CFormLabel for="responsibles">Responsáveis</CFormLabel>
                </CFormFloating>
                <CFormFloating>
                  <CFormSelect
                    v-model="task.labels"
                    aria-label="Floating label select example"
                    class="mb-3"
                  >
                    <option
                      v-for="(taskLabel, key) in labelCollection"
                      :value="key"
                      :key="key"
                    >
                      {{ taskLabel.name }}
                    </option>
                  </CFormSelect>
                  <CFormLabel for="labels">Etiquetas</CFormLabel>
                </CFormFloating>
                <CFormFloating>
                  <CFormSelect
                    v-model="task.projects"
                    aria-label="Floating label select example"
                    class="mb-3"
                  >
                    <option>Nenhum</option>
                    <option value="1">One</option>
                    <option value="2">Two</option>
                    <option value="3">Three</option>
                  </CFormSelect>
                  <CFormLabel for="projects">Projetos</CFormLabel>
                </CFormFloating>
                <CFormFloating>
                  <CFormSelect
                    v-model="task.status"
                    aria-label="Floating label select example"
                    class="mb-3"
                  >
                    <option>Em progresso</option>
                    <option value="1">One</option>
                    <option value="2">Two</option>
                    <option value="3">Three</option>
                  </CFormSelect>
                  <CFormLabel for="status">Situação</CFormLabel>
                </CFormFloating>
                <CFormFloating>
                  {{ epicCollection }}
                  <CFormSelect
                    v-model="task.epic"
                    aria-label="Floating label select example"
                    class="mb-3"
                  >
                    <option>Em progresso</option>
                    <option value="1">One</option>
                    <option value="2">Two</option>
                    <option value="3">Three</option>
                  </CFormSelect>
                  <CFormLabel for="epic">Épico (milistone)</CFormLabel>
                </CFormFloating>
                <CFormFloating>
                  <CFormSelect
                    v-model="task.affectedVersions"
                    aria-label="Floating label select example"
                    class="mb-3"
                  >
                    <option>1.0.1</option>
                    <option value="1">One</option>
                    <option value="2">Two</option>
                    <option value="3">Three</option>
                  </CFormSelect>
                  <CFormLabel for="affectedVersions"
                    >Versões afetadas</CFormLabel
                  >
                </CFormFloating>
                <CCard>
                  <CCardBody>
                    <CCardSubtitle class="mb-2 text-medium-emphasis"
                      >Desenvolvimento</CCardSubtitle
                    >
                    <CCardLink href="#">Criar branch (modal)</CCardLink>
                    <CCardLink href="#"
                      >Criar registro de trabalho (modal)</CCardLink
                    >
                  </CCardBody>
                </CCard>
              </CCol>
            </CRow>
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
      task: {
        title: '',
        description: '',
        responsibles: [],
        labels: [],
        projects: [],
        status: 0,
        epic: 0,
        affectedVersions: [],
      },
      labelCollection: '',
      epicCollection: '',
    }
  },
  props: {
    title: {
      type: String,
      default: '',
    },
  },
  mounted() {
    this.getLabels()
    this.getEpics()
  },
  methods: {
    async save() {
      localStorage.setItem('task', JSON.stringify(this.task))
      // const task = localStorage.getItem('task')
      console.log('task', this.task)
    },
    getLabels() {
      const labelCollection = localStorage.getItem('labelCollection')
      if (labelCollection) this.labelCollection = JSON.parse(labelCollection)
    },
    getEpics() {
      this.epicCollection = localStorage.getItem('epicCollection')
    },
  },
  watch: {
    /*
    task(newTask) {
      localStorage.task = newTask
      console.log('localStorage', localStorage)
    },
    */
  },
}
</script>
