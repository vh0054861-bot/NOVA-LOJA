<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Gestão - Oficina 3.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    
    <style>
        /* Animação suave entre as abas */
        .fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Barra de rolagem personalizada */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #888; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #555; }
    </style>
</head>
<body class="bg-slate-100 font-sans h-screen flex overflow-hidden">

    <aside class="w-64 bg-slate-900 text-white flex flex-col shadow-2xl z-50">
        <div class="p-6 border-b border-slate-700 flex items-center gap-3">
            <i class="fa-solid fa-wrench text-blue-400 text-2xl"></i>
            <div>
                <h1 class="text-lg font-bold">Oficina Pro</h1>
                <p class="text-xs text-slate-400">Gestão Integrada</p>
            </div>
        </div>
        
        <nav class="flex-1 p-4 space-y-2 mt-4">
            <button onclick="navegar('dashboard')" class="nav-item w-full text-left py-3 px-4 rounded hover:bg-slate-700 transition flex items-center gap-3 bg-slate-800 border-l-4 border-blue-500">
                <i class="fa-solid fa-chart-pie w-6 text-center"></i> Visão Geral
            </button>
            <button onclick="navegar('vendas')" class="nav-item w-full text-left py-3 px-4 rounded hover:bg-slate-700 transition flex items-center gap-3 border-l-4 border-transparent">
                <i class="fa-solid fa-cash-register w-6 text-center"></i> Nova Venda
            </button>
            <button onclick="navegar('produtos')" class="nav-item w-full text-left py-3 px-4 rounded hover:bg-slate-700 transition flex items-center gap-3 border-l-4 border-transparent">
                <i class="fa-solid fa-boxes-stacked w-6 text-center"></i> Estoque / Peças
            </button>
            <button onclick="navegar('historico')" class="nav-item w-full text-left py-3 px-4 rounded hover:bg-slate-700 transition flex items-center gap-3 border-l-4 border-transparent">
                <i class="fa-solid fa-list w-6 text-center"></i> Relatório Completo
            </button>
        </nav>

        <div class="p-4 border-t border-slate-700">
            <button onclick="limparDados()" class="w-full py-2 px-4 rounded bg-red-900/50 hover:bg-red-800 text-red-200 text-sm transition flex items-center justify-center gap-2">
                <i class="fa-solid fa-trash-can"></i> Resetar Sistema
            </button>
        </div>
    </aside>

    <main class="flex-1 overflow-y-auto bg-slate-100 p-8 relative">
        
        <section id="dashboard" class="tela fade-in">
            <header class="flex justify-between items-center mb-8">
                <h2 class="text-3xl font-bold text-slate-800">Painel de Controle</h2>
                <span class="text-sm text-slate-500 bg-white px-3 py-1 rounded shadow" id="data-hoje">Data</span>
            </header>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="bg-white p-6 rounded-xl shadow-sm border-l-4 border-green-500">
                    <div class="flex justify-between items-center">
                        <div>
                            <p class="text-sm font-bold text-slate-400 uppercase">Faturamento Total</p>
                            <h3 class="text-3xl font-bold text-slate-800 mt-1" id="dash-faturamento">R$ 0,00</h3>
                        </div>
                        <div class="p-3 bg-green-100 rounded-full text-green-600">
                            <i class="fa-solid fa-dollar-sign text-xl"></i>
                        </div>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-xl shadow-sm border-l-4 border-blue-500">
                    <div class="flex justify-between items-center">
                        <div>
                            <p class="text-sm font-bold text-slate-400 uppercase">Vendas Realizadas</p>
                            <h3 class="text-3xl font-bold text-slate-800 mt-1" id="dash-qtd-vendas">0</h3>
                        </div>
                        <div class="p-3 bg-blue-100 rounded-full text-blue-600">
                            <i class="fa-solid fa-bag-shopping text-xl"></i>
                        </div>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-xl shadow-sm border-l-4 border-orange-500">
                    <div class="flex justify-between items-center">
                        <div>
                            <p class="text-sm font-bold text-slate-400 uppercase">Itens no Catálogo</p>
                            <h3 class="text-3xl font-bold text-slate-800 mt-1" id="dash-qtd-produtos">0</h3>
                        </div>
                        <div class="p-3 bg-orange-100 rounded-full text-orange-600">
                            <i class="fa-solid fa-box-open text-xl"></i>
                        </div>
                    </div>
                </div>
            </div>

            <div class="bg-white rounded-xl shadow-sm overflow-hidden">
                <div class="p-5 border-b border-slate-100 flex justify-between items-center">
                    <h3 class="font-bold text-slate-700">Últimas 5 Vendas</h3>
                    <button onclick="navegar('historico')" class="text-sm text-blue-500 hover:underline">Ver tudo</button>
                </div>
                <table class="w-full text-left text-sm">
                    <thead class="bg-slate-50 text-slate-500">
                        <tr>
                            <th class="p-4">Produto</th>
                            <th class="p-4">Data</th>
                            <th class="p-4 text-right">Valor</th>
                        </tr>
                    </thead>
                    <tbody id="dash-tabela-body">
                        </tbody>
                </table>
            </div>
        </section>

        <section id="vendas" class="tela hidden fade-in">
            <h2 class="text-3xl font-bold text-slate-800 mb-6">Registrar Saída / Venda</h2>
            <div class="flex flex-col md:flex-row gap-8">
                
                <div class="w-full md:w-1/2">
                    <div class="bg-white p-8 rounded-xl shadow-lg border-t-4 border-blue-600">
                        <label class="block mb-2 font-bold text-slate-700">Selecione o Produto/Serviço:</label>
                        <select id="venda-select" class="w-full p-3 border border-slate-300 rounded-lg mb-6 focus:ring-2 focus:ring-blue-500 outline-none bg-slate-50">
                            <option value="">Carregando...</option>
                        </select>

                        <div class="bg-slate-50 p-4 rounded-lg mb-6 flex justify-between items-center border border-slate-200">
                            <span class="text-slate-500">Valor Unitário:</span>
                            <span class="text-2xl font-bold text-green-600" id="venda-valor-display">R$ 0,00</span>
                        </div>

                        <button onclick="confirmarVenda()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-4 rounded-lg shadow-lg transform transition active:scale-95 flex justify-center items-center gap-2">
                            <i class="fa-solid fa-check"></i> CONFIRMAR VENDA
                        </button>
                    </div>
                </div>

                <div class="w-full md:w-1/2 flex items-center justify-center">
                    <div class="text-center text-slate-400">
                        <i class="fa-solid fa-cart-arrow-down text-6xl mb-4"></i>
                        <p class="text-lg">Selecione um item ao lado para registrar a saída do estoque e entrada financeira.</p>
                        <p class="text-sm mt-2 text-orange-400"><i class="fa-solid fa-triangle-exclamation"></i> Isso não baixa o estoque automaticamente nesta versão simples.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="produtos" class="tela hidden fade-in">
            <h2 class="text-3xl font-bold text-slate-800 mb-6">Gerenciar Catálogo</h2>
            
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                <div class="bg-white p-6 rounded-xl shadow-md h-fit">
                    <h3 class="font-bold text-lg mb-4 text-slate-700 border-b pb-2">Cadastrar Novo Item</h3>
                    
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Nome do Item/Serviço</label>
                    <input type="text" id="prod-nome" class="w-full p-2 border rounded mb-3 focus:outline-blue-500" placeholder="Ex: Filtro de Óleo">
                    
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Preço (R$)</label>
                    <input type="number" id="prod-preco" step="0.01" class="w-full p-2 border rounded mb-6 focus:outline-blue-500" placeholder="0.00">
                    
                    <button onclick="cadastrarProduto()" class="w-full bg-green-600 text-white font-bold py-2 rounded hover:bg-green-700 transition">
                        <i class="fa-solid fa-plus"></i> Adicionar
                    </button>
                </div>

                <div class="col-span-2 bg-white rounded-xl shadow-md overflow-hidden">
                    <div class="p-4 bg-slate-800 text-white font-bold flex justify-between">
                        <span>Itens Cadastrados</span>
                        <span class="text-xs bg-slate-700 px-2 py-1 rounded" id="contador-lista-prod">0 itens</span>
                    </div>
                    <div class="overflow-y-auto max-h-[500px]">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-slate-100 text-slate-600 sticky top-0">
                                <tr>
                                    <th class="p-3">Nome</th>
                                    <th class="p-3 text-right">Preço</th>
                                    <th class="p-3 text-center">Ação</th>
                                </tr>
                            </thead>
                            <tbody id="tabela-produtos-body">
                                </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

        <section id="historico" class="tela hidden fade-in">
            <h2 class="text-3xl font-bold text-slate-800 mb-6">Relatório de Vendas</h2>
            <div class="bg-white rounded-xl shadow-md overflow-hidden">
                <table class="w-full text-left">
                    <thead class="bg-slate-800 text-white">
                        <tr>
                            <th class="p-4">Data / Hora</th>
                            <th class="p-4">Produto / Serviço</th>
                            <th class="p-4 text-right">Valor</th>
                        </tr>
                    </thead>
                    <tbody id="tabela-historico-body" class="divide-y divide-slate-100">
                        </tbody>
                </table>
            </div>
        </section>

    </main>

    <script>
        // --- ESTADO INICIAL DO SISTEMA ---
        let produtos = JSON.parse(localStorage.getItem('oficina_produtos')) || [];
        let vendas = JSON.parse(localStorage.getItem('oficina_vendas')) || [];

        // Ao carregar a página
        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('data-hoje').innerText = new Date().toLocaleDateString('pt-BR');
            atualizarInterface();
            
            // Listener para atualizar o preço na tela de venda quando troca o select
            document.getElementById('venda-select').addEventListener('change', function() {
                const index = this.value;
                if (index !== "") {
                    const preco = parseFloat(produtos[index].preco);
                    document.getElementById('venda-valor-display').innerText = preco.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
                    document.getElementById('venda-valor-display').classList.remove('text-slate-400');
                    document.getElementById('venda-valor-display').classList.add('text-green-600');
                } else {
                    document.getElementById('venda-valor-display').innerText = "R$ 0,00";
                }
            });
        });

        // --- FUNÇÕES DE NAVEGAÇÃO ---
        function navegar(telaId) {
            // Esconde todas as telas
            document.querySelectorAll('.tela').forEach(t => t.classList.add('hidden'));
            // Remove estilo ativo do menu
            document.querySelectorAll('.nav-item').forEach(n => {
                n.classList.remove('bg-slate-800', 'border-blue-500');
                n.classList.add('border-transparent');
            });

            // Mostra a tela selecionada
            document.getElementById(telaId).classList.remove('hidden');
            
            // Ativa o botão no menu (lógica simples baseada na ordem, pode ser melhorada mas funciona)
            const map = { 'dashboard': 0, 'vendas': 1, 'produtos': 2, 'historico': 3 };
            const botoes = document.querySelectorAll('.nav-item');
            if(botoes[map[telaId]]) {
                botoes[map[telaId]].classList.add('bg-slate-800', 'border-blue-500');
                botoes[map[telaId]].classList.remove('border-transparent');
            }
        }

        // --- FUNÇÕES PRINCIPAIS ---

        function atualizarInterface() {
            // 1. Atualizar Dashboard
            const totalFaturado = vendas.reduce((acc, v) => acc + Number(v.preco), 0);
            document.getElementById('dash-faturamento').innerText = totalFaturado.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
            document.getElementById('dash-qtd-vendas').innerText = vendas.length;
            document.getElementById('dash-qtd-produtos').innerText = produtos.length;

            // 2. Atualizar Select de Vendas
            const select = document.getElementById('venda-select');
            select.innerHTML = '<option value="">Selecione um item...</option>';
            produtos.forEach((prod, idx) => {
                select.innerHTML += `<option value="${idx}">${prod.nome}</option>`;
            });

            // 3. Atualizar Tabela de Produtos
            const tbodyProd = document.getElementById('tabela-produtos-body');
            tbodyProd.innerHTML = '';
            produtos.forEach((prod, idx) => {
                tbodyProd.innerHTML += `
                    <tr class="hover:bg-slate-50">
                        <td class="p-3 font-medium text-slate-700">${prod.nome}</td>
                        <td class="p-3 text-right text-blue-600 font-bold">R$ ${parseFloat(prod.preco).toFixed(2)}</td>
                        <td class="p-3 text-center">
                            <button onclick="deletarProduto(${idx})" class="text-red-400 hover:text-red-600">
                                <i class="fa-solid fa-trash"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            document.getElementById('contador-lista-prod').innerText = `${produtos.length} itens`;

            // 4. Atualizar Tabelas de Vendas (Dashboard e Histórico)
            renderizarTabelasVendas();
        }

        function renderizarTabelasVendas() {
            const vendasInvertidas = [...vendas].reverse(); // Cópia invertida para mostrar mais recentes primeiro
            
            // Dashboard (Apenas 5)
            const tbodyDash = document.getElementById('dash-tabela-body');
            tbodyDash.innerHTML = vendasInvertidas.length ? '' : '<tr><td colspan="3" class="p-4 text-center text-slate-400">Nenhuma venda hoje.</td></tr>';
            
            vendasInvertidas.slice(0, 5).forEach(v => {
                tbodyDash.innerHTML += `
                    <tr class="border-b border-slate-50 last:border-0">
                        <td class="p-4 font-bold text-slate-700">${v.produto}</td>
                        <td class="p-4 text-slate-500 text-xs">${v.data}</td>
                        <td class="p-4 text-right text-green-600 font-bold">+ R$ ${parseFloat(v.preco).toFixed(2)}</td>
                    </tr>
                `;
            });

            // Histórico Completo
            const tbodyHist = document.getElementById('tabela-historico-body');
            tbodyHist.innerHTML = vendasInvertidas.length ? '' : '<tr><td colspan="3" class="p-8 text-center text-slate-400">Sem registros.</td></tr>';
            
            vendasInvertidas.forEach(v => {
                tbodyHist.innerHTML += `
                    <tr class="hover:bg-slate-50">
                        <td class="p-4 text-slate-500">${v.data}</td>
                        <td class="p-4 font-bold text-slate-700">${v.produto}</td>
                        <td class="p-4 text-right text-green-600 font-bold">R$ ${parseFloat(v.preco).toFixed(2)}</td>
                    </tr>
                `;
            });
        }

        // --- AÇÕES DO USUÁRIO ---

        function cadastrarProduto() {
            const nomeInput = document.getElementById('prod-nome');
            const precoInput = document.getElementById('prod-preco');
            const nome = nomeInput.value.trim();
            const preco = parseFloat(precoInput.value);

            if (!nome || isNaN(preco)) {
                alert("Por favor, preencha o nome e um preço válido.");
                return;
            }

            produtos.push({ nome, preco });
            salvarDados();
            
            // Limpar campos
            nomeInput.value = '';
            precoInput.value = '';
            nomeInput.focus();
            
            alert("Produto cadastrado!");
        }

        function deletarProduto(index) {
            if(confirm("Tem certeza que deseja remover este item do catálogo?")) {
                produtos.splice(index, 1);
                salvarDados();
            }
        }

        function confirmarVenda() {
            const select = document.getElementById('venda-select');
            const index = select.value;

            if (index === "") {
                alert("Selecione um produto primeiro!");
                return;
            }

            const produtoSelecionado = produtos[index];
            const dataAgora = new Date().toLocaleString('pt-BR');

            vendas.push({
                produto: produtoSelecionado.nome,
                preco: produtoSelecionado.preco,
                data: dataAgora
            });

            salvarDados();
            
            // Feedback visual e reset
            alert(`Venda Confirmada: ${produtoSelecionado.nome}`);
            select.value = "";
            document.getElementById('venda-valor-display').innerText = "R$ 0,00";
            
            navegar('dashboard'); // Volta para o início para ver o dinheiro entrar
        }

        function salvarDados() {
            localStorage.setItem('oficina_produtos', JSON.stringify(produtos));
            localStorage.setItem('oficina_vendas', JSON.stringify(vendas));
            atualizarInterface();
        }

        function limparDados() {
            if(confirm("ATENÇÃO: Isso apagará TODO o histórico de vendas e produtos cadastrados. Deseja continuar?")) {
                localStorage.removeItem('oficina_produtos');
                localStorage.removeItem('oficina_vendas');
                location.reload();
            }
        }
    </script>
</body>
</html>
