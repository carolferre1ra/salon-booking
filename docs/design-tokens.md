# 🎨 Tokens de Design

**Projeto:** SalonHub
**Versão:** 1.0.0 · preenchido a partir do protótipo
**Última atualização:** 2026-09-13

> 🤖 **Este documento existe para a IA parar de inventar um botão diferente a cada
> tela.** Não é um design system — é o mínimo que dá à prototipagem assistida algo a
> que obedecer.

---

## Paleta

Nome semântico, nunca `azul-2` — a cor muda, o papel dela não.

| Token | Valor | Onde se usa |
| --- | --- | --- |
| `primaria` | `#9B4564` | ações principais, cabeçalho e botões |
| `superficie` | `#E4D2C5` | fundo principal |
| `painel` | `#F8EDE5` | painéis e formulários |
| `texto` | `#4A3A35` | texto principal |
| `texto-suave` | `#9A8880` | textos secundários |
| `perigo` | `#C96B6B` | erros, conflitos e cancelamentos |
| `sucesso` | `#7C8A68` | confirmações |
| `desabilitado` | `#D9D4D1` | controles desabilitados |
| `texto-claro` | `#FFF9F6` | texto sobre fundos escuros |
| `agendamento` | `#CFA5B3` | cartões de agendamento |
| `cabecalho-grade` | `#F2E5DC` | cabeçalho da grade |
| `linhas-grade` | `#D6C4B8` | linhas da agenda |

## Escala de espaçamento

Uma progressão só, usada em tudo.

| Token | Valor |
| --- | --- |
| `xs` | 4px |
| `sm` | 8px |
| `md` | 16px |
| `lg` | 24px |
| `xl` | 32px |

## Tipografia

Família: **Poppins**.

| Token | Família · tamanho · peso | Papel |
| --- | --- | --- |
| `titulo` | Poppins · 40px · 600 | título de tela |
| `subtitulo` | Poppins · 24px · 600 | subtítulos e cabeçalhos de seção |
| `texto` | Poppins · 16px · 400 | texto de corpo |
| `legenda` | Poppins · 14px · 400 | legendas e textos de apoio |

## Estados de botão

| Estado | Aparência |
| --- | --- |
| normal | fundo `primaria` (`#9B4564`), texto `texto-claro` (`#FFF9F6`), cantos levemente arredondados |
| hover | mesmo fundo com leve escurecimento do `primaria` e cursor pointer; realça a área clicável |
| foco (teclado) | adiciona um anel de foco visível ao redor do botão, para navegação por teclado |
| desabilitado | fundo `desabilitado` (`#D9D4D1`) com texto apagado; não responde a clique nem a hover |
| carregando | ícone de carregamento no lugar do texto (ou ao lado), com o botão levemente esmaecido; não responde a novos cliques |

## Protótipo

**Link:** a definir — o link do Figma será adicionado aqui quando o protótipo estiver disponível.

**Telas (4):**

1. **Agenda semanal** — grade com as profissionais em colunas, cartões de agendamento (`agendamento`), cabeçalho (`cabecalho-grade`), linhas (`linhas-grade`) e o próximo atendimento destacado.
2. **Novo agendamento** — formulário (`painel`) com cliente, procedimento, profissional, horários e valor, e a mensagem de conflito (`perigo`).
3. **Editar agendamento** — o mesmo formulário em modo de edição, com as validações de conflito reaplicadas.
4. **Confirmação/cancelamento** — confirmação de ação com feedback de `sucesso` para confirmações e `perigo` para cancelamentos.