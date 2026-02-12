<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel | Gestão Total</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .aba { display: none; }
        .aba.ativa { display: grid; animation: fadeIn 0.2s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .sidebar-active { background-color: #2563eb; color: white; box-shadow: 0 10px 15px -3px rgba(37, 99, 235, 0.4); }
        .custom-scroll::-webkit-scrollbar { width: 4px; }
        .custom-scroll::-webkit-scrollbar-thumb { background: #e2e8f0; border-radius: 10px; }
    </style>
</head>
<body class="flex h-screen overflow-hidden text-slate-900">

    <aside class="w-72 bg-slate-900 text-slate-400 flex flex-col shadow-2xl h-full">
        <div class="p-8 border-b border-slate-800">
            <h1 class="text-white font-black tracking-tighter text-2xl uppercase">Admin<span class="text-blue-500">Panel</span></h1>
            <p class="text-[10px] text-slate-500 font-bold tracking-widest mt-1">SISTEMA DE GESTÃO</p>
        </div>

        <nav class="flex-1 p-4 space-y-2 mt-6">
            <button onclick="mudar('p', this)" class="sidebar-btn sidebar-active w-full flex items-center gap-4 p-4 rounded-xl transition-all">
                <i class="fa-solid fa-grid-2"></i>
                <span class="font-bold text-sm uppercase tracking-tight">Dashboard</span>
            </button>
            <button onclick="mudar('v', this)" class="sidebar-btn w-full flex items-center gap-4 p-4 rounded-xl transition-all hover:bg-slate-800">
                <i class="fa-solid fa-cart-shopping"></i>
                <span class="font-bold text-sm uppercase tracking-tight">Vendas</span>
            </button>
            <button onclick="mudar('e', this)" class="sidebar-btn w-full flex items-center gap-4 p-4 rounded-xl transition-all hover:bg-slate-800">
                <i class="fa-solid fa-box-open"></i>
                <span class="font-bold text-sm uppercase tracking-tight">Estoque</span>
            </button>
        </nav>

        <div class="p-6 border-t border-slate-800">
            <button onclick="resetar()" class="w-full p-3 text-red-500 hover:bg-red-500/10 rounded-xl transition-all flex items-center justify-center gap-2 font-bold text-xs uppercase">
                <i class="fa-solid fa-trash"></i> Limpar Tudo
            </button>
        </div>
    </aside>

    <main class="flex-1 flex flex-col h-full overflow-hidden">
        
        <header class="bg-white px-10 py-5 flex justify-between items-center border-b border-slate-200 shadow-sm">
            <div>
                <h2 id="titulo-aba" class="text-xl font-black text-slate-800 uppercase tracking-tighter">Resumo Geral</h2>
                <p id="data-display" class="text-[10px] text-slate-400 font-bold uppercase tracking-widest mt-1"></p>
            </div>
            <div class="flex items-center gap-4">
                <div class="h-10 w-10 bg-blue-600 rounded-xl flex items-center justify-center text-white shadow-lg shadow-blue-200">
                    <i class="fa-solid fa-user-tie"></i>
                </div>
            </div>
        </header>

        <div class="flex-1 p-8 overflow-y-auto custom-scroll">
            
            <section id="p" class="aba ativa grid-cols-12 gap-8">
                <div class="col-span-12 lg:col-span-4 space-y-6">
                    <div class="bg-white p-8 rounded-[2rem] shadow-sm border border-slate-200 relative overflow-hidden">
                        <div class="absolute -right-4 -top-4 text-slate-50 text-7xl"><i class="fa-solid fa-wallet"></i></div>
                        <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest">Saldo Disponível</p>
                        <h3 id="txt-caixa" class="text-4xl font-black text-slate-900 mt-2 tracking-tighter">R$ 0,00</h3>
                    </div>

                    <div class="bg-slate-900 p-8 rounded-[2rem] shadow-xl text-white relative overflow-hidden">
                        <div class="absolute -right-4 -top-4 text-white/5 text-7xl"><i class="fa-solid fa-receipt"></i></div>
                        <p class="text-[10px] font-black text-slate-500 uppercase tracking-widest">Vendas Totais</p>
                        <h3 id="txt-vendas" class="text-4xl font-black text-white mt-2 tracking-tighter">0</h3>
                    </div>
                </div>

                <div class="col-span-12 lg:col-span-8 bg-white rounded-[2rem] shadow-sm border border-slate-200 flex flex-col h-full min-h-[500px]">
                    <div class="p-8 border-b border-slate-100 flex justify-between items-center">
                        <h4 class="font-black text-slate-800 uppercase text-xs">Últimas Movimentações</h4>
                        <span class="text-[10px] font-bold text-green-600 bg-green-50 px-3 py-1 rounded-full uppercase italic">Atualizado agora</span>
                    </div>
                    <div class="flex-1 overflow-auto">
                        <table class="w-full text-left">
                            <thead class="bg-slate-50 text-[10px] text-slate-400 font-black uppercase border-b">
                                <tr>
                                    <th class="px-8 py-4 italic">Hora</th>
                                    <th class="px-8 py-4">Descrição</th>
                                    <th class="px-8 py-4 text-right">Valor Líquido</th>
                                </tr>
                            </thead>
                            <tbody id="lista-historico" class="divide-y divide-slate-100 font-medium"></tbody>
                        </table>
                    </div>
                </div>
            </section>

            <section id="v" class="aba grid-cols-1 gap-8 max-w-2xl mx-auto pt-10">
                <div class="bg-white p-12 rounded-[3rem] shadow-2xl border border-slate-200">
                    <div class="text-center mb-10">
                        <h3 class="text-3xl font-black text-slate-900 uppercase tracking-tighter">Registrar Transação</h3>
                        <p class="text-slate-400 text-sm mt-2">Selecione um produto para processar a venda</p>
                    </div>
                    <div class="space-y-6">
                        <select id="sel-venda" class="w-full bg-slate-50 border-2 border-slate-100 p-5 rounded-2xl text-lg font-bold outline-none focus:border-blue-500 transition-all"></select>
                        <button onclick="fazerVenda()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black p-6 rounded-2xl text-xl shadow-lg shadow-blue-200 transition-all active:scale-95 uppercase">
                            Finalizar Venda
                        </button>
                    </div>
                </div>
            </section>

            <section id="e" class="aba grid-cols-12 gap-8">
                <div class="col-span-12 lg:col-span-4">
                    <div class="bg-white p-8 rounded-[2rem] shadow-sm border border-slate-200">
                        <h4 class="font-black text-slate-800 uppercase text-xs mb-6">Cadastro de Itens</h4>
                        <div class="space-y-4">
                            <input id="in-nome" type="text" placeholder="Nome da Peça" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-200 outline-none focus:border-blue-500 font-semibold">
                            <input id="in-preco" type="number" placeholder="Valor (R$)" class="w-full bg-slate-50 p-4 rounded-xl border border-slate-200 outline-none focus:border-blue-500 font-semibold">
                            <button onclick="salvarItem()" class="w-full bg-slate-900 text-white font-black p-4 rounded-xl hover:bg-blue-600 transition-all uppercase text-xs tracking-widest">
                                Adicionar ao Estoque
                            </button>
                        </div>
                    </div>
                </div>
                <div class="col-span-12 lg:col-span-8 bg-white rounded-[2rem] shadow-sm border border-slate-200 overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] text-slate-400 font-black uppercase border-b">
                            <tr><th class="px-8 py-4">Peça/Serviço</th><th class="px-8 py-4 text-right">Preço</th><th class="px-8 py-4 text-center">Ação</th></tr>
                        </thead>
                        <tbody id="lista-e" class="divide-y divide-slate-100 font-bold text-slate-700"></tbody>
                    </table>
                </div>
            </section>

        </div>
    </main>

    <script>
        let DB_ITENS = JSON.parse(localStorage.getItem('f_it')) || [];
        let DB_VENDAS = JSON.parse(localStorage.getItem('f_ve')) || [];

        function init() {
            document.getElementById('data-display').innerText = new Date().toLocaleDateString('pt-br', { day: '2-digit', month: 'long', year: 'numeric' });
            atualizar();
        }

        function atualizar() {
            let soma = DB_VENDAS.reduce((a, b) => a + Number(b.preco), 0);
            document.getElementById('txt-caixa').innerText = soma.toLocaleString('pt-BR', {style: 'currency', currency: 'BRL'});
            document.getElementById('txt-vendas').innerText = DB_VENDAS.length;

            document.getElementById('lista-historico').innerHTML = DB_VENDAS.slice().reverse().slice(0, 10).map(v => `
                <tr class="hover:bg-slate-50 transition-all">
                    <td class="px-8 py-4 text-[10px] text-slate-400 font-bold italic">${v.data}</td>
                    <td class="px-8 py-4 font-bold text-slate-700 uppercase text-xs">${v.nome}</td>
                    <td class="px-8 py-4 text-right text-blue-600 font-black tracking-tighter text-sm">R$ ${Number(v.preco).toFixed(2)}</td>
                </tr>
            `).join('');

            document.getElementById('lista-e').innerHTML = DB_ITENS.map((i, idx) => `
                <tr>
                    <td class="px-8 py-4 uppercase text-xs">${i.nome}</td>
                    <td class="px-8 py-4 text-right font-black text-slate-900">R$ ${Number(i.preco).toFixed(2)}</td>
                    <td class="px-8 py-4 text-center">
                        <button onclick="deletarItem(${idx})" class="text-red-400 hover:text-red-600 transition-all"><i class="fa-solid fa-circle-xmark"></i></button>
                    </td>
                </tr>
            `).join('');

            document.getElementById('sel-venda').innerHTML = DB_ITENS.length ? DB_ITENS.map((i, idx) => `<option value="${idx}">${i.nome.toUpperCase()} - R$ ${Number(i.preco).toFixed(2)}</option>`).join('') : '<option>NENHUM ITEM NO ESTOQUE</option>';
        }

        function mudar(abaId, btn) {
            document.querySelectorAll('.aba').forEach(a => a.classList.remove('ativa'));
            document.getElementById(abaId).classList.add('ativa');
            document.querySelectorAll('.sidebar-btn').forEach(b => b.classList.remove('sidebar-active'));
            btn.classList.add('sidebar-active');
            const titulos = { 'p': 'Resumo Geral', 'v': 'Registrar Transação', 'e': 'Cadastro de Itens' };
            document.getElementById('titulo-aba').innerText = titulos[abaId];
            atualizar();
        }

        function salvarItem() {
            let n = document.getElementById('in-nome').value;
            let p = document.getElementById('in-preco').value;
            if(!n || !p) return;
            DB_ITENS.push({nome: n, preco: p});
            localStorage.setItem('f_it', JSON.stringify(DB_ITENS));
            document.getElementById('in-nome').value = ''; document.getElementById('in-preco').value = '';
            atualizar();
        }

        function fazerVenda() {
            let idx = document.getElementById('sel-venda').value;
            if(idx === "" || !DB_ITENS[idx]) return;
            let h = new Date().toLocaleTimeString('pt-br', {hour: '2-digit', minute:'2-digit'});
            DB_VENDAS.push({...DB_ITENS[idx], data: h});
            localStorage.setItem('f_ve', JSON.stringify(DB_VENDAS));
            mudar('p', document.querySelectorAll('.sidebar-btn')[0]);
        }

        function deletarItem(idx) {
            DB_ITENS.splice(idx, 1);
            localStorage.setItem('f_it', JSON.stringify(DB_ITENS));
            atualizar();
        }

        function resetar() { if(confirm("Limpar base de dados?")) { localStorage.clear(); location.reload(); } }
        init();
    </script>
</body>
</html>
