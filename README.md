<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema Oficina Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        .aba { display: none; }
        .aba.ativa { display: block; }
    </style>
</head>
<body class="bg-gray-100 flex h-screen m-0 overflow-hidden font-sans">

    <nav class="w-64 bg-slate-900 text-white flex flex-col shadow-2xl">
        <div class="p-6 border-b border-slate-800 text-center font-bold text-xl text-blue-400">
            OFICINA V4
        </div>
        <div class="p-4 space-y-2 flex-1">
            <button onclick="mudar('p')" class="w-full text-left p-3 rounded hover:bg-slate-800 transition flex items-center gap-3">
                <i class="fa-solid fa-chart-line text-blue-400"></i> Painel
            </button>
            <button onclick="mudar('v')" class="w-full text-left p-3 rounded hover:bg-slate-800 transition flex items-center gap-3">
                <i class="fa-solid fa-cart-shopping text-green-400"></i> Vendas
            </button>
            <button onclick="mudar('e')" class="w-full text-left p-3 rounded hover:bg-slate-800 transition flex items-center gap-3">
                <i class="fa-solid fa-box text-orange-400"></i> Estoque
            </button>
        </div>
        <div class="p-4">
            <button onclick="resetar()" class="w-full text-xs text-red-500 hover:text-red-300 underline">
                Limpar Banco de Dados
            </button>
        </div>
    </nav>

    <main class="flex-1 p-10 overflow-auto">
        
        <section id="p" class="aba ativa">
            <h2 class="text-3xl font-bold mb-8 text-gray-800">Painel Financeiro</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-white p-8 rounded-xl shadow-lg border-l-8 border-green-500">
                    <p class="text-gray-400 font-bold uppercase text-sm">Total em Caixa</p>
                    <h3 id="txt-caixa" class="text-5xl font-black text-green-600 mt-2">R$ 0,00</h3>
                </div>
                <div class="bg-white p-8 rounded-xl shadow-lg border-l-8 border-blue-500">
                    <p class="text-gray-400 font-bold uppercase text-sm">Vendas Realizadas</p>
                    <h3 id="txt-vendas" class="text-5xl font-black text-blue-600 mt-2">0</h3>
                </div>
            </div>
        </section>

        <section id="v" class="aba">
            <h2 class="text-3xl font-bold mb-8 text-gray-800">Nova Venda</h2>
            <div class="bg-white p-8 rounded-xl shadow-lg max-w-lg">
                <label class="block mb-2 font-bold text-gray-700">Escolha o item:</label>
                <select id="sel-venda" class="w-full border-2 border-gray-200 p-4 rounded-lg mb-6 text-lg focus:border-green-500 outline-none">
                </select>
                <button onclick="fazerVenda()" class="w-full bg-green-600 text-white font-bold p-5 rounded-lg text-xl hover:bg-green-700 shadow-lg transform active:scale-95 transition">
                    CONCLUIR VENDA 💰
                </button>
            </div>
        </section>

        <section id="e" class="aba">
            <h2 class="text-3xl font-bold mb-8 text-gray-800">Gerenciar Estoque</h2>
            <div class="bg-white p-8 rounded-xl shadow-lg max-w-md mb-10">
                <div class="space-y-4">
                    <input id="in-nome" type="text" placeholder="Nome da Peça ou Serviço" class="w-full border p-3 rounded-lg outline-none focus:border-blue-500">
                    <input id="in-preco" type="number" placeholder="Preço (Ex: 150.00)" class="w-full border p-3 rounded-lg outline-none focus:border-blue-500">
                    <button onclick="salvarItem()" class="w-full bg-blue-600 text-white font-bold p-3 rounded-lg hover:bg-blue-700 transition">
                        CADASTRAR ITEM
                    </button>
                </div>
            </div>
            
            <div class="bg-white rounded-xl shadow-lg overflow-hidden">
                <table class="w-full text-left border-collapse">
                    <thead class="bg-slate-800 text-white">
                        <tr>
                            <th class="p-4">Produto / Serviço</th>
                            <th class="p-4 text-right">Preço Unitário</th>
                        </tr>
                    </thead>
                    <tbody id="lista-e" class="divide-y divide-gray-100">
                    </tbody>
                </table>
            </div>
        </section>

    </main>

    <script>
        // Armazenamento Local
        let DB_ITENS = JSON.parse(localStorage.getItem('oficina_it')) || [];
        let DB_VENDAS = JSON.parse(localStorage.getItem('oficina_ve')) || [];

        // Atualiza a tela
        function atualizar() {
            // Financeiro
            let soma = DB_VENDAS.reduce((a, b) => a + Number(b.preco), 0);
            document.getElementById('txt-caixa').innerText = "R$ " + soma.toLocaleString('pt-BR', {minimumFractionDigits: 2});
            document.getElementById('txt-vendas').innerText = DB_VENDAS.length;

            // Tabela Estoque
            document.getElementById('lista-e').innerHTML = DB_ITENS.map(i => `
                <tr class="hover:bg-gray-50 transition">
                    <td class="p-4 font-medium">${i.nome}</td>
                    <td class="p-4 text-right font-bold text-blue-600">R$ ${Number(i.preco).toFixed(2)}</td>
                </tr>
            `).join('');

            // Menu de Vendas
            let select = document.getElementById('sel-venda');
            if(DB_ITENS.length === 0) {
                select.innerHTML = '<option value="">Cadastre itens primeiro no estoque!</option>';
            } else {
                select.innerHTML = DB_ITENS.map((i, idx) => `
                    <option value="${idx}">${i.nome} - R$ ${Number(i.preco).toFixed(2)}</option>
                `).join('');
            }
        }

        // Troca de abas
        function mudar(id) {
            document.querySelectorAll('.aba').forEach(a => a.classList.remove('ativa'));
            document.getElementById(id).classList.add('ativa');
            atualizar();
        }

        // Salva novo item
        function salvarItem() {
            let n = document.getElementById('in-nome').value;
            let p = document.getElementById('in-preco').value;
            
            if(!n || !p) {
                alert("Preencha o nome e o preço corretamente!");
                return;
            }

            DB_ITENS.push({nome: n, preco: p});
            localStorage.setItem('oficina_it', JSON.stringify(DB_ITENS));
            
            document.getElementById('in-nome').value = '';
            document.getElementById('in-preco').value = '';
            atualizar();
            alert("Item adicionado ao estoque!");
        }

        // Faz uma venda
        function fazerVenda() {
            let idx = document.getElementById('sel-venda').value;
            if(idx === "" || !DB_ITENS[idx]) {
                alert("Selecione um produto válido!");
                return;
            }

            DB_VENDAS.push(DB_ITENS[idx]);
            localStorage.setItem('oficina_ve', JSON.stringify(DB_VENDAS));
            
            alert("Venda concluída!");
            mudar('p'); // Volta para o painel
        }

        // Reseta tudo
        function resetar() {
            if(confirm("Deseja apagar todos os dados permanentemente?")) {
                localStorage.clear();
                location.reload();
            }
        }

        // Inicia o sistema carregando os dados
        atualizar();
    </script>
</body>
</html>
