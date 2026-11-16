# Gestão de Estoque - Frontend

> Projeto acadêmico. Este repositório contém **apenas o frontend**. A API está em [gestao-estoque-backend](https://github.com/lucasr4m0n/gestao-estoque-backend).

**Aplicação em produção:** TODO: cole aqui o link da Vercel
**API em produção:** https://gestao-estoque-api-hmee.onrender.com/swagger-ui.html

> A API roda no plano gratuito do Render e "dorme" quando fica sem uso: o primeiro login pode levar cerca de 1 minuto.

Interface web de um sistema interno de gestão de estoque e compras: cadastro de produtos, categorias e fornecedores, controle de movimentações de estoque, pedidos de compra e dashboard com indicadores.

## Funcionalidades

- **Login** com autenticação JWT e redirecionamento automático quando a sessão expira
- **Dashboard:** total estocado, valor investido, produtos críticos, pedidos pendentes, estoque por categoria, movimentação por dia
- **Produtos:** busca, filtro por categoria, paginação, cadastro, edição, ativação e inativação
- **Categorias e Fornecedores:** cadastro e manutenção
- **Movimentações:** registro de entradas e saídas de estoque
- **Pedidos de compra:** criação com itens, solicitação, aprovação, entrega (que dá entrada no estoque) e cancelamento

## Tecnologias

- React 18 e React Router 6
- Vite 5
- Tailwind CSS 3
- Axios (cliente HTTP com interceptors de autenticação)
- Recharts (gráficos)
- Jest e Testing Library (testes)
- ESLint

## Como executar localmente

Pré-requisitos: Node.js 18 ou superior e o [backend](https://github.com/lucasr4m0n/gestao-estoque-backend) em execução.

```bash
npm install
npm run dev
```

A aplicação abre em `http://localhost:5173`.

### Variável de ambiente

| Variável | Padrão | Descrição |
|---|---|---|
| `VITE_API_URL` | `http://localhost:8090/api` | URL base da API, **terminando em `/api` e sem barra no final** |

Crie um arquivo `.env` (que não é versionado) a partir do `.env.example` para apontar para outra API:

```
VITE_API_URL=https://gestao-estoque-api-hmee.onrender.com/api
```

> As variáveis `VITE_*` são embutidas no momento do build. Ao alterá-las em produção, é preciso fazer um novo deploy.

### Scripts

| Comando | O que faz |
|---|---|
| `npm run dev` | Servidor de desenvolvimento |
| `npm run build` | Build de produção (pasta `dist`) |
| `npm run preview` | Serve o build localmente |
| `npm run lint` | Análise estática com ESLint |
| `npm test` | Executa os testes |
| `npm run test:coverage` | Testes com relatório de cobertura |

## Deploy (Vercel)

1. Importe o repositório na Vercel (preset **Vite**, Root Directory vazio).
2. Defina `VITE_API_URL` com a URL da API terminando em `/api`.
3. O arquivo `vercel.json` redireciona todas as rotas para o `index.html`, necessário para o React Router funcionar ao recarregar a página.
4. No backend, libere o domínio da Vercel na variável `CORS_ORIGINS` (sem barra no final).

## Estrutura do projeto

```
src/
├── components/   # Table, Modal, Input, Select, Button, Badge, Pagination...
├── hooks/        # useFetch, useToast...
├── pages/        # telas (Dashboard, Produtos, Compras...)
├── services/     # chamadas à API (api.js, produtoService.js...)
├── context/      # AuthContext (sessão do usuário)
└── utils/        # formatadores e validadores
```

## Problemas comuns

| Sintoma | Causa provável |
|---|---|
| Login não chama a API ou chama `localhost` em produção | `VITE_API_URL` não definida, ou não houve novo deploy depois de defini-la |
| Erro de CORS no console | `CORS_ORIGINS` do backend diferente do domínio exato do site |
| Login demora ou dá erro de rede na primeira vez | API do Render acordando (cerca de 1 minuto) |
| 404 ao recarregar uma rota | `vercel.json` ausente |

## Melhorias planejadas

- Gráfico de movimentação por dia: preencher dias sem movimento e usar o fuso local
- Pedido de compra: listar apenas produtos ativos no formulário
- Divisão do bundle (code splitting) para reduzir o tamanho do build
