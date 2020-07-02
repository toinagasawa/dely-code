<template>
  <div class="container">
    <div>
      <h1 class="title">dely 暗号解読くん v1</h1>
      <div class="input-block">
        <p>暗号を入力する</p>
        <input type="text" v-model="enc_str" />
      </div>
      <div class="answer-block">
        <p>解読後</p>
        <h2>{{answer}}</h2>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'

interface encSet {
  en: string
  num: number
}
interface pokemon {
  index: number
}

export default Vue.extend({
  data() {
    return {
      enc_str:
        'k46927595597b23999455421q67482438962k54959663593t66323632674o97789468516j64445937397y88486785969s72835486740c83666964247g32493592926i57254538568g24323636572x76885739897v32752784694f49994245441b25742799753',
    }
  },
  computed: {
    answer() {
      const keywords: encSet[] = []
      var count = (this.enc_str.match(/([a-zA-Z])/g) || []).length
      if (count === 0) {
        return false
      }
      const str_array = this.enc_str.match(/([a-zA-Z])/g) || []
      let serchedIndex = 0
      str_array.forEach((str, i) => {
        if (this.enc_str.slice(serchedIndex).match(/([a-zA-Z])/) === null)
          return
        const searchRe = this.enc_str.slice(serchedIndex)?.match(/([a-zA-Z])/)
        if (searchRe === null) return
        let posi: number =
          typeof searchRe['index'] === 'number' ? searchRe['index'] : 0
        serchedIndex += posi + 1
        const res = this.enc_str.slice(serchedIndex).match(/[0-9]+/) || []
        const res1 = res[0]
        serchedIndex += res.length
        let num: Number = 1
        let obj: encSet = {
          en: str,
          num: Number(res1),
        }
        keywords.push(obj)
      })
      const alp = this.makeAlphabetsBetween('a', 'z')
      let answer = ''
      keywords.forEach((set: encSet) => {
        const amari = set.num % alp.length
        const currentIndex = alp.indexOf(set.en)
        if (set.num % 2 === 0) {
          //偶数
          if (alp[amari + currentIndex]) {
            answer += alp[amari + currentIndex]
          } else {
            answer += alp[currentIndex - (alp.length - amari)]
          }
        } else {
          //奇数
          if (alp[currentIndex - amari]) {
            answer += alp[currentIndex - amari]
          } else {
            answer += alp[currentIndex + (alp.length - amari)]
          }
        }
      })
      return answer
    },
  },
  methods: {
    makeAlphabetsBetween(start: string, end: string) {
      const s = start.charCodeAt(0)
      const e = end.charCodeAt(0)
      const num = e - s + 1
      return Array.apply(null, new Array(num)).map((v, i) => {
        return String.fromCharCode(s + i)
      })
    },
  },
})
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  padding-top: 60px;
  text-align: center;
}
.input-block {
  margin-top: 40px;
}
.answer-block {
  margin-top: 40px;
}
input {
  display: inline-block;
  width: 600px;
  font-size: 16px;
  padding: 5px;
}
just a .title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
