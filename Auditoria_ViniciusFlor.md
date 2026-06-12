# Auditoria DesignOps — Framework REACH

**Aluno:** Vinicius Flor
**Papel no grupo:** Tooling & Process Engineer (Seção 3 — Processos e Ferramentas)
**Projeto:** Operating System v0.1 — Firmware embarcado de drones Jacto
**Módulo:** CI/CD | Inteli T13 2026-1B | Grupo 05
**Repositório:** https://git.inteli.edu.br/graduacao/2026-1b/t13/g05

---

## Parte 1 — Triangulação de Eficiência e Habilidade (Efficiency & Ability)

### Processo auditado

O fluxo de trabalho definido na Seção 3 do `DESIGN_OPS.md` (minha responsabilidade direta): toda mudança relevante deveria nascer em uma **Issue** com critério de aceite, virar branch vinculada, passar por **Pull Request** com pipeline aprovado, e só então entrar em `develop`. A regra operacional era explícita: *"Nenhuma mudança relevante de interface deve seguir para merge sem issue vinculada, protótipo anexado, revisão técnica e evidência do pipeline."*

### Evidência: o processo falhou de forma mensurável

**Evidência 1 — 0 issues criadas em todo o projeto**
O tracker de issues do GitLab está vazio: `0 open · 0 closed · All 0`.
URL: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/issues
A cadeia `Issue → Protótipo → MR/PR → Build → Tag` nunca foi ativada. Não há uma única issue de UX, não há uma de engenharia, não há nenhuma. O plano inteiro pressupunha esse ponto de entrada — e ele não existe.

**Evidência 2 — MR !57: pipeline falhou, fechado sem diagnóstico**
`feat(ci): adiciona teste para verificação da pipeline #55` — criado por Karine Paixao, fechado por Anna Riciopo sem merge. O pipeline #30526 falhou no segundo stage (1 job com erro). Não há nenhum comentário de revisão, nenhum Problem Report aberto, nenhuma issue de raiz criada após a falha.
URL: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/57

**Evidência 3 — MRs !61, !62, !63, !64: quatro tentativas do mesmo fix, todas descartadas**
Todos intitulados `fix(ci): remove unnecessary file #1`, todos criados por Anna Riciopo na mesma semana, todos fechados sem merge. Apenas o quinto — MR !65 — foi mergeado.
URL base: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/61

**Evidência 4 — "Approval is optional" em todos os MRs**
O plano previa aprovação obrigatória com no mínimo um revisor antes do merge. Na prática, o GitLab mostra "Approval is optional" em todos os MRs inspecionados, incluindo o !57. Nenhuma branch protection com required approvals foi configurada.

### Análise crítica

O processo falhou nas duas dimensões do framework REACH simultaneamente.

**Ability comprometida:** a equipe não tinha domínio técnico suficiente para configurar o pipeline sem tentativa-e-erro. Quatro MRs idênticos fechados e um quinto finalmente mergeado é evidência direta de que o time estava operando no modo "rodar e ver o que quebra" — sem diagnóstico formal, sem log de causa raiz registrado, sem issue vinculando o problema à solução. Isso não é falha de processo: é lacuna de conhecimento técnico que o processo não conseguiu compensar.

**Efficiency comprometida:** criar issues era uma etapa a mais que ninguém priorizou. Com disponibilidade real de ~1 hora por dia por pessoa, o custo de abrir uma issue bem descrita (problema, critério de aceite, link do protótipo) representava uma fração não trivial do tempo disponível. O resultado foi que o time pulou direto para o branch e o MR, eliminando o ponto de rastreabilidade que o plano considerava não negociável.

O contorno adotado na prática: fechar o MR, criar outro com o mesmo nome, tentar de novo. Sem registro de o que foi tentado, sem aprendizado capturado, sem redução do tempo de diagnóstico nas tentativas seguintes. O processo não foi abandonado por má vontade — foi abandonado porque era caro demais para o contexto e ninguém o tinha operacionalizado antes do primeiro commit.

---

## Parte 2 — Clareza e Resultados (Clarity & Results)

### Decisão auditada

Na Sprint Review 4, o grupo recebeu feedback sobre a arquitetura do pipeline CI/CD. Os dois pontos levantados foram:

1. *"Ter pipelines por módulo"* — separar a esteira por unidade de validação em vez de um pipeline monolítico.
2. *"Ter outros pipelines menores e mais personalizáveis para conseguir e deixar o pipeline que a gente fez hoje como um dos pipes geralzão para testar tudo na hora de fazer a integração"* — o pipeline atual seria mantido como integração geral, com pipelines menores e especializados sendo criados em paralelo.

### Evidência

O feedback foi dado verbalmente durante a Sprint Review 4 e anotado pelo próprio grupo. Não existe issue aberta no GitLab descrevendo a mudança antes ou depois, não existe comentário no MR relacionado, não existe ata formal registrada em nenhum artefato do repositório.

### Análise crítica

Esse episódio ilustra com precisão a falha de **Clarity** descrita pelo framework REACH: decisões de arquitetura que deveriam ser baseadas em dados (métricas de cycle time por estágio, taxa de falha por job, tempo médio de diagnóstico) foram validadas a partir de percepção oral em uma reunião de sprint.

O DesignOps do grupo previu na Seção 7 métricas como *cycle time de design* (≤ 2 dias úteis), *lead time UX → deploy* (≤ 5 dias úteis) e extração de dados via API do GitLab com as labels `ux` e `problem-report`. Nenhuma dessas métricas foi coletada. As labels nunca foram criadas. Quando chegou a hora de justificar ou questionar a arquitetura do pipeline com a parceira, o time não tinha dados — tinha impressões.

O resultado (**Results** no REACH) é que a mudança de arquitetura foi aceita com base em parecer subjetivo de quem estava na sala, não em evidência de que a modularização reduziria o tempo de diagnóstico ou a taxa de falha. O DesignOps falhou em provar o valor da mudança porque nunca instrumentou o processo para gerar essa prova.

A armadilha da "opinião visual e subjetiva" não se aplica aqui apenas a decisões de interface — ela se aplica a decisões de engenharia de processo que o plano prometia medir objetivamente e não mediu.

---

## Parte 3 — Saúde do Time (Health)

### Etapa irrealista identificada

A Seção 2 do `DESIGN_OPS.md` definiu seis rituais de colaboração:

| Ritual | Duração prevista |
|---|---|
| Design Sync semanal | 30 min |
| Protótipo Review (sob demanda) | 20 min |
| Handoff assíncrono | contínuo |
| Revisão de MR/PR | 15–30 min |
| Sprint Review | 60 min |
| Sprint Retrospective com revisão da matriz de riscos | 45 min |

O próprio plano reconhecia o risco R-04: *"Processo pesado demais para a capacidade do grupo"*. A mitigação documentada era "manter apenas controles mínimos obrigatórios". Na prática, os controles mínimos também não foram mantidos.

### Evidência

**MR !8 — `docs: adiciona template de design ops`**
André Lobo criou a branch `docs/design-ops-template`, abriu o MR com descrição estruturada (contexto, o que foi feito, tabela de mudanças), e o MR foi fechado sem merge. Zero pipelines executados. Zero comentários de revisão. Zero aprovações.
URL: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/8

O artefato mais simbólico da falha de saúde do processo é esse: o template do próprio DesignOps nunca entrou na base. O instrumento de governança foi descartado antes de ser usado.

### Análise crítica

O plano foi elaborado com rigor acadêmico e lógica operacional consistente. O problema é que foi dimensionado para um time corporativo com alocação em tempo integral, não para seis estudantes em estágio com disponibilidade real de ~1 hora por dia.

Cada ritual adicionado ao plano era mais uma obrigação que o time teria que encaixar entre provas, entregas paralelas de outros módulos e o próprio trabalho de implementação. O Design Sync semanal de 30 minutos pressupõe que seis pessoas com agendas distintas consigam se reunir toda semana com pauta preparada. A matriz RACI com seis papéis distintos pressupõe que cada pessoa absorveu sua responsabilidade e age sobre ela autonomamente. A Sprint Retrospective com revisão obrigatória da matriz de riscos pressupõe que sobrou energia e tempo para reflexo de processo depois de entregar o conteúdo técnico.

Nada disso se sustentou. O que deveria ser cortado do plano para que o DesignOps fosse um facilitador e não burocracia punitiva:

1. **Design Sync semanal** — substituir por uma mensagem assíncrona de status no grupo (5 minutos, não 30).
2. **Matriz RACI de 6 papéis** — em um time de 6 pessoas com 1h/dia, um único papel de "responsável pelo MR" é suficiente e rastreável.
3. **Protótipo Review formal** — em um projeto de firmware embarcado sem telas de usuário final, esse ritual não tinha objeto de aplicação real.
4. **Coleta de métricas via API do GitLab** — exige configuração de script, labels padronizadas e alguém dedicando tempo de sprint para extração. Nunca aconteceu e não aconteceria.

O que deveria ter permanecido: a regra de vincular todo MR a uma descrição mínima do que foi feito e por quê. Isso custa 2 minutos e teria gerado o rastro que o processo inteiro prometia.

O DesignOps não falhou por falta de intenção. Falhou porque o plano foi escrito para provar maturidade conceitual em uma avaliação acadêmica, não para ser operado por pessoas reais com restrições reais.
