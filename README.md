# Expense Tracker (ReactJS)
## Date:24/5/25
## Reg no:212223040102

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
## App.js
```import React, { useState } from "react";
import "./App.css";

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState("");
  const [amount, setAmount] = useState("");

  const handleAddTransaction = (e) => {
    e.preventDefault();
    if (!description || !amount) return;

    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount),
    };

    setTransactions([newTransaction, ...transactions]);
    setDescription("");
    setAmount("");
  };

  const handleDelete = (id) => {
    setTransactions(transactions.filter((t) => t.id !== id));
  };

  const balance = transactions.reduce(
    (acc, transaction) => acc + transaction.amount,
    0
  );

  const income = transactions
    .filter((t) => t.amount > 0)
    .reduce((acc, t) => acc + t.amount, 0)
    .toFixed(2);

  const expense = (
    transactions
      .filter((t) => t.amount < 0)
      .reduce((acc, t) => acc + t.amount, 0) * -1
  ).toFixed(2);

  return (
    <div className="container">
      <h2>Expense Tracker</h2>

      <div className="balance-box">
        <h3>Your Balance</h3>
        <h1>${balance.toFixed(2)}</h1>
      </div>

      <div className="summary">
        <div className="income">
          <h4>Income</h4>
          <p className="money plus">+${income}</p>
        </div>
        <div className="expense">
          <h4>Expense</h4>
          <p className="money minus">-${expense}</p>
        </div>
      </div>

      <h3>Transactions</h3>
      <ul className="list">
        {transactions.map((t) => (
          <li key={t.id} className={t.amount < 0 ? "minus" : "plus"}>
            {t.description}
            <span>
              {t.amount < 0 ? "-" : "+"}${Math.abs(t.amount)}
            </span>
            <button
              onClick={() => handleDelete(t.id)}
              className="delete-btn"
            >
              x
            </button>
          </li>
        ))}
      </ul>

      <h3>Add New Transaction</h3>
      <form onSubmit={handleAddTransaction}>
        <div className="form-control">
          <label>Description</label>
          <input
            type="text"
            placeholder="Enter text..."
            value={description}
            onChange={(e) => setDescription(e.target.value)}
          />
        </div>
        <div className="form-control">
          <label>
            Amount <br /> (positive = income, negative = expense)
          </label>
          <input
            type="number"
            placeholder="Enter amount..."
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
          />
        </div>
        <button className="btn">Add Transaction</button>
      </form>
    </div>
  );
}

export default App;
```
## App.css
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
}

.container {
  width: 100%;
  max-width: 600px;
  background: #fffdf7;
  padding: 20px 18px;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  display: grid;
  gap: 18px;
}

h2 {
  text-align: center;
  font-size: 28px;
  font-weight: 700;
  color: #4d2d52;
  text-decoration: underline;
}

.balance-box {
  background: #fffbea;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.balance-box h3 {
  color: #826d6d;
  font-size: 18px;
  margin-bottom: 6px;
}

.balance-box h1 {
  font-size: 36px;
  color: #f57c00;
}

.summary {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  background: #f8f7ff;
  padding: 15px;
  border-radius: 12px;
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.05);
}

.income,
.expense {
  text-align: center;
  padding: 10px;
  border-radius: 8px;
}

.income {
  background: #e9f9f1;
  border-left: 5px solid #16a085;
}

.expense {
  background: #fff1f1;
  border-left: 5px solid #e63946;
}

.money {
  font-size: 20px;
  font-weight: 700;
  margin-top: 5px;
}

.money.plus {
  color: #16a085;
}

.money.minus {
  color: #e63946;
}

.list {
  list-style: none;
  padding: 0;
}

.list li {
  display: grid;
  grid-template-columns: 1fr auto auto;
  align-items: center;
  gap: 10px;
  background: #fefefe;
  padding: 12px 16px;
  border-radius: 10px;
  margin-bottom: 12px;
  border-left: 6px solid;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.list li.plus {
  border-color: #16a085;
}

.list li.minus {
  border-color: #e63946;
}

.list li span {
  font-weight: 700;
  color: #4d2d52;
}

.delete-btn {
  background: #e63946;
  color: #fff;
  border: none;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.25s ease;
}

.delete-btn:hover {
  background: #b02d36;
}

form {
  display: grid;
  gap: 15px;
}

.form-control label {
  display: block;
  margin-bottom: 6px;
  font-weight: 700;
  color: #4d2d52;
}

.form-control input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s ease;
}

.form-control input:focus {
  border-color: #f57c00;
  outline: none;
}

/* Add button -------------------------------------------------------------- */
.btn {
  width: 100%;
  padding: 12px;
  background: linear-gradient(135deg, #f7971e 0%, #ffd200 100%);
  color: #4d2d52;
  font-size: 16px;
  border: none;
  border-radius: 10px;
  font-weight: 700;
  cursor: pointer;
  transition: opacity 0.25s;
}

.btn:hover {
  opacity: 0.85;
}

@media (max-width: 480px) {
  .summary {
    grid-template-columns: 1fr;
  }

  .list li {
    grid-template-columns: 1fr auto;
    grid-template-areas:
      "desc amount"
      "btn  btn";
  }
}
```

## OUTPUT
![image](https://github.com/user-attachments/assets/a29cc2c8-9b44-4060-905f-5b0f9e019c8e)



## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
