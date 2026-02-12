<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema Oficina V3</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
</head>
<body class="bg-slate-100 font-sans flex h-screen overflow-hidden">

    <aside class="w-64 bg-slate-900 text-white flex flex-col shadow-2xl">
        <div class="p-6 border-b border-slate-800 text-center">
            <h1 class="text-xl font-bold text-blue-400">OFICINA PRO</h1>
        </div>
        <nav class="flex-1 p-4 space-y-2">
            <button onclick="aba('painel')" class="w-full text-left p-3 rounded hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-chart-line"></i> Painel</button>
            <button onclick="aba('venda')" class="w-full text-left p-3 rounded hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-cart-shopping"></i> Nova Venda</button>
            <button onclick="aba('estoque')" class="w-full text-left p-3 rounded hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-box"></i> Estoque</button>
        </nav>
        <button onclick="reset()" class="p-4 text-xs text-red-500 hover:text-red-300 underline">Resetar Sistema</button>
    </aside>

    <main class="flex-1 p-8 overflow-y-auto">
        
        <div id="painel" class="content">
            <h2 class="text-2xl font-bold mb-6">Resumo Geral</h2>
            <div class="grid grid-cols-2 gap-6 mb-8">
                <div class="bg-white p-6 rounded shadow border-l-4 border-green-500">
                    <p class="text-sm text-gray-500">TOTAL VENDIDO</p>
                    <p id="resumo-total" class="text-3xl font-bold text-green-600">R$ 0,00</p>
                </div>
                <div class="bg-white p-6 rounded shadow border-l-4 border-blue-500">
                    <p class="text-sm text-gray-500">VENDAS HOJE</p>
                    <p id="resumo-vendas" class="text-3xl font-bold">0</p>
                </div>
            </div>
            <div class="bg-white p-4 rounded shadow">
                <h3 class="font-bold mb-4">Últimas Movimentações</h3>
                <div id="lista-historico" class="space-y-2 text-sm"></div>
            </div>
        </div>

        <div id="venda" class="content hidden text-center max-w-md mx-auto">
            <h2 class="text-2xl font-bold mb-6">Registrar Venda</h2>
            <div class="bg-white p-8 rounded shadow-xl border-t-4 border-green-500">
                <select id="sel-prod" class="w-full p-3 border rounded mb-6 text-lg"></select>
                <button onclick="vender()" class="w-full bg-green-600 text-white p-4 rounded font-bold text-lg hover:bg-green-700">FINALIZAR VENDA</button>
            </div>
        </div>

        <div id="estoque" class="content hidden">
            <h2 class="text-2xl font-bold mb-6">Gerenciar Peças</h2>
            <div class="bg-white p-6 rounded shadow mb-8 max-w-md">
                <input type="text" id="add-nome" placeholder="Nome da Peça" class="w-full p-2 border rounded mb-2">
                <input type="number" id="add-preco" placeholder="Preço R$" class="w-full p-2 border rounded mb-4">
                <button onclick="adicionar()" class="w-full bg-blue-600 text-white p-2 rounded font-bold">CADASTRAR</button>
            </div>
            <div class="bg-white p-4 rounded shadow">
                <table class="w-full text-left">
                    <thead class="bg-gray-100 font-bold"><tr><th class="p-2">Item</th><th class="p-2">Preço</th></tr></thead>
                    <tbody id="lista-estoque"></tbody>
                </table>
            </div>
        </div>

    </main>

    <script>
        // Memória do sistema
        let itens = JSON.parse(localStorage.getItem('loja_itens')) || [];
        let vendas = JSON.parse(localStorage.getItem('loja_vendas')) || [];

        function render() {
            // Atualiza Dashboard
            const total = vendas.reduce((acc, v) => acc + Number(v.preco), 0);
            document.getElementById('resumo-total').innerText = 'R$ ' + total.toFixed(2);
            document.getElementById('resumo-vendas').innerText = vendas.length;

            // Atualiza Lista de Estoque
            const listEstoque = document.getElementById('lista-estoque');
            listEstoque.innerHTML = itens.map(i => `<tr><td class="p-2 border-b">${i.nome}</td><td class="p-2 border-b font-bold">R$ ${Number(i.preco).toFixed(2)}</td></tr>`).join('');

            // Atualiza Select de Vendas
            const select = document.getElementById('sel-prod');
            if(itens.length === 0) {
                select.innerHTML = '<option value="">Cadastre peças primeiro!</option>';
            } else {
                select.innerHTML = itens.map((i, index) => `<option value="${index}">${i.nome} - R$ ${Number(i.preco).toFixed(2)}</option>`).join('');
            }

            // Atualiza Histórico
            document.getElementById('lista-historico').innerHTML = vendas.slice().reverse().slice(0, 5).map(v => `<div class="p-2 bg-gray-50 rounded border flex justify-between"><span>${v.nome}</span><span class="font-bold text-green-600">R$ ${Number(v.preco).toFixed(2)}</span></div>`).join('');
        }

        function aba(id) {
            document.querySelectorAll('.content').forEach(c => c.classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
            render();
        }

        function adicionar() {
            const n = document.getElementById('add-nome').value;
            const p = document.getElementById('add-preco').value;
            if(!n || !p) return alert("Preencha tudo!");
            itens.push({nome: n, preco: p});
            localStorage.setItem('loja_itens', JSON.stringify(itens));
            document.getElementById('add-nome').value = '';
            document.getElementById('add-preco').value = '';
            render();
            alert("Salvo!");
        }

        function vender() {
            const idx = document.getElementById('sel-prod').value;
            if(idx === "" || itens.length === 0) return alert("Selecione um item!");
            const p = itens[idx];
            vendas.push({nome: p.nome, preco: p.preco});
            localStorage.setItem('loja_vendas', JSON.stringify(vendas));
            alert("Venda realizada!");
            aba('painel');
        }

        function reset() {
            if(confirm("Apagar tudo?")) { localStorage.clear(); location.reload(); }
        }

        // Inicia o sistema
        render();
    </script>
</body>
</html>
