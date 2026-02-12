<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel | Controle Total</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f1f5f9; }
        .aba { display: none; }
        .aba.ativa { display: grid; animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }
        .sidebar-active { background-color: #2563eb; color: white; shadow: 0 4px 12px rgba(37, 99, 235, 0.3); }
        .card-stats { transition: all 0.3s ease; }
        .card-stats:hover { transform: translateY(-5px); }
    </style>
</head>
<body class="flex h-screen overflow-hidden text-slate-900">

    <aside class="w-20 lg:w-64 bg-slate-900 text-slate-400 flex flex-col shadow-2xl transition-all">
        <div class="p-6 border-b border-slate-800 flex items-center justify-center lg:justify-start gap-3">
            <div class="bg-blue-600 p-2 rounded-lg text-white">
                <i class="fa-solid fa-gauge-high"></i>
            </div>
            <h1 class="hidden lg:block text-white font-black tracking-tighter text-xl">ADMIN<span class="text-blue-500">PRO</span></h1>
        </div>

        <nav class="flex-1 p-4 space-y-3 mt-4">
            <button onclick="mudar('p', this)" class="sidebar-btn sidebar-active w-full flex items-center justify-center lg:justify-start gap-4 p-4 rounded-xl transition-all">
                <i class="fa-solid fa-chart-pie"></i>
                <span class="hidden lg:block font-bold text-sm uppercase tracking-wider">Painel</span>
            </button>
            <button onclick="mudar('v', this)" class="sidebar-btn w-full flex items-center justify-center lg:justify-start gap-4 p-4 rounded-xl transition-all hover:bg-slate-800">
                <i class="fa-solid fa-cash-register"></i>
                <span class="hidden lg:block font-bold text-sm uppercase tracking-wider">Vendas</span>
            </button>
            <button onclick="mudar('e', this)" class="sidebar-btn w-full flex items-center justify-center lg:justify-start gap-4 p-4 rounded-xl transition-all hover:bg-slate-800">
                <i class="fa-solid fa-boxes-stacked"></i>
                <span class="hidden lg:block font-bold text-sm uppercase tracking-wider">Estoque</span>
            </button>
        </nav>

        <div class="p-4 border-t border-slate-800">
            <button onclick="resetar()" class="w-full p-3 text-red-500 hover:bg-red-500/10 rounded-lg transition-all">
                <i class="fa-solid fa-power-off"></i>
            </button>
        </div>
    </aside>

    <main class="flex-1 flex flex-col overflow-hidden">
        
        <header class="bg-white px-8 py-4 flex justify-between items-center border-b border-slate-200">
            <div>
                <h2 id="titulo-aba" class="text-2xl font-black text-slate-800 uppercase tracking-tight">Painel de Controle</h2>
                <p id="data-display" class="text-xs text-slate-400 font-bold uppercase tracking-widest"></p>
            </div>
            <div class="flex items-center gap-4 bg-slate-100 p-2 rounded-2xl px-4 border border-slate-200">
                <div class="text-right">
                    <p class="text-[10px] font-black text-slate-400 uppercase">Administrador</p>
                    <p class="text-xs font-bold text-slate-700">Online</p>
                </div>
                <div class="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center text-white text-xs font-bold">AD</div>
            </div>
        </header>

        <div class="flex-1 p-8 overflow-y-auto bg-slate-50">
            
            <section id="p" class="aba ativa grid-cols-1 lg:grid-cols-12 gap-8">
                <div class="lg:col-span-4 space-y-6">
                    <div class="card-stats bg-white p-8 rounded-[2rem] shadow-sm border border-slate-200">
                        <div class="bg-green-100 w-12 h-12 rounded-2xl flex items-center justify-center text-green-600 mb-6">
                            <i class="fa-solid fa-wallet text-xl"></i>
                        </div>
                        <p class="text-xs font-black text-slate-400 uppercase tracking-widest">Saldo em Caixa</p>
                        <h3 id="txt-caixa" class="text-4xl font-black text-slate-900 mt-2 tracking-tighter">R$ 0,00</h3>
                    </div>

                    <div class="card-stats bg-slate-900 p-8 rounded-[2rem] shadow-xl text-white">
                        <div class="bg-blue-500/20 w-12 h-12 rounded-2xl flex items-center justify-center text-blue-400 mb-6 border border-blue-500/30">
                            <i class="fa-solid fa-cart-shopping text-xl"></i>
                        </div>
                        <p class="text-xs font-black text-slate-400 uppercase tracking-widest">Vendas Concluídas</p>
                        <h3 id="txt-vendas" class="text-4xl font-black text-white mt-2 tracking-tighter">0</h3>
                    </div>
                </div>

                <div class="lg:col-span-8 bg-white rounded-[2rem] shadow-sm border border-slate-200 overflow-hidden flex flex-col">
                    <div class="p-8 border-b border-slate-100 flex justify-between items-center">
                        <h4 class="font-black text-slate-800 uppercase text-sm tracking-tighter">Fluxo de Transações</h4>
                        <span class="text-[10px] font-bold text-blue-600 bg-blue-50 px-3 py-1 rounded-full uppercase">Tempo Real</span>
                    </div>
                    <div class="overflow-auto max-h-[400px]">
                        <table class="w-full text-left">
                            <thead class="bg-slate-50 text-[10px] text-slate-400 font-black uppercase border-b">
                                <tr>
                                    <th class="px-8 py-4">Data/Hora</th>
                                    <th class="px-8 py-4">Produto</th>
                                    <th class="px-8 py-4 text-right">Valor</th>
                                </tr>
                            </thead>
                            <tbody id="lista-historico" class="divide-y divide-slate-100 font-medium"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <section id="v" class="aba grid-cols-1 lg:grid-cols-2 gap-8 items-center pt-10">
                <div class="text-center lg:text-left space-y-4">
                    <h3 class="text-5xl font-black text-slate-900 tracking-tighter">Nova <span class="text-blue-600">Venda.</span></h3>
                    <p class="text-slate-500 text-lg">Selecione o item cadastrado no estoque para registrar a entrada de capital no caixa.</p>
                </div>
                <div class="bg-white p-10 rounded-[2.5rem] shadow-2xl border border-slate-200">
                    <div class="space-y-6">
                        <select id="sel-venda" class="w-full bg-slate-100 border-none p-5 rounded-2xl text-lg font-bold outline-none focus:ring-4 focus:ring-blue-500/20 transition-all"></select>
                        <button onclick="fazerVenda()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black p-6 rounded-2xl text-xl shadow-lg shadow-blue-200 transition-all active:scale-95 uppercase tracking-tighter">
                            Confirmar Pagamento
                        </button>
                    </div>
                </div>
            </section>

            <section id="e" class="aba grid-cols-1 lg:grid-cols-12 gap-8">
                <div class="lg:col-span-4 h-fit">
                    <div class="bg-white p-8 rounded-[2rem] shadow-sm border border-slate-200">
                        <h4 class="font-black text-slate-800 uppercase text-sm mb-6">Novo Cadastro</h4>
                        <div class="space-y-4">
                            <input id="in-nome" type="text" placeholder="Nome do Item" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-200 outline-none focus:border-blue-500 font-medium">
                            <input id="in-preco" type="number" placeholder="Preço (0.00)" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-200 outline-none focus:border-blue-500 font-medium">
                            <button onclick="salvarItem()" class="w-full bg-slate-900 text-white font-black p-4 rounded-xl hover:bg-blue-600 transition-all uppercase text-xs tracking-widest">
                                Adicionar
                            </button>
                        </div>
                    </div>
                </div>
                <div class="lg:col-span-8 bg-white rounded-[2rem] shadow-sm border border-slate-200 overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] text-slate-400 font-black uppercase border-b">
                            <tr><th class="px-8 py-4">Item</th><th class="px-8 py-4 text-right">Valor</th><th class="px-8 py-4 text-center">Ação</th></tr>
                        </thead>
                        <tbody id="lista-e" class="divide-y divide-slate-100 font-semibold text-slate-700"></tbody>
                    </table>
                </div>
            </section>

        </div>
    </main>

    <script>
        let DB_ITENS = JSON.parse(localStorage.getItem('sys_it')) || [];
        let DB_VENDAS = JSON.parse(localStorage.getItem('sys_ve')) || [];

        function init() {
            document.getElementById('data-display').innerText = new Date().toLocaleDateString('pt-br', { day: '2-digit', month: 'long', year: 'numeric' });
            atualizar();
        }

        function atualizar() {
            // Financeiro
            let soma = DB_VENDAS.reduce((a, b) => a + Number(b.preco), 0);
            document.getElementById('txt-caixa').innerText = soma.toLocaleString('pt-BR', {style: 'currency', currency: 'BRL'});
            document.getElementById('txt-vendas').innerText = DB_VENDAS.length;

            // Histórico
            document.getElementById('lista-historico').innerHTML = DB_VENDAS.length === 0 ? '<tr><td colspan="3" class="p-10 text-center text-slate-300">Sem registros.</td></tr>' : 
            DB_VENDAS.slice().reverse().slice(0, 10).map(v => `<tr><td class="px-8 py-4 text-[10px] text-slate-400 font-bold">${v.data}</td><td class="px-8 py-4">${v.nome}</td><td class="px-8 py-4 text-right text-blue-600 font-black">R$ ${Number(v.preco).toFixed(2)}</td></tr>`).join('');

            // Estoque
            document.getElementById('lista-e').innerHTML = DB_ITENS.map((i, idx) => `<tr><td class="px-8 py-4">${i.nome}</td><td class="px-8 py-4 text-right font-black">R$ ${Number(i.preco).toFixed(2)}</td><td class="px-8 py-4 text-center"><button onclick="deletarItem(${idx})" class="text-red-400 hover:text-red-600 transition-all"><i class="fa-solid fa-trash-can"></i></button></td></tr>`).join('');

            // Select
            document.getElementById('sel-venda').innerHTML = DB_ITENS.length ? DB_ITENS.map((i, idx) => `<option value="${idx}">${i.nome} - R$ ${Number(i.preco).toFixed(2)}</option>`).join('') : '<option>Cadastre itens no estoque!</option>';
        }

        function mudar(abaId, btn) {
            document.querySelectorAll('.aba').forEach(a => a.classList.remove('ativa'));
            document.getElementById(abaId).classList.add('ativa');
            document.querySelectorAll('.sidebar-btn').forEach(b => b.classList.remove('sidebar-active'));
            btn.classList.add('sidebar-active');
            
            const nomes = { 'p': 'Painel de Controle', 'v': 'Processar Vendas', 'e': 'Gestão de Estoque' };
            document.getElementById('titulo-aba').innerText = nomes[abaId];
            atualizar();
        }

        function salvarItem() {
            let n = document.getElementById('in-nome').value;
            let p = document.getElementById('in-preco').value;
            if(!n || !p) return;
            DB_ITENS.push({nome: n, preco: p});
            localStorage.setItem('sys_it', JSON.stringify(DB_ITENS));
            document.getElementById('in-nome').value = ''; document.getElementById('in-preco').value = '';
            atualizar();
        }

        function fazerVenda() {
            let idx = document.getElementById('sel-venda').value;
            if(idx === "" || !DB_ITENS[idx]) return;
            let h = new Date().toLocaleTimeString('pt-br', {hour: '2-digit', minute:'2-digit'});
            DB_VENDAS.push({...DB_ITENS[idx], data: h});
            localStorage.setItem('sys_ve', JSON.stringify(DB_VENDAS));
            mudar('p', document.querySelectorAll('.sidebar-btn')[0]);
        }

        function deletarItem(idx) {
            DB_ITENS.splice(idx, 1);
            localStorage.setItem('sys_it', JSON.stringify(DB_ITENS));
            atualizar();
        }

        function resetar() { if(confirm("Limpar base de dados?")) { localStorage.clear(); location.reload(); } }
        init();
    </script>
</body>
</html>
