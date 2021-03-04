<template>
  <div class="hello">
    <h1>質因數分解</h1>
    <input type = "number" min = "1" v-model="myNum"/>
    <h1 v-html="pHTML"></h1>
    <h4>
      HTML碼: <code ref="h">{{ pHTML }}</code><button @click="copyHTML()">COPY!</button>
    </h4>
    <h4>
      LATEX碼: <code ref="l">{{ pLATEX }}</code><button @click="copyLATEX()">COPY!</button>
    </h4>
    <hr/>
    <h4>
      小於等於{{myNum}}的質數: <code ref="h">{{ primes }}</code>
    </h4>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      myNum: 6
    }
  },
  computed: {
    pLATEX () {
      var r = this.pFS
      var latex = '$' + this.myNum + '=' + r.map((l) => {
        return l[0] + '^{' + l[1] + '}'
      }).join('') + '$'
      return latex
    },
    pHTML () {
      var r = this.pFS
      var html = this.myNum + '=' + r.map((l) => {
        return l[0] + '<sup>' + l[1] + '</sup>'
      }).join('')
      return html
    },
    pFactors () {
      return this.getPF(this.myNum)
    },
    pFS () {
      function getPower(p, n) {
        var power = 0
        while (n % p == 0) {
          n /= p
          power++
        }
        return power
      }
      var pfs = this.pFactors
      var list = pfs.map((o) => {
        return [
          o,
          getPower(o, this.myNum)
        ]
      })
      return list
    },
    primes () {
      return this.range(this.myNum).filter((o) => {
        return this.isPrime(o)
      })
    }
  },
  methods: {
    factors (o) {
      var ms = this.range(o)
      return ms.filter((m) => {
        return o % m == 0
      })
    },
    range(k) {
      k = parseInt(k)
      var list = []
      for (var i = 1; i < k + 1; i++) {
        list.push(i)
      }
      return list
    },
    isPrime(p) {
      return this.factors(p).length == 2
    },
    selectText(element) {
      var range;
      if (document.selection) {
        // IE
        range = document.body.createTextRange();
        range.moveToElementText(element);
        range.select();
      } else if (window.getSelection) {
        range = document.createRange();
        range.selectNode(element);
        window.getSelection().removeAllRanges();
        window.getSelection().addRange(range);
      }
    },
    copyHTML () {
      this.selectText(this.$refs.h); // e.g. <div ref="h">
      document.execCommand('copy');
      alert('HTML Code copied!');
    },
    copyLATEX () {
      this.selectText(this.$refs.l); // e.g. <div ref="h">
      document.execCommand('copy');
      alert('LATEX Code copied!');
    },
    getPF (n) {
      var nums = this.range(n)
      return nums.filter((m) => {
        return n % m == 0 && this.isPrime(m)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  font-size: 22px;
}

code {
  border-radius: 5px;
  border: 1px solid #333;
  background-color: #ddd;
  padding: .2em;
}
</style>
