# Nuxt 3 Minimal Starter

Look at the [Nuxt 3 documentation](https://nuxt.com/docs/getting-started/introduction) to learn more.

## Requisitos

- Node >= 20
- pnpm >= 9.1.2
- npm mais recente
- yarn mais recente

## Setup

Instale as dependências:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install

# bun
bun install
```

Visualização do projeto:

```bash
# npm
npm run preview

# pnpm
pnpm run preview

# yarn
yarn preview

# bun
bun run preview
```

## Considerações

A escolha do nuxt é devido ao movimento do mercado para aplicações SSR (Server Side Rendering). Para projetos onde SEO não é necessário, ou tão requisitado, optaria por opções CSR (Client Side Rendering). Minha motivação de escolher preferencialmente o CSR é devido à complexidade de lidar com componentes, hora renderizados no servidor, hora no cliente, não valendo o tempo gasto a mais para este tipo de aplicação. Porém, no caso deste teste, temos o benefício do SSR no SEO e entrega de conteúdo mais rápido ao usuário final.

Optei também por não utilizar bibliotecas de estilização para demonstrar melhor as minhas capacidades de criar layouts responsivos com qualidade e organização. Esta escolha gerou trade-offs que não previ inicialmente, como criação do componente de select para ficar o mais fiel possível à interface proposta, fazendo com que criasse ele do zero e não sendo o caso ideal para sistemas que irão para produção.

Não utilizei estado global por não acreditar ser necessário, mas caso fosse, usaria Pinia, o qual é recomendado pelo próprio Vue.

Agradeço a oportunidade de participar do teste e fico à disposição para quaisquer dúvidas.
