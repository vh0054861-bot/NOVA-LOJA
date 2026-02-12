<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Loja - Simples</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 font-sans">

    <div class="flex h-screen">
        <div class="w-64 bg-slate-800 text-white p-6 shadow-xl">
            <h1 class="text-2xl font-bold mb-8 border-b border-slate-700 pb-4 text-blue-400">Minha Loja</h1>
            <nav class="space-y-2">
                <button onclick="showTab('tab-vendas')" class="w-full text-left py-3 px-4 hover:bg-slate-700 rounded transition-colors flex items-center gap-2">🛒 Registrar Venda</button>
                <button onclick="showTab('tab-produtos')" class="w-full text-left py-3 px-4 hover:bg-slate-700 rounded transition-colors flex items-center gap-2">📦 Estoque / Peças</button>
                <button onclick="showTab('tab-relatorio')" class="w-full text-left py-3 px-4 hover:bg-slate-700 rounded transition-colors flex items-center gap-2">📊 Relatórios</button>
            </nav>
        </div>

        <div class="flex-1 p-10 overflow-y-auto">
            
            <div id="tab-vendas" class="tab-content">
                <h2 class="text-3xl font-bold mb-6 text-slate-800">Nova Venda</h2>
                <div class="bg-white p-6 rounded-lg shadow-md max-w-md border-t-4 border-green-500">
                    <label class="block mb-2 font-semibold">Selecione o Produto:</label>
                    <select id="select-produto" class="w-full border p-3 mb-4 rounded bg-gray-50 focus:ring-2 focus:ring-green-500 outline-none">
                        <option value="">Carregando produtos...</option>
                    </select>
                    <button onclick="registrarVenda()" class="bg-green-600 text-white px-6 py-3 rounded-lg hover:bg-green-700 w-full font-bold shadow-lg transition-transform active:scale-95">Finalizar Venda</button>
                </div>
            </div>

            <div id="tab-produtos" class="tab-content hidden">
                <h2 class="text-3xl font-bold mb-6 text-slate-800">Gerenciar Estoque</h2>
                <div class="bg-white p-6 rounded-lg shadow-md max-w-md border-t-4 border-blue-500">
                    <label class="block mb-1 font-semibold">Nome do Item:</label>
                    <input type="text" id="nome-p" placeholder="Ex: Chave de Fenda" class="w-full border p-3 mb-4 rounded bg-gray-50 focus:ring-2 focus:ring-blue-500 outline-none">
                    
                    <label class="block mb-1 font-semibold">Preço (R$):</label>
                    <input type="number" id="preco-p" placeholder="0.00" class="w-full border p-3 mb-6 rounded bg-gray-50 focus:ring-2 focus:ring-blue-500 outline-none">
                    
                    <button onclick="salvarProduto()" class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 w-full font-bold shadow-lg transition-transform active:scale-95">Adicionar ao Catálogo</button>
                </div>
            </div>

            <div id="tab-relatorio" class="tab-content hidden">
                <h2 class="text-3xl font-bold mb-6 text-slate-800">Relatório de Vendas</h2>
                <div class="bg-white rounded-lg shadow-md overflow-hidden">
                    <table class="w-full text-left border-collapse">
                        <thead class="bg-slate-800 text-white">
                            <tr>
                                <th class="p-4">Data</th>
                                <th class="p-4">Produto</th>
                                <th class="p-4">Valor</th>
                            </tr>
                        </thead>
                        <tbody id="lista-vendas-body">
                            </tbody>
                    </table>
                </div>
            </div>

        </div>
    </div>

    <script>
        // Inicia carregando o seletor
        window.onload = () => {
            atualizarSeletorProdutos();
            carregarRelatorio();
        };

        function showTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');
            if(tabId === 'tab-relatorio') carregarRelatorio();
        }

        function salvarProduto() {
            const nome = document.getElementById('nome-p').value;
            const preco = document.getElementById('preco-p').value;

            if(!nome || !preco) return alert("Preencha todos os campos!");

            let produtos = JSON.parse(localStorage.getItem('produtos') || '[]');
            produtos.push({ nome, preco: parseFloat(preco) });
            localStorage.setItem('produtos', JSON.stringify(produtos));

            alert('Produto cadastrado com sucesso!');
            document.getElementById('nome-p').value = "";
            document.getElementById('preco-p').value = "";
            atualizarSeletorProdutos();
        }

        function atualizarSeletorProdutos() {
            const select = document.getElementById('select-produto');
            let produtos = JSON.parse(localStorage.getItem('produtos') || '[]');
            
            select.innerHTML = produtos.length === 0 ? '<option value="">Nenhum produto cadastrado</option>' : '';
            
            produtos.forEach((prod, index) => {
                select.innerHTML += `<option value="${index}">${prod.nome} - R$ ${prod.preco.toFixed(2)}</option>`;
            });
        }

        function registrarVenda() {
            const select = document.getElementById('select-produto');
            const index = select.value;
            if(index === "") return alert("Selecione um produto!");

            let produtos = JSON.parse(localStorage.getItem('produtos') || '[]');
            let vendas = JSON.parse(localStorage.getItem('vendas') || '[]');
            
            const prodVendido = produtos[index];
            vendas.push({
                data: new Date().toLocaleString(),
                nome: prodVendido.nome,
                preco: prodVendido.preco
            });

            localStorage.setItem('vendas', JSON.stringify(vendas));
            alert('Venda registrada!');
        }

        function carregarRelatorio() {
            const tbody = document.getElementById('lista-v
