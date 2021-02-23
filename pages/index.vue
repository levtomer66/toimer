<template>
  <div class="content countdown-timer">
    <no-ssr>
      <flip-countdown deadline="2021-03-01 00:00:00"></flip-countdown>
    </no-ssr>
    <br /><br />
    <div style="direction: rtl">
        בקשת סכום: <input v-model.number="myval" type="number">
        <input type="button" value="שלח" @click="clicked"/>
        <p>{{ mytext }}</p>
        <p>הצעה נוכחית: ₪{{ proposal.price }}</p>
        <p>סטטוס: {{ proposal.status }}</p>
    </div>
    <div id="approve" style="direction: rtl">
      <button type="button" class="btn btn-success" @click="approveProposal">אישור בקשה</button>
    </div>
      <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">

    </div>

</template>

<style scoped>
.content {
  max-width: 500px;
  margin: auto;
  text-align: right;
}
</style>

<script>
  import FlipCountdown from 'vue2-flip-countdown'

  export default {
    data() {
      return {
        myval: 500,
        mytext: "",
        proposal: {
          price: 0,
          status: ""
        }
      }
    },
    async fetch() {
      const data = await this.$axios.get("https://crudcrud.com/api/c6d57d7648b644f18c317b3837dead21/req")
      if (data.data.length > 0) {
        this.proposal = data.data[0].proposal;
        this.myval = data.data[0].proposal.price
      }
      // console.log(data.data[0].proposal)
    },
    components: { FlipCountdown },
    methods: {
      async clicked() {
        var valid = true;
        if (this.myval <= 1500) {
          this.mytext = "💗 בקשתך התקבלה חיים שלי"
        } else if (this.myval <= 3429) {
          this.mytext = "בקשתך התקבלה, אבל רק כי אני אוהב אותך 😅 "
        } else {
          this.mytext = "🖕🏽"
          valid = false
        }
        if (valid) {
          const data = await this.$axios.get("https://crudcrud.com/api/c6d57d7648b644f18c317b3837dead21/req")
          console.log(data.data[0]._id)
          const postData = { proposal: {price: this.myval, status: "מחכה לאישור"} }
          if (data.data.length > 0) {
            const responseText = await this.$axios.put(`https://crudcrud.com/api/c6d57d7648b644f18c317b3837dead21/req/${data.data[0]._id}`, postData, {headers: {'Content-Type': 'application/json'}})
          } else {
            const responseText = await this.$axios.post("https://crudcrud.com/api/c6d57d7648b644f18c317b3837dead21/req", postData, {headers: {'Content-Type': 'application/json'}})
          }

          this.proposal = postData.proposal
        }

      },
      async approveProposal() {
        const data = await this.$axios.get("https://crudcrud.com/api/c6d57d7648b644f18c317b3837dead21/req")
        if (data.data.length > 0) {
          var password = prompt("enter password");
          if (password === "yeledtoharehad") {
            const postData = { proposal: {price: this.proposal.price, status: "מאושר"} }
            await this.$axios.put(`https://crudcrud.com/api/c6d57d7648b644f18c317b3837dead21/req/${data.data[0]._id}`, postData, {headers: {'Content-Type': 'application/json'}})
            this.proposal = postData.proposal
          } else {
            alert("🖕🏽")
          }
        }

      }
    }
  }
</script>
