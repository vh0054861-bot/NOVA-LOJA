<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Oficina Pro | Gestão Inteligente</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        .aba { display: none; }
        .aba.ativa { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .sidebar-item.active { background-color: #3730a3; border-right: 4px solid #818cf8; }
    </style>
</head>
<body class="bg-[#f8fafc] flex h-screen overflow-hidden text-slate-700">

    <aside class="w-72 bg-[#1e1b4b] text-white flex flex-col shadow-2xl z-20">
        <div class="p-8 text-center">
            <div class="inline-flex items-center justify-center w-12 h-12 bg-indigo-500 rounded-xl mb-3 shadow-lg">
                <i class="fa-solid fa-bolt-lightning text-xl"></i>
            </div>
            <h1 class="text-xl font-extrabold tracking-tight">OFICINA<span class="text-indigo-400">PRO</span></h1>
        </div>

        <nav class="flex-1 px-4 space-y-1 mt-4">
            <button onclick="mudar('p', this)" class="sidebar-item active w-full text-left p-4 rounded-xl transition-all flex items-center gap-4 hover:bg-white/10 group">
                <i class="fa-solid fa-house text-indigo-400"></i>
                <span class="font-medium">Painel Principal</span>
            </button>
            <button onclick="mudar('v', this)" class="sidebar-item w-full text-left p-4 rounded-xl transition-all flex items-center gap-4 hover:bg-white/10 group">
                <i class="fa-solid fa-cart-shopping text-indigo-400"></i>
                <span class="font-medium">Registrar Venda</span>
            </button>
            <button onclick="mudar('e', this)" class="sidebar-item w-full text-left p-4 rounded-xl transition-all flex items-center gap-4 hover:bg-white/10 group">
                <i class="fa-solid fa-boxes-stacked text-indigo-400"></i>
                <span class="font-medium">Estoque e Peças</span>
            </button>
        </nav>

        <div class="p-6">
            <button onclick="resetar()" class="w-full py-3 rounded-lg text-xs font-bold text-red-400 hover:bg-red-500/10 transition-colors uppercase">
                Limpar Sistema
            </button>
        </div>
    </aside>

    <main class="flex-1 overflow-y-auto relative bg-slate-50">
        <header class="sticky top-0 bg-white/80 backdrop-blur-md z-10 px-10 py-6 border-b border-slate-200 flex justify-between items-center">
            <h2 id="titulo-pagina" class="text-2xl font-bold text-slate-800">Boas-vindas ao Painel</h2>
            <span id="data-display" class="text-sm font-medium text-slate-500 bg-slate-100 px-4 py-2 rounded-full"></span>
        </header>

        <div class="p-10">
            <section id="p" class="aba ativa">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-10">
                    <div class="bg-white p-8 rounded-3xl shadow-sm border border-slate-100">
                        <p class="text-slate-400 font-bold uppercase text-xs tracking-wider">Faturamento Total</p>
                        <h3 id="txt-caixa" class="text-4xl font-black text-slate-800 mt-2">R$ 0,00</h3>
                    </div>
                    <div class="bg-white p-8 rounded-3xl shadow-sm border border-slate-100">
                        <p class="text-slate-400 font-bold uppercase text-xs tracking-wider">Vendas Realizadas</p>
                        <h3 id="txt-vendas" class="text-4xl font-black text-slate-800 mt-2">0</h3>
                    </div>
                </div>

                <div class="bg-white rounded-3xl shadow-sm border border-slate-100 p-8">
                    <h4 class="font-bold text-slate-800 mb-6 italic">Últimos Registros:</h4>
                    <div id="lista-historico" class="space-y-3"></div>
                </div>
            </section>

            <section id="v" class="aba">
                <div class="max-w-md bg-white p-10 rounded-3xl shadow-xl border border-slate-100 mx-auto">
                    <h3 class="text-2xl font-bold text-slate-800 mb-6 text-center">Nova Venda</h3>
                    <select id="sel-venda" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl mb-6 outline-none focus:border-indigo-500 transition-all"></select>
                    <button onclick="fazerVenda()" class="w-full bg-indigo-600 text-white font-black p-5 rounded-2xl shadow-lg hover:bg-indigo-700 active:scale-95 transition-all">CONCLUIR VENDA</button>
                </div>
            </section>

            <section id="e" class="aba">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div class="bg-white p-6 rounded-3xl shadow-md border border-slate-100 h-fit">
                        <h3 class="font-bold mb-4">Adicionar Item</h3>
                        <input id="in-nome" type="text" placeholder="Nome" class="w-full border p-3 rounded-xl mb-3 outline-none focus:border-indigo-500">
                        <input id="in-preco" type="number" placeholder="Preço" class="w-full border p-3 rounded-xl mb-4 outline-none focus:border-indigo-500">
                        <button onclick="salvarItem()" class="w-full bg-indigo-600 text-white font-bold p-3 rounded-xl hover:bg-indigo-700">SALVAR</button>
                    </div>
                    <div class="lg:col-span-2 bg-white rounded-3xl shadow-md overflow-hidden">
                        <table class="w-full text-left">
                            <thead class="bg-slate-50 border-b border-slate-100">
                                <tr><th class="p-6">Nome</th><th class="p-6 text-right">Preço</th><th class="p-6"></th></tr>
                            </thead>
                            <tbody id="lista-e" class="divide-y divide-slate-50"></tbody>
                        </table>
                    </div>
                </div>
            </section>
        </div>
    </main>

    <script>
        let DB_ITENS = JSON.parse(localStorage.getItem('oficina_it')) || [];
        let DB_VENDAS = JSON.parse(localStorage.getItem('oficina_ve')) || [];

        function atualizar() {
            let soma = DB_VENDAS.reduce((a, b) => a + Number(b.preco), 0);
            document.getElementById('txt-caixa').innerText = soma.toLocaleString('pt-BR', {style: 'currency', currency: 'BRL'});
            document.getElementById('txt-vendas').innerText = DB_VENDAS.length;
            document.getElementById('data-display').innerText = new Date().toLocaleDateString('pt-br', { weekday: 'long', day: 'numeric', month: 'long' });

            document.getElementById('lista-historico').innerHTML = DB_VENDAS.slice().reverse().slice(0, 5).map(v => `<div class="p-4 bg-slate-50 rounded-xl flex justify-between border border-slate-100"><span>${v.nome}</span><span class="font-bold">R$ ${Number(v.preco).toFixed(2)}</span></div>`).join('');
            document.getElementById('lista-e').innerHTML = DB_ITENS.map((i, idx) => `<tr><td class="p-6 font-semibold">${i.nome}</td><td class="p-6 text-right font-bold text-indigo-600">R$ ${Number(i.preco).toFixed(2)}</td><td class="p-6 text-center"><button onclick="deletarItem(${idx})" class="text-red-300 hover:text-red-500"><i class="fa-solid fa-trash-can"></i></button></td></tr>`).join('');
            document.getElementById('sel-venda').innerHTML = DB_ITENS.map((i, idx) => `<option value="${idx}">${i.nome} (R$ ${Number(i.preco).toFixed(2)})</option>`).join('');
        }

        function mudar(abaId, btn) {
            document.querySelectorAll('.aba').forEach(a => a.classList.remove('ativa'));
            document.getElementById(abaId).classList.add('ativa');
            document.querySelectorAll('.sidebar-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            atualizar();
        }

        function salvarItem() {
            let n = document.getElementById('in-nome').value;
            let p = document.getElementById('in-preco').value;
            if(!n || !p) return alert("Preencha tudo!");
            DB_ITENS.push({nome: n, preco: p});
            localStorage.setItem('oficina_it', JSON.stringify(DB_ITENS));
            document.getElementById('in-nome').value = ''; document.getElementById('in-preco').value = '';
            atualizar();
        }

        function deletarItem(idx) {
            DB_ITENS.splice(idx, 1);
            localStorage.setItem('oficina_it', JSON.stringify(DB_ITENS));
            atualizar();
        }

        function fazerVenda() {
            let idx = document.getElementById('sel-venda').value;
            if(idx === "") return alert("Selecione um item!");
            DB_VENDAS.push(DB_ITENS[idx]);
            localStorage.setItem('oficina_ve', JSON.stringify(DB_VENDAS));
            alert("Vendido!");
            mudar('p', document.querySelectorAll('.sidebar-item')[0]);
        }

        function resetar() { if(confirm("Apagar tudo?")) { localStorage.clear(); location.reload(); } }
        atualizar();
    </script>
</body>
</html>
