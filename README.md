# 🚀 Rick and Morty Character Explorer

Uma aplicação web moderna e interativa para explorar personagens do universo Rick and Morty, construída com Vue 3 e consumindo a [Rick and Morty API](https://rickandmortyapi.com/).

![Vue.js](https://img.shields.io/badge/Vue.js-3.5.22-4FC08D?style=flat&logo=vue.js)
![Vite](https://img.shields.io/badge/Vite-7.1.7-646CFF?style=flat&logo=vite)
![Axios](https://img.shields.io/badge/Axios-Latest-5A29E4?style=flat&logo=axios)

---

## ✨ Funcionalidades

### 🔍 Busca e Filtros

- **Busca por nome** com debounce para otimização de performance
- **Filtro por status** (Vivo, Morto, Desconhecido)
- **Filtro por espécie** (Humano, Alienígena, Humanoide, etc.)
- **Combinação de filtros** para busca refinada
- **Persistência de filtros** no localStorage

### 📄 Paginação

- Navegação entre páginas com botões anterior/próximo
- Seleção direta de páginas
- Informações de total de páginas e personagens
- Scroll automático ao trocar de página

### 🎨 Tema Claro/Escuro

- Alternância entre modo claro e escuro
- Persistência da preferência no localStorage
- Transições suaves entre temas
- CSS Variables para fácil customização

### 🖼️ Modal de Detalhes

- Visualização completa dos dados do personagem
- Informações detalhadas (origem, localização, episódios)
- Animações de entrada e saída
- Fechamento com ESC ou clique fora
- Design responsivo

### 🎯 UX/UI

- Loading spinner animado
- Empty state quando não encontra resultados
- Feedback visual em todos os estados
- Design responsivo (mobile-friendly)
- Animações e transições suaves

---

## 🛠️ Tecnologias Utilizadas

- **Vue 3** - Framework JavaScript progressivo
- **Composition API** - API moderna do Vue com `<script setup>`
- **Vite** - Build tool extremamente rápido
- **Axios** - Cliente HTTP para requisições
- **CSS Variables** - Para sistema de temas dinâmicos
- **localStorage** - Persistência de dados no navegador

---

## 📋 Pré-requisitos

- Node.js (versão 16 ou superior)
- npm ou yarn

---

## 🚀 Como Executar o Projeto

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/rick-and-morty.git
cd rick-and-morty
```

### 2. Instale as dependências

```bash
npm install
```

### 3. Execute o servidor de desenvolvimento

```bash
npm run dev
```

### 4. Acesse no navegador

```
http://localhost:5173
```

---

## 📦 Scripts Disponíveis

- `npm run dev` - Inicia o servidor de desenvolvimento
- `npm run build` - Cria build de produção
- `npm run preview` - Preview do build de produção

---

## 🏗️ Estrutura do Projeto

```
rick-and-morty/
├── public/                 # Arquivos públicos estáticos
├── src/
│   ├── assets/            # Imagens e recursos
│   ├── components/        # Componentes Vue
│   │   └── CharacterModal.vue  # Modal de detalhes
│   ├── App.vue           # Componente principal
│   ├── main.js           # Entry point da aplicação
│   └── style.css         # Estilos globais e variáveis de tema
├── index.html            # HTML base
├── package.json          # Dependências e scripts
├── vite.config.js        # Configuração do Vite
└── README.md            # Este arquivo
```

---

## 🎓 Conceitos Vue 3 Implementados

### Composition API

- `ref()` - Reatividade para valores primitivos
- `computed()` - Propriedades computadas
- `watch()` - Observadores de mudanças
- `onMounted()` - Hook de lifecycle
- `onUnmounted()` - Cleanup de recursos

### Recursos Avançados

- **Teleport** - Renderização de modal fora da hierarquia
- **Transitions** - Animações automáticas
- **Props** - Comunicação pai → filho
- **Emits** - Comunicação filho → pai
- **Event Handling** - Manipulação de eventos
- **Conditional Rendering** - v-if, v-else-if, v-else
- **List Rendering** - v-for com :key
- **Two-way Binding** - v-model

### Padrões e Boas Práticas

- Componentização e separação de responsabilidades
- CSS Scoped para evitar conflitos
- Debounce para otimização de performance
- Cleanup de event listeners (prevenção de memory leaks)
- Persistência de dados com localStorage
- Estados visuais claros (loading, empty, error)

---

## 🎨 Sistema de Temas

O projeto utiliza CSS Variables para implementar temas claro/escuro:

### Variáveis Disponíveis

```css
--bg-primary      # Fundo principal
--bg-secondary    # Fundo secundário
--bg-card         # Fundo dos cards
--text-primary    # Texto principal
--text-secondary  # Texto secundário
--border-color    # Cor das bordas
--shadow         # Sombras
```

### Customização

Para adicionar novos temas, modifique as variáveis em `src/style.css`:

```css
/* Tema personalizado */
body.custom-theme {
  --bg-primary: #yourcolor;
  --text-primary: #yourcolor;
  /* ... outras variáveis */
}
```

---

## 🌐 API Utilizada

**Rick and Morty API**

- URL Base: `https://rickandmortyapi.com/api`
- Documentação: [rickandmortyapi.com/documentation](https://rickandmortyapi.com/documentation)
- Endpoints utilizados:
  - `GET /character` - Lista personagens com filtros e paginação
  - Parâmetros: `page`, `name`, `status`, `species`

---

## 📱 Responsividade

O projeto é totalmente responsivo e se adapta a diferentes tamanhos de tela:

- **Desktop** - Layout em grid com múltiplas colunas
- **Tablet** - Grid adaptativo
- **Mobile** - Layout em coluna única com ajustes de UI

---

## ⚡ Otimizações de Performance

- **Debounce na busca** - Evita requisições desnecessárias
- **Lazy loading** - Carregamento sob demanda via paginação
- **CSS Transitions** - Animações performáticas via GPU
- **Scoped Styles** - CSS otimizado por componente
- **Computed properties** - Cache inteligente de cálculos

---

## 🔮 Possíveis Melhorias Futuras

- [ ] Sistema de favoritos com localStorage
- [ ] Vue Router para navegação entre páginas
- [ ] Pinia/Vuex para gerenciamento de estado global
- [ ] TypeScript para tipagem estática
- [ ] Testes unitários com Vitest
- [ ] Infinite scroll como alternativa à paginação
- [ ] PWA para funcionamento offline
- [ ] Compartilhamento de personagens
- [ ] Filtros avançados (múltiplas espécies, origem, episódios)
- [ ] Comparação de personagens lado a lado

---

## 📚 Aprendizados do Projeto

Este projeto foi desenvolvido como estudo e implementa conceitos fundamentais e avançados de Vue 3:

✅ Composition API com `<script setup>`  
✅ Reatividade e gerenciamento de estado  
✅ Consumo de APIs REST  
✅ Componentização e comunicação entre componentes  
✅ Persistência de dados  
✅ Otimização de performance  
✅ UX/UI e feedback visual  
✅ Responsividade  
✅ Boas práticas de desenvolvimento

---

## 👨‍💻 Autor

Desenvolvido como projeto de estudos de Vue 3.

---

## 📄 Licença

Este projeto está sob a licença MIT. Sinta-se livre para usar, modificar e distribuir.

---

## 🙏 Agradecimentos

- [Rick and Morty API](https://rickandmortyapi.com/) pela API gratuita
- [Vue.js Team](https://vuejs.org/) pelo framework incrível
- [Vite Team](https://vitejs.dev/) pela ferramenta de build

---

**Feito com ❤️ e Vue 3**
