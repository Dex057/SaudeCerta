# SaúdeCerta — Documento de Visão e Requisitos (E1)

**Disciplina:** TEADS0347 — Desenvolvimento de Aplicações Multicamadas
**Turma:** NOITE C795NL
**Docente:** Márcio Goes do Nascimento
**Período:** 2026.2
**Entrega:** E1 — Documento de Visão e Requisitos

**Equipe:**
| Nome completo |
|---|
| Edson Davi Farias Martins |
| Marcus Carreira Júnior |
| Vergílio Junior |

**Repositório Git:** `https://github.com/Dex057/SaudeCerta`

---

## 1. Visão do produto

O SaúdeCerta é um sistema web para apoiar o funcionamento da Clínica SaúdeCerta, uma unidade de saúde de médio porte com cerca de vinte profissionais atendendo em especialidades diferentes. O objetivo é substituir o controle atual, feito em planilha para a agenda e em pasta física para o prontuário, por um sistema único que cubra o fluxo completo do atendimento: desde a marcação da consulta até o registro clínico e a prescrição.

A proposta não é informatizar tudo de uma vez. O sistema nasce cobrindo o essencial — cadastro, agenda, atendimento, prontuário e controle de acesso — e é pensado para crescer por módulos depois, sem precisar ser reescrito. Isso importa porque a clínica pretende evoluir aos poucos (por exemplo, incluindo faturamento ou telemedicina no futuro), e a arquitetura precisa aguentar esse crescimento.

## 2. Descrição do problema

Hoje a recepção usa planilhas compartilhadas para marcar consultas. Isso já gerou mais de uma vez o mesmo profissional com dois pacientes marcados no mesmo horário, porque duas pessoas mexem na planilha ao mesmo tempo e não existe nenhuma trava contra isso. Quando um paciente remarca, alguém da recepção precisa refazer a marcação manualmente, o que consome tempo em um momento — o balcão — onde tempo é justamente o que falta.

O prontuário em papel traz um problema diferente: quando um paciente que já foi atendido antes retorna, o profissional depende de encontrar a pasta física certa para saber o histórico dele. Isso atrasa o atendimento e, em alguns casos, o profissional acaba atendendo sem ter acesso ao histórico completo. Existe ainda um problema mais sério por trás disso: como o prontuário é físico, não há nenhum registro de quem consultou os dados de um paciente e quando. Dado que se trata de dado de saúde, essa ausência de rastro é um risco real, não só um incômodo operacional.

Resumindo o que o sistema precisa resolver:
- Impedir marcação de consulta em horário já ocupado pelo mesmo profissional.
- Dar acesso rápido ao histórico do paciente no momento do atendimento.
- Registrar quem acessou ou alterou o prontuário de cada paciente.
- Reduzir o retrabalho da recepção ao lidar com remarcações e cancelamentos.

> **Tensão central:** quanto mais forte for o controle de segurança e auditoria sobre o prontuário, mais passos isso pode adicionar ao fluxo da recepção e ao tempo de resposta do sistema. A arquitetura vai precisar equilibrar isso, e não simplesmente maximizar segurança ignorando o resto.

## 3. Stakeholders

Nem todo stakeholder usa o sistema diretamente. Por isso separamos stakeholders (quem tem interesse ou é afetado pelo sistema) dos atores (quem efetivamente opera o sistema), listados na seção seguinte.

| Stakeholder | Interesse no projeto |
|---|---|
| Direção da clínica | Contratante do sistema; quer reduzir erros operacionais e ter visibilidade gerencial (relatórios). |
| Recepção | Vai operar o sistema no dia a dia para marcar e remarcar consultas; precisa que seja rápido. |
| Profissionais de saúde | Usam o sistema para consultar histórico, registrar atendimento e prescrever; dependem da confiabilidade dos dados. |
| Pacientes | Titulares dos dados armazenados; têm interesse direto na privacidade do prontuário e em poder ver seus próprios agendamentos. |
| Equipe de desenvolvimento (nós) | Responsável por entregar e manter a arquitetura ao longo do semestre. |
| Órgãos reguladores (LGPD) | Não usam o sistema, mas impõem exigências sobre tratamento de dado sensível de saúde, o que reforça o driver de segurança/auditoria. |

## 4. Atores do sistema

| Ator | O que faz no sistema |
|---|---|
| Recepcionista | Cadastra pacientes, marca, remarca e cancela consultas. |
| Profissional de saúde | Consulta o histórico do paciente, registra atendimento, emite prescrição e solicita exames. |
| Paciente | Consulta seus próprios agendamentos e dados. |
| Administrador | Gerencia usuários, perfis de acesso e especialidades; acompanha relatórios e trilha de auditoria. |

## 5. Requisitos funcionais

| ID | Requisito | Critério de aceitação |
|---|---|---|
| RF01 | Cadastrar e manter pacientes | Ao informar nome, CPF e data de nascimento válidos, o sistema salva o paciente e passa a listá-lo na busca. |
| RF02 | Cadastrar profissionais e vincular a especialidades | Um profissional precisa ter pelo menos uma especialidade; o cadastro não pode ser salvo sem isso. |
| RF03 | Definir a agenda do profissional | Profissional ou admin define dias e faixas de horário; o sistema gera os horários disponíveis a partir disso. |
| RF04 | Marcar consulta | Ao escolher profissional, data e horário livres, a consulta é criada com status AGENDADA. |
| RF05 | Remarcar ou cancelar consulta | Uma consulta AGENDADA pode ser remarcada ou cancelada; o horário liberado volta a ficar disponível. |
| RF06 | Impedir conflito de horário | O sistema recusa marcar uma consulta em horário já ocupado do mesmo profissional. |
| RF07 | Registrar atendimento no prontuário | O profissional registra queixa, evolução e conduta, vinculadas ao paciente e à consulta atendida. |
| RF08 | Emitir prescrição | Uma prescrição tem um ou mais medicamentos (nome, dose, posologia) e fica vinculada ao atendimento. |
| RF09 | Solicitar exames | O atendimento pode gerar uma ou mais solicitações de exame, cada uma com status de acompanhamento. |
| RF10 | Consultar histórico do paciente | O profissional visualiza, em ordem cronológica, os atendimentos, prescrições e exames anteriores do paciente. |
| RF11 | Autenticar usuários e aplicar perfis | O acesso a cada funcionalidade depende do perfil do usuário logado (recepção, profissional, admin, paciente). |
| RF12 | Registrar auditoria de acesso ao prontuário | Todo acesso ou alteração no prontuário grava quem fez, quando e em qual paciente. |
| RF13 | Gerar relatórios | O sistema gera relatórios de consultas por período, por profissional e por especialidade, com totais. |

## 6. Requisitos não funcionais

Estes são os requisitos que mais vão pesar nas decisões de arquitetura mais adiante (ver seção 8).

| ID | Atributo | Cenário de qualidade |
|---|---|---|
| RNF01 | Segurança/Privacidade | Um usuário sem perfil clínico tenta abrir um prontuário → o sistema nega o acesso e registra a tentativa → meta: 100% das tentativas indevidas bloqueadas. |
| RNF02 | Auditabilidade | Um profissional abre um prontuário → o sistema grava usuário, paciente, data/hora e ação → o registro fica disponível para o admin consultar em até 1 minuto. |
| RNF03 | Desempenho | Recepcionista busca um paciente pelo nome, com a base em uso normal → resultado retorna em menos de 2 segundos no percentil 95. |
| RNF04 | Disponibilidade | Durante o horário de funcionamento (12h/dia) → a agenda continua operante → disponibilidade mensal de pelo menos 99% nesse período. |
| RNF05 | Modificabilidade | Ao incluir um módulo novo (ex.: faturamento) no futuro → não deve ser necessário alterar o núcleo de agenda/prontuário → esforço estimado de no máximo 5 dias de equipe. |
| RNF06 | Usabilidade | Recepcionista marca uma consulta simples → conclui em até 1 minuto e no máximo 5 cliques a partir da tela inicial. |
| RNF07 | Segurança de credenciais | Senha nunca é armazenada em texto puro → o sistema usa hash com sal (BCrypt) → verificável em revisão de código. |

## 7. Regras de negócio

| ID | Regra |
|---|---|
| RN01 | Um paciente é identificado unicamente pelo CPF; não pode existir dois cadastros ativos com o mesmo CPF. |
| RN02 | Um profissional não pode ter duas consultas com status AGENDADA no mesmo horário. |
| RN03 | Só é possível registrar atendimento para consultas cuja data já ocorreu ou é a do dia atual. |
| RN04 | Prescrição e solicitação de exame só existem vinculadas a um atendimento — não existem de forma avulsa. |
| RN05 | O prontuário é somente-anexação: nenhum registro de atendimento é apagado, apenas complementado por novos registros. |
| RN06 | Paciente só enxerga os próprios dados; a recepção não tem acesso ao conteúdo clínico do prontuário. |
| RN07 | Cancelamento de consulta só é permitido se ela estiver com status AGENDADA. |

## 8. Drivers arquiteturais

Nem todo requisito não funcional pesa igual na hora de desenhar a arquitetura. Olhando para os RNFs listados, destacamos três como drivers principais:

**Segurança e privacidade dos dados clínicos (RNF01, RNF07).** É dado de saúde, então esse requisito não é negociável e influencia praticamente todas as camadas do sistema — autenticação, controle de acesso por perfil (RBAC) e como o prontuário é exposto pela API.

**Auditabilidade do acesso ao prontuário (RNF02).** Diretamente ligado a RF12 e à regra RN06. É um driver porque não dá pra tratar como um "log a mais": o guia do caso já deixa explícito que hoje não existe nenhum registro de quem acessa o prontuário físico, e isso é apontado como um dos riscos centrais do cenário atual.

**Desempenho na busca e no acesso a dados (RNF03).** Essencial para a recepção conseguir marcar consulta rápido (RNF06) e para o profissional abrir o prontuário sem esperar.

O conflito entre esses drivers é o ponto central da arquitetura: registrar auditoria de forma síncrona a cada leitura do prontuário atende RNF02, mas pode penalizar RNF03. Da mesma forma, um controle de acesso mais rígido reduz risco de segurança mas pode tornar o fluxo da recepção mais lento, o que vai contra RNF06. Essas decisões de tradeoff (por exemplo, auditoria assíncrona por eventos) serão detalhadas em ADRs na próxima entrega (E2); aqui o objetivo é só deixar registrado que esse conflito existe e que ele veio direto da análise dos requisitos.

Disponibilidade (RNF04) e modificabilidade (RNF05) são drivers secundários nesta fase: importam, mas não geram o mesmo nível de tensão entre si que segurança/auditoria versus desempenho/usabilidade.

## 9. Estilo arquitetural inicial

**Decisão: arquitetura em camadas (apresentação, aplicação, domínio, persistência), organizada como monólito modular, com o domínio isolado da infraestrutura por interfaces (estilo hexagonal / ports and adapters).**

Justificativa:

A equipe é pequena e o prazo é de um semestre — um cenário de microsserviços desde o início adicionaria complexidade de deploy, comunicação entre serviços e observabilidade sem trazer benefício real nesta fase do projeto. Uma arquitetura em camadas resolve o que o caso pede agora: separar claramente a parte que expõe a API (apresentação), a que orquestra os casos de uso (aplicação), a que concentra as regras de negócio (domínio, onde entram RN01 a RN07) e a que fala com o banco (persistência).

A escolha por isolar o domínio da infraestrutura (estilo hexagonal) vem direto do driver de modificabilidade (RNF05): o sistema precisa crescer por módulos sem reescrita, e o guia do caso já indica que no Projeto Final vamos extrair um serviço (agenda ou prontuário) do monólito. Se o domínio depender de detalhes do Spring Data JPA ou de qualquer outra tecnologia de persistência, essa extração fica muito mais difícil. Definindo interfaces no domínio e implementando-as na camada de persistência, mantemos essa porta aberta desde já sem pagar o custo de operar microsserviços agora.

Também pensamos no driver de segurança e auditoria: como o RBAC e a trilha de auditoria são coisas que atravessam vários módulos (agenda, prontuário, cadastro), faz sentido tratá-los como uma camada transversal separada, em vez de espalhar essa lógica dentro de cada módulo de negócio.

Em resumo, a arquitetura em camadas com monólito modular atende ao tamanho da equipe e ao prazo da disciplina, e a separação por portas/domínio garante que a evolução prevista para as próximas entregas (extração de serviço no Projeto Final) não exija reescrever o que for feito agora.
