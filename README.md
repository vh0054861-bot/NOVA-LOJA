<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Enterprise Admin | Sistema de Gestão Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; margin: 0; padding: 0; }
        .aba { display: none; width: 100%; height: 100%; }
        .aba.ativa { display: block; animation: fadeIn 0.3s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
        .sidebar-active { background-color: #4f46e5; color: white; border-right: 4px solid #818cf8; }
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        input:focus, select:focus { border-color: #6366f1 !important; ring: 2px; ring-color: #818cf8; }
    </style>
</head>
<body class="flex h-screen overflow-hidden text-slate-900">

    <aside class="w-80 bg-[#0f172a] text-slate-400 flex flex-col h-full flex-shrink-0 z-50 shadow-2xl">
        <div class="p-8 border-b border-slate-800/50 flex items-center gap-3">
            <div class="bg-indigo-600 p-2.5 rounded-xl text-white shadow-lg shadow-indigo-500/20">
                <i class="fa-solid fa-layer-group text-xl"></i>
            </div>
            <div>
                <h1 class="text-white font-extrabold text-xl tracking-tight leading-none uppercase">Admin<span class="text-indigo-400">Pro</span></h1>
                <span class="text-[10px] text-slate-500 font-bold uppercase tracking-[2px]">Enterprise v5.0</span>
            </div>
        </div>

        <nav class="flex-1 p-6 space-y-2 overflow-y-auto custom-scrollbar">
            <button onclick="mudar('p', this)" class="sidebar-btn sidebar-active w-full flex items-center gap-4 p-4 rounded-xl transition-all font-semibold text-sm uppercase tracking-wide">
                <i class="fa-solid fa-chart-pie text-lg"></i> Painel de Controle
            </button>
            <button onclick="mudar('v', this)" class="sidebar-btn w-full flex items-center gap-4 p-4 rounded-xl transition-all hover:bg-slate-800/50 font-semibold text-sm uppercase tracking-wide">
                <i class="fa-solid fa-cart-shopping text-lg"></i> Processar Venda
            </button>
            <button onclick="mudar('e', this)" class="sidebar-btn w-full flex items-center gap-4 p-4 rounded-xl transition-all hover:bg-slate-800/50 font-semibold text-sm uppercase tracking-wide">
                <i class="fa-solid fa-boxes-stacked text-lg"></i> Gestão de Inventário
            </button>
        </nav>

        <div class="p-6 border-t border-slate-800/50">
            <button onclick="resetar()" class="w-full p-4 rounded-xl border border-red-500/30 text-red-400 hover:bg-red-500 hover:text-white transition-all text-xs font-black uppercase tracking-widest flex items-center justify-center gap-2">
                <i class="fa-solid fa-triangle-exclamation"></i> Limpar Banco de Dados
            </button>
        </div>
    </aside>

    <main class="flex-1 flex flex-col bg-slate-50 relative">
        
        <header class="bg-white px-12 py-6 border-b border-slate-200 flex justify-between items-center shadow-sm z-10">
            <div class="flex items-center gap-4">
                <div class="h-8 w-1 bg-indigo-600 rounded-full"></div>
                <h2 id="titulo-aba" class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Painel de Controle</h2>
            </div>
            <div class="flex items-center gap-8">
                <div class="text-right">
                    <p id="data-display" class="text-[10px] font-black text-slate-400 uppercase tracking-widest"></p>
                    <p class="text-xs font-bold text-slate-600">Admin Logged <span class="text-green-500 text-[8px] ml-1">● ONLINE</span></p>
                </div>
                <div class="w-12 h-12 bg-slate-100 rounded-2xl flex items-center justify-center text-indigo-600 border border-slate-200 font-black">AD</div>
            </div>
        </header>

        <div class="flex-1 overflow-y-auto p-12 custom-scrollbar">
            
            <section id="p" class="aba ativa">
                <div class="grid grid-cols-1 xl:grid-cols-3 gap-8 mb-12">
                    <div class="bg-white p-10 rounded-[2.5rem] shadow-sm border border-slate-200 relative overflow-hidden group">
                        <div class="absolute -right-6 -bottom-6 text-indigo-50/50 text-9xl group-hover:scale-110 transition-transform"><i class="fa-solid fa-wallet"></i></div>
                        <p class="text-xs font-black text-slate-400 uppercase tracking-[2px] mb-2">Faturamento Líquido</p>
                        <h3 id="txt-caixa" class="text-5xl font-black text-slate-900 tracking-tighter relative z-10">R$ 0,00</h3>
                        <div class="mt-6 flex items-center text-green-500 text-xs font-bold relative z-10">
                            <i class="fa-solid fa-arrow-trend-up mr-2"></i> Atualizado em tempo real
                        </div>
                    </div>

                    <div class="bg-indigo-600 p-10 rounded-[2.5rem] shadow-xl shadow-indigo-200 text-white relative overflow-hidden group">
                        <div class="absolute -right-6 -bottom-6 text-white/10 text-9xl group-hover:scale-110 transition-transform"><i class="fa-solid fa-receipt"></i></div>
                        <p class="text-xs font-black text-indigo-200 uppercase tracking-[2px] mb-2">Volume de Vendas</p>
                        <h3 id="txt-vendas" class="text-5xl font-black text-white tracking-tighter relative z-10">0</h3>
                        <div class="mt-6 flex items-center text-indigo-200 text-xs font-bold relative z-10">
                            <i class="fa-solid fa-check-double mr-2"></i> Transações concluídas
                        </div>
                    </div>

                    <div class="bg-white p-10 rounded-[2.5rem] shadow-sm border border-slate-200 flex flex-col justify-center italic text-slate-400">
                        <p class="text-sm leading-relaxed">"A boa gestão é a arte de tornar os problemas tão interessantes e as suas soluções tão construtivas que todos queiram trabalhar neles."</p>
                    </div>
                </div>

                <div class="bg-white rounded-[2.5rem] shadow-sm border border-slate-200 overflow-hidden">
                    <div class="px-10 py-8 border-b border-slate-100 flex justify-between items-center bg-slate-50/50">
                        <h4 class="font-black text-slate-800 uppercase text-xs tracking-widest">Fluxo de Caixa Recente</h4>
                        <button class="text-[10px] font-bold text-indigo-600 hover:underline uppercase">Exportar Relatório</button>
                    </div>
                    <table class="w-full text-left">
                        <thead class="bg-white text-[10px] font-black text-slate-400 uppercase border-b">
                            <tr>
                                <th class="px-10 py-5">Identificador/Data</th>
                                <th class="px-10 py-5">Descrição do Item</th>
                                <th class="px-10 py-5 text-right">Valor Creditado</th>
                            </tr>
                        </thead>
                        <tbody id="lista-historico" class="divide-y divide-slate-100 font-medium text-slate-700"></tbody>
                    </table>
                </div>
            </section>

            <section id="v" class="aba">
                <div class="w-full bg-white p-16 rounded-[3rem] shadow-sm border border-slate-200">
                    <div class="max-w-4xl mx-auto">
                        <div class="mb-12">
                            <h3 class="text-4xl font-black text-slate-900 uppercase tracking-tighter">Registrar Operação</h3>
                            <p class="text-slate-400 font-medium mt-2">Selecione o produto ou serviço no inventário para processar a transação.</p>
                        </div>
                        
                        <div class="grid grid-cols-1 lg:grid-cols-4 gap-8 items-end">
                            <div class="lg:col-span-3">
                                <label class="block text-xs font-black text-slate-500 uppercase tracking-widest mb-3 ml-2">Item do Inventário</label>
                                <select id="sel-venda" class="w-full p-6 bg-slate-50 border-2 border-slate-200 rounded-3xl text-xl font-bold outline-none transition-all appearance-none cursor-pointer"></select>
                            </div>
                            <button onclick="fazerVenda()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-black p-6 rounded-3xl text-xl uppercase shadow-2xl shadow-indigo-200 transition-all active:scale-95">
                                Finalizar <i class="fa-solid fa-arrow-right ml-2"></i>
                            </button>
                        </div>
                    </div>
                </div>
            </section>

            <section id="e" class="aba">
                <div class="grid grid-cols-1 lg:grid-cols-12 gap-10">
                    <div class="lg:col-span-4">
                        <div class="bg-white p-10 rounded-[2.5rem] shadow-sm border border-slate-200 sticky top-0">
                            <h3 class="font-black text-slate-800 uppercase text-xs tracking-widest mb-8">Cadastro de Itens</h3>
                            <div class="space-y-6">
                                <div>
                                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-2 ml-1">Descrição</label>
                                    <input id="in-nome" type="text" placeholder="Nome da Peça ou Serviço" class="w-full bg-slate-50 border-2 border-slate-200 p-4 rounded-2xl font-bold outline-none transition-all">
                                </div>
                                <div>
                                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-2 ml-1">Valor Unitário</label>
                                    <input id="in-preco" type="number" step="0.01" placeholder="0.00" class="w-full bg-slate-50 border-2 border-slate-200 p-4 rounded-2xl font-bold outline-none transition-all">
                                </div>
                                <button onclick="salvarItem()" class="w-full bg-slate-900 text-white font-black p-5 rounded-2xl hover:bg-indigo-600 transition-all uppercase text-xs tracking-widest shadow-lg">
                                    Adicionar ao Inventário
                                </button>
                            </div>
                        </div>
                    </div>

                    <div class="lg:col-span-8">
                        <div class="bg-white rounded-[2.5rem] shadow-sm border border-slate-200 overflow-hidden">
                            <table class="w-full text-left">
                                <thead class="bg-slate-50 text-[10px] font-black text-slate-400 uppercase border-b">
                                    <tr>
                                        <th class="px-10 py-5">Nome do Item</th>
                                        <th class="px-10 py-5 text-right">Preço de Venda</th>
                                        <th class="px-10 py-5 text-center">Ações</th>
                                    </tr>
                                </thead>
                                <tbody id="lista-e" class="divide-y divide-slate-100 font-bold text-slate-700 uppercase text-xs"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </section>

        </div>
    </main>

    <script>
        let DB_ITENS = JSON.parse(localStorage.getItem('sys_pro_it')) || [];
        let DB_VENDAS = JSON.parse(localStorage.getItem('sys_pro_ve')) || [];

        function init() {
            const data = new Date();
            const options = { weekday: 'long', day: '2-digit', month: 'long', year: 'numeric' };
            document.getElementById('data-display').innerText = data.toLocaleDateString('pt-br', options);
            atualizar();
        }

        function atualizar() {
            // Financeiro
            let soma = DB_VENDAS.reduce((a, b) => a + Number(b.preco), 0);
            document.getElementById('txt-caixa').innerText = soma.toLocaleString('pt-BR', {style: 'currency', currency: 'BRL'});
            document.getElementById('txt-vendas').innerText = DB_VENDAS.length;

            // Histórico
            const hist = document.getElementById('lista-historico');
            hist.innerHTML = DB_VENDAS.length === 0 
                ? '<tr><td colspan="3" class="p-20 text-center text-slate-300 italic">Nenhum registro encontrado.</td></tr>' 
                : DB_VENDAS.slice().reverse().slice(0, 15).map(v => `
                    <tr class="hover:bg-slate-50 transition-colors">
                        <td class="px-10 py-6 text-[10px] text-slate-400 font-bold">${v.hora}</td>
                        <td class="px-10 py-6 font-black text-slate-800 tracking-tight">${v.nome}</td>
                        <td class="px-10 py-6 text-right text-indigo-600 font-black text-base">+ R$ ${Number(v.preco).toFixed(2)}</td>
                    </tr>`).join('');

            // Estoque
            document.getElementById('lista-e').innerHTML = DB_ITENS.length === 0
                ? '<tr><td colspan="3" class="p-20 text-center text-slate-300">Inventário vazio.</td></tr>'
                : DB_ITENS.map((i, idx) => `
                    <tr class="hover:bg-slate-50 transition-colors">
                        <td class="px-10 py-6">${i.nome}</td>
                        <td class="px-10 py-6 text-right font-black text-slate-900 text-sm">R$ ${Number(i.preco).toFixed(2)}</td>
                        <td class="px-10 py-6 text-center">
                            <button onclick="deletarItem(${idx})" class="w-10 h-10 rounded-xl bg-red-50 text-red-500 hover:bg-red-500 hover:text-white transition-all">
                                <i class="fa-solid fa-trash-can text-xs"></i>
                            </button>
                        </td>
                    </tr>`).join('');

            // Select de Vendas
            const select = document.getElementById('sel-venda');
            select.innerHTML = DB_ITENS.length 
                ? DB_ITENS.map((i, idx) => `<option value="${idx}">${i.nome} — R$ ${Number(i.preco).toFixed(2)}</option>`).join('')
                : '<option value="">ADICIONE ITENS NO ESTOQUE PRIMEIRO</option>';
        }

        function mudar(abaId, btn) {
            document.querySelectorAll('.aba').forEach(a => a.classList.remove('ativa'));
            document.getElementById(abaId).classList.add('ativa');
            
            document.querySelectorAll('.sidebar-btn').forEach(b => b.classList.remove('sidebar-active', 'bg-slate-800'));
            btn.classList.add('sidebar-active');

            const titulos = { 'p': 'Painel de Controle', 'v': 'Processar Venda', 'e': 'Gestão de Inventário' };
            document.getElementById('titulo-aba').innerText = titulos[abaId];
            atualizar();
        }

        function salvarItem() {
            let n = document.getElementById('in-nome').value;
            let p = document.getElementById('in-preco').value;
            if(!n || !p) return alert("Erro: Preencha todos os campos.");

            DB_ITENS.push({nome: n, preco: p});
            localStorage.setItem('sys_pro_it', JSON.stringify(DB_ITENS));
            document.getElementById('in-nome').value = '';
            document.getElementById('in-preco').value = '';
            atualizar();
        }

        function fazerVenda() {
            let idx = document.getElementById('sel-venda').value;
            if(idx === "" || !DB_ITENS[idx]) return alert("Erro: Selecione um item válido.");

            let h = new Date().toLocaleString('pt-br', {day:'2-digit', month:'2-digit', hour:'2-digit', minute:'2-digit'});
            DB_VENDAS.push({...DB_ITENS[idx], hora: h});
            localStorage.setItem('sys_pro_ve', JSON.stringify(DB_VENDAS));
            
            mudar('p', document.querySelectorAll('.sidebar-btn')[0]);
        }

        function deletarItem(idx) {
            if(confirm("Deseja remover este item do estoque permanentemente?")) {
                DB_ITENS.splice(idx, 1);
                localStorage.setItem('sys_pro_it', JSON.stringify(DB_ITENS));
                atualizar();
            }
        }

        function resetar() {
            if(confirm("ALERTA CRÍTICO: Todos os dados serão apagados. Deseja prosseguir?")) {
                localStorage.clear();
                location.reload();
            }
        }

        init();
    </script>
</body>
</html>
