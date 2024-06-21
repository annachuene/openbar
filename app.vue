<template>
  <div class="container">
    <div class="main-content">
      <div class="form-container">
	  
	  <!-- Form to add drinks to the tab -->
	  
        <h1>Open Bar Panel</h1>
        <form @submit.prevent="addRound" class="form">
          <div v-for="(drink, index) in drinks" :key="index" class="form-group">
            <label :for="drink.name">{{ drink.name }} Price: R{{ drink.price.toFixed(2) }}</label>
            <input type="number" v-model.number="drink.quantity" min="0" :id="drink.name" />
          </div>
          <div class="form-group">
            <label for="people">Number of people splitting the bill.(optional):</label>
			
			<!-- Bind input field -->
			
            <input type="number" v-model.number="people" min="1" id="people" />
          </div>
          <button type="submit" class="btn">Add Beverage</button>
        </form>
      </div>
      <div class="open-tab">
        <h2>Beverage Panel</h2>
        <ul v-if="tab.length > 0">
          <li v-for="(item, index) in tab" :key="index">
            <span>{{ item.name }} x{{ item.quantity }}</span>
            <span class="remove-btn" @click="removeFromTab(index)">Remove</span>
          </li>
          <li v-if="people > 1">Number of people splitting the bill: {{ people }}</li>
        </ul>
        <p v-else>No items added to the tab yet.</p>
        <p v-if="tab.length > 0">Total: R{{ total.toFixed(2) }}</p>
        <p v-if="people > 1 && tab.length > 0">Price per person: R{{ (total / people).toFixed(2) }}</p>
		
		<!-- Buttons to export tab as CSV or PDF -->
		
        <div class="export-buttons" v-if="tab.length > 0">
          <button @click="exportCSV" class="btn">Download CSV</button>
          <button @click="exportPDF" class="btn">Download PDF</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { saveAs } from 'file-saver';
import jsPDF from 'jspdf';

export default {
  data() {
    return {
	
	// Array of drinks, price, and amount properties
	
      drinks: [
        { name: 'Beer', price: 45.0, quantity: 0 },
        { name: 'Cider', price: 52.0, quantity: 0 },
        { name: 'Premix', price: 59.0, quantity: 0 },
      ],
	  
	 // Number of people
      people: 1,
	  
	 // Array to store items added to the tab
	   
      tab: [],
    };
  },
  computed: {
  
  //Caculate the total Amount
  
    total() {
      return this.tab.reduce((sum, item) => sum + item.price * item.quantity, 0);
    },
  },
  methods: {
    addRound() {
      this.drinks.forEach((drink) => {
        if (drink.quantity > 0) {
          const existingItem = this.tab.find((item) => item.name === drink.name);
          if (existingItem) {
            existingItem.quantity += drink.quantity;
          } else {
            this.tab.push({ ...drink });
          }
		  //Reset amount to 0
          drink.quantity = 0;
        }
      });
    },
	//Remove item from panel
    removeFromTab(index) {
      this.tab.splice(index, 1);
    },
    exportCSV() {
      const csvContent =
        'data:text/csv;charset=utf-8,' +
        this.tab
          .map((item) => `${item.name},${item.quantity},${(item.price * item.quantity).toFixed(2)}`)
          .join('\n');
      const encodedUri = encodeURI(csvContent);
      saveAs(encodedUri, 'exporttab.csv');
    },
	
	//Export tab as PDF file
	
    exportPDF() {
      const doc = new jsPDF();
      doc.text('Open Bar Tab', 10, 10);
      this.tab.forEach((item, index) => {
        doc.text(
          `${item.name} x${item.quantity}: R${(item.price * item.quantity).toFixed(2)}`,
          10,
          20 + index * 10
        );
      });
      doc.text(`Total: R${this.total.toFixed(2)}`, 10, 20 + this.tab.length * 10);
      if (this.people > 1) {
        doc.text(
          `Price per person: R${(this.total / this.people).toFixed(2)}`,
          10,
          30 + this.tab.length * 10
        );
        doc.text(`Number of people splitting the bill: ${this.people}`, 10, 40 + this.tab.length * 10);
      }
      doc.save('exporttab.pdf');
    },
  },
};
</script>
/* CSS styles section */
<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f0f4f8;
  margin: 0;
  padding: 0;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.main-content {
  width: 100%;
  max-width: 800px;
  display: flex;
  justify-content: space-between;
}

.form-container {
  width: 50%;
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

h1 {
  color: #007bff;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
  margin-bottom: 5px;
  color: #333;
  font-size: 1.2em;
  color: #007bff;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 10px;
}

.btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin-top: 10px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #0056b3;
}

.open-tab {
  width: 40%;
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

h2 {
  color: #007bff;
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 4px;
  background-color: #f8f9fa;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.remove-btn {
  color: #dc3545;
  cursor: pointer;
}

.export-buttons {
  margin-top: 20px;
}

.export-buttons button {
  margin-right: 10px;
}
</style>
