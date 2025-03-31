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
    
    <script>
        let users = [
            { username: "admin", password: "1234" },
            { username: "loja", password: "123" }
        ];

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
                alert("Login bem-sucedido!");
            } else {
                alert("Usuário ou senha incorretos!");
            }
        }
    </script>
</body>
</html>
