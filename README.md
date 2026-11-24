# Dashboard de Pedidos · Molokai

Dashboard de vendas da loja **Molokai** focado na **atribuição dos pedidos**: de onde vieram as vendas, quanto cada canal trouxe e qual o perfil dos compradores. Desenvolvido pela **B16** com base na lista de pedidos de **01 a 23/11/2026**.

É uma página HTML única e estática, sem servidor, sem banco de dados e sem build. Os dados ficam dentro do próprio `index.html`.

---

## O que o dashboard mostra

### Indicadores principais
| Indicador | Definição |
|---|---|
| **Vendas totais** | Quantidade de pedidos com status `nfeemitida` |
| **Valor líquido** | Soma da coluna "Vendas Líquidas" (sem frete), com ticket médio |
| **Itens vendidos** | Soma da coluna "Itens vendidos", com média de itens por pedido |
| **Compradores novos** | Pedidos de clientes `new` ÷ total de pedidos (%) |

### Gráficos
- **Atribuição**: vendas e valor líquido por canal, com tabela-resumo (% das vendas, % do valor, ticket médio, itens e % de novos)
- **Evolução diária**: vendas, itens vendidos e valor líquido por dia
- **Tipo de cliente**: vendas e valor líquido de novos vs recorrentes
- **Produtos**: vendas e valor líquido por livro (os 12 principais e "Outros")

### Filtros
- **Data inicial / final**: pela data do pedido
- **Produto**: mostra só os pedidos que contêm o livro escolhido
- **Limpar**: volta ao período completo e a todos os produtos

---

## Regras de cálculo

1. **Só conta como venda o pedido com status `nfeemitida`.** Pedidos em `processing` ou `faturamento` ficam fora de todos os números e aparecem apenas numa nota no topo da página (quantidade e valor).
2. **Valor líquido** usa a coluna "Vendas Líquidas", não a "Receita líquida (formatado)", que inclui frete.
3. **O valor por produto é estimado.** Quando um pedido tem vários livros, o valor é dividido entre eles pelo preço de referência de cada título (mediana dos pedidos em que o livro foi vendido sozinho). A soma por pedido continua igual à original.
4. **Com o filtro de produto ativo:** vendas = pedidos que contêm o produto; itens e valor = apenas a parte daquele produto.
5. A atribuição usa o texto da coluna "Atribuição" sem agrupamento (por exemplo, `Encaminhamento: Youtube.com` e `Origem: Fbads-frio` são canais separados).

---

## Como publicar no GitHub Pages

1. Crie um repositório (por exemplo, `dashboard-molokai`) e suba o `index.html` e este `README.md` na raiz.
2. Vá em **Settings → Pages**.
3. Em **Source**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`, e clique em **Save**.
4. Em um ou dois minutos o dashboard fica no ar em
   `https://<seu-usuario>.github.io/dashboard-molokai/`

> Se o repositório for privado, o GitHub Pages depende do plano da conta. Confira se a página pode ficar pública antes de publicar, porque ela tem nomes de clientes.

---

## Como atualizar os dados

Os dados ficam embutidos no `index.html`, na linha que começa com:

```js
const DATA = {"prods":[...],"orders":[...]};
```

Para um novo período, exporte de novo a lista de pedidos no mesmo formato de CSV e gere um novo `index.html` a partir dela (o tratamento separa os produtos de cada pedido, calcula os preços de referência e faz o rateio do valor). Depois é só substituir o arquivo no repositório.

Estrutura de cada pedido em `orders`:

| Campo | Conteúdo |
|---|---|
| `id` | Número do pedido |
| `dt` | Data (AAAA-MM-DD) |
| `st` | Status (`nfeemitida`, `processing`, `faturamento`) |
| `tp` | Tipo de cliente (`new` / `returning`) |
| `at` | Atribuição |
| `cp` | Cupom |
| `pg` | Forma de pagamento |
| `v` | Vendas líquidas do pedido (R$) |
| `l` | Itens: `[índice do produto em prods, quantidade, valor rateado]` |

---

## Tecnologias

- HTML, CSS e JavaScript puros
- [Chart.js 4](https://www.chartjs.org/) via CDN (jsDelivr)
- Fontes Bebas Neue e DM Sans via Google Fonts
- Identidade visual B16: fundo `#F4F4F2`, dourado `#D4A800`, preto `#111111`

A página precisa de internet para carregar o Chart.js e as fontes. Abrir o arquivo direto no navegador também funciona.

---

## Estrutura do repositório

```
├── index.html   # dashboard completo (layout, lógica e dados)
└── README.md    # este arquivo
```

---

Desenvolvido por **B16**.
