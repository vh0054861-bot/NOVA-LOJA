# NOVA-LOJA<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Loja - Simples</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 font-sans">

    <div class="flex h-screen">
        <div class="w-64 bg-slate-800 text-white p-6">
            <h1 class="text-2xl font-bold mb-8">Minha Loja</h1>
            <nav class="space-y-4">
                <a href="#" onclick="showTab('tab-vendas')" class="block py-2 px-4 hover:bg-slate-700 rounded">🛒 Vendas</a>
                <a href="#" onclick="showTab('tab-produtos')" class="block py-2 px-4 hover:bg-slate-700 rounded">📦 Produtos</a>
                <a href="#" onclick="showTab('tab-relatorio')" class="block py-2 px-4 hover:bg-slate-700 rounded">📊 Relatórios</a>
            </nav>
        </div>

        <div class="flex-1 p-10 overflow-y-auto">
            
            <div id="tab-vendas" class="tab-content">
                <h2 class="text-3xl font-bold mb-6 text-slate-800">Registrar Venda</h2>
                <div class="bg-white p-6 rounded-lg shadow-md max-w-md">
                    <label class="block mb-2">Selecione o Produto:</label>
                    <select id="select-produto" class="w-full border p-2 mb-4 rounded"></select>
                    <button onclick="registrarVenda()" class="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700 w-full">Finalizar Venda</button>
                </div>
            </div>

            <div id="tab-produtos" class="tab-content hidden">
                <h2 class="text-3xl font-bold mb-6 text-slate-800">Gerenciar Estoque</h2>
                <div class="bg-white p-6 rounded-lg shadow-md max-w-md">
                    <input type="text" id="nome-p" placeholder="Nome do Produto" class="w-full border p-2 mb-4 rounded">
                    <input type="number" id="preco-p" placeholder="Preço R$" class="w-full border p-2 mb-4 rounded">
                    <button onclick="salvarProduto()" class="bg-blue-600 text-white px-6 py-2 rounded hover:bg-blue-700 w-full">Adicionar Produto</button>
                </div>
            </div>

        </div>
    </div>

    <script>
        function showTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');
        }

        // Lógica simples de salvar no navegador (Local Storage)
        function salvarProduto() {
            const nome = document.getElementById('nome-p').value;
            const preco = document.getElementById('preco-p').value;
            let produtos = JSON.parse(localStorage.getItem('produtos') || '[]');
            produtos.push({nome, preco});
            localStorage.setItem('produtos', JSON.stringify(produtos));
            alert('Produto salvo!');
        }
    </script>
</body>
</html>
