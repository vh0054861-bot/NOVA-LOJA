<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel de Administração | Gestão de Negócios</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f1f5f9; }
        .aba { display: none; }
        .aba.ativa { display: block; animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .sidebar-active { background-color: #1e293b; border-left: 4px solid #3b82f6; }
        .custom-scroll::-webkit-scrollbar { width: 6px; }
        .custom-scroll::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="flex h-screen overflow-hidden text-slate-900">

    <aside class="w-72 bg-slate-900 text-slate-300 flex flex-col shadow-xl">
        <div class="p-8 border-b border-slate-800">
            <h1 class="text-white text-xl font-bold tracking-tight uppercase">Admin <span class="text-blue-500">Panel</span></h1>
            <p class="text-[10px] text-slate-500 font-semibold tracking-[3px] mt-1">SISTEMA DE GESTÃO</p>
        </div>

        <nav class="flex-1 p-4 space-y-2 mt-4">
            <button onclick="mudar('p', this)" class="sidebar-btn sidebar-active w-full text-left p-4 rounded-lg transition-all flex items-center gap-4 hover:bg-slate-800 group">
                <i class="fa-solid fa-chart-line text-blue-500"></i>
                <span class="font-medium">Dashboard Geral</span>
            </button>
            <button onclick="mudar('v', this)" class="sidebar-btn w-full text-left p-4 rounded-lg transition-all flex items-center gap-4 hover:bg-slate-800 group border-left-4 border-transparent">
                <i class="fa-solid fa-cart-plus text-slate-500"></i>
                <span class="font-medium">Nova Transação</span>
            </button>
            <button onclick="mudar('e', this)" class="sidebar-btn w-full text-left p-4 rounded-lg transition-all flex items-center gap-4 hover:bg-slate-800 group border-left-4 border-transparent">
                <i class="fa-solid fa-boxes-stacked text-slate-500"></i>
                <span class="font-medium">Gestão de Estoque</span>
            </button>
        </nav>

        <div class="p-6 border-t border-slate-800">
            <button onclick="resetar()" class="w-full py-2 px-4 rounded border border-slate-700 hover:bg-red-500/10 hover:border-red-500/50 hover:text-red-500 transition-all text-xs font-bold uppercase tracking-widest">
                Zerar Dados
            </button>
        </div>
    </aside>

    <main class="flex-1 flex flex-col min-w-0">
        <header class="bg-white border-b border-slate-200 px-10 py-6 flex justify-between items-center shadow-sm">
            <h2 id="titulo-aba" class="text-2xl font-bold text-slate-800">Dashboard Geral</h2>
            <div class="flex items-center gap-6">
                <div class="text-right">
                    <p id="data-display" class="text-sm font-semibold text-slate-600 uppercase"></p>
                    <p class="text-[10px] text-slate-400">Status: Conectado ao LocalDB</p>
                </div>
                <div class="h-10 w-10 bg-blue-500 rounded-full flex items-center justify-center text-white font-bold shadow-lg">AD</div>
            </div>
        </header>

        <div class="flex-1 p-10 overflow-y-auto custom-scroll">
            
            <section id="p" class="aba ativa">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-10">
                    <div class="bg-white p-8 rounded-2xl shadow-sm border border-slate-200">
                        <div class="flex justify-between items-start mb-4">
                            <span class="text-xs font-bold text-slate-400 uppercase tracking-widest">Saldo Bruto</span>
                            <i class="fa-solid fa-wallet text-blue-500"></i>
                        </div>
                        <h3 id="txt-caixa" class="text-3xl font-black text-slate-800 tracking-tighter">R$ 0,00</h3>
                    </div>
                    <div class="bg-white p-8 rounded-2xl shadow-sm border border-slate-200">
                        <div class="flex justify-between items-start mb-4">
                            <span class="text-xs font-bold text-slate-400 uppercase tracking-widest">Volume de Vendas</span>
                            <i class="fa-solid fa-receipt text-blue-500"></i>
                        </div>
                        <h3 id="txt-vendas" class="text-3xl font-black text-slate-800 tracking-tighter">0</h3>
                    </div>
                    <div class="bg-blue-600 p-8 rounded-2xl shadow-xl text-white">
                        <p class="text-xs font-bold text-blue-200 uppercase tracking-widest mb-4">Eficiência de Gestão</p>
                        <h3 class="text-xl font-bold">Monitoramento Ativo</h3>
                        <p class="text-sm text-blue-100 mt-2 opacity-80">Todos os dados são salvos localmente no seu navegador para sua privacidade.</p>
                    </div>
                </div>

                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                    <div class="px-8 py-5 border-b border-slate-100 flex justify-between items-center">
                        <h4 class="font-bold text-slate-800 tracking-tight">Relatório de Transações Recentes</h4>
                        <span class="text-[10px] bg-slate-100 px-2 py-1 rounded font-bold text-slate-500 uppercase">Últimos 10 registros</span>
                    </div>
                    <div class="p-0">
                        <table class="w-full text-left">
                            <thead class="bg-slate-50 text-[10px] text-slate-400 font-black uppercase tracking-widest border-b">
                                <tr>
                                    <th class="px-8 py-4">Data/Hora</th>
                                    <th class="px-8 py-4">Descrição do Produto</th>
                                    <th class="px-8 py-4 text-right">Valor Líquido</th>
                                </tr>
                            </thead>
                            <tbody id="lista-historico" class="divide-y divide-slate-100">
                                </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <section id="v" class="aba">
                <div class="max-w-xl mx-auto bg-white p-12 rounded-3xl shadow-xl border border-slate-200 mt-10 text-center">
                    <div class="w-20 h-20 bg-blue-50 text-blue-600 rounded-full flex items-center justify-center mx-auto mb-8">
                        <i class="fa-solid fa-cart-shopping text-3xl"></i>
                    </div>
                    <h3 class="text-3xl font-black text-slate-800 mb-2">Processar Pagamento</h3>
                    <p class="text-slate-400 mb-10">Os itens vendidos serão descontados contabilmente no caixa.</p>
                    
                    <div class="text-left space-y-6">
                        <div>
                            <label class="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-2 ml-1">Selecione o Item para Saída</label>
                            <select id="sel-venda" class="w-full bg-slate-50 border border-slate-200 p-4 rounded-xl text-lg font-medium focus:ring-4 focus:ring-blue-500/10 focus:border-blue-500 outline-none transition-all"></select>
                        </div>
                        <button onclick="fazerVenda()" class="w-full bg-slate-900 hover:bg-blue-600 text-white font-bold p-5 rounded-xl text-xl shadow-lg transition-all active:scale-95 flex items-center justify-center gap-4 uppercase tracking-tighter">
                            Finalizar Transação <i class="fa-solid fa-chevron-right"></i>
                        </button>
                    </div>
                </div>
            </section>

            <section id="e" class="aba">
                <div class="flex flex-col xl:flex-row gap-10">
                    <div class="w-full xl:w-96">
                        <div class="bg-white p-8 rounded-3xl shadow-lg border border-slate-200 sticky top-4">
                            <h3 class="text-xl font-bold mb-6 text-slate-800">Cadastrar Produto</h3>
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-2 ml-1">Nome</label>
                                    <input id="in-nome" type="text" class="w-full bg-slate-50 border border-slate-200 p-3 rounded-xl focus:border-blue-500 outline-none transition-all" placeholder="Ex: Filtro de Óleo">
                                </div>
                                <div>
                                    <label class="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-2 ml-1">Preço unitário</label>
                                    <input id="in-preco" type="number" step="0.01" class="w-full bg-slate-50 border border-slate-200 p-3 rounded-xl focus:border-blue-500 outline-none transition-all" placeholder="R$ 0,00">
                                </div>
                                <button onclick="salvarItem()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold p-4 rounded-xl shadow-md transition-all uppercase text-xs tracking-widest">
                                    Adicionar ao Inventário
                                </button>
                            </div>
                        </div>
                    </div>

                    <div class="flex-1">
                        <div class="bg-white rounded-3xl shadow-sm border border-slate-200 overflow-hidden">
                            <div class="p-6 border-b flex items-center justify-between">
                                <h3 class="font-bold text-slate-800">Inventário Ativo</h3>
                                <div class="relative w-64">
                                    <i class="fa-solid fa-magnifying-glass absolute left-3 top-3 text-slate-400 text-sm"></i>
                                    <input type="text" id="busca-estoque" onkeyup="filtrarEstoque()" placeholder="Buscar item..." class="w-full pl-10 pr-4 py-2 bg-slate-100 rounded-lg text-sm outline-none focus:ring-2 focus:ring-blue-500/20 transition-all">
                                </div>
                            </div>
                            <table class="w-full text-left">
                                <thead class="bg-slate-50 border-b border-slate-100 text-[10px] font-black text-slate-400 uppercase tracking-widest">
                                    <tr>
                                        <th class="px-8 py-4">Descrição do Item</th>
                                        <th class="px-8 py-4 text-right">Valor de Registro</th>
                                        <th class="px-8 py-4 text-center">Ações</th>
                                    </tr>
                                </thead>
                                <tbody id="lista-e" class="divide-y divide-slate-100 font-medium text-slate-700">
                                    </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </section>
        </div>
    </main>

    <script>
        let DB_ITENS = JSON.parse(localStorage.getItem('admin_it')) || [];
        let DB_VENDAS = JSON.parse(localStorage.getItem('admin_ve')) || [];

        function init() {
            document.getElementById('data-display').innerText = new Date().toLocaleDateString('pt-br', { weekday: 'long', day: 'numeric', month: 'short' });
            atualizar();
        }

        function atualizar() {
            // Financeiro
            let soma = DB_VENDAS.reduce((a, b) => a + Number(b.preco), 0);
            document.getElementById('txt-caixa').innerText = soma.toLocaleString('pt-BR', {style: 'currency', currency: 'BRL'});
            document.getElementById('txt-vendas').innerText = DB_VENDAS.length;

            // Histórico Transações
            const histContainer = document.getElementById('lista-historico');
            if(DB_VENDAS.length === 0) {
                histContainer.innerHTML = `<tr><td colspan="3" class="px-8 py-10 text-center text-slate-300 italic">Sem transações registradas no período.</td></tr>`;
            } else {
                histContainer.innerHTML = DB_VENDAS.slice().reverse().slice(0, 10).map(v => `
                    <tr class="hover:bg-slate-50 transition-colors">
                        <td class="px-8 py-4 text-slate-400 text-xs">${v.data || 'Registrado'}</td>
                        <td class="px-8 py-4 font-bold text-slate-700">${v.nome}</td>
                        <td class="px-8 py-4 text-right font-black text-blue-600">+ R$ ${Number(v.preco).toFixed(2)}</td>
                    </tr>
                `).join('');
            }

            // Tabela Estoque
            renderEstoque(DB_ITENS);

            // Select Vendas
            let select = document.getElementById('sel-venda');
            select.innerHTML = DB_ITENS.length === 0 ? '<option value="">Estoque Vazio</option>' : DB_ITENS.map((i, idx) => `<option value="${idx}">${i.nome} - R$ ${Number(i.preco).toFixed(2)}</option>`).join('');
        }

        function renderEstoque(dados) {
            document.getElementById('lista-e').innerHTML = dados.length ? dados.map((i, idx) => `
                <tr class="hover:bg-blue-50/20 transition-all border-b last:border-0 border-slate-50">
                    <td class="px-8 py-5 font-semibold text-slate-700">${i.nome}</td>
                    <td class="px-8 py-5 text-right font-black text-slate-900">R$ ${Number(i.preco).toFixed(2)}</td>
                    <td class="px-8 py-5 text-center">
                        <button onclick="deletarItem(${idx})" class="w-8 h-8 rounded-full bg-red-50 text-red-400 hover:bg-red-500 hover:text-white transition-all">
                            <i class="fa-solid fa-trash-can text-xs"></i>
                        </button>
                    </td>
                </tr>
            `).join('') : `<tr><td colspan="3" class="px-8 py-10 text-center text-slate-300">Nenhum item localizado.</td></tr>`;
        }

        function mudar(abaId, btn) {
            document.querySelectorAll('.aba').forEach(a => a.classList.remove('ativa'));
            document.getElementById(abaId).classList.add('ativa');
            document.querySelectorAll('.sidebar-btn').forEach(b => {
                b.classList.remove('sidebar-active', 'text-white');
                b.querySelector('i').className = b.querySelector('i').className.replace('text-blue-500', 'text-slate-500');
            });
            btn.classList.add('sidebar-active', 'text-white');
            btn.querySelector('i').className = btn.querySelector('i').className.replace('text-slate-500', 'text-blue-500');
            
            const titles = { 'p': 'Dashboard Geral', 'v': 'Processar Transação', 'e': 'Gestão de Inventário' };
            document.getElementById('titulo-aba').innerText = titles[abaId];
            atualizar();
        }

        function salvarItem() {
            let n = document.getElementById('in-nome').value;
            let p = document.getElementById('in-preco').value;
            if(!n || !p) return;
            DB_ITENS.push({nome: n, preco: p});
            localStorage.setItem('admin_it', JSON.stringify(DB_ITENS));
            document.getElementById('in-nome').value = ''; document.getElementById('in-preco').value = '';
            atualizar();
        }

        function fazerVenda() {
            let idx = document.getElementById('sel-venda').value;
            if(idx === "" || !DB_ITENS[idx]) return;
            
            let dataHora = new Date().toLocaleString('pt-br', {day: '2-digit', month: '2-digit', hour: '2-digit', minute: '2-digit'});
            DB_VENDAS.push({...DB_ITENS[idx], data: dataHora});
            localStorage.setItem('admin_ve', JSON.stringify(DB_VENDAS));
            mudar('p', document.querySelectorAll('.sidebar-btn')[0]);
        }

        function deletarItem(idx) {
            if(confirm("Remover este item do inventário?")) {
                DB_ITENS.splice(idx, 1);
                localStorage.setItem('admin_it', JSON.stringify(DB_ITENS));
                atualizar();
            }
        }

        function filtrarEstoque() {
            let busca = document.getElementById('busca-estoque').value.toLowerCase();
            let filtrados = DB_ITENS.filter(i => i.nome.toLowerCase().includes(busca));
            renderEstoque(filtrados);
        }

        function resetar() { if(confirm("ALERTA: Isso limpará toda a base de dados local!")) { localStorage.clear(); location.reload(); } }
        init();
    </script>
</body>
</html>
