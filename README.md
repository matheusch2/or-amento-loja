<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Orçamento da Loja</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  :root {
    --azul: #2563eb; --azul-dk: #1e3a8a; --azul-cl: #eff6ff;
    --tinta: #1f2937; --cinza: #6b7280; --linha: #e5e7eb;
    --sombra: 0 1px 2px rgba(15,23,42,.04), 0 6px 16px -8px rgba(15,23,42,.10);
    --sombra-lg: 0 10px 30px -12px rgba(30,58,138,.28);
  }
  html { scroll-behavior: smooth; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background: linear-gradient(180deg, #eef2ff 0%, #f3f4f6 220px, #f3f4f6 100%);
    color: var(--tinta); min-height: 100vh; -webkit-font-smoothing: antialiased;
  }

  .topo {
    background: linear-gradient(135deg, var(--azul) 0%, var(--azul-dk) 100%);
    color: #fff; padding: 22px 16px 20px; text-align: center;
    position: sticky; top: 0; z-index: 40;
    box-shadow: 0 8px 24px -12px rgba(30,58,138,.55);
  }
  .topo h1 { font-size: 19px; font-weight: 800; letter-spacing: .2px; }
  .topo p { font-size: 12.5px; opacity: .82; margin-top: 3px; }

  .wrap { max-width: 720px; margin: 0 auto; padding: 16px 14px 40px; }

  .abas { display: flex; gap: 8px; margin-bottom: 16px; background: #e9ecf2; padding: 5px; border-radius: 14px; }
  .aba {
    flex: 1; background: transparent; border: none; color: #55607a;
    border-radius: 10px; padding: 11px; font-size: 13.5px; font-weight: 700;
    cursor: pointer; transition: background .18s ease, color .18s ease, box-shadow .18s ease;
  }
  .aba:hover { color: var(--azul-dk); }
  .aba.ativa { background: #fff; color: var(--azul); box-shadow: var(--sombra); }

  .card {
    background: #fff; border: 1px solid var(--linha); border-radius: 16px;
    padding: 18px; margin-bottom: 14px; box-shadow: var(--sombra);
    transition: box-shadow .18s ease;
  }
  .card h2 { font-size: 14px; font-weight: 800; margin-bottom: 12px; color: var(--azul-dk); letter-spacing: .1px; }
  label { display: block; font-size: 12.5px; font-weight: 700; color: #374151; margin: 10px 0 5px; }
  input {
    width: 100%; border: 1.5px solid #d1d5db; border-radius: 10px; padding: 12px;
    font-size: 15px; outline: none; background: #fafbfc; transition: border-color .15s ease, box-shadow .15s ease, background .15s ease;
  }
  input:focus { border-color: var(--azul); background: #fff; box-shadow: 0 0 0 3.5px rgba(37,99,235,.14); }
  input::placeholder { color: #9ca3af; }
  .linha2 { display: flex; gap: 10px; }
  .linha2 > div { flex: 1; }

  .btn {
    width: 100%; margin-top: 14px; background: linear-gradient(135deg, var(--azul), var(--azul-dk));
    color: #fff; border: none; border-radius: 12px; padding: 13px; font-size: 15px; font-weight: 700;
    cursor: pointer; box-shadow: var(--sombra-lg); transition: transform .12s ease, box-shadow .12s ease, filter .12s ease;
  }
  .btn:hover { filter: brightness(1.05); }
  .btn:active { transform: translateY(1px) scale(.99); box-shadow: 0 4px 14px -8px rgba(30,58,138,.5); }
  .btn-sec { background: #fff; color: var(--azul); border: 1.5px solid var(--azul); box-shadow: none; }
  .btn-sec:hover { background: var(--azul-cl); }
  .msg { display: none; margin-top: 10px; font-size: 13px; font-weight: 600; text-align: center; animation: aparecer .18s ease; }
  .msg.ok { color: #059669; } .msg.erro { color: #dc2626; }

  .item {
    display: flex; align-items: center; gap: 10px; padding: 12px 8px;
    border-bottom: 1px solid #f0f1f3; border-radius: 10px; transition: background .15s ease;
  }
  .item:hover { background: #f8fafc; }
  .item:last-child { border-bottom: none; }
  .item-info { flex: 1; min-width: 0; }
  .item-num { font-size: 14px; font-weight: 800; }
  .item-desc { font-size: 12px; color: var(--cinza); }
  .item-vals { font-size: 12px; color: #374151; margin-top: 2px; }
  .item-vals b { color: #111827; }
  .lixo {
    background: #fee2e2; color: #b91c1c; border: none; border-radius: 9px;
    width: 34px; height: 34px; font-size: 15px; cursor: pointer; flex-shrink: 0; transition: background .15s ease, transform .12s ease;
  }
  .lixo:hover { background: #fecaca; }
  .lixo:active { transform: scale(.92); }
  .add {
    background: #dcfce7; color: #166534; border: none; border-radius: 9px; padding: 8px 12px;
    font-size: 13px; font-weight: 700; cursor: pointer; flex-shrink: 0; transition: background .15s ease, transform .12s ease;
  }
  .add:hover { background: #bbf7d0; }
  .add:active { transform: scale(.96); }
  .vazio { text-align: center; color: #9ca3af; font-size: 13px; padding: 30px 0; }
  .vazio::before { content: "📭"; display: block; font-size: 26px; margin-bottom: 8px; }

  .qtd { width: 56px !important; padding: 8px !important; text-align: center; }
  .orc-total { display: flex; justify-content: space-between; gap: 10px; margin-top: 14px; }
  .orc-total .bloco {
    flex: 1; background: linear-gradient(160deg, var(--azul-cl), #e0e9ff);
    border: 1px solid #dbe6ff; border-radius: 12px; padding: 13px; text-align: center;
  }
  .orc-total small { font-size: 11px; color: var(--cinza); font-weight: 700; text-transform: uppercase; letter-spacing: .3px; }
  .orc-total b { display: block; font-size: 19px; color: var(--azul-dk); margin-top: 2px; }

  .toast {
    position: fixed; left: 50%; bottom: 24px; transform: translateX(-50%);
    background: #111827; color: #fff; font-size: 13px; font-weight: 600;
    border-radius: 12px; padding: 11px 18px; z-index: 50; box-shadow: 0 12px 28px -10px rgba(0,0,0,.45);
    animation: subir .22s ease;
  }

  @keyframes subir { from { opacity: 0; transform: translate(-50%, 10px); } to { opacity: 1; transform: translate(-50%, 0); } }
  @keyframes aparecer { from { opacity: 0; } to { opacity: 1; } }
</style>
</head>
<body>

<div class="topo">
  <h1>🧾 Orçamento da Loja</h1>
  <p>Cadastre os produtos e monte orçamentos</p>
</div>

<div class="wrap">
  <div class="abas">
    <button class="aba ativa" id="tab-cad" onclick="mostrarAba('cad')">Cadastrar produto</button>
    <button class="aba" id="tab-orc" onclick="mostrarAba('orc')">Novo orçamento</button>
  </div>

  <div id="aba-cad">
    <div class="card">
      <h2>Cadastrar produto</h2>
      <label>Número do produto</label>
      <input type="text" id="c-num" inputmode="numeric" placeholder="Ex: 1024">
      <label>Descrição (opcional)</label>
      <input type="text" id="c-desc" placeholder="Ex: Furadeira 500W">
      <div class="linha2">
        <div>
          <label>Valor à vista (R$)</label>
          <input type="text" id="c-vista" inputmode="decimal" placeholder="0,00">
        </div>
        <div>
          <label>Valor no cartão (R$)</label>
          <input type="text" id="c-cartao" inputmode="decimal" placeholder="0,00">
        </div>
      </div>
      <button class="btn" onclick="cadastrarProduto()">Cadastrar</button>
      <div class="msg" id="c-msg"></div>
    </div>

    <div class="card">
      <h2>Produtos cadastrados (<span id="cad-qtd">0</span>)</h2>
      <input type="search" id="cad-busca" placeholder="Buscar por número ou descrição…" oninput="renderCadastrados()" style="margin-bottom:6px">
      <div id="lista-cad"></div>
    </div>
  </div>

  <div id="aba-orc" style="display:none">
    <div class="card">
      <h2>Adicionar produtos ao orçamento</h2>
      <input type="search" id="orc-busca" placeholder="Pesquisar produto por número ou descrição…" oninput="renderBuscaOrc()">
      <div id="lista-busca"></div>
    </div>

    <div class="card">
      <h2>Orçamento atual (<span id="orc-qtd">0</span>)</h2>
      <div id="lista-orc"></div>
      <div class="orc-total" id="orc-totais" style="display:none">
        <div class="bloco"><small>Total à vista</small><b id="tot-vista">R$ 0,00</b></div>
        <div class="bloco"><small>Total cartão</small><b id="tot-cartao">R$ 0,00</b></div>
      </div>
      <button class="btn" id="btn-imprimir" onclick="imprimirOrcamento()" style="display:none">🖨️ Imprimir orçamento</button>
      <button class="btn btn-sec" id="btn-limpar" onclick="limparOrcamento()" style="display:none;margin-top:8px">Limpar orçamento</button>
    </div>
  </div>
</div>

<script>
  let produtos = JSON.parse(localStorage.getItem("orc_produtos") || "[]");
  let orcamento = [];

  const salvarProdutos = () => localStorage.setItem("orc_produtos", JSON.stringify(produtos));
  const rs = v => "R$ " + Number(v || 0).toLocaleString("pt-BR", { minimumFractionDigits: 2, maximumFractionDigits: 2 });
  function parseMoeda(txt) {
    if (txt == null) return NaN;
    let s = String(txt).trim().replace(/\s/g, "").replace(/R\$/i, "");
    if (s.indexOf(",") > -1) s = s.replace(/\./g, "").replace(",", ".");
    return parseFloat(s);
  }
  function toast(m) {
    document.querySelectorAll(".toast").forEach(t => t.remove());
    const t = document.createElement("div"); t.className = "toast"; t.textContent = m;
    document.body.appendChild(t); setTimeout(() => t.remove(), 2500);
  }
  function esc(s) { return String(s == null ? "" : s).replace(/[&<>"]/g, c => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" }[c])); }

  function mostrarAba(qual) {
    document.getElementById("aba-cad").style.display = qual === "cad" ? "block" : "none";
    document.getElementById("aba-orc").style.display = qual === "orc" ? "block" : "none";
    document.getElementById("tab-cad").classList.toggle("ativa", qual === "cad");
    document.getElementById("tab-orc").classList.toggle("ativa", qual === "orc");
    if (qual === "cad") renderCadastrados();
    else { renderBuscaOrc(); renderOrcamento(); }
  }

  function cadastrarProduto() {
    const num = document.getElementById("c-num").value.trim();
    const desc = document.getElementById("c-desc").value.trim();
    const vista = parseMoeda(document.getElementById("c-vista").value);
    const cartao = parseMoeda(document.getElementById("c-cartao").value);
    const msg = document.getElementById("c-msg");
    const erro = t => { msg.textContent = t; msg.className = "msg erro"; msg.style.display = "block"; };

    if (!num) return erro("Informe o número do produto.");
    if (isNaN(vista) || vista < 0) return erro("Informe o valor à vista.");
    if (isNaN(cartao) || cartao < 0) return erro("Informe o valor no cartão.");
    if (produtos.some(p => p.num.toLowerCase() === num.toLowerCase()))
      return erro(`Já existe um produto com o número "${num}".`);

    produtos.push({ id: "p" + Date.now(), num, desc, vista, cartao });
    salvarProdutos();
    document.getElementById("c-num").value = "";
    document.getElementById("c-desc").value = "";
    document.getElementById("c-vista").value = "";
    document.getElementById("c-cartao").value = "";
    msg.textContent = "Produto cadastrado! ✅"; msg.className = "msg ok"; msg.style.display = "block";
    setTimeout(() => { msg.style.display = "none"; }, 2000);
    renderCadastrados();
  }

  function excluirProduto(id) {
    produtos = produtos.filter(p => p.id !== id);
    orcamento = orcamento.filter(o => o.produtoId !== id);
    salvarProdutos();
    renderCadastrados();
  }

  function renderCadastrados() {
    const f = (document.getElementById("cad-busca").value || "").toLowerCase();
    const lista = produtos.filter(p => !f || p.num.toLowerCase().includes(f) || (p.desc || "").toLowerCase().includes(f));
    document.getElementById("cad-qtd").textContent = produtos.length;
    const el = document.getElementById("lista-cad");
    if (!lista.length) { el.innerHTML = `<div class="vazio">Nenhum produto cadastrado.</div>`; return; }
    el.innerHTML = lista.map(p => `
      <div class="item">
        <div class="item-info">
          <div class="item-num">Nº ${esc(p.num)}</div>
          ${p.desc ? `<div class="item-desc">${esc(p.desc)}</div>` : ""}
          <div class="item-vals">À vista <b>${rs(p.vista)}</b> · Cartão <b>${rs(p.cartao)}</b></div>
        </div>
        <button class="lixo" onclick="excluirProduto('${p.id}')">🗑️</button>
      </div>`).join("");
  }

  function renderBuscaOrc() {
    const f = (document.getElementById("orc-busca").value || "").toLowerCase();
    const el = document.getElementById("lista-busca");
    if (!produtos.length) { el.innerHTML = `<div class="vazio">Cadastre produtos primeiro.</div>`; return; }
    const lista = produtos.filter(p => !f || p.num.toLowerCase().includes(f) || (p.desc || "").toLowerCase().includes(f));
    if (!lista.length) { el.innerHTML = `<div class="vazio">Nenhum produto encontrado.</div>`; return; }
    el.innerHTML = lista.map(p => `
      <div class="item">
        <div class="item-info">
          <div class="item-num">Nº ${esc(p.num)}${p.desc ? ` <span class="item-desc">· ${esc(p.desc)}</span>` : ""}</div>
          <div class="item-vals">À vista <b>${rs(p.vista)}</b> · Cartão <b>${rs(p.cartao)}</b></div>
        </div>
        <button class="add" onclick="addAoOrcamento('${p.id}')">+ Adicionar</button>
      </div>`).join("");
  }

  function addAoOrcamento(id) {
    const item = orcamento.find(o => o.produtoId === id);
    if (item) item.qtd++;
    else orcamento.push({ produtoId: id, qtd: 1 });
    toast("Adicionado ao orçamento");
    renderOrcamento();
  }

  function mudarQtd(id, valor) {
    const item = orcamento.find(o => o.produtoId === id);
    if (!item) return;
    item.qtd = Math.max(1, parseInt(valor) || 1);
    renderOrcamento();
  }

  function removerDoOrcamento(id) {
    orcamento = orcamento.filter(o => o.produtoId !== id);
    renderOrcamento();
  }

  function totaisOrcamento() {
    let vista = 0, cartao = 0;
    orcamento.forEach(o => {
      const p = produtos.find(x => x.id === o.produtoId);
      if (!p) return;
      vista += p.vista * o.qtd;
      cartao += p.cartao * o.qtd;
    });
    return { vista, cartao };
  }

  function renderOrcamento() {
    document.getElementById("orc-qtd").textContent = orcamento.length;
    const el = document.getElementById("lista-orc");
    const temItens = orcamento.length > 0;
    document.getElementById("orc-totais").style.display = temItens ? "flex" : "none";
    document.getElementById("btn-imprimir").style.display = temItens ? "block" : "none";
    document.getElementById("btn-limpar").style.display = temItens ? "block" : "none";
    if (!temItens) { el.innerHTML = `<div class="vazio">Pesquise e adicione produtos acima.</div>`; return; }
    el.innerHTML = orcamento.map(o => {
      const p = produtos.find(x => x.id === o.produtoId);
      if (!p) return "";
      return `
      <div class="item">
        <input type="number" class="qtd" min="1" value="${o.qtd}" onchange="mudarQtd('${p.id}', this.value)">
        <div class="item-info">
          <div class="item-num">Nº ${esc(p.num)}${p.desc ? ` <span class="item-desc">· ${esc(p.desc)}</span>` : ""}</div>
          <div class="item-vals">À vista <b>${rs(p.vista * o.qtd)}</b> · Cartão <b>${rs(p.cartao * o.qtd)}</b></div>
        </div>
        <button class="lixo" onclick="removerDoOrcamento('${p.id}')">✕</button>
      </div>`;
    }).join("");
    const t = totaisOrcamento();
    document.getElementById("tot-vista").textContent = rs(t.vista);
    document.getElementById("tot-cartao").textContent = rs(t.cartao);
  }

  function limparOrcamento() {
    if (!orcamento.length) return;
    orcamento = [];
    renderOrcamento();
  }

  function imprimirOrcamento() {
    if (!orcamento.length) return;
    const hoje = new Date().toLocaleDateString("pt-BR");
    const linhas = orcamento.map(o => {
      const p = produtos.find(x => x.id === o.produtoId);
      if (!p) return "";
      return `<tr>
        <td>${esc(p.num)}</td>
        <td>${esc(p.desc || "-")}</td>
        <td style="text-align:center">${o.qtd}</td>
        <td style="text-align:right">${rs(p.vista)}</td>
        <td style="text-align:right">${rs(p.cartao)}</td>
        <td style="text-align:right">${rs(p.vista * o.qtd)}</td>
        <td style="text-align:right">${rs(p.cartao * o.qtd)}</td>
      </tr>`;
    }).join("");
    const t = totaisOrcamento();
    const html = `<!DOCTYPE html><html><head><meta charset="utf-8"><title>Orçamento</title>
    <style>
      body{font-family:Arial,sans-serif;padding:24px;color:#1f2937;max-width:800px;margin:0 auto}
      h1{color:#1e40af;font-size:22px;margin:0 0 2px}
      .sub{color:#6b7280;font-size:13px;margin-bottom:18px}
      table{width:100%;border-collapse:collapse;font-size:13px}
      th{background:#1e40af;color:#fff;padding:9px 8px;text-align:left}
      td{padding:8px;border-bottom:1px solid #e5e7eb}
      .tot td{font-weight:800;font-size:15px;border-top:2px solid #1e40af;color:#1e40af;background:#eff6ff}
      @media print{body{padding:0}}
    </style></head><body>
      <h1>🧾 Orçamento</h1>
      <div class="sub">Emitido em ${hoje} · ${orcamento.length} item(ns)</div>
      <table>
        <thead><tr>
          <th>Nº</th><th>Descrição</th><th style="text-align:center">Qtd</th>
          <th style="text-align:right">À vista (un)</th><th style="text-align:right">Cartão (un)</th>
          <th style="text-align:right">Subtotal à vista</th><th style="text-align:right">Subtotal cartão</th>
        </tr></thead>
        <tbody>
          ${linhas}
          <tr class="tot"><td colspan="5">TOTAL</td><td style="text-align:right">${rs(t.vista)}</td><td style="text-align:right">${rs(t.cartao)}</td></tr>
        </tbody>
      </table>
      <div class="no-print" style="text-align:center;margin-top:22px">
        <button onclick="window.print()" style="background:#1e40af;color:#fff;border:none;border-radius:8px;padding:10px 18px;font-size:14px;font-weight:700;cursor:pointer">🖨️ Imprimir / Salvar PDF</button>
      </div>
    </body></html>`;
    const w = window.open("", "_blank");
    w.document.write(html);
    w.document.close();
  }

  renderCadastrados();
</script>
</body>
</html>
