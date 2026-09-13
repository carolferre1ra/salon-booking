# 🗺️ Jornadas de Usuário

**Projeto:** SalonHub
**Versão:** 1.0.0 · elaborado a partir do `docs/prd.md`
**Última atualização:** 2026-09-13

> 🤖 **Este documento é a fonte da verdade sobre O QUE A PESSOA VIVE na tela** —
> o caminho do primeiro clique até o objetivo, e principalmente os pontos onde ela
> trava, espera ou desiste.
>
> 🚫 **Não duplico:** regra de negócio mora no `prd.md`; estado, entidade e contrato
> moram no `architecture.md`. Aqui mora o caminho. As regras aqui respeitadas são as
> do PRD (RN01–RN16): horários **sobrepostos** são bloqueados, horários **encostados**
> são permitidos e um agendamento **cancelado libera o horário**.

---

## Fluxo 1 — Visualizar a agenda semanal

**Story:** US06
**Critérios que marca:** depende do tempo · pode ser abandonada na leitura (perder o próximo atendimento)

```mermaid
flowchart TD
    A(["«pessoa» abre a agenda"]) --> B{"Escolhe a visão"}
    B -->|"diária"| C["Sistema monta a grade do dia<br/>com todas as profissionais em colunas"]
    B -->|"semanal"| D["Sistema monta a grade da semana<br/>com todas as profissionais em colunas"]
    C --> E{"Há agendamentos Cancelados no período?"}
    D --> E
    E -->|"não"| F{"Há um próximo Agendado<br/>no futuro (que ainda<br/>não passou)?"}
    E -->|"sim"| G["Cancelados aparecem diferenciados,<br/>identificados como 'Cancelado' no<br/>horário original — sem ocupar o horário"] --> F
    F -->|"sim"| H["Destaca o próximo atendimento<br/>(selo ou cor) — a pessoa<br/>sabe qual é o próximo"]
    F -->|"não"| I["Nenhum destaque — a grade<br/>continua visível com indicação<br/>de que não há agendamentos"]
```

**O que decidimos:** a visualização é somente leitura e vale para todos os usuários. Um agendamento cancelado continua aparecendo no horário original — diferenciado — para preservar o histórico, mas não ocupa o horário: outro agendamento pode ser criado naquele período. O destaque do próximo atendimento é o recurso que combate o esquecimento no MVP.

---

## Fluxo 2 — Criar um novo agendamento

**Story:** US03
**Critérios que marca:** depende do tempo · conflito de horário (RN01) · pode ser abandonada na escolha do horário

```mermaid
flowchart TD
    A(["«pessoa» abre 'Novo agendamento'"]) --> B["Escolhe a profissional<br/>(administradora escolhe qualquer;<br/>a profissional fica fixada nela)"]
    B --> C["Escolhe a cliente"] --> D["Escolhe o procedimento"]
    D --> E["Sistema preenche o término<br/>(duração padrão) e o valor<br/>(preço padrão) — ajustáveis"]
    E --> F["Informa início, término e valor"]
    F --> G{"Há sobreposição real com outro<br/>agendamento da mesma profissional —<br/>e não apenas encostar?"}
    G -->|"livre ou encosta"| H{"Validações:<br/>futuro · término > início · valor ≥ 0"}
    G -->|"sobrepõe"| X1[["CONFLITO — o sistema bloqueia e<br/>informa; a pessoa desiste ou<br/>volta a escolher outro horário"]]
    X1 --> F
    H -->|"ok"| I["Agendamento salvo com status<br/>Agendado e aparece na agenda"]
    H -->|"inválida"| J["Bloqueia e indica o campo<br/>a corrigir"] --> F

    style X1 fill:#ff0000,color:#ffffff
```

**O que decidimos sobre o nó vermelho:** o conflito de horário é o ponto em que a pessoa pode **desistir** de agendar — ou **voltar para escolher outro horário**. O sistema trata os dois casos da mesma forma: a tentativa é **bloqueada** e o sistema informa a sobreposição, sem criar o agendamento. O agendamento já existente continua ocupando o período, e a pessoa refaz a escolha. Só a **sobreposição real** vira conflito: um horário que **encosta** de ponta a ponta com outro é permitido, porque não há dois atendimentos da mesma profissional ao mesmo tempo.

---

## Fluxo 3 — Editar um agendamento

**Story:** US04
**Critérios que marca:** depende do tempo · conflito de horário ao mudar data/profissional (RN01) · pode ser abandonada

```mermaid
flowchart TD
    A(["«pessoa» abre um agendamento"]) --> B{"Status do agendamento"}
    B -->|"Cancelado"| C["Não é editável nem reativável —<br/>remarcar é criar um novo agendamento"]
    B -->|"Agendado"| D["«pessoa» altera cliente, procedimento,<br/>profissional, horários ou valor"]
    D --> E{"Mudou horário ou profissional?"}
    E -->|"não"| F{"Validações:<br/>futuro · término > início · valor ≥ 0"}
    E -->|"sim"| G{"Sistema re-verifica o conflito,<br/>desconsiderando o próprio<br/>agendamento na comparação"}
    G -->|"livre ou encosta"| F
    G -->|"sobrepõe"| X1[["CONFLITO — a alteração é bloqueada;<br/>a pessoa refaz o horário"]]
    X1 --> D
    F -->|"ok"| H["Alterações salvas —<br/>o agendamento permanece Agendado"]
    F -->|"inválida"| I["Bloqueia e indica o campo<br/>a corrigir"] --> D
```

**O que decidimos sobre o nó vermelho:** quando a edição muda o **horário ou a profissional**, o sistema re-verifica o conflito do zero — ignorando o próprio agendamento, para ele não "conflitar consigo mesmo". Se houver sobreposição real com outro agendamento da mesma profissional, a alteração é **bloqueada** e a pessoa refaz o horário (Encontros de ponta a ponta seguem permitidos). Agendamentos com status **Cancelado** não podem ser editados nem reativados: para remarcar, cria-se um novo agendamento.

---

## Fluxo 4 — Cancelar um agendamento

**Story:** US05
**Critérios que marca:** depende do tempo · pode ser abandonada no meio · registro de não comparecimento

```mermaid
flowchart TD
    A(["«pessoa» abre um agendamento<br/>e escolhe cancelar"]) --> B{"Status?"}
    B -->|"Cancelado"| C["Já está cancelado — o sistema<br/>informa e não repete a ação"]
    B -->|"Agendado"| D{"O horário do agendamento<br/>já passou?"}
    D -->|"sim"| E["Cancelamento permitido<br/>(ex.: cliente não compareceu)"]
    D -->|"não"| F["Cancelamento normal"]
    E --> G{"«pessoa» confirma<br/>o cancelamento?"}
    F --> G
    G -->|"não"| H["Permanece Agendado — nada muda"]
    G -->|"sim"| I["Status vira Cancelado —<br/>o horário volta a ficar livre"]
    I --> J["Fica no histórico, diferenciado na agenda"]
```

**O que decidimos:** o cancelamento precisa de **confirmação**; sem ela, nada muda. Ao confirmar, o status passa a **Cancelado** e o horário **deixa de ocupar a agenda** — volta a aceitar um novo agendamento naquele período. Não há motivo obrigatório. O cancelamento também vale para horários **já passados** (ex.: não comparecimento), já que o MVP não tem o status "Concluído" — a restrição de "não mexer no passado" vale apenas para criar/editar.

---

## Dúvidas em aberto

| # | Dúvida | Onde ela precisa ser resolvida |
| --- | --- | --- |
| 1 | Forma de pagamento da mensalidade (Pix, boleto, cartão) e gateway — a jornada de pagamento da US07 será desenhada quando essa decisão existir | `docs/architecture.md` / planejamento da US07 |