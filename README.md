<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Compras no Mercado</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: var(--background-color, #e0e7ff);
            color: var(--text-color, #333);
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }
        main {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: left;
            width: 100%;
            max-width: 800px;
            padding: 20px;
            box-sizing: border-box;
        }
        h1 {
            color: var(--heading-color, #333);
        }
        .theme-selector {
            margin: 10px;
        }
        .input-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin: 20px 0;
            width: 100%;
            max-width: 400px;
        }
        .input-container input, .input-container textarea, .input-container select {
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
            font-size: 16px;
            width: 100%;
            box-sizing: border-box;
        }
        .input-container button {
            padding: 10px 20px;
            background-color: var(--button-bg-color, #4CAF50);
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s;
            width: 100%;
            box-sizing: border-box;
        }
        .input-container button:hover {
            background-color: var(--button-hover-color, #45a049);
        }
        ul {
            list-style-type: none;
            padding: 0;
            width: 100%;
            max-width: 400px;
        }
        li {
            padding: 10px;
            margin: 5px 0;
            border-radius: 5px;
            background-color: #f4f4f4;
            position: relative;
            transition: background-color 0.3s, transform 0.2s ease-in-out;
            cursor: pointer; /* Changed cursor to pointer */
        }
        li:hover {
            background-color: #e0e0e0; /* Optional: Add a hover effect */
        }
        li .button-group {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            gap: 10px;
        }
        li button {
            padding: 5px 10px;
            background-color: var(--button-bg-color, #4CAF50);
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        li button:hover {
            background-color: var(--button-hover-color, #45a049);
        }
        .highlight {
            background-color: var(--highlight-color, #039689) !important;
            font-weight: bold;
        }
        footer {
            width: 100%;
            background-color: var(--button-bg-color, #4CAF50);
            text-align: center;
            padding: 10px 0;
            color: white;
        }
        /* Modal styles */
        .modal {
            display: none; 
            position: fixed; 
            z-index: 1; 
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto; 
            background-color: rgb(0,0,0); 
            background-color: rgba(0,0,0,0.4); 
            padding-top: 60px;
        }
        .modal-content {
            background-color: #fefefe;
            margin: 5% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
            max-width: 500px;
            border-radius: 10px;
            text-align: center;
        }
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }
        .close:hover,
        .close:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <main>
        <h1>Lista de Compras no Mercado</h1>

        <!-- Theme Selector -->
        <div class="theme-selector">
            <label for="theme">Escolha o Tema: </label>
            <select id="theme" onchange="changeTheme(this.value)">
                <option value="default">Padrão</option>
                <option value="dark">Escuro</option>
                <option value="light">Claro</option>
            </select>
        </div>

        <div class="input-container">
            <input type="text" id="item-title" placeholder="Nome do Alimento/Objeto">
            <div style="display: flex; gap: 10px;">
                <input type="number" id="item-quantity" placeholder="Quantidade">
                <select id="item-unit">
                    <option value="g">Gramas (g)</option>
                    <option value="u">Unidades (u)</option>
                </select>
            </div>
            <input type="file" id="item-image">
            <button onclick="addItem()">Adicionar</button>
        </div>
        <ul id="item-list"></ul>
    </main>

    <footer>
        Desenvolvido por Gabriel Oliveira Chaves dos Santos e Erick da Costa Correia
    </footer>

    <!-- Modal -->
    <div id="item-modal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <h2 id="modal-title"></h2>
            <p><strong>Quantidade:</strong> <span id="modal-quantity"></span> <span id="modal-unit"></span></p>
            <img id="modal-image" src="" alt="Imagem do Item" style="max-width: 100%; border-radius: 10px;">
        </div>
    </div>

    <script>
        // Theme change functionality
        function changeTheme(theme) {
            if (theme === 'dark') {
                document.documentElement.style.setProperty('--background-color', '#333');
                document.documentElement.style.setProperty('--text-color', '#00000');
                document.documentElement.style.setProperty('--button-bg-color', '#444');
                document.documentElement.style.setProperty('--button-hover-color', '#555');
                document.documentElement.style.setProperty('--highlight-color', '#222');
                document.documentElement.style.setProperty('--heading-color', '#f0f0f0');
            } else if (theme === 'light') {
                document.documentElement.style.setProperty('--background-color', '#f0f0f0');
                document.documentElement.style.setProperty('--text-color', '#333');
                document.documentElement.style.setProperty('--button-bg-color', '#ddd');
                document.documentElement.style.setProperty('--button-hover-color', '#ccc');
                document.documentElement.style.setProperty('--highlight-color', '#bbb');
                document.documentElement.style.setProperty('--heading-color', '#333');
            } else {
                document.documentElement.style.setProperty('--background-color', '#e0e7ff');
                document.documentElement.style.setProperty('--text-color', '#333');
                document.documentElement.style.setProperty('--button-bg-color', '#4CAF50');
                document.documentElement.style.setProperty('--button-hover-color', '#45a049');
                document.documentElement.style.setProperty('--highlight-color', '#039689');
                document.documentElement.style.setProperty('--heading-color', '#333');
            }
        }

        function addItem() {
            const title = document.getElementById('item-title').value;
            const quantity = document.getElementById('item-quantity').value;
            const unit = document.getElementById('item-unit').value;
            const imageInput = document.getElementById('item-image');
            const listItem = document.createElement('li');

            if (title === "" || quantity === "" || unit === "") {
                alert("Por favor, preencha todos os campos.");
                return;
            }

            const imageUrl = imageInput.files[0] ? URL.createObjectURL(imageInput.files[0]) : '';

            listItem.innerHTML = `
                <div onclick="openModal('${title}', '${quantity}', '${unit}', '${imageUrl}')">
                    <span>${title}</span>
                </div>
                <div class="button-group">
                    <button onclick="highlightItem(this)">Destacar</button>
                    <button onclick="removeItem(this)">Remover</button>
                </div>
            `;
            document.getElementById('item-list').appendChild(listItem);

            // Clear inputs
            document.getElementById('item-title').value = "";
            document.getElementById('item-quantity').value = "";
            document.getElementById('item-image').value = "";
        }

        function highlightItem(button) {
            const listItem = button.parentNode.parentNode;
            listItem.classList.toggle('highlight');
        }

        function removeItem(button) {
            const listItem = button.parentNode.parentNode;
            listItem.remove();
        }

        function openModal(title, quantity, unit, imageUrl) {
            document.getElementById('modal-title').innerText = title;
            document.getElementById('modal-quantity').innerText = quantity;
            document.getElementById('modal-unit').innerText = unit;
            document.getElementById('modal-image').src = imageUrl;
            document.getElementById('item-modal').style.display = "block";
        }

        function closeModal() {
            document.getElementById('item-modal').style.display = "none";
        }
    </script>
</body>
</html>
