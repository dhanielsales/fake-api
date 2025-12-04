# API Fake

Oii :) Esta é uma API REST simulada, projetada para te ajudar nos estudos de **React** e consumo de dados. Ela utiliza o `json-server` para fornecer um backend fake.

Voce pode encontrar o repositório do `json-server` aqui: [https://www.npmjs.com/package/json-server](https://www.npmjs.com/package/json-server)

## Como Iniciar

### 1. Instalação

Execute uma primeira vez esse comando para instalar as dependências:

```bash
npm install
```

### 2. Rodando o Servidor

```bash
npm run dev
```

---

## 📂 Estrutura de Dados

O banco de dados fake (db.json) possui 3 entidades principais:

1.  **`products`** (Produtos): Lista principal.
2.  **`categories`** (Categorias): Tipos de produtos.
3.  **`brands`** (Marcas): Fabricantes.

### Relacionamentos

- **Produto** tem `categoryId` (Se trata do ID que pertence a uma Categoria).
- **Produto** tem `brandId` (Se trata do ID que pertence a uma Marca).

---

## 🔗 Endpoints e Recursos

A URL base é: `http://localhost:3001`

### 1. Filtros Básicos e Operadores

Condições:

- `lt` → < (Menor que)
- `lte` → <= (Menor ou igual a)
- `gt` → > (Maior que)
- `gte` → >= (Maior ou igual a)
- `ne` → != (Diferente de)

#

- **Filtro Exato:**
  `GET /products?categoryId=1`
- **Maior que (`_gt`):** Preço maior que 500
  `GET /products?price_gt=500`
- **Menor que (`_lt`):** Preço menor que 100
  `GET /products?price_lt=100`
- **Diferente de (`_ne`):** Todos exceto categoria 1
  `GET /products?categoryId_ne=1`
- **Múltiplos Filtros:** Categoria 2 e preço menor que 300
  `GET /products?categoryId=2&price_lt=300`

Campos aninhados:

- x.y.z...

- **Filtro em Campo Aninhado:** Produtos onde o nome da categoria é "Notebooks"
  `GET /products?categories.name=Notebooks`

### 2. Ordenação (`_sort`)

- **Preço Crescente (Menor para o Maior):**
  `GET /products?_sort=price`
- **Preço Decrescente (Maior para o Menor):**
  `GET /products?_sort=-price`
- **Múltiplos Campos:** Ordenar por categoria e depois por preço decrescente
  `GET /products?_sort=categoryId,-price`

### 3. Paginação (`_page` e `_per_page`)

- **Página 1 com 10 itens:**
  `GET /products?_page=1&_per_page=10`

### 4. Incluir Dados Relacionados (`_embed`)

Usamos `_embed` tanto para trazer filhos quanto para trazer pais.

- **Trazer o produto com a Categoria (Pai):**
  `GET /products?_embed=category`
  _(Isso adicionará um objeto `category` dentro de cada produto)_

- **Trazer o produto com a Marca (Pai):**
  `GET /products?_embed=brand`

- **Trazer Categoria com seus Produtos (Filhos):**
  `GET /categories/1?_embed=products`

<br>

# Exercicio de React

Seu objetivo é criar uma aplicação React que consuma a a API Fake. O foco é manipular dados, filtros e navegação. Fique à vontade para usar qualquer versão do React ou bibliotecas auxiliares (Axios, etc).

## Parte 1: A Tela de Listagem (`/`)

Esta será a página inicial. Ela deve ser capaz de buscar dados do servidor e filtrar dinamicamente.

### 📋 Requisitos da Interface

1.  **Tabela de Produtos:**

    - Deve exibir colunas: ID, Titulo, Preço, ID da Categoria, ID da Marca e um botão "Ver Detalhes".

2.  **Barra de Filtros (Acima da tabela):**
    - **Categoria:** Um componente de `Select` ou `Autocomplete` carregado com as categorias da API (`/categories`).
    - **Marca:** Um componente de `Select` ou `Autocomplete` carregado com as marcas da API (`/brands`).
    - **Preço:** Dois inputs numéricos: "Preço Mínimo" e "Preço Máximo". (_Dica: use os operadores `gte` e `lte` da API_).

### Lógica

Sempre que o usuário alterar um filtro ou digitar na busca, a tabela deve atualizar automaticamente (ou ao clicar em um botão "Filtrar").

**Como montar a URL da API:**
Você precisará combinar vários parâmetros.

- **Categoria:** `categoryId=1`
- **Marca:** `brandId=2`
- **Preço Mínimo:** `price_gte=100` (Greater Than or Equal)
- **Preço Máximo:** `price_lte=500` (Less Than or Equal)

**Exemplo de URL final:**
`http://localhost:3001/products?categoryId=1&price_gte=1000&price_lte=5000`

---

## Parte 2: A Tela de Detalhes (`/produto/:id`)

Ao clicar no botão "Ver Detalhes" na tabela, o usuário deve ser levado para esta tela.

### 📋 Requisitos da Interface

1.  **Botão Voltar:** Um link ou botão para voltar para a listagem (`/`).
2.  **Layout:**
    - Exibir todos os dados do produto: ID, Titulo, Descrição, Preço, ID da Categoria, ID da Marca, Em Estoque e Avaliação.
    - Exibir uma seção com os dados da **Marca** e **Categoria** com seus respectivos nomes e descrições e todos os outros campos.

### Lógica

1.  Faça um fetch para buscar **apenas** aquele produto. (`/products/:id`)
2.  Use o recurso de `_embed` para trazer os dados completos da marca e categoria em uma única chamada.

**URL da API para detalhes:**
`http://localhost:3001/products/1?_embed=category&_embed=brand`

## Bônus (Opcional)

Se quiser se desafiar mais, implemente essas funcionalidades extras:

1.  **Debounce na Busca:** Pesquise sobre `debounce` e faça com que a busca por texto só dispare a requisição 500ms depois que o usuário parar de digitar (para não chamar a API a cada letra).
2.  **Feedback de "Nenhum produto encontrado":** Se os filtros não retornarem nada, mostre uma mensagem amigável.

## Dicas

### Formatando Dinheiro

Use o `Intl` nativo do navegador para formatar valores monetários em Real (BRL):

```javascript
new Intl.NumberFormat("pt-BR", { style: "currency", currency: "BRL" }).format(
  valor
);
```

substitua `valor` pela variável que contém o número que você quer formatar.
