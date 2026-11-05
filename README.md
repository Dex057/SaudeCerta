# SaúdeCerta — Sistema de Gestão de Clínica

Estudo de caso da disciplina TEADS0347 — Desenvolvimento de Aplicações Multicamadas (IFPA, turma NOITE C795NL, 2026.2, Prof. Márcio Goes do Nascimento).

Sistema web multicamadas para digitalizar o fluxo de atendimento de uma clínica de médio porte: cadastro de pacientes e profissionais, agenda de consultas, prontuário eletrônico, prescrição/exames e controle de acesso com auditoria.

## Equipe

| Integrante | Frente no E2 | ADRs sob responsabilidade | Revisa os PRs de |
|---|---|---|---|
| Edson Davi Farias Martins | Coordenação, auditoria, implantação e cenários de qualidade | ADR-001, ADR-002 | Vergílio |
| Marcus Carreira Júnior | Diagramas C4 (draw.io) e modelo de domínio | ADR-003, ADR-006 | Edson |
| Vergílio Junior | Contrato REST, segurança e rastreabilidade | ADR-004, ADR-005 | Marcus |

## Entregas

Cada entrega acrescenta ao repositório e é marcada com uma tag.

- [x] **E1** — Visão e Requisitos (tag [`E1`](../../tree/E1)): [`docs/visao.md`](docs/visao.md), [`docs/requisitos.md`](docs/requisitos.md)
- [ ] **E2** — Projeto Arquitetural (em andamento, ver [issues](../../issues?q=is%3Aissue+milestone%3AE2)): [`docs/arquitetura.md`](docs/arquitetura.md), [`docs/adr/`](docs/adr/README.md), [`docs/rastreabilidade.md`](docs/rastreabilidade.md)
- [ ] **E3** — Protótipo em Spring Boot (`src/`)
- [ ] **Projeto Final** — Arquitetura baseada em serviços

## Estrutura do repositório

```
docs/
  visao.md            visão, stakeholders, atores, escopo e estilo arquitetural (E1)
  requisitos.md       RF, RNF, RN, casos de uso e drivers (E1)
  arquitetura.md      C4, camadas, modelo de domínio e cenários de qualidade (E2)
  adr/                uma decisão arquitetural por arquivo (E2)
  rastreabilidade.md  matriz requisito → decisão → componente → teste (E2)
ref/                  material do professor
src/                  código Spring Boot (a partir do E3)
```

## Como trabalhamos

1. Pegue uma issue sua. Cada uma diz o que fazer, em que branch e quando está pronta.
2. Crie a branch a partir da principal: `git checkout -b e2-nome-da-tarefa`.
3. Faça commits pequenos, com mensagem clara: `E2: adiciona alternativas ao ADR-004`.
4. Abra um Pull Request com `Closes #N` na descrição e peça revisão a quem revisa você (tabela acima).
5. Só entra na branch principal depois de revisado. No fim da entrega: `git tag E2 && git push --tags`.

Nunca suba senha, chave ou `target/`. Dúvidas sobre a tarefa ficam na própria issue.
