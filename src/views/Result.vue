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
        <div>이름 궁합 결과</div>
      </v-layout>
    </v-toolbar>
    <v-content>
      <v-container>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs2 v-for="(character,i) in nametable" :key="i" px-1 my-1>
            <v-card dark color="primary" height="50">
              <v-card-text class="px-0 align-center">
                {{character}}
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs2 v-for="(character,i) in table1" :key="'1'+i" px-1 my-1>
            <v-card dark color="secondary" height="50">
              <v-card-text class="px-0 align-center">
                {{character}}
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs2 v-for="(character,i) in table2" :key="'2'+i" px-1 my-1>
            <v-card dark color="#311e3e" height="50">
              <v-card-text class="px-0 align-center">
                {{character}}
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs2 v-for="(character,i) in table3" :key="'3'+i" px-1 my-1>
            <v-card dark color="#512645" height="50">
              <v-card-text class="px-0 align-center">
                {{character}}
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs2 v-for="(character,i) in table4" :key="'4'+i" px-1 my-1>
            <v-card dark color="#87314e" height="50">
              <v-card-text class="px-0 align-center">
                {{character}}
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs3 v-for="(character,i) in table5" :key="'5'+i" px-1 my-1>
            <v-card dark color="primary" height="50">
              <v-card-text class="px-0 align-center title">
                {{character}}
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs12 px-1 my-1>
            <v-card dark color="primary">
              <v-card-text class="px-0 align-center title">
                {{$route.params.from}} 님은
                {{$route.params.to}} 님을 <br>
                <span class="yellow--text">{{ table5.length === 2 ? ' '+ table5[0] + '' + table5[1] : ' ?'}}%</span> 만큼 좋아합니다
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout justify-center xs12 sm12 md4 row wrap>
          <v-flex xs12 px-1>
            <v-card dark class="yellow align-center subtitle black--text line-height-30">
              <v-btn flat block class="black--text" height="30px" @click="sendLink">
                <v-img class="" :src="require('../assets/kakao_talk_icon.png')" height="25" width="25" max-width="30" aspect-ratio="1:1" contain></v-img>
                카카오톡으로 결과 전송
              </v-btn>
            </v-card>
          </v-flex>
          <v-flex xs12 px-1>
            <v-btn dark block color="secondary" class="align-center subtitle line-height-30" height="30px" @click="$router.push('/')">
              다른 이름으로 궁합 맞춰보러 가기
            </v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    from: [],
    to: [],
    nametable: [],
    table1: [],
    table2: [],
    table3: [],
    table4: [],
    table5: []
  }),
  async mounted () {
    // 파라미터 받기
    const from = this.$route.params.from
    const to = this.$route.params.to
    this.from = from.split('')
    if (from.length === 2) this.from.push(' ')
    this.to = to.split('')
    if (to.length === 2) this.to.push(' ')
    // 글자 획 계산하기
    this.nametable = [this.from[0], this.to[0], this.from[1], this.to[1], this.from[2], this.to[2]]
    for (const str of this.nametable) {
      this.table1.push(this.calScore(this.calCharArr(str)))
    }
    // 숫자 계산
    for (let i = 0; i < this.table1.length - 1; i++) {
      await this.sleep(100)
      this.table2.push((this.table1[i] + this.table1[i + 1]) % 10)
    }
    // 숫자 계산
    for (let i = 0; i < this.table2.length - 1; i++) {
      await this.sleep(100)
      this.table3.push((this.table2[i] + this.table2[i + 1]) % 10)
    }
    // 숫자 계산
    for (let i = 0; i < this.table3.length - 1; i++) {
      await this.sleep(100)
      this.table4.push((this.table3[i] + this.table3[i + 1]) % 10)
    }
    // 숫자 계산
    for (let i = 0; i < this.table4.length - 1; i++) {
      await this.sleep(100)
      this.table5.push((this.table4[i] + this.table4[i + 1]) % 10)
    }
  },
  methods: {
    sendLink () {
      const from = this.$route.params.from
      const to = this.$route.params.to
      const point = this.table5 !== [] ? this.table5[0] + '' + this.table5[1] : '?'
      window.Kakao.Link.sendDefault({
        objectType: 'feed',
        content: {
          title: '재미로 보는 이름 궁합',
          description: from + '님은 ' + to + '님을 ' + point + '%만큼 좋아합니다',
          imageUrl: 'https://firebasestorage.googleapis.com/v0/b/kunghap-d8e2c.appspot.com/o/namekunghap.jpg?alt=media&token=3d30a2fa-c2fc-45a0-aa92-2d009af46ea6',
          link: {
            mobileWebUrl: 'https://kunghap-d8e2c.web.app/result/from/' + from + '/to/' + to,
            webUrl: 'https://kunghap-d8e2c.web.app/result/from/' + from + '/to/' + to
          }
        }
      })
    },
    sleep (ms) {
      return new Promise(resolve => setTimeout(resolve, ms))
    },
    calCharArr (from) {
      let cCho = [ 'ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ' ]
      let cJung = [ 'ㅏ', 'ㅐ', 'ㅑ', 'ㅒ', 'ㅓ', 'ㅔ', 'ㅕ', 'ㅖ', 'ㅗ', 'ㅘ', 'ㅙ', 'ㅚ', 'ㅛ', 'ㅜ', 'ㅝ', 'ㅞ', 'ㅟ', 'ㅠ', 'ㅡ', 'ㅢ', 'ㅣ' ]
      let cJong = [ '', 'ㄱ', 'ㄲ', 'ㄳ', 'ㄴ', 'ㄵ', 'ㄶ', 'ㄷ', 'ㄹ', 'ㄺ', 'ㄻ', 'ㄼ', 'ㄽ', 'ㄾ', 'ㄿ', 'ㅀ', 'ㅁ', 'ㅂ', 'ㅄ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ' ]
      let cho
      let jung
      let jong
      let str = from
      let cnt = str.length
      let chars = []
      let cCode
      for (let i = 0; i < cnt; i++) {
        cCode = str.charCodeAt(i)
        if (cCode === 32) { continue } // 한글이 아닌 경우
        if (cCode < 0xAC00 || cCode > 0xD7A3) {
          chars.push(str.charAt(i))
          continue
        }
        cCode = str.charCodeAt(i) - 0xAC00
        jong = cCode % 28 // 종성
        jung = ((cCode - jong) / 28) % 21 // 중성
        cho = (((cCode - jong) / 28) - jung) / 21 // 초성

        chars.push(cCho[cho], cJung[jung])
        if (cJong[jong] !== '') {
          chars.push(cJong[jong])
        }
      }
      return chars
    },
    calScore (charArr) {
      let result = 0
      for (const character of charArr) {
        result += this.calScoreCharacter(character)
      }
      return result
    },
    calScoreCharacter (character) {
      switch (character) {
        case 'ㄱ' :
          return 2
        case 'ㄴ' :
          return 2
        case 'ㄷ' :
          return 3
        case 'ㄹ' :
          return 5
        case 'ㅁ' :
          return 4
        case 'ㅂ' :
          return 4
        case 'ㅅ' :
          return 2
        case 'ㅇ' :
          return 1
        case 'ㅈ' :
          return 3
        case 'ㅊ' :
          return 4
        case 'ㅋ' :
          return 3
        case 'ㅌ' :
          return 4
        case 'ㅍ' :
          return 4
        case 'ㅎ' :
          return 3
        case 'ㅏ' :
          return 2
        case 'ㅑ' :
          return 3
        case 'ㅓ' :
          return 2
        case 'ㅕ' :
          return 3
        case 'ㅗ' :
          return 2
        case 'ㅛ' :
          return 3
        case 'ㅜ' :
          return 2
        case 'ㅠ' :
          return 3
        case 'ㅡ' :
          return 1
        case 'ㅣ' :
          return 1
        case 'ㅐ' :
          return 3
        case 'ㅒ' :
          return 4
        case 'ㅔ' :
          return 3
        case 'ㅖ' :
          return 4
        case 'ㅘ' :
          return 4
        case 'ㅙ' :
          return 5
        case 'ㅚ' :
          return 3
        case 'ㅝ' :
          return 4
        case 'ㅞ' :
          return 5
        case 'ㅟ' :
          return 3
        case 'ㅢ' :
          return 2
        case 'ㄲ' :
          return 4
        case 'ㄳ' :
          return 4
        case 'ㄵ' :
          return 5
        case 'ㄺ' :
          return 7
        case 'ㄻ' :
          return 9
        case 'ㄼ' :
          return 9
        case 'ㄽ' :
          return 7
        case 'ㄾ' :
          return 9
        case 'ㄿ' :
          return 9
        case 'ㅀ' :
          return 8
        case 'ㅄ' :
          return 6
        case 'ㅆ' :
          return 4
        case 'ㄸ' :
          return 6
        case 'ㅉ' :
          return 6
        case 'ㅃ' :
          return 8
        default :
          return 0
      }
    }
  }
}
</script>

<style lang="scss">
  .align-center{
    text-align: center;
  }

  .line-height-30 {
    line-height: 30px;
  }
  .img-align{
    display:block;
  }
</style>
