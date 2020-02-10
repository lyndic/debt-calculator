<template>
  <div>
    <h1>Debt Calculator</h1>
    <h2>Find out how long it will take you to pay off debt!</h2>
    <form v-on:submit.prevent="calculate(income, expenses, owed, interest)">
      <label>Monthly Income:</label>
      <input v-model="income" type="number">
      <label>Monthly Expenses:</label>
      <input v-model="expenses" type="number">
      <label>Total Debt Owed:</label>
      <input v-model="owed" type="number">
      <label>Annual Interest Rate:</label>
      <input v-model="interest" type="number">
      <button type="submit">Calculate</button>
    </form>
    <div>{{result}}</div>
    <button @click="clear" type="button">Start Over</button>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      income: null,
      expenses: null,
      owed: null,
      interest: null,
      result: ''
    }
  },
  methods: {
    clear() {
      this.result = '';
    },
    calculate(income, expenses, owed, interest) {
      let payment = income - expenses;
      let adjustedDebt = parseFloat(owed) + parseFloat(owed * (interest / 100))
      console.log(adjustedDebt)
      let payoffTime = adjustedDebt / payment;
      let conclusion = 'With the $' + payment + ' left over each month, it would take you ' +
        payoffTime + ' months to pay off $' + owed + ' at a ' + interest + '% rate.';
      this.result = `${conclusion}`;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
