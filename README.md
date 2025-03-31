<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loja Virtual</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #007BFF;
            color: white;
        }
        button {
            background-color: #28a745;
            color: white;
            padding: 10px;
            border: none;
            cursor: pointer;
            width: 100%;
            border-radius: 5px;
            margin-top: 10px;
        }
        button:hover {
            background-color: #218838;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container" id="register-container">
        <h1>Cadastro</h1>
        <input type="text" id="new-username" placeholder="Novo Usuário">
        <input type="password" id="new-password" placeholder="Nova Senha">
        <button onclick="register()">Cadastrar</button>
        <button onclick="showLogin()">Já tenho uma conta</button>
    </div>

    <div class="container hidden" id="login-container">
        <h1>Login</h1>
        <input type="text" id="username" placeholder="Usuário">
        <input type="password" id="password" placeholder="Senha">
        <button onclick="login()">Entrar</button>
    </div>
    
    <div class="container hidden" id="store-container">
        <h1>Loja Virtual</h1>
        <h2>Produtos</h2>
        <table>
            <thead>
                <tr>
                    <th>Nome</th>
                    <th>Estoque</th>
                    <th>Preço</th>
                </tr>
            </thead>
            <tbody id="product-list"></tbody>
        </table>
        
        <h2>Novo Pedido</h2>
        <input type="text" id="customer-name" placeholder="Nome do Cliente">
        <input type="text" id="customer-address" placeholder="Endereço">
        <input type="text" id="customer-phone" placeholder="Telefone">
        <select id="payment-method">
            <option value="Pix">Pix</option>
            <option value="Cartão">Cartão</option>
            <option value="Dinheiro">Dinheiro</option>
        </select>
        <button onclick="placeOrder()">Fazer Pedido</button>
        
        <h2>Relatórios de Vendas</h2>
        <button onclick="generateReport('daily')">Relatório Diário</button>
        <button onclick="generateReport('monthly')">Relatório Mensal</button>
        <div id="report-output"></div>
    </div>
    
    <script>
        let users = [{ username: "admin", password: "1234" }];
        const sales = [];

        function register() {
            const newUsername = document.getElementById("new-username").value;
            const newPassword = document.getElementById("new-password").value;
            if (newUsername && newPassword) {
                users.push({ username: newUsername, password: newPassword });
                alert("Usuário cadastrado com sucesso!");
                document.getElementById("register-container").classList.add("hidden");
                document.getElementById("login-container").classList.remove("hidden");
            } else {
                alert("Preencha todos os campos!");
            }
        }

        function showLogin() {
            document.getElementById("register-container").classList.add("hidden");
            document.getElementById("login-container").classList.remove("hidden");
        }

        function login() {
            const username = document.getElementById("username").value;
            const password = document.getElementById("password").value;
            const user = users.find(u => u.username === username && u.password === password);
            
            if (user) {
                document.getElementById("login-container").classList.add("hidden");
                document.getElementById("store-container").classList.remove("hidden");
                loadProducts();  // Carregar os produtos quando o usuário logar
            } else {
                alert("Usuário ou senha incorretos!");
            }
        }

        function loadProducts() {
            const productList = document.getElementById("product-list");
            // Simulação de produtos
            const products = [
                { name: "Produto A", stock: 10, price: 25.0 },
                { name: "Produto B", stock: 5, price: 15.0 },
                { name: "Produto C", stock: 8, price: 30.0 }
            ];
            
            products.forEach(product => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td>${product.name}</td>
                    <td>${product.stock}</td>
                    <td>R$ ${product.price.toFixed(2)}</td>
                `;
                productList.appendChild(row);
            });
        }

        function placeOrder() {
            const name = document.getElementById("customer-name").value;
            const payment = document.getElementById("payment-method").value;
            const date = new Date().toISOString().split('T')[0];
            
            if (name) {
                sales.push({ date, name, payment });
                alert(`Pedido realizado por ${name} via ${payment}.`);
            } else {
                alert("Preencha todos os campos!");
            }
        }

        function generateReport(type) {
            const today = new Date().toISOString().split('T')[0];
            const currentMonth = new Date().toISOString().slice(0, 7);
            let filteredSales = [];
            
            if (type === 'daily') {
                filteredSales = sales.filter(sale => sale.date === today);
            } else if (type === 'monthly') {
                filteredSales = sales.filter(sale => sale.date.startsWith(currentMonth));
            }
            
            let reportOutput = `<h3>Relatório ${type === 'daily' ? 'Diário' : 'Mensal'}</h3>`;
            if (filteredSales.length > 0) {
                reportOutput += `<ul>`;
                filteredSales.forEach(sale => {
                    reportOutput += `<li>${sale.date} - Cliente: ${sale.name} - Pagamento: ${sale.payment}</li>`;
                });
                reportOutput += `</ul>`;
            } else {
                reportOutput += `<p>Nenhuma venda registrada.</p>`;
            }
            document.getElementById("report-output").innerHTML = reportOutput;
        }
    </script>
</body>
</html>
