# Nuvem Store - Catálogo de produtos com carrinho

Projeto acadêmico desenvolvido para o trabalho prático de Programação Web I. A aplicação utiliza **Vue.js 3**, **Vite**, JavaScript, HTML e CSS.

## Funcionalidades

- Exibição de seis produtos em cards reutilizáveis.
- Componente `ProductCard.vue` recebe nome, preço, imagem, descrição, categoria e selo por `props`.
- Ao clicar em **Adicionar**, o card emite o evento `add-to-cart`; o componente pai atualiza o carrinho.
- Contador do carrinho atualizado reativamente, sem recarregar a página.
- Busca de produtos por nome em tempo real usando `v-model` e `computed`.
- Resumo do carrinho com quantidades, subtotal e controles para aumentar ou diminuir itens.
- Mensagens de confirmação, estado vazio e aviso quando a busca não encontra resultados.
- Layout responsivo para computadores, tablets e celulares.
- Ilustrações SVG locais, sem depender de serviços externos de imagens.

## Requisitos

- Node.js 18 ou superior.
- npm (instalado junto com o Node.js).

## Como executar

1. Extraia o arquivo ZIP e abra a pasta `vue_trabalho` no VS Code.
2. Abra o terminal nessa pasta.
3. Instale as dependências:

   ```bash
   npm install
   ```

4. Inicie o servidor de desenvolvimento:

   ```bash
   npm run dev
   ```

5. Abra no navegador o endereço exibido pelo Vite (normalmente `http://localhost:5173`).

Para verificar a compilação de produção:

```bash
npm run build
```

O resultado será criado na pasta `dist/`.

## Organização dos arquivos

```text
vue_trabalho/
├── public/
│   └── images/             # Ilustrações SVG dos produtos
├── src/
│   ├── components/
│   │   ├── CartSummary.vue # Resumo e controles do carrinho
│   │   └── ProductCard.vue # Card reutilizável de produto
│   ├── App.vue             # Estado, busca, catálogo e integração
│   ├── main.js             # Inicialização da aplicação Vue
│   └── style.css           # Estilos e responsividade
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

## Onde estão os conceitos pedidos no trabalho

- **Props:** `src/components/ProductCard.vue` declara as propriedades recebidas pelo card. O componente não altera esses dados; apenas os apresenta.
- **Evento do componente:** o botão do card dispara `add-to-cart` com `defineEmits`. `App.vue` escuta o evento e chama `addToCart(product)`.
- **Reatividade:** `cart` e `searchQuery` são estados criados com `ref`. As mudanças nesses estados refletem automaticamente na interface.
- **Data binding:** `v-model="searchQuery"` conecta o campo de busca à variável reativa; `:name`, `:price` e `:image` passam valores para os cards.
- **Computed:** `filteredProducts`, `cartItemCount` e `cartSubtotal` calculam resultados derivados do estado. A busca não altera a lista original de produtos.
- **Componentização:** o catálogo é separado em `ProductCard.vue` e `CartSummary.vue`, deixando cada parte com uma responsabilidade clara.

## Teste rápido

1. Ao abrir a aplicação, o contador deve mostrar `0` e o carrinho deve exibir a mensagem de vazio.
2. Adicione o fone sem fio. O contador deve passar para `1`.
3. Adicione o mesmo fone novamente. O contador deve passar para `2` e a quantidade do item deve ser atualizada.
4. Pesquise por `mouse`. Apenas o Mouse sem fio Click deve permanecer na vitrine.
5. Use os botões `+` e `−` do carrinho. O contador e o subtotal devem acompanhar as alterações.
6. Remova todas as unidades de um produto. O item deve desaparecer do carrinho.
7. Teste a página em uma janela estreita para verificar o layout móvel.

## Observações

Este é um catálogo demonstrativo para fins acadêmicos. Não possui banco de dados, autenticação, estoque real, integração de pagamento ou envio de pedidos a uma loja. O botão **Finalizar pedido** apenas apresenta uma confirmação local e limpa o carrinho.

## Referências técnicas

- Vue.js. **Introduction**. https://vuejs.org/guide/introduction.html
- Vue.js. **Reactivity Fundamentals**. https://vuejs.org/guide/essentials/reactivity-fundamentals.html
- Vue.js. **Component Events**. https://vuejs.org/guide/components/events.html
- Vue.js. **Props**. https://vuejs.org/guide/components/props.html
- Vue.js. **Form Input Bindings**. https://vuejs.org/guide/essentials/forms.html
- Vue.js. **List Rendering**. https://vuejs.org/guide/essentials/list.html

## Publicação no GitHub

Depois de testar o projeto localmente, crie um repositório público no GitHub, envie todos os arquivos desta pasta (exceto `node_modules/` e `dist/`) e copie o endereço do repositório para informar na entrega da atividade.
