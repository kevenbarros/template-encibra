# Documentação Técnica do Template Frontend

## 1. Objetivo
Este repositório é o **template oficial** para novos projetos frontend da equipe.

Objetivos:
- Padronizar arquitetura, bibliotecas e convenções.
- Reduzir tempo de setup de novos projetos.
- Garantir consistência entre código, UI e lógica de negócio.

---

## 2. Stack Padrão (Obrigatório)

### Base
- React
- Vite
- TypeScript

### Roteamento
- `react-router`

### Requisições e cache de servidor
- `axios`
- `@tanstack/react-query`

### Estado 
- Context API (nativa do React)

### Validação e formulário
- `zod`
- `react-hook-form`

### UI e componentes
- `shadcn/ui`
- `lucide-react`
- `sonner`

### Tabelas
- `@tanstack/react-table`

---

## 3. Convenção de Idioma



Exemplos:
- Técnico: `QueryClientProvider`, `apiClient`, `AppRouter`, `usePagination`.
- Domínio: `user`, `modulo`, `sub-modulo1`, `sub-modulo2`, `auth`.

---

## 4. Arquitetura

### Padrão arquitetural
- Atomic Design para componentes de UI (`atoms`, `molecules`, `organisms`) com composição de tela em `pages`.


## 5. Estrutura de Pastas (Atual)

```text
src/
├── App.css
├── App.tsx
├── assets/
│   └── react.svg
├── components/
│   ├── atoms/
│   ├── molecules/
│   ├── organisms/
│   └── ui/
├── contexts/
│   ├── modulo/
│   │   ├── modulo-context.ts
│   │   └── modulo-provider.tsx
│   └── user/
│       ├── user-context.ts
│       └── user-provider.tsx
├── hooks/
├── index.css
├── lib/
├── main.tsx
├── pages/
│   ├── auth/
│   └── user/
└── service/
	├── modulo/
	│   ├── sub-modulo1/
	│   │   ├── mutations/
	│   │   └── queries/
	│   └── sub-modulo2/
	│       ├── mutations/
	│       └── queries/
	└── user/
		├── mutations/
		│   ├── delete-user.ts
		│   ├── post-user.ts
		│   └── update-user.ts
		└── queries/
			├── get-user-by-id.ts
			└── get-user-list.ts
```

Regras do modelo atual:
- `components/ui`: base de componentes reutilizáveis (sem regra de negócio).
- `atoms`, `molecules`, `organisms`: composição de interface seguindo Atomic Design.
- `service/*`: camada de acesso a dados separada por domínio/subdomínio e por operação (`queries` e `mutations`).
- `pages/*`: organização de telas por área funcional (`auth`, `user`).

---

## 6. Exemplo de Responsabilidade por Camada

### `service/*`
- Organização da camada de serviços por domínio técnico (`modulo`, `user`).
- `mutations` para operações de escrita.
- `queries` para operações de leitura.

### `service/user/mutations` e `service/user/queries`
- Funções de comunicação HTTP segmentadas por tipo de operação.
- Não contém estado de tela.

### `hooks/<dominio>`
- Hooks com React Query.
- Chaves de cache padronizadas por domínio.

### `contexts/<dominio>`
- Estado da feature.
- Regras de negócio e orquestração.
- Composição entre formulário, queries e ações de UI.

### `pages/<dominio>`
- Composição de tela (UI + contexto).

### Exemplo mínimo funcional (mapa de arquivos)

```text
src/
├── service/
│   └── user/
│       ├── mutations/post-user.ts
│       └── queries/get-user-list.ts
├── components/
│   ├── atoms/app-input.tsx
│   └── organisms/user-table.tsx
├── contexts/
│   └── user/user-provider.tsx
├── hooks/
│   └── use-user-query.ts
├── lib/
│   ├── router/app-router.tsx
│   └── table/table.tsx
└── pages/
	└── user/list-user-page.tsx
```

---

## 6. Exemplo de Fluxo (Criar Usuário)

1. Usuário preenche formulário em `pages/user/create-user-page.tsx`.
2. `react-hook-form` controla estado do formulário.
3. `zod` valida payload com o schema da feature.
4. Mutation (`post-user.ts`) envia via camada `service/user`.
5. Em sucesso/erro, notificação com `sonner`.
6. Lista é invalidada/refetch com React Query.

---

## 7. Padrões de Implementação

### React Router
- Centralizar definição de rotas em `lib/router/app-router.tsx`.
- Rotas por domínio mapeadas para páginas em `pages/*`.

### Context API
- Um provider por domínio quando houver regra de negócio própria.
- Evitar contexto global único com responsabilidades misturadas.

### React Query
- Padronizar query keys por domínio:
	- `['user', 'list']`
	- `['user', 'detail', userId]`

### shadcn/ui
- Componentes base ficam em `components/ui`.
- Evitar regra de negócio dentro de `components/ui`; compor casos de uso em `molecules/organisms/pages`.

### TanStack Table
- Reutilizar tabela base em `lib/table/table.tsx`.

### Ícones e feedback
- `lucide-react` para ícones.
- `sonner` para toast/feedback de operação.

---

## 8. Dependências esperadas

```bash
pnpm add react-router axios @tanstack/react-query zod react-hook-form @tanstack/react-table lucide-react sonner
```

> Observação: `shadcn/ui` é adicionado via CLI/configuração do próprio pacote e gera componentes locais no projeto.

---

## 9. Governança

- Toda nova feature deve seguir esta estrutura.
- Exceções arquiteturais devem ser documentadas neste arquivo.
- Este documento é a referência central para novos templates e evoluções.
