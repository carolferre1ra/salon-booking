# 📄 Product Requirements Document (PRD)

**Projeto:** SalonHub
**Versão:** 1.0.0 · preenchido via `/utf-prd`
**Última atualização:** 2026-09-13

> 🤖 **Este documento é a fonte da verdade sobre o QUE o produto faz.** Regra de
> negócio que não estiver aqui não existe — nem para a equipe, nem para a IA.
> Tecnologia **não** se discute aqui: isso é assunto do `architecture.md`.

---

## 🎯 1. Visão Geral e Objetivo

**O problema:** a organização da agenda de um salão de beleza é feita de forma manual e descentralizada, o que gera conflitos de horário — uma profissional acaba com dois atendimentos marcados no mesmo período — e esquecimentos dos próximos atendimentos.

**A solução:** o SalonHub é um sistema de agendamento para salões de beleza que centraliza os atendimentos em uma agenda única e organizada. A administradora e as profissionais cadastram clientes e procedimentos. Todos os profissionais podem visualizar as agendas de todas as profissionais, mas cada profissional pode gerenciar apenas os seus próprios agendamentos; a administradora pode gerenciar os agendamentos de todas as profissionais. O sistema impede que uma mesma profissional tenha dois agendamentos no mesmo horário.

**Como saberemos que deu certo:** ao tentar marcar um horário já ocupado, o sistema bloqueia o agendamento e informa o conflito; a profissional visualiza facilmente sua agenda diária ou semanal e identifica o próximo atendimento, que fica destacado. A desorganização diminui porque os horários ficam centralizados, os conflitos são evitados e a profissional sabe claramente quais atendimentos terá ao longo do dia.

---

## 📖 2. Glossário Ubíquo

> Os termos do negócio, como o cliente fala. É daqui que o `architecture.md` deriva os nomes das entidades.

| Termo | Significa | Não confundir com |
| :---- | :-------- | :---------------- |
| **Agendamento** | Compromisso marcado antecipadamente para uma cliente ser atendida por uma determinada profissional em um dia e horário definidos. Registra horário, cliente, procedimento, profissional e o valor daquele atendimento. Pode estar com status **Agendado** ou **Cancelado**. | Procedimento (serviço cadastrado); atendimento já realizado. |
| **Procedimento** | Serviço oferecido pelo salão, cadastrado com nome, duração padrão e preço padrão. Serve de referência para os agendamentos. O valor e o horário podem ser ajustados especificamente dentro de um agendamento. | Agendamento (compromisso em horário específico); atendimento já realizado. |
| **Cliente** | Pessoa atendida pelo salão, que possui um ou mais agendamentos. Não acessa o sistema nem agenda por conta própria. | Assinante (quem contrata o SalonHub); profissional (quem atende). |
| **Assinante** | Responsável pelo salão que contrata e paga a mensalidade do SalonHub. No projeto, é a mesma pessoa que a administradora. | Cliente (pessoa atendida). |
| **Profissional** | Pessoa que realiza os procedimentos e gerencia seus próprios agendamentos; visualiza as agendas das demais, sem alterá-las. Pode acumular o papel de administradora. | Cliente (pessoa atendida). |
| **Administradora** | Responsável por gerenciar o salão no SalonHub: visualiza, edita e cancela agendamentos de todas as profissionais. Também responsável pela contratação e pagamento da assinatura mensal. Quando também atua como profissional, gerencia os seus próprios agendamentos. | Cliente (pessoa atendida). |
| **Agenda** | Visão organizada dos horários e compromissos do salão em um período, mostrando os agendamentos separados por profissional; visualizada por dia ou por semana. | Agendamento individual (um único compromisso). |
| **Assinatura / Mensalidade** | Cobrança recorrente mensal paga pela administradora para manter o acesso do salão ao SalonHub; renovada a cada mês e, se não paga, suspende o acesso até regularização. | Valor do atendimento (preço do procedimento cobrado da cliente, registrado no agendamento). |

---

## 👤 3. Atores e Permissões

> ⚠️ A coluna **"Não pode"** vira Guard e controle de role na API.

| Ator | Quem é | Pode | Não pode |
| :--- | :----- | :--- | :------- |
| **Administradora** | Responsável por gerenciar o salão no SalonHub e contratante/assinante da mensalidade. Quando também atua como profissional, além de gerenciar os agendamentos de todas as profissionais, gerencia os seus próprios agendamentos. | Visualizar as agendas de todas as profissionais; cadastrar clientes e procedimentos; criar, editar e cancelar agendamentos de qualquer profissional; gerenciar a assinatura mensal | Criar um agendamento em conflito de horário com outro da mesma profissional (o sistema bloqueia); realizar operações técnicas do pagamento (processar/aprovar — fica a cargo do serviço de pagamento) |
| **Profissional** | Pessoa que realiza os procedimentos e organiza seus próprios agendamentos. | Visualizar as agendas de todas as profissionais; cadastrar clientes e procedimentos; criar, editar e cancelar apenas os seus próprios agendamentos | Editar ou cancelar agendamentos de outras profissionais; gerenciar a assinatura; criar/editar agendamento com sobreposição de horário com outro seu (o sistema bloqueia) |

---

## 📝 4. Escopo Funcional (User Stories)

> Prioridade **MoSCoW** — o conjunto de `Must Have` é o escopo comprometido do projeto; `Should`/`Could` entram se sobrar tempo. Tamanho **S/M/L**. Toda story nasce `⚪ Draft` — só o aluno promove a `🟡 Ready`; `🟢 Live` é quando o PR da história mescla (o auditor final confere).

### US01 — Cadastrar e editar cliente · `Must Have` · `S` · Status: `⚪ Draft`

<!-- Status: `⚪ Draft` (não codificar) · `🟡 Ready` (vira Issue) · `🟢 Live` (PR mesclado) -->

**Como** administradora ou profissional, **eu quero** cadastrar e editar clientes com nome e telefone, **para que** eu possa identificá-las e contatá-las ao organizar os agendamentos.

**Critérios de aceite:**

- [ ] **Dado** nome e telefone válidos, **quando** salvo o cadastro, **então** a cliente fica registrada e disponível para ser selecionada em novos agendamentos.
- [ ] **Dado** o formulário sem nome ou sem telefone, ou com telefone inválido, **quando** alguém salva, **então** o sistema bloqueia o cadastro e indica o campo que precisa ser corrigido.
- [ ] **Dado** um telefone que já pertence a outra cliente, **quando** alguém tenta cadastrar, **então** o sistema bloqueia e informa que o telefone já está associado a uma cliente existente.
- [ ] **Dado** uma cliente cadastrada, **quando** alguém edita o nome ou o telefone e salva, **então** as alterações ficam registradas.
- [ ] **Dado** uma edição em que o novo telefone já pertence a outra cliente, **quando** alguém tenta salvar, **então** o sistema bloqueia a alteração e informa a duplicidade.

**Fora de escopo desta story:** excluir clientes.

**Regras relacionadas:** RN03

---

### US02 — Cadastrar e editar procedimento · `Must Have` · `S` · Status: `⚪ Draft`

**Como** administradora ou profissional, **eu quero** cadastrar e editar os procedimentos com nome, duração padrão e preço padrão, **para que** eu possa usá-los como referência na criação dos agendamentos.

**Critérios de aceite:**

- [ ] **Dado** um procedimento com nome, duração padrão e preço padrão válidos, **quando** salvo o cadastro, **então** o procedimento fica registrado e disponível para ser selecionado em novos agendamentos.
- [ ] **Dado** nome em branco, duração padrão zero ou negativa, ou preço padrão zero ou negativo, **quando** alguém tenta cadastrar, **então** o sistema bloqueia e indica o campo que precisa ser corrigido.
- [ ] **Dado** um nome que já pertence a outro procedimento, **quando** alguém tenta cadastrar, **então** o sistema bloqueia e informa que já existe um procedimento com esse nome.
- [ ] **Dado** um procedimento cadastrado, **quando** alguém edita o nome, a duração ou o preço, **então** as alterações ficam registradas, com as mesmas validações aplicadas.
- [ ] **Dado** uma edição em que o novo nome já pertence a outro procedimento, **quando** alguém salva, **então** o sistema bloqueia a alteração e informa a duplicidade.

**Fora de escopo desta story:** excluir procedimentos (o cadastro é preservado mesmo que o procedimento deixe de ser oferecido).

**Regras relacionadas:** RN04, RN05

---

### US03 — Criar agendamento · `Must Have` · `M` · Status: `⚪ Draft`

**Como** administradora ou profissional, **eu quero** criar um agendamento informando cliente, procedimento, profissional e os horários de início e término — com o valor do atendimento preenchido automaticamente e ajustável — **para que** o atendimento fique registrado na agenda, com conflitos de horário evitados.

**Critérios de aceite:**

- [ ] **Dado** um procedimento selecionado e um horário de início informado, **quando** o sistema monta o agendamento, **então** o horário de término é preenchido automaticamente com a duração padrão e o valor com o preço padrão do procedimento — ambos podendo ser ajustados naquele agendamento, sem alterar o cadastro do procedimento.
- [ ] **Dado** um período sem sobreposição com outros agendamentos da mesma profissional, **quando** o agendamento é salvo, **então** ele fica registrado com status **Agendado** e aparece na agenda da profissional.
- [ ] **Dado** um período que se **sobrepõe** a outro agendamento da mesma profissional, **quando** alguém tenta salvar, **então** o sistema bloqueia e informa o conflito de horário.
- [ ] **Dado** um agendamento que começa exatamente quando outro termina (ou termina exatamente quando outro começa), **quando** salvo, **então** ele é permitido, sem conflito.
- [ ] **Dado** um horário no passado (mesmo dia ou data anterior), **quando** alguém tenta salvar, **então** o sistema bloqueia o agendamento.
- [ ] **Dado** o formulário sem cliente, sem procedimento, sem horário de início ou sem horário de término, **quando** alguém tenta salvar, **então** o sistema bloqueia e indica o campo que precisa ser corrigido.
- [ ] **Dado** um horário de término igual ou anterior ao de início, **quando** alguém tenta salvar, **então** o sistema bloqueia e informa que o término precisa ser posterior ao início.
- [ ] **Dado** um valor negativo de atendimento, **quando** alguém tenta salvar, **então** o sistema bloqueia.
- [ ] **Dado** que quem cria é uma **profissional**, **quando** o agendamento é salvo, **então** o campo profissional fica fixado nela mesma; sendo uma **administradora**, ela pode escolher qualquer profissional.

**Fora de escopo desta story:** registro de atendimentos já realizados (retroativos) e status "Concluído".

**Regras relacionadas:** RN01, RN02, RN06, RN07, RN08, RN11, RN15

---

### US04 — Editar agendamento · `Must Have` · `M` · Status: `⚪ Draft`

**Como** administradora ou profissional, **eu quero** editar um agendamento com status Agendado — cliente, procedimento, profissional, horários ou valor — **para que** eu possa corrigir ou remarcar mantendo a agenda sem conflitos.

**Critérios de aceite:**

- [ ] **Dado** um agendamento com status **Agendado**, **quando** alguém com permissão altera um ou mais campos e salva, **então** as alterações ficam registradas e o agendamento permanece Agendado.
- [ ] **Dado** que a edição alterou o **horário** ou a **profissional**, **quando** alguém salva, **então** o sistema re-verifica o conflito (sobreposição real com outro agendamento da mesma profissional bloqueia; encostar de ponta a ponta é permitido), **desconsiderando o próprio agendamento** na comparação.
- [ ] **Dado** um novo horário no passado, **quando** alguém salva a edição, **então** o sistema bloqueia.
- [ ] **Dado** um término igual ou anterior ao início após a edição, **quando** alguém salva, **então** o sistema bloqueia e informa que o término precisa ser posterior ao início.
- [ ] **Dado** um valor negativo após a edição, **quando** alguém salva, **então** o sistema bloqueia.
- [ ] **Dado** que a edição deixa sem cliente, sem procedimento, sem horário de início ou sem horário de término, **quando** alguém salva, **então** o sistema bloqueia e indica o campo que precisa ser corrigido.
- [ ] **Dado** que quem edita é uma **profissional**, **quando** ela tenta salvar, **então** só consegue editar agendamentos próprios; a **administradora** pode editar de qualquer profissional.
- [ ] **Dado** um agendamento com status **Cancelado**, **quando** alguém tenta editá-lo ou reativá-lo, **então** o sistema bloqueia — para remarcar, cria-se um novo agendamento.

**Fora de escopo desta story:** reativar agendamentos cancelados.

**Regras relacionadas:** RN01, RN02, RN06, RN08, RN09, RN11, RN15

---

### US05 — Cancelar agendamento · `Must Have` · `S` · Status: `⚪ Draft`

**Como** administradora ou profissional, **eu quero** cancelar um agendamento, **para que** o horário deixe de ocupar a agenda e fique registrado que aquele atendimento não vai acontecer.

**Critérios de aceite:**

- [ ] **Dado** um agendamento com status **Agendado**, **quando** alguém com permissão solicita o cancelamento **e confirma**, **então** o status passa para **Cancelado** e o horário deixa de ocupar a agenda.
- [ ] **Dado** o pedido de cancelamento, **quando** a pessoa **não confirma**, **então** o agendamento permanece Agendado.
- [ ] **Dado** um agendamento cujo horário **já passou**, **quando** alguém o cancela, **então** o cancelamento é permitido (ex.: cliente não compareceu) e ele fica no histórico com status Cancelado.
- [ ] **Dado** um agendamento com status **Cancelado**, **quando** alguém tenta cancelá-lo de novo, **então** o sistema informa que ele já está cancelado e não repete a ação.
- [ ] **Dado** que quem cancela é uma **profissional**, **quando** ela tenta cancelar, **então** só consegue cancelar agendamentos próprios; a **administradora** pode cancelar de qualquer profissional.

**Fora de escopo desta story:** campo obrigatório de motivo do cancelamento; reativar agendamentos cancelados.

**Regras relacionadas:** RN02, RN09, RN10, RN15

---

### US06 — Visualizar agenda · `Must Have` · `M` · Status: `⚪ Draft`

**Como** administradora ou profissional, **eu quero** visualizar a agenda de uma profissional em visão **diária ou semanal**, **para que** eu tenha uma visão organizada dos atendimentos e identifique rapidamente o próximo.

**Critérios de aceite:**

- [ ] **Dado** que uma pessoa com acesso abre a agenda, **quando** ela seleciona a visão (diária ou semanal) e a profissional, **então** o sistema exibe a grade de horários com os agendamentos com status **Agendado** ocupando seus períodos — todas as pessoas com acesso podem visualizar a agenda de qualquer profissional.
- [ ] **Dado** um agendamento **Cancelado**, **quando** a agenda é exibida, **então** ele aparece no horário original, **visualmente diferenciado** e identificado como "Cancelado", sem ocupar o horário (um novo agendamento pode ser criado naquele período).
- [ ] **Dado** um dia ou semana **sem agendamentos**, **quando** a agenda é exibida, **então** a grade de horários continua sendo mostrada normalmente, com uma indicação de que não há agendamentos naquele período.
- [ ] **Dado** que existe um agendamento com status **Agendado** futuro no período visualizado, **quando** a agenda é exibida, **então** o próximo atendimento (o mais próximo no futuro que ainda não passou) fica **destacado** — com cor diferenciada ou selo visual, sem alterar os dados do agendamento. Na visão diária, é o próximo do dia; na semanal, o próximo da semana.
- [ ] **Dado** que **não existe** nenhum agendamento Agendado futuro no período, **quando** a agenda é exibida, **então** nenhum atendimento é destacado.

**Fora de escopo desta story:** alterar dados pelos quais a pessoa não tem permissão (a visualização é só leitura).

**Regras relacionadas:** RN15

---

### US07 — Assinatura e mensalidade · `Must Have` · `L` · Status: `⚪ Draft`

**Como** administradora, que também é a assinante, **eu quero** contratar a assinatura do SalonHub e pagar a mensalidade mensal, **para que** o salão mantenha o acesso ao sistema.

**Critérios de aceite:**

- [ ] **Dado** que a administradora contrata a assinatura e paga a **primeira mensalidade**, **quando** o pagamento é confirmado pelo serviço de pagamento, **então** a assinatura começa, essa mensalidade fica **Paga** e o acesso ao SalonHub é liberado.
- [ ] **Dado** uma assinatura ativa, **quando** um novo período mensal se inicia, **então** uma nova mensalidade é gerada com status **Pendente** e vencimento no mesmo dia do mês em que a assinatura começou (se o dia não existir no mês, usa-se o **último dia** daquele mês).
- [ ] **Dado** uma mensalidade **Pendente**, **quando** a administradora paga antes do vencimento e o pagamento é confirmado, **então** ela passa a **Paga** e o acesso permanece ativo.
- [ ] **Dado** uma mensalidade **Pendente** cujo vencimento passou sem pagamento, **quando** passa a data de vencimento, **então** ela passa a **Em atraso** e, a partir do dia seguinte ao vencimento, o acesso é **suspenso automaticamente para todos os usuários do salão**.
- [ ] **Dado** o acesso suspenso, **quando** a administradora paga a mensalidade em atraso e o pagamento é confirmado, **então** a mensalidade passa a **Paga** e o acesso de todos é **restabelecido automaticamente**.
- [ ] **Dado** que a mensalidade está em atraso e o acesso está suspenso, **quando** qualquer usuária do salão tenta entrar, **então** o login fica **bloqueado** pela situação da assinatura — e volta a funcionar após a regularização.
- [ ] **Dado** que o acesso foi suspenso e depois restabelecido, **quando** a administradora volta a entrar, **então** os dados e agendamentos cadastrados permanecem intactos.

**Fora de escopo desta story:** decisão da forma de pagamento e do gateway (em aberto); cancelamento/reembolso de mensalidade; plano com múltiplos salões.

**Regras relacionadas:** RN12, RN15, RN16

---

### US08 — Acessar o sistema · `Should Have` · `S` · Status: `⚪ Draft`

**Como** administradora ou profissional, **eu quero** entrar no SalonHub com e-mail e senha, **para que** eu acesse a agenda e as funcionalidades conforme o meu papel.

**Critérios de aceite:**

- [ ] **Dado** um e-mail e uma senha corretos, **quando** a pessoa envia o login, **então** o acesso é liberado conforme o seu papel (administradora gerencia os agendamentos de todas as profissionais; profissional gerencia apenas os próprios).
- [ ] **Dado** um e-mail ou uma senha incorretos, **quando** a pessoa envia o login, **então** o acesso é negado com a mensagem única e genérica **"E-mail ou senha incorretos"**, sem revelar qual campo está errado.
- [ ] **Dado** o formulário de login sem e-mail ou sem senha, **quando** a pessoa tenta entrar, **então** o sistema bloqueia e indica o campo que precisa ser preenchido.
- [ ] **Dado** que a conta da administradora foi criada na contratação, mas a **primeira mensalidade ainda não foi paga**, **quando** ela tenta entrar, **então** o acesso é bloqueado pela situação da assinatura.
- [ ] **Dado** que a assinatura do salão está **suspensa** (mensalidade em atraso), **quando** qualquer usuária do salão tenta entrar, **então** o login é bloqueado; após a confirmação do pagamento da mensalidade em atraso, o acesso de todos é **restabelecido automaticamente**.
- [ ] **Dado** que uma profissional teve a conta criada pela administradora, **quando** ela entra com o e-mail e a senha cadastrados, **então** o acesso funciona conforme o papel dela.

**Fora de escopo desta story:** cadastro público de contas; criação de conta pela própria profissional.

**Regras relacionadas:** RN12, RN13, RN14, RN15, RN16

---

### US09 — Recuperar senha · `Should Have` · `S` · Status: `⚪ Draft`

**Como** usuária (administradora ou profissional), **eu quero** recuperar minha senha quando a esqueço, **para que** eu volte a acessar o sistema sem depender de terceiros.

**Critérios de aceite:**

- [ ] **Dado** que a usuária esqueceu a senha, **quando** ela escolhe "Esqueci minha senha" e informa um e-mail **não cadastrado**, **então** o sistema informa que o e-mail não está cadastrado.
- [ ] **Dado** um e-mail **cadastrado**, **quando** o pedido é feito, **então** é gerado um link/token **temporário e de uso único** para redefinir a senha.
- [ ] **Dado** um token válido, **quando** a usuária define uma nova senha com **no mínimo 8 caracteres**, **então** a senha é alterada e ela consegue entrar com a nova senha.
- [ ] **Dado** um token **expirado ou já utilizado**, **quando** a usuária tenta redefinir, **então** o sistema rejeita e ela precisa solicitar um novo link.
- [ ] **Dado** uma nova senha com **menos de 8 caracteres**, **quando** a usuária confirma, **então** o sistema rejeita e informa a regra.

**Fora de escopo desta story:** redefinição de senha pela administradora; cadastro público.

**Regras relacionadas:** RN13, RN14

---

## 🛡️ 5. Regras de Negócio (Constraints)

| ID | Regra | Stories |
| :-- | :---- | :------ |
| **RN01** | Dois agendamentos da mesma profissional entram em conflito **somente quando os períodos se sobrepõem**; agendamentos que se encostam de ponta a ponta são permitidos. O sistema bloqueia a ação e informa o conflito. | US03, US04 |
| **RN02** | Agendamentos **só podem ser criados ou editados para horários futuros** (mesmo dia ou datas futuras). Exceção: o **cancelamento** é permitido mesmo para horários já passados. | US03, US04, US05 |
| **RN03** | O **telefone da cliente é único**. Cadastro ou edição com telefone já usado por outra cliente é **bloqueado**, com aviso. | US01 |
| **RN04** | O **nome do procedimento é único**. Cadastro ou edição com nome já usado por outro procedimento é **bloqueado**, com aviso. | US02 |
| **RN05** | Procedimento: nome obrigatório; **duração padrão e preço padrão maiores que zero**. Validações aplicadas no cadastro e na edição. | US02 |
| **RN06** | Valor do agendamento: pode ser **zero ou positivo**; apenas **negativo** é bloqueado. | US03, US04 |
| **RN07** | Ao criar um agendamento, o **término é preenchido com a duração padrão** e o **valor com o preço padrão** do procedimento; ambos podem ser **ajustados naquele agendamento**, sem alterar o cadastro do procedimento. | US03 |
| **RN08** | O horário de **término deve ser posterior ao de início**; término igual ou anterior bloqueia o agendamento. | US03, US04 |
| **RN09** | Agendamento com status **Cancelado não é editado nem reativado**; para remarcar, cria-se um novo agendamento. | US04, US05 |
| **RN10** | O cancelamento **não exige motivo** e é feito após **confirmação**; o status passa a Cancelado e o horário deixa de ocupar a agenda. | US05 |
| **RN11** | Campos obrigatórios do agendamento: cliente, procedimento, profissional, horário de **início** e horário de **término**. A ausência bloqueia e indica o campo. O valor não é campo obrigatório (pode ser zero e é preenchido automaticamente a partir do procedimento). | US03, US04 |
| **RN12** | Assinatura/mensalidade: a **primeira mensalidade**, após a confirmação do primeiro pagamento, fica **Paga** e **inicia a assinatura**. As mensalidades **seguintes nascem Pendente**, passam a **Paga** após a confirmação do pagamento e a **Em atraso** após o vencimento. Vencimento no mesmo dia do mês do início (ou último dia quando o dia não existe). A partir do dia seguinte ao vencimento não pago, o acesso é **suspenso automaticamente para todos os usuários do salão** e restabelecido após pagamento confirmado; os dados permanecem intactos. | US07, US08 |
| **RN13** | Senha com **mínimo de 8 caracteres**, sem exigir números ou símbolos; vale para a senha inicial da profissional e para a redefinição. | US08, US09 |
| **RN14** | O **login** usa a **mensagem genérica** "E-mail ou senha incorretos", sem revelar qual campo errou. Na **recuperação de senha**, o sistema **informa diretamente** quando o e-mail não está cadastrado; para e-mails cadastrados, gera-se um link/token **de uso único e com validade limitada**. | US08, US09 |
| **RN15** | Permissões por papel: a **profissional** gerencia apenas os próprios agendamentos e não gerencia a assinatura; a **administradora** gerencia todos e a assinatura; ambos cadastram clientes e procedimentos e visualizam todas as agendas. | US01–US08 |
| **RN16** | A **administradora cria as contas das profissionais** (nome, e-mail, senha inicial). A conta da administradora nasce na contratação e é **liberada após a confirmação do primeiro pagamento**. Não há cadastro público. | US08 |

---

## 🚫 6. Fora de Escopo (Non-goals)

> O que o produto deliberadamente **não** faz neste semestre — o `Won't Have` do MoSCoW, com o motivo de cada corte.

- **Área/autoagendamento do cliente** — porque a cliente não acessa o sistema: os agendamentos são feitos por alguém do salão (telefone, presencial ou outro meio).
- **Pagamento online pelo cliente no agendamento** — porque o pagamento dos atendimentos é feito na loja, no momento do atendimento; o que o SalonHub cobra é a assinatura mensal do salão.
- **Exclusão de clientes e de procedimentos** — porque preserva o histórico dos agendamentos que os referenciam.
- **Status "Concluído" e registro retroativo de atendimentos** — porque o MVP só tem Agendado e Cancelado; atendimentos passados permanecem registrados no histórico.
- **Editar/reativar agendamentos cancelados** — porque remarcar é criar um novo agendamento, respeitando as regras.
- **Motivo obrigatório de cancelamento** — porque o cancelamento registra só o estado, sem justificativa.
- **Redefinição de senha pela administradora e cadastro público de contas** — porque as contas são criadas pela administradora e a recuperação é autônoma pela usuária.
- **Cancelamento/reembolso de mensalidade e plano com múltiplos salões** — porque o MVP tem um único plano mensal por salão.
- **Cobrança automática (recorrente sem ação da administradora)** — porque no MVP a mensalidade é paga manualmente pela administradora.
- **Notificações/lembretes automáticos (e-mail, WhatsApp, SMS)** — o combate ao esquecimento nesta versão é o destaque do próximo atendimento na agenda.

---

## ⚙️ 7. Requisitos Não Funcionais (Qualidade)

> Só os que o aluno consegue justificar na defesa.

- **Segurança de acesso** — o sistema possui login, dados de clientes e permissões diferentes entre administradora e profissionais; as senhas não devem ser armazenadas em texto puro.
- **Confiabilidade da agenda** — o sistema deve garantir que dois agendamentos conflitantes para a mesma profissional não sejam criados, inclusive em tentativas simultâneas.
- **Desempenho** — as telas principais e a agenda devem ter bom tempo de resposta, sem meta rígida de segundos.

---

## ❓ Dúvidas em aberto

- **Forma de pagamento da mensalidade (Pix, boleto, cartão) e eventual gateway de pagamento** — decisão a ser tomada na etapa de arquitetura/planejamento; a US07 depende disso para ser implementada.
- **Aceite do professor** — o tema precisa ser aceito pela disciplina (único na turma) antes de qualquer story virar Issue.

---

## 🛠️ 8. Histórico

| Data | Versão | O que mudou |
| :--- | :----- | :---------- |
| 2026-09-13 | 1.0.0 | Versão inicial via `/utf-prd` |