# listadesupermercado
Projeto - Lista de Supermercado


README
Projeto: Lista do Supermercado
Descrição
Este projeto é uma aplicação web simples que permite aos usuários criar e gerenciar uma lista de compras de supermercado. A interface é intuitiva, permitindo adicionar, editar e remover itens da lista. Além disso, a lista é salva no armazenamento local do navegador para garantir que os dados persistam mesmo após o fechamento da página.

Estrutura do Projeto
O projeto é composto por um único arquivo HTML que inclui código CSS embutido para estilização e código JavaScript para a funcionalidade da aplicação.

Funcionalidades
Adicionar Item: Permite ao usuário adicionar um novo item à lista, desde que o campo de entrada não esteja vazio e o item não seja duplicado.
Editar Item: Permite ao usuário editar um item existente na lista.
Remover Item: Permite ao usuário remover um item da lista com uma animação de fade-out.
Limpar Lista: Permite ao usuário limpar todos os itens da lista.
Persistência de Dados: Os itens da lista são salvos no armazenamento local do navegador, garantindo que a lista permaneça após o fechamento e reabertura da página.
Como Usar
Adicionar Item: Digite o nome do item no campo de entrada e clique no botão "Adicionar".
Editar Item: Clique no botão "Edit" ao lado do item que deseja editar, faça as alterações e clique em "Save".
Remover Item: Clique no botão "Delete" ao lado do item que deseja remover.
Limpar Lista: Clique no botão "Limpar Lista" para remover todos os itens da lista.
Estrutura do Código
HTML
O arquivo HTML contém a estrutura básica da aplicação, incluindo um campo de entrada para adicionar itens, uma lista para exibir os itens adicionados e botões para adicionar, editar, remover e limpar itens.

CSS
O CSS embutido estiliza a interface da aplicação, incluindo a aparência dos botões, a lista de itens e animações para adicionar e remover itens.

JavaScript
O JavaScript gerencia a lógica da aplicação, incluindo funções para adicionar, editar, remover e salvar itens, bem como para carregar itens do armazenamento local ao iniciar a página.

Código HTML
html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Adicionar à Lista</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
        }
        #container {
            text-align: center;
            width: 90%;
            max-width: 600px;
        }
        input[type="text"] {
            padding: 10px;
            font-size: 16px;
            width: 70%;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            margin-left: 10px;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.2s;
        }
        button:hover {
            background-color: #007BFF;
            color: white;
        }
        button:active {
            transform: scale(0.95);
        }
        #itemList {
            margin-top: 20px;
            padding: 0;
            list-style-type: none;
        }
        .item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: #ffffff;
            border: 1px solid #cccccc;
            margin: 5px 0;
            padding: 10px;
            border-radius: 5px;
            text-align: left;
            transition: background-color 0.2s, transform 0.2s;
        }
        .item button {
            padding: 5px 10px;
            font-size: 14px;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.2s;
        }
        .item button:hover {
            background-color: #ff4d4d;
            color: white;
        }
        .item button:active {
            transform: scale(0.95);
        }
        .item input {
            font-size: 14px;
            width: 100%;
            margin-right: 10px;
            border: none;
            background-color: transparent;
        }
        .item-added {
            animation: fadeIn 0.5s;
        }
        .item-removed {
            animation: fadeOut 0.5s forwards;
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        @keyframes fadeOut {
            from {
                opacity: 1;
                transform: translateY(0);
            }
            to {
                opacity: 0;
                transform: translateY(-20px);
            }
        }
        #controls {
            margin-top: 20px;
        }
        #controls button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            margin: 5px;
            transition: transform 0.2s, background-color 0.2s;
        }
        #controls button:hover {
            background-color: #007BFF;
            color: white;
        }
        #controls button:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Lista do Supermercado</h1>
        <input type="text" id="itemInput" placeholder="Digite um item...">
        <button onclick="addItem()">Adicionar</button>
        <ul id="itemList"></ul>
        <div id="controls">
            <button onclick="clearList()">Limpar Lista</button>
            <p>Total de Itens: <span id="itemCount">0</span></p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', loadItems);

        function addItem() {
            const itemInput = document.getElementById('itemInput');
            const itemList = document.getElementById('itemList');
            const newItemText = itemInput.value.trim();

            if (newItemText !== '') {
                if (!isDuplicate(newItemText)) {
                    const listItem = document.createElement('li');
                    listItem.className = 'item item-added';
                    listItem.innerHTML = `
                        <input type="text" value="${newItemText}" readonly>
                        <button onclick="editItem(event)">Edit</button>
                        <button onclick="removeItem(event)">Delete</button>
                    `;
                    itemList.appendChild(listItem);
                    itemInput.value = '';
                    itemInput.focus();
                    updateItemCount();
                    saveItems();
                } else {
                    alert('Item já existe na lista!');
                }
            } else {
                alert('Por favor, digite um item.');
            }
        }

        function editItem(event) {
            const item = event.target.parentElement;
            const input = item.querySelector('input');
            const isEditing = input.readOnly === false;
            
            if (isEditing) {
                input.readOnly = true;
                event.target.textContent = 'Edit';
                saveItems();
            } else {
                input.readOnly = false;
                input.focus();
                event.target.textContent = 'Save';
            }
        }

        function removeItem(event) {
            const item = event.target.parentElement;
            item.classList.add('item-removed');
            item.addEventListener('animationend', () => {
                item.remove();
                updateItemCount();
                saveItems();
            });
        }

        function clearList() {
            const itemList = document.getElementById('itemList');
            itemList.innerHTML = '';
            updateItemCount();
            saveItems();
        }

        function isDuplicate(newItemText) {
            const items = document.querySelectorAll('#itemList .item input');
            for (let item of items) {
                if (item.value.trim() === newItemText) {
                    return true;
                }
            }
            return false;
        }

        function updateItemCount() {
            const itemCount = document.getElementById('itemCount');
            const items = document.querySelectorAll('#itemList .item');
            itemCount.textContent = items.length;
        }

        function saveItems() {
            const items = [];
            document.querySelectorAll('#itemList .item input').forEach(input => {
                items.push(input.value);
            });
            localStorage.setItem('itemList', JSON.stringify(items));
        }

        function loadItems() {
            const items = JSON.parse(localStorage.getItem('itemList')) || [];
            items.forEach(text => {
                const itemList = document.getElementById('itemList');
                const listItem = document.createElement('li');
                listItem.className = 'item item-added';
                listItem.innerHTML = `
                    <input type="text" value="${text}" readonly>
                    <button onclick="editItem(event)">Edit</button>
                    <button onclick="removeItem(event)">Delete</button>
                `;
                itemList.appendChild(listItem);
            });
            updateItemCount();
        }
    </script>
</body>
</html>


Instalação e Execução
Download do Projeto: Baixe o arquivo HTML do projeto.
Execução: Abra o arquivo HTML em um navegador web moderno (por exemplo, Google Chrome).
Uso: Utilize os controles da interface para adicionar, editar e remover itens da lista de compras.
