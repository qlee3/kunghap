<template>
  <v-app>
    <v-toolbar
      app
      height="40"
      color="primary"
    >
      <v-layout
        justify-center
        class="title white--text"
      >
        <div>이름 궁합</div>
      </v-layout>
    </v-toolbar>
    <v-content>
      <v-container>
        <v-layout row wrap justify-center px-0>
          <v-flex xs12>
            <v-img :src="require('../assets/namekunghap.jpg')"></v-img>
          </v-flex>
          <v-flex xs6>
            <v-text-field solo height="30" v-model="from" suffix="님이"></v-text-field>
          </v-flex>
          <v-flex xs6>
            <v-text-field solo height="30" v-model="to" suffix="님을"></v-text-field>
          </v-flex>
          <v-flex xs12>
            <v-btn color="primary" @click="goResult">얼마나 좋아하는지 이름 궁합으로 알아보기</v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    from: '',
    to: ''
  }),
  methods: {
    goResult () {
      if ((this.from.length === 2 || this.from.length === 3) && (this.from.length === 2 || this.from.length === 3)) this.$router.push('result/from/' + this.from + '/to/' + this.to)
      else alert('이름은 두글자 또는 세글자여야 합니다')
    }
  }
}
</script>
