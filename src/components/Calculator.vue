<template>
  <div class="container card mt-4">
    <b-jumbotron class="mt-4">
      <h1>Debt Calculator</h1>
      <h2>Find out how long it will take you to pay off debt!</h2>
    </b-jumbotron>
    <b-form v-on:submit.prevent="calculate(income, expenses, owed, interest)" 
      @submit="onSubmit"
      class="mb-4">
      <b-form-group
        id="input-group-1"
        label="Monthly Income:"
        label-for="input-1"
        description="Household income each month (in dollars)">
        <b-form-input
          id="input-1"
          v-model="income"
          type="float"
          required
          placeholder="2000"></b-form-input>
      </b-form-group>
      <b-form-group
        id="input-group-2"
        label="Monthly Expenses:"
        label-for="input-2"
        description="Household expenses each month (in dollars)">
        <b-form-input 
          id="input-2"
          v-model="expenses"
          type="float"
          required
          placeholder="1400"></b-form-input>
      </b-form-group>
        <form class="form-group" v-for="(debt, index) in debtForm" v-bind:key="index">
          <input class="form-control mb-2" type="float" placeholder="Debt Owed" v-model="debt.owed">
          <input class="form-control mb-2" type="float" placeholder="Annual Interest Rate" v-model="debt.interest">
        </form>
          <!-- <b-form-group
            id="input-group-3"
            label="Total Debt Owed:"
            label-for="input-3"
            description="All debts owed (in dollars)">
            <b-form-input
              id="input-3"
              v-model="debt.owed"
              type="float"
              required
              placeholder="10000"></b-form-input>
          </b-form-group>
          <b-form-group
            id="input-group-4"
            label="Annual Interest Rate:"
            label-for="input-4"
            description="Interest rate of debt (by percentage)">
            <b-form-input
              id="input-4"
              v-model="debt.interest"
              type="float"
              required
              placeholder="5"></b-form-input>
          </b-form-group> -->
        <b-button @click="addDebtField()" class="mb-4">
          Add Debt
        </b-button>
      <b-button type="submit" pill block variant="outline-success">Calculate</b-button>
    </b-form>
    <b-jumbotron class="result">{{result}}</b-jumbotron>
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
      result: '',
      debtForm: [{
        'owed': '',
        'interest': '',
      }],
    }
  },
  methods: {
    calculate(income, expenses, owed, interest) {
      let payment = income - expenses;
      let adjustedDebt = parseFloat(owed) + parseFloat(owed * (interest / 100))
      console.log(adjustedDebt)
      let payoffTime = adjustedDebt / payment;
      let conclusion = 'With the $' + payment + ' left over each month, it would take you ' +
        payoffTime + ' months to pay off $' + owed + ' at a ' + interest + '% rate.';
      this.result = `${conclusion}`;
    },
    addDebtField() {
      this.debtForm.push({
        'owed': '',
        'interest': ''
      }),
      console.log('It has been pressed!')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1 {
    font-size: 50px;
  }
  h2 {
    font-size: 25px;
  }
  .result {
    text-align: center;
  }
</style>
