# WLC Boilerplate React

Boilerplate de interface web em React para aplicações Single Page Application. O projeto já reúne as ferramentas base para desenvolvimento, tipos, componentes acessíveis, formulários, comunicação HTTP, testes e publicação, permitindo iniciar uma nova tela sem montar toda a infraestrutura novamente.

## Visão geral

| Tecnologia | Versão declarada | Uso no boilerplate |
| --- | ---: | --- |
| React | 19.3.1 | Construção da interface baseada em componentes |
| React DOM | 19.3.1 | Inicialização da aplicação no navegador |
| TypeScript | 6.0.3 | Tipagem estática e segurança durante o desenvolvimento |
| Vite | 8.3.6 | Servidor de desenvolvimento e empacotamento |
| Tailwind CSS | 4.3.5 | Sistema de estilos utilitário e tema visual |
| React Router DOM | 7.18.6 | Navegação por rotas, ainda não configurada |
| React Hook Form | 7.92.0 | Gestão de formulários e estado dos campos |
| Zod | 4.6.7 | Validação e definição de esquemas |
| Axios | 1.21.1 | Cliente HTTP, ainda não utilizado |
| React Aria Components | 1.22.1 | Primitivos de interface acessíveis |
| Lucide React | 1.78.0 | Biblioteca de ícones |
| Vitest | 5.1.1 | Executor de testes |
| Testing Library | 16.3.3 | Testes de componentes a partir do comportamento |
| jsdom | 31.0.1 | Ambiente DOM usado nos testes |
| Biome | 2.6.8 | Formatação, lint e organização de imports |

As versões exatas podem ser consultadas em [package.json](./package.json).

## Tecnologias escolhidas

### React 19 e React DOM

O React é responsável pela interface baseada em componentes. `src/main.tsx` cria o ponto de entrada da aplicação, localiza o elemento `#root` e monta o componente `App` em `StrictMode`.

O modo estrito ajuda a detectar efeitos colaterais e padrões inseguros durante o desenvolvimento. Ele não cria componentes protegidos em produção nem substitui testes.

### TypeScript

O projeto usa TypeScript em todo o código da pasta `src`. O compilador está configurado para:

- gerar JavaScript moderno para ES2023;
- resolver módulos no modo usado por ferramentas de bundle;
- manter a sintaxe das imports conforme a declaração original;
- não emitir arquivos JavaScript durante a compilação;
- usar JSX no modo automático do React;
- informar variáveis e parâmetros não utilizados;
- rejeitar `fallthrough` em instruções `switch`.

A configuração principal referencia configurações separadas para a aplicação e para o Vite:

- [tsconfig.app.json](./tsconfig.app.json): configura os arquivos em `src`;
- [tsconfig.node.json](./tsconfig.node.json): configura `vite.config.ts`;
- [tsconfig.json](./tsconfig.json): reúne as duas configurações em uma referência de projeto.

O diretório `src` pode usar APIs do navegador. As configurações do Vite podem usar APIs do Node.

### Vite

O Vite combina o servidor de desenvolvimento rápido com o processo de build. Ele transforma o JSX e o TypeScript e gera os arquivos estáticos da pasta `dist`.

A configuração em [vite.config.ts](./vite.config.ts) também integra:

- o plugin oficial do React;
- o plugin do Tailwind CSS;
- o servidor em `0.0.0.0` na porta `8000`;
- Vitest com ambiente `jsdom`;
- testes em `src/**/*.{test,spec}.{ts,tsx}`;
- o arquivo de setup em `src/test/setup.ts`.

O servidor disponível em desenvolvimento fica em `http://localhost:8000` por padrão.

### Tailwind CSS 4

O Tailwind é instalado como plugin do Vite e importado no CSS por:

```css
@import "tailwindcss";
```

O tema é definido em [src/index.css](./src/index.css) por meio de `@theme`. Ele disponibiliza nomes personalizados de:

- fonte: `--font-serif` e `--font-sans`;
- superfície e fundo: `canvas`, `surface` e `subtle`;
- marca: `brand-primary`, `brand-primary-hover` e `brand-secondary`;
- texto: `ink-primary` e `ink-secondary`.

Essas propriedades podem ser utilizadas pelas classes geradas pelo Tailwind. Por exemplo, `text-ink-primary`, `bg-canvas` e `font-sans` seguem os nomes configurados no tema.

As fontes carregadas na página são Lora e Plus Jakarta Sans. O projeto usa um tamanho base reduzido de `62.5%` no elemento `html`, permitindo interpretar `1.6rem` como `16px` no corpo.

### React Router DOM

React Router já está disponível no projeto para criação de rotas entre páginas. Ele ainda não está configurado no `App.tsx`, onde a aplicação atualmente apenas renderiza a seção `Hello App`.

Ao evoluir o projeto, a pasta de páginas pode centralizar componentes como telas, enquanto o roteamento pode ficar em um componente ou arquivo separado. Evite misturar regras de navegação complexas diretamente em componentes visuais de baixo nível.

### React Hook Form

React Hook Form foi escolhido para controlar formulários sem manter manualmente todo o estado dos campos. A biblioteca reduz renders desnecessários e fornece recursos como validação, estado de envio, campos controlados e não controlados.

Ele pode ser combinado com Zod para manter uma única fonte de verdade do formato e das regras dos dados.

### Zod e resolvers

Zod define esquemas de validação tipados. A dependência `@hookform/resolvers` conecta esses esquemas ao React Hook Form.

Um fluxo típico seria:

1. definir o formato dos dados com Zod;
2. inferir o tipo TypeScript a partir do esquema;
3. passar o resolver ao `useForm`;
4. enviar os dados validados para a API.

Essa combinação evita duplicar manualmente interfaces TypeScript e regras de validação.

### Axios

Axios está instalado como cliente HTTP. Ele ainda não é usado no boilerplate. Pode ser adotado para serviços de API que precisam de:

- `baseURL`;
- interceptadores;
- cabeçalhos comuns;
- tratamento centralizado de respostas e erros;
- transformação de dados;
- cancelamento de requisições.

Caso o projeto faça poucas chamadas e não precise dessas capacidades, a API `fetch` do navegador também seria suficiente.

### Firebase

Firebase está instalado, mas nenhuma integração foi criada ainda. Dependendo do produto, ele pode ser usado para autenticação, banco de dados, armazenamento de arquivos ou funções backend.

A configuração de Firebase não deve ser adicionada diretamente ao componente principal. Recomenda-se criar uma inicialização separada e manter credenciais, modos de ambiente e regras de acesso fora do código controlado indevidamente.

### React Aria Components

React Aria Components fornece componentes com comportamento de interação e acessibilidade já preparado. É uma alternativa útil para elementos complexos que precisam considerar teclado, foco, estados e leitores de tela.

Ele complementa o Tailwind: React Aria cuida da semântica e da interação; Tailwind cuida da aparência.

### Lucide React

Lucide fornece ícones como componentes React. Isso permite reaproveitar ícones com propriedades como tamanho e cor, sem inserir SVG ou arquivos de ícone manualmente em cada componente.

## Interface e estilos

A interface atual é deliberadamente pequena. [src/App.tsx](./src/App.tsx) contém apenas um componente React chamado `App`, enquanto [src/index.css](./src/index.css) define o tema global e o reset básico.

[src/main.tsx](./src/main.tsx) é o ponto de entrada da aplicação. As demais pastas estão organizadas assim:

```text
src/
├── App.tsx          Componente principal da aplicação
├── assets/          Imagens e outros recursos estáticos importados pelo código
├── index.css        Imports do Tailwind e tema global
├── main.tsx         Inicialização do React no navegador
└── test/
    └── setup.ts     Configuração compartilhada dos testes
```

Os assets públicos ficam em `public/`. O arquivo [index.html](./index.html) carrega as fontes da Google Fonts, define o idioma da página e contém o elemento `root`.

## Testes

O projeto usa Vitest em ambiente de navegador simulada por jsdom. [src/test/setup.ts](./src/test/setup.ts) importa os matchers estendidos do jest-dom e executa a limpeza do DOM após cada teste.

Testing Library incentiva testes que encontram elementos da forma como uma pessoa usaria a interface: por texto, função ou papel acessível, em vez de depender excessivamente de detalhes internos.

Os testes podem ser colocados em `src` com nomes como:

```text
Componente.test.tsx
Componente.spec.tsx
```

As configurações de descoberta estão centralizadas em `vite.config.ts`.

## Qualidade de código

Biome substitui separadamente ferramentas como ESLint e Prettier neste boilerplate. A configuração em [biome.json](./biome.json):

- habilita o linter recomendado;
- formata com espaçamento de dois caracteres;
- limita linhas a 100 caracteres;
- organiza imports e atributos;
- usa o recomendador de Tailwind;
- reconhece diretivas próprias do Tailwind;
- usa o arquivo `.gitignore` quando disponível.

O pacote está integrado a scripts do npm ou Yarn.

## Scripts disponíveis

Instalação das dependências:

```bash
yarn
```

Servidor de desenvolvimento:

```bash
yarn dev
```

Geração da versão de produção:

```bash
yarn build
```

O build executa primeiro `tsc -b` e depois `vite build`.

Pré-visualização local da saída:

```bash
yarn preview
```

Execução dos testes em modo observador:

```bash
yarn test
```

Execução única dos testes:

```bash
yarn test:run
```

Verificação do código:

```bash
yarn lint
```

Formatação dos arquivos:

```bash
yarn format
```

O projeto possui `yarn.lock`, portanto Yarn é o gerenciador de pacotes recomendado.

## Build e deploy

A configuração [netilify.toml](./netilify.toml) prepara o projeto para a Netlify:

- usa Node.js 22;
- executa `yarn build`;
- publica a pasta `dist`;
- redireciona todas as rotas para `index.html`;
- configura cabeçalhos defensivos básicos.

O redirect é necessário quando React Router gerencia rotas no navegador, pois o servidor precisa entregar o mesmo `index.html` para URLs como `/pacientes/123` em vez de procurar um arquivo físico desse caminho.

Os cabeçalhos configurados são:

- `X-Content-Type-Options: nosniff`;
- `X-Frame-Options: DENY`;
- `Referrer-Policy: strict-origin-when-cross-origin`.

## Estrutura de diretórios

```text
.
├── public/             Arquivos estáticos públicos
├── src/
│   ├── assets/         Recursos usados pela interface
│   ├── test/           Setup e suporte aos testes
│   ├── App.tsx         Componente inicial
│   ├── index.css       Tema e estilos globais
│   └── main.tsx        Ponto de entrada
├── biome.json          Configuração de lint e formatação
├── index.html          Documento HTML entregue pelo Vite
├── netilify.toml       Configuração de deploy na Netlify
├── package.json        Dependências e scripts
├── tsconfig.*.json     Configurações TypeScript
├── vite.config.ts      Configurações do Vite e Vitest
└── yarn.lock           Versões exatas das dependências
```

## Próximos passos recomendados

O boilerplate está pronto para receber funcionalidades. Uma organização comum seria:

```text
src/
├── components/         Componentes reutilizáveis
├── features/           Funcionalidades orientadas ao domínio
├── lib/                Integrações e inicializações
├── pages/              Páginas da aplicação
├── routes/             Configuração de navegação
├── services/           Comunicação com APIs
├── test/               Utilidades e setup dos testes
├── App.tsx
├── index.css
└── main.tsx
```

Antes de adicionar integração real, recomenda-se configurar também variáveis de ambiente, um padrão de tratamento de erros, regras de autenticação e testes dos fluxos principais.