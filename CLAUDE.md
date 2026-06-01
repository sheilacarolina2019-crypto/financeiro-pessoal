# Projeto: Financeiro Pessoal — Daniel Gomes

## Contexto do usuário
- **Nome:** Daniel Paulo Oliveira Gomes, Personal Trainer, Rio de Janeiro (Duque de Caxias)
- **Renda média:** R$ 7.000–8.000/mês (maio/2026 foi R$ 9.000, acima da média)
- **Objetivo:** Controlar finanças, sair do déficit mensal, construir reserva de emergência
- **Problema principal:** Déficit de ~R$ 1.478 nos meses de renda média

## Stack do projeto
- HTML/CSS/JavaScript puro — arquivo único `index.html`
- Chart.js 4.4.1 (via CDN)
- `localStorage` para persistência de dados
- Sem frameworks, sem build, sem dependências locais
- Hospedado no **GitHub Pages** (gratuito): https://sheilacarolina2019-crypto.github.io/financeiro-pessoal/

## Login
- Usuário: `daniel`
- Senha: `personal2026`
- Definido na constante `CREDS` no topo do `<script>`

## Estrutura do arquivo index.html
```
<style>          → CSS completo com variáveis CSS (dark theme)
<body>
  #login-screen  → tela de login
  #app
    .sidebar     → navegação lateral (8 abas)
    .main
      #page-*    → cada aba é uma div.page
  modais         → modal-lanc, modal-fixo, modal-alert, modal-meta
<script>
  AUTH           → doLogin(), doLogout()
  STORAGE        → load(key, default), save(key, val)
  DATA           → DEFAULT_MONTHS, DEFAULT_LANC, DEFAULT_FIXOS, DEFAULT_ALERTS, DEFAULT_METAS
  NAVIGATION     → showPage(), notify(), uid(), fmt(), fmtDate()
  INIT           → initApp()
  DASHBOARD      → renderDashboard()
  LANÇAMENTOS    → renderLancamentos(), openLancModal(), saveLanc(), deleteLanc()
  FIXOS          → renderFixos(), openFixoModal(), saveFixo(), deleteFixo()
  TRANSAÇÕES     → renderTxn(), renderTxnFilters()
  MESES          → renderMeses(), showMonthDetail(), addMonth()
  ALERTAS        → renderAlertas(), saveAlert(), deleteAlert(), updateAlertBadge()
  PROJEÇÃO       → updateSim(), renderProjChart(), renderCortes()
  METAS          → renderMetas(), saveMeta(), deleteMeta(), editAcao()
```

## Storage keys (localStorage)
| Chave | Conteúdo |
|---|---|
| `fin_months` | Array de meses com renda e cats |
| `fin_lanc` | Array de lançamentos diários/semanais/mensais |
| `fin_fixos` | Array de gastos fixos (parcela/débito/medicamento) |
| `fin_alerts` | Array de alertas por categoria |
| `fin_metas` | Objeto `{ curto: [], medio: [] }` |
| `fin_acoes` | Array de ações do plano |

## Categorias disponíveis (CAT_META)
```js
trabalho | saude | alimentacao | delivery | moradia | noiva | assinatura | outros | renda
```
Cada uma tem: `label`, `color` (hex), `icon` (emoji)

## Gastos fixos reais do Daniel
**Parcelas:**
- MacBook noiva (Shopee): R$ 542,17/mês — 6x restantes
- Financiamento carro: R$ 672/mês — 7x restantes
- CREF/RJ: R$ 42/mês — 8x restantes
- Shopee PlatinumHairCa (noiva): R$ 124,91/mês — 9x restantes

**Débitos obrigatórios mensais:**
- Smartfit (academia trabalho): R$ 428,40
- Simple Gym (academia trabalho): R$ 347
- Condomínio: R$ 200
- Celular (plano): R$ 84
- Spotify: R$ 31,90
- TreinarMe (app treino): R$ 140
- Areia do gato: R$ 41,02

**Medicamentos:**
- Venvanse Daniel (TDAH genérico): ~R$ 280/mês
- Venvanse noiva (TDAH genérico): ~R$ 270/mês

**Fixos fora do cartão (débito/dinheiro):**
- Gasolina: R$ 875–1.000/mês
- Barbearia: R$ 50/mês
- PIX pais (internet): R$ 85/mês
- PIX noiva (mensal): R$ 300–600/mês

## Faturas analisadas
- **Itaú 2796** (maio/2026): R$ 2.056,08
- **Santander 2706 + 5961** (junho/2026): R$ 3.762,45
- Total faturas: R$ 5.818,53
- Santander está com 93,7% do limite usado (R$ 8.255 de R$ 8.810) ⚠️

## Regras de desenvolvimento importantes
1. **Nunca reescrever o arquivo inteiro** — editar apenas o trecho necessário
2. **Charts devem ser destruídos antes de recriar:** `if(charts.X) charts.X.destroy();`
3. **Sempre usar `load(SK.chave, DEFAULT_CHAVE)`** para ler dados — nunca acessar localStorage diretamente
4. **Sempre usar `save(SK.chave, dados)`** para gravar
5. **Modais:** abrir com `display:'flex'`, fechar com `display:'none'`
6. **Notificações:** usar `notify('mensagem', 'success'|'error')`
7. **IDs únicos:** usar `uid()` ao criar novos registros
8. **Formatação de moeda:** sempre usar `fmt(valor)` → retorna "R$ X.XXX,XX"
9. **Formatação de data:** usar `fmtDate('YYYY-MM-DD')` → retorna "DD/MM/AA"
10. **Não alterar** os blocos `DEFAULT_*` — são os dados iniciais/fallback

## Abas existentes (sidebar)
| ID | Função principal |
|---|---|
| `dashboard` | Visão geral, gráficos, alertas, lançamentos recentes |
| `lancamentos` | CRUD de lançamentos (receita/despesa/pix, recorrência) |
| `fixos` | Parcelas, débitos obrigatórios, medicamentos |
| `transacoes` | Fatura detalhada Itaú+Santander (dados estáticos) |
| `meses` | Histórico mensal com detalhe por mês |
| `alertas` | Limites por categoria, notificações automáticas |
| `projecao` | Simulador com sliders, projeção 12 meses |
| `metas` | Metas curto/médio prazo, plano de ação |
| `adicionar` | Formulário para novo mês |

## Ideias de próximas funcionalidades (prioridade)
- [ ] Exportar mês como PDF ou CSV
- [ ] Comparativo lado a lado entre dois meses
- [ ] Gráfico de tendência por categoria (subindo/descendo)
- [ ] Campo de saldo atual em conta (para controle de caixa real)
- [ ] Notificação de parcelas próximas do fim
- [ ] Modo mobile com menu hambúrguer
- [ ] Meta de reserva com valor atual informado pelo usuário
- [ ] Relatório mensal automático com resumo textual

## Como adicionar uma nova aba
1. Adicionar `<div class="nav-item" onclick="showPage('novaaba',this)">` no sidebar
2. Adicionar `<div id="page-novaaba" class="page">` no main
3. Adicionar a função `renderNovaAba()` no script
4. Registrar no objeto `renders` dentro de `showPage()`

## Como subir alterações no GitHub Pages
1. Abrir o repositório: https://github.com/sheilacarolina2019-crypto/financeiro-pessoal
2. Clicar no arquivo `index.html`
3. Clicar no ícone de lápis (Edit)
4. Colar o novo conteúdo (ou usar upload)
5. Commit → o site atualiza em ~1 minuto
