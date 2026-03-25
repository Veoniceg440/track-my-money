# track-my-money
A lightweight money tracker to log income and expenses, monitor balance, and stay financially organized.
<!DOCTYPE html>
<html>
<head>
  <title>Money Tracker</title>
  <style>
    body { font-family: Arial; max-width: 400px; margin: auto; }
    input, button { margin: 5px 0; width: 100%; padding: 8px; }
    li { cursor: pointer; }
  </style>
</head>
<body>

<h2>Money Tracker</h2>

<h3>Balance: $<span id="balance">0</span></h3>

<input id="desc" placeholder="Description">
<input id="amount" placeholder="Amount (+income, -expense)">
<button onclick="addTransaction()">Add</button>

<ul id="list"></ul>

<script>
let transactions = JSON.parse(localStorage.getItem("transactions")) || [];

function saveData() {
  localStorage.setItem("transactions", JSON.stringify(transactions));
}

function addTransaction() {
  let desc = document.getElementById("desc").value;
  let amount = parseFloat(document.getElementById("amount").value);

  if (!desc || isNaN(amount)) return;

  transactions.push({ desc, amount });
  saveData();
  render();

  document.getElementById("desc").value = "";
  document.getElementById("amount").value = "";
}

function render() {
  let list = document.getElementById("list");
  let balance = 0;

  list.innerHTML = "";

  transactions.forEach((t, index) => {
    balance += t.amount;

    let li = document.createElement("li");
    li.textContent = `${t.desc}: $${t.amount}`;

    li.onclick = () => {
      transactions.splice(index, 1);
      saveData();
      render();
    };

    list.appendChild(li);
  });

  document.getElementById("balance").textContent = balance.toFixed(2);
}

// load on start
render();
</script>

</body>
</html>

