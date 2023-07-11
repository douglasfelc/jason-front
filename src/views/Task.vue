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
                <CCol>
                  <CFormLabel for="title">Título</CFormLabel>
                  <CFormInput
                    id="title"
                    v-model="task.title"
                    type="text"
                    placeholder="Título do épico"
                    required
                  />
                  <CFormFeedback invalid>
                    Preencha um título válido
                  </CFormFeedback>
                </CCol>

                <CCol>
                  <CFormLabel for="description">Descrição</CFormLabel>
                  <QuillEditor
                    id="description"
                    v-model="task.description"
                    :options="quillEditorOptions"
                    style="height: 300px"
                  >
                  </QuillEditor>
                </CCol>

                <CCol>
                  <CButton color="primary" type="submit" @click="save()"
                    >Criar tarefa</CButton
                  >
                </CCol>
              </CCol>
              <CCol :md="4">
                <CCol md>
                  <label>Responsáveis</label>
                  <VueMultiselect
                    v-model="value"
                    tag-placeholder="Add this as new tag"
                    placeholder="Busque ou adicione um responsável"
                    label="name"
                    track-by="code"
                    :options="userCollection"
                    :multiple="true"
                    :taggable="true"
                  ></VueMultiselect>
                </CCol>
                <CCol md>
                  <label>Etiquetas</label>
                  <VueMultiselect
                    v-model="task.taskLabel"
                    tag-placeholder="Add this as new tag"
                    placeholder="Busque ou adicione uma etiqueta"
                    label="name"
                    track-by="name"
                    :options="labelCollection"
                    :multiple="true"
                    :taggable="true"
                  ></VueMultiselect>
                </CCol>
                <CCol md>
                  <label>Projetos (selecionar no menu)</label>
                  <VueMultiselect
                    v-model="value"
                    tag-placeholder="Add this as new tag"
                    placeholder="Busque um adicione um projeto"
                    label="name"
                    track-by="code"
                    :options="projectCollection"
                    :multiple="true"
                    :taggable="true"
                    disabled
                  ></VueMultiselect>
                </CCol>
                <CCol>
                  <CFormLabel for="status">Situação</CFormLabel>
                  <CFormSelect id="status" v-model="task.status">
                    <option
                      v-for="(status, key) in statusCollection"
                      :value="key"
                      :key="key"
                    >
                      {{ status.name }}
                    </option>
                  </CFormSelect>
                  <CFormFeedback invalid> Selecione um épico </CFormFeedback>
                </CCol>
                <CCol>
                  <CFormLabel for="epic">Épicos</CFormLabel>
                  <CFormSelect id="epic" v-model="task.epic">
                    <option
                      v-for="(epic, key) in epicCollection"
                      :value="key"
                      :key="key"
                    >
                      {{ epic.title }}
                    </option>
                  </CFormSelect>
                  <CFormFeedback invalid> Selecione um épico </CFormFeedback>
                </CCol>
                <CCol md>
                  <label>Versões afetadas</label>
                  <VueMultiselect
                    v-model="task.affectedVersions"
                    tag-placeholder="Add this as new tag"
                    placeholder="Busque ou adicione uma versão"
                    label="name"
                    track-by="name"
                    :options="versionCollection"
                    :multiple="true"
                    :taggable="true"
                  ></VueMultiselect>
                </CCol>
                <CCol>
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
              </CCol>
            </CRow>
          </CForm>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import VueMultiselect from 'vue-multiselect'
import { QuillEditor } from '@vueup/vue-quill'
import '@vueup/vue-quill/dist/vue-quill.snow.css'

export default {
  name: 'Task',
  components: { VueMultiselect, QuillEditor },
  data() {
    return {
      quillEditorOptions: {
        debug: 'info',
        modules: {
          toolbar: ['bold', 'italic', 'underline'],
        },
        placeholder: 'Adicione um comentarário...',
        theme: 'snow',
      },
      task: {
        title: '',
        description: '',
        responsibles: [],
        labels: [],
        projects: [],
        status: 0,
        epic: [],
        affectedVersions: [],
      },
      userCollection: [],
      labelCollection: [],
      projectCollection: [],
      statusCollection: [],
      epicCollection: [],
      versionCollection: [],
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
    this.getStatus()
    this.getEpics()
    this.getVersions()
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
    getStatus() {
      const statusCollection = localStorage.getItem('statusCollection')
      if (statusCollection) this.statusCollection = JSON.parse(statusCollection)
    },
    getEpics() {
      const epicCollection = localStorage.getItem('epicCollection')
      if (epicCollection) this.epicCollection = JSON.parse(epicCollection)
    },
    getVersions() {
      const versionCollection = localStorage.getItem('versionCollection')
      if (versionCollection)
        this.versionCollection = JSON.parse(versionCollection)
    },
  },
}
</script>

<style src="vue-multiselect/dist/vue-multiselect.css"></style>
