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

---
