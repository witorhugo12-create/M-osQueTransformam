# Mão que Transforma

Plataforma web para uma ONG divulgar projetos, captar recursos (doações) e atrair voluntários.
Single Page Application feita com **HTML5, CSS3 e JavaScript puro**, sem dependências.

## Como executar

Não há instalação nem build: basta abrir o `index.html` no navegador (duplo clique).
Opcionalmente, use um servidor local (ex.: extensão *Live Server* do VS Code, ou `python3 -m http.server`).

## Funcionalidades

- **Navegação SPA** por hash (`#/projetos`, `#/impacto`, `#/participe`), sem recarregar a página, com rota 404 e suporte ao botão voltar.
- **Templates dinâmicos**: os cards de projeto são gerados a partir de um array de dados.
- **Formulário de voluntários** com validação em tempo real e no envio (nome, e-mail e área de interesse).
- **Persistência com `localStorage`**: rascunho do formulário e histórico de inscrições.
- **Componentes de feedback**: badges, alertas, toasts e modal (`<dialog>`).
- **Tema claro/escuro** automático e layout responsivo.

## Estrutura do projeto

```
mao-que-transforma/
├── index.html            # Casca da SPA (cabeçalho, <main id="app">, rodapé, modal)
├── html/
│   └── componentes.html  # Demonstração dos componentes de feedback
├── css/
│   ├── style.css         # Tokens, base, layout, seções e validação
│   └── feedback.css      # Badges, alertas, toast e modal
├── imagens/
│   └── favicon.svg       # Logotipo/ícone
└── js/
    ├── utils.js          # escapar(): previne XSS ao usar innerHTML
    ├── dados.js          # Dados de origem (projetos)
    ├── templates.js      # Funções de template (Template Literals)
    ├── storage.js        # Acesso seguro ao localStorage (JSON)
    ├── validacao.js      # Regras e mensagens de validação
    ├── inscricoes.js     # Rascunho e histórico de inscrições
    ├── views.js          # HTML de cada tela
    ├── router.js         # Roteador SPA
    ├── feedback.js       # Toasts, modais e alertas
    └── main.js           # Ponto de entrada
```

Os scripts são carregados em ordem no `index.html`, dos independentes para os dependentes, e se comunicam pelo objeto global `App` (e `Feedback`).

## Como usar os componentes de feedback

```html
<span class="badge badge--success">Ativo</span>
<div class="alert alert--danger" role="alert">…</div>
<button data-modal-open="modal-doar">Doar</button>
```

```js
Feedback.toast('success', 'Título', 'Mensagem');   // success | info | warning | danger | neutral
Feedback.openModal('modal-doar');
```

## Como adicionar um projeto

Edite o array em `js/dados.js`; os cards são gerados automaticamente:

```js
{ titulo: 'Novo projeto', descricao: '…', status: 'Ativo', tipo: 'success', acao: 'Participar' }
```

## Fluxo de trabalho (GitFlow)

| Branch | Função |
|---|---|
| `main` | Versões estáveis, marcadas com tag (`v1.0.0`) |
| `develop` | Integração do desenvolvimento |
| `feature/*` | Uma branch por funcionalidade, criada a partir da `develop` |
| `release/*` | Preparação de versão |
| `hotfix/*` | Correções urgentes a partir da `main` |

**Commits semânticos:** `feat:` (funcionalidade), `fix:` (correção), `docs:` (documentação), `style:`, `refactor:`, `test:`, `chore:`.

## Testes realizados

Testes manuais e automatizados no Chromium: navegação sem recarregar, botão voltar, rota inexistente, validação (vazio, nome curto, e-mail inválido), foco no primeiro erro, persistência após recarregar, dado corrompido no `localStorage` e tentativa de XSS no formulário.

## Próximos passos

- Auditoria de acessibilidade (WCAG 2.1 AA) e correções.
- Otimização para produção e deploy.
- Testes em Firefox e Safari.
