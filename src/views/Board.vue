<template>
  <CRow>
    <CCol :xs="12">
      <CCard class="mb-4">
        <CCardHeader>
          <strong>Card</strong> <small>Grid cards</small>
        </CCardHeader>
        <CCardBody>
          <DocsExample href="components/card.html/#grid-cards">
            <CRow :xs="{ cols: 1, gutter: 4 }" :md="{ cols: 3 }">
              <CCol xs>
                <CCard>
                  <CCardImage orientation="top" :src="VueImg" />
                  <CCardBody>
                    <CCardTitle>Não iniciado</CCardTitle>
                    <CCardText>
                      This is a wider card with supporting text below as a
                      natural lead-in to additional content. This content is a
                      little bit longer.
                    </CCardText>
                  </CCardBody>
                  <CCardFooter>
                    <small class="text-medium-emphasis"
                      >Last updated 3 mins ago</small
                    >
                  </CCardFooter>
                </CCard>
              </CCol>
              <CCol xs>
                <CCard>
                  <CCardImage orientation="top" :src="VueImg" />
                  <CCardBody>
                    <CCardTitle>Em progresso</CCardTitle>
                    <CCardText>
                      This is a wider card with supporting text below as a
                      natural lead-in to additional content. This content is a
                      little bit longer.
                    </CCardText>
                  </CCardBody>
                  <CCardFooter>
                    <small class="text-medium-emphasis"
                      >Last updated 3 mins ago</small
                    >
                  </CCardFooter>
                </CCard>
              </CCol>
              <CCol xs>
                <CCard>
                  <CCardImage orientation="top" :src="VueImg" />
                  <CCardBody>
                    <CCardTitle>Concluído</CCardTitle>
                    <CCardText>
                      This is a wider card with supporting text below as a
                      natural lead-in to additional content. This content is a
                      little bit longer.
                    </CCardText>
                  </CCardBody>
                  <CCardFooter>
                    <small class="text-medium-emphasis"
                      >Last updated 3 mins ago</small
                    >
                  </CCardFooter>
                </CCard>
              </CCol>
              <CCol xs>
                <CCard>
                  <CCardImage orientation="top" :src="VueImg" />
                  <CCardBody>
                    <CCardTitle>Card title</CCardTitle>
                    <CCardText>
                      This is a wider card with supporting text below as a
                      natural lead-in to additional content. This content is a
                      little bit longer.
                    </CCardText>
                  </CCardBody>
                  <CCardFooter>
                    <small class="text-medium-emphasis"
                      >Last updated 3 mins ago</small
                    >
                  </CCardFooter>
                </CCard>
              </CCol>
            </CRow>
          </DocsExample>
        </CCardBody>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
import VueImg from '@/assets/images/vue.jpg'
export default {
  name: 'Cards',
  setup() {
    return {
      VueImg,
    }
  },
}
</script>
