// Função para alternar entre as abas
function showTab(tabId) {
    document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
    document.getElementById(tabId).classList.remove('hidden');
    
    // Se abrir a aba de vendas, atualiza a lista de produtos
    if(tabId === 'tab-vendas') {
        atualizarSelectProdutos();
    }
    
    // Se abrir a aba de relatórios, atualiza a tabela
    if(tabId === 'tab-relatorio') {
        atualizarRelatorio();
    }
}

// SALVAR PRODUTO
function salvarProduto() {
    const nome = document.getElementById('nome-p').value;
    const preco = parseFloat(document.getElementById('preco-p').value);

    if(!nome || !preco) return alert("Preencha todos os campos!");

    let produtos = JSON.parse(localStorage.getItem('produtos') || '[]');
    produtos.push({ id: Date.now(), nome, preco }); // Adicionamos um ID único
    localStorage.setItem('produtos', JSON.stringify(produtos));

    // Limpa os campos
    document.getElementById('nome-p').value = '';
    document.getElementById('preco-p').value = '';
    
    alert('Produto cadastrado com sucesso!');
}

// ATUALIZAR O SELECT (A mágica que faltava)
function atualizarSelectProdutos() {
    const select = document.getElementById('select-produto');
    const produtos = JSON.parse(localStorage.getItem('produtos') || '[]');

    select.innerHTML = '<option value="">Escolha um produto...</option>';

    produtos.forEach(prod => {
        const option = document.createElement('option');
        option.value = prod.id;
        option.textContent = `${prod.nome} - R$ ${prod.preco.toFixed(2)}`;
        select.appendChild(option);
    });
}

// REGISTRAR VENDA
function registrarVenda() {
    const select = document.getElementById('select-produto');
    const idProduto = select.value;
    
    if(!idProduto) return alert("Selecione um produto primeiro!");

    const produtos = JSON.parse(localStorage.getItem('produtos') || '[]');
    const produtoVendido = produtos.find(p => p.id == idProduto);

    let vendas = JSON.parse(localStorage.getItem('vendas') || '[]');
    vendas.push({
        data: new Date().toLocaleString(),
        nome: produtoVendido.nome,
        preco: produtoVendido.preco
    });

    localStorage.setItem('vendas', JSON.stringify(vendas));
    alert(`Venda de ${produtoVendido.nome} registrada!`);
}
