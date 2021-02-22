<template>
  <div class="countdown-timer">
    <no-ssr>
      <flip-countdown deadline="2021-03-01 00:00:00"></flip-countdown>
    </no-ssr>
    <div style="direction: rtl">
        בקשת סכום: <input v-model.number="myval" type="number">
        <input type="button" value="שלח" @click="clicked"/>
        <p>{{ mytext }}</p>

    </div>
    </div>

</template>

<script>
  import FlipCountdown from 'vue2-flip-countdown'

  export default {
    data() {
      return {
        myval: 500,
        mytext: ""
      }
    },
    components: { FlipCountdown },
    methods: {
      clicked() {
        if (this.myval <= 1500) {
          this.mytext = "💗 בקשתך התקבלה חיים שלי"
        } else if (this.myval <= 3429) {
          this.mytext = "בקשתך התקבלה, אבל רק כי אני אוהב אותך 😅 "
        } else {
          this.mytext = "🖕🏽"
        }
      }
    }
  }
</script>
