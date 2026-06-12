# Plano de DesignOps - Grupo 05

Projeto: Operating System v0.1 para firmware embarcado de drones da Jacto Drones  
Módulo: CI/CD - Inteli T13 2026-1B  
Grupo: Kauã, Lucas, Vinicius, Anna, Karine e André

## Como Usar Este Documento

Este arquivo organiza a entrega individual e coletiva de DesignOps. As seções dos integrantes estão intencionalmente sinalizadas como `A COMPLETAR`, com instruções claras para que cada pessoa acrescente sua contribuição sem quebrar a visão do plano. Ao completar, atualizar o status para `Preenchido`.

| Integrante | Seção principal | Status |
| --- | --- | --- |
| Kauã | Introdução, objetivo e integração DesignOps com CI/CD | Preenchido |
| Lucas | Pessoas, papéis e rituais de colaboração | Preenchido |
| Vinicius | Processos e ferramentas de trabalho | Preenchido |
| Anna | Design system, tokens e handoff design-to-code | Preenchido |
| Karine | Métricas, impacto e dashboard de UX | Preenchido |
| André | Riscos, governança e critérios de operação | Preenchido |

## 1. Introdução e Objetivo

`DESENVOLVIDO POR KAUÃ`

O DesignOps deste projeto será tratado como a camada operacional que conecta decisões de design, validação de experiência do usuário e entrega contínua do firmware embarcado. A Nielsen Norman Group define DesignOps como a orquestração e otimização de pessoas, processos e prática de design para ampliar o valor e o impacto do design em escala [1]. No contexto do Grupo 05, isso significa aplicar ao design a mesma lógica que o projeto já propõe para DevOps: reduzir transições manuais, tornar o trabalho rastreável, criar evidências automáticas e acelerar o feedback antes que uma mudança chegue aos loops PIL e HIL.

O problema central da Jacto Drones é que o fluxo MIL -> SIL -> PIL -> HIL é tecnicamente consistente, mas operacionalmente fragmentado. A validação depende de intervenção humana, não há dashboard centralizado, os Problem Reports não ficam conectados automaticamente às versões testadas e pequenas alterações podem levar dias para receber feedback. O DesignOps entra para evitar que a interface, os protótipos e as decisões de UX sofram o mesmo problema: uma decisão de tela ou fluxo não pode existir apenas em uma conversa, em um arquivo isolado do Figma ou em um comentário sem vínculo com issue, commit, build e evidência de teste.

O objetivo operacional é criar um sistema de trabalho no qual cada alteração de UX siga uma cadeia rastreável:

```text
Necessidade de usuário -> Issue de UX -> Protótipo -> Revisão técnica -> Implementação -> Testes de acessibilidade/usabilidade -> Pipeline -> Evidência no dashboard -> Release
```

Essa cadeia será integrada ao ciclo proposto do projeto: commit vinculado a issue, pipeline MIL/SIL automático, resultado registrado no PR, falha gerando Problem Report e versão tagueada após correção. Assim, DesignOps funciona como o "DevOps do Design": não substitui o trabalho criativo, mas cria a infraestrutura para que o design flua com qualidade, previsibilidade e evidência até a entrega contínua.

Para este projeto, o DesignOps terá quatro metas práticas:

1. Garantir que requisitos de interface, decisões de UX e artefatos visuais tenham rastreabilidade bidirecional com issues, PRs, builds, tags e Problem Reports.
2. Reduzir retrabalho causado por handoff incompleto entre design, firmware embarcado, dashboard operacional e validações MIL/SIL/PIL/HIL.
3. Criar critérios objetivos de pronto para mudanças de interface, incluindo acessibilidade, consistência visual, evidência de teste e documentação.
4. Medir impacto de UX com indicadores operacionais, como cycle time de design, taxa de retrabalho, tempo de tarefa, taxa de erro de usuário e lead time de mudança de interface até deploy validado.

Essa abordagem também se alinha à ISO 9241-210, que recomenda práticas de design centrado no humano ao longo do ciclo de vida de sistemas interativos [2]. Como o produto envolve drones e firmware embarcado, a experiência do usuário não é apenas estética: clareza, feedback, prevenção de erro e rastreabilidade são elementos de segurança operacional.

Responsável: Kauã

## 2. Como Trabalhamos Juntos: Pessoas

`DESENVOLVIDO POR LUCAS`

A operação de DesignOps deste projeto é executada por seis estagiários do Inteli em um contexto curto, de aproximadamente 10 semanas, e com disponibilidade média próxima de 1 hora por dia por integrante. Esse cenário impede a adoção literal dos papéis corporativos descritos pela Nielsen Norman Group [1] e exige uma adaptação leve, em que cada pessoa acumula um papel funcional principal e contribui de forma compartilhada para os demais. O objetivo desta seção é deixar explícito quem decide o quê, em qual ritual a decisão é validada e onde ela fica registrada de forma rastreável, em coerência com a recomendação da ISO 9241-210 de tratar o design centrado no humano como atividade contínua dentro do ciclo de vida do sistema interativo [2].

### 2.1 Papéis funcionais

A divisão abaixo se apoia nas responsabilidades já assumidas pelos integrantes nas demais seções deste plano (4, 5, 7 e 8) e nos papéis recomendados pela Nielsen Norman Group para times pequenos de DesignOps [1].

| Papel | Integrante | Responsabilidade principal |
|---|---|---|
| CI/CD & DevOps Engineer | Kauã | Integração da esteira automatizada, gates do pipeline e métricas DORA aplicadas a UX |
| Process Owner / Facilitador | Lucas | Facilitação dos rituais, garantia de registro das decisões e respeito aos papéis |
| Tooling & Process Engineer | Vinicius | Configuração das ferramentas (GitLab/GitHub, Figma, labels, templates) e do fluxo entre elas |
| UX/UI Lead | Anna | Design system, tokens, componentes e handoff design-to-code |
| UX Researcher / Métricas | Karine | Coleta, análise e divulgação das métricas de usabilidade e DesignOps |
| QA, Risk & Governance Lead | André | Matriz de risco, Problem Reports, critérios de aprovação e governança das mudanças |

Cada papel acumula a função de revisor cruzado quando a mudança é interfacial: ninguém aprova o próprio MR/PR. Em ausências previsíveis (provas, recessos), o Process Owner reatribui a revisão para evitar que o fluxo pare.

### 2.2 Rituais de colaboração

Os rituais foram dimensionados ao tempo realista do projeto. A duração curta e a separação clara entre síncrono e assíncrono são deliberadas para preservar o R-04 da matriz de riscos da seção 8 (processo pesado demais para a capacidade do grupo).

| Ritual | Periodicidade | Duração | Objetivo | Participantes |
|---|---|---|---|---|
| Design Sync | Semanal | 30 min | Alinhar issues de UX em andamento, riscos e bloqueios | Todos |
| Protótipo Review | Sob demanda, antes do dev | 20 min | Aprovar protótipo antes de virar branch de implementação | UX/UI + 1 engenheiro |
| Handoff assíncrono | Contínuo | — | Entrega formal de protótipo via issue com checklist preenchido | Autor + revisor |
| Revisão de MR/PR | Por entrega | 15-30 min | Validar checklist, evidência de pipeline e aprovação técnica | Designer + engenheiro |
| Sprint Review | Por sprint | 60 min | Demonstração das mudanças com evidência registrada no dashboard | Todos + stakeholders |
| Sprint Retrospective | Por sprint | 45 min | Revisar matriz de risco da seção 8 e ajustar a operação de DesignOps | Todos |

A separação entre Design Sync (síncrono curto) e handoff assíncrono evita que o time precise estar online ao mesmo tempo, condição incompatível com a disponibilidade média do projeto.

### 2.3 Registro e rastreabilidade de decisões

Nenhuma decisão de UX vive apenas em conversa. Toda decisão precisa estar refletida em pelo menos dois pontos auditáveis:

1. **Issue de origem**, com problema, hipótese, link do protótipo, critério de aceite e responsável.
2. **MR/PR de implementação**, com o checklist da seção 4.4.1 preenchido, link para a issue e evidência do pipeline.

A trilha mínima exigida é: **Issue → Protótipo (Figma) → MR/PR → Build → Tag → Problem Report (se houver)**. Esse vínculo aproveita o suporte nativo do GitHub para conexão de PRs com issues por palavras-chave de fechamento e o Dev Mode do Figma, que permite aos engenheiros inspecionarem especificações sem licença de edição ([Figma Dev Mode](https://help.figma.com/hc/en-us/articles/15023124644247)). A revisão formal usa o mecanismo de aprovações de Merge Request do GitLab, que permite exigir um número mínimo de aprovadores e bloquear merge sem aprovação ([GitLab Merge Request Approvals](https://docs.gitlab.com/user/project/merge_requests/approvals/)).

### 2.4 Matriz RACI simplificada

Para evitar disputa de responsabilidade em mudanças que cruzam papéis, adota-se uma matriz RACI enxuta sobre as atividades centrais de DesignOps. A matriz é a referência usada em retrospectivas para verificar se algum papel foi sobrecarregado ou ignorado.

| Atividade | Kauã | Lucas | Vinicius | Anna | Karine | André |
|---|---|---|---|---|---|---|
| Abrir issue de UX | I | C | C | R | C | I |
| Aprovar protótipo | I | I | I | A | C | C |
| Implementar componente | C | I | C | R | I | I |
| Configurar pipeline de design | A | I | R | C | I | C |
| Revisar MR/PR de UI | C | I | C | R/A | I | C |
| Coletar métricas de UX | C | I | C | C | A/R | I |
| Triar Problem Report de UX | C | C | C | C | C | A/R |
| Auditar aderência ao processo | I | R | I | I | I | A |

Legenda: R = Responsável (executa), A = Aprovador (decide), C = Consultado, I = Informado.

### 2.5 Critério de sucesso desta seção

Esta seção será considerada operacionalmente válida se, ao longo do projeto, for possível verificar que: (1) toda mudança de interface relevante teve um responsável claramente identificado pela matriz RACI; (2) os rituais aconteceram com periodicidade compatível com o planejado, mesmo quando simplificados; e (3) nenhuma decisão de UX foi implementada sem registro em issue e MR/PR vinculados.

Responsável: Lucas

## 3. Como o Trabalho é Feito: Processos e Ferramentas

`DESENVOLVIDO POR VINICIUS`

O objetivo desta frente é garantir que o design não fique separado da automação de entrega. Para isso, o fluxo de UX deve seguir a mesma lógica do projeto: toda decisão relevante precisa nascer no backlog, virar protótipo rastreável, ser implementada em branch vinculada e gerar evidência no pipeline e no dashboard.

### 3.1 Ferramentas propostas

| Etapa | Ferramenta | Uso no processo |
| --- | --- | --- |
| Backlog | GitHub Issues / Projects | Registrar problema, critério de aceite e prioridade |
| Prototipação | Figma | Anexar fluxos, telas e decisões de UX |
| Implementação | Git + branches + Pull Requests | Vincular código à issue e revisar a mudança |
| Automação | GitHub Actions | Executar build, testes e validações automáticas |
| Modelagem | MATLAB/Simulink | Apoiar validações relacionadas aos loops MIL/SIL |
| Evidência | Dashboard operacional | Mostrar status, versão testada e histórico |
| Documentação | `DESIGN_OPS.md` e README | Registrar regras, links e decisões |

### 3.2 Fluxo mínimo

1. A issue de UX é criada no GitHub com descrição do problema, usuário impactado, critério de aceite e link do protótipo.
2. O protótipo é produzido no Figma e revisado tecnicamente antes do desenvolvimento.
3. A implementação acontece em branch vinculada à issue.
4. O Pull Request referencia a issue e inclui evidências da mudança.
5. O GitHub Actions executa build, testes de interface, acessibilidade e regressão visual.
6. O resultado do pipeline fica visível no PR e no dashboard operacional.

Esse fluxo reduz a chance de uma decisão de design existir apenas no Figma ou em conversa informal. No GitHub, PRs podem ser vinculados às issues e encerrá-las automaticamente quando a mudança é mergeada, o que ajuda a manter a rastreabilidade entre backlog e código ([GitHub Docs](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue)).

### 3.3 Labels sugeridas

- `ux`
- `design-review`
- `ready-for-dev`
- `accessibility`
- `visual-regression`
- `problem-report`

Essas labels ajudam a distinguir o que ainda está em revisão, o que já pode ser implementado e o que gerou falha rastreável.

### 3.4 Tratamento de falhas

Se o pipeline identificar falha de UX, acessibilidade ou regressão visual, a mudança não avança e deve gerar um Problem Report vinculado à issue, ao PR e à versão testada. O registro mínimo precisa indicar o erro, a evidência anexada, a SHA avaliada e o loop afetado. Esse encadeamento é compatível com a ISO/IEC/IEEE 29119-2, que enfatiza processos de teste com registro formal de execução e evidências ([ISO](https://www.iso.org/standard/79428.html)).

### 3.5 Regra operacional

Nenhuma mudança relevante de interface deve seguir para merge sem issue vinculada, protótipo anexado, revisão técnica e evidência do pipeline. Assim, o design deixa de ser um artefato isolado e passa a fazer parte da cadeia rastreável de CI/CD. O fluxo de automação adotado no GitHub Actions segue a lógica de pipelines compostos por etapas automatizadas de build, teste e validação ([GitHub Actions Docs](https://docs.github.com/en/actions)).

Responsável: Vinicius

## 4. Design System, Tokens e Handoff Design-to-Code

`DESENVOLVIDO POR ANNA`

### 4.1 Design Tokens

Tokens são os valores centralizados de cor, tipografia, espaçamento e estado que eliminam divergências entre design e código. São organizados em três camadas:

| Camada | Exemplo |
|---|---|
| **Primitivos** | `color.green.500 = #22C55E` |
| **Semânticos** | `color.status.success = color.green.500` |
| **Por componente** | `pipeline.stage.active.background = color.status.success` |

Tokens residem em `/design/tokens/tokens.json`, versionados semanticamente (`V.x.y.z`) e exportados automaticamente para CSS Custom Properties e SCSS via pipeline. Toda alteração gera entrada no `CHANGELOG.md` via Conventional Commits (`feat(tokens):`, `fix(tokens):`, `BREAKING CHANGE:`).

### 4.2 Componentes

Inventário mínimo para o dashboard operacional:

| Componente | Função |
|---|---|
| `PipelineStage` | Status visual de cada loop (MIL/SIL/PIL/HIL) |
| `PipelineTimeline` | Sequência MIL → SIL → PIL → HIL |
| `BuildBadge` | SHA, branch e tag do build |
| `CycleTimeChart` | Cycle time por etapa |
| `ProblemReportCard` | Problem Report com versão, loop e status de triagem |
| `TraceabilityMatrix` | Mapa Issue → Commit → Build → Tag → Loop |
| `FailureAlert` | Notificação de falha com link ao log (≤5 min) |

### 4.3 Fluxo de Handoff
### 4.1 Visão Geral

O Design System do projeto atua como camada de contrato entre as decisões de interface e a implementação no código embarcado e no dashboard operacional. Ele garante que componentes visuais — como indicadores de status de pipeline, painéis de métricas MIL/SIL/PIL/HIL e telas de Problem Reports — sejam desenvolvidos uma única vez, versionados, e reutilizados de forma rastreável ao longo da esteira CI/CD.

A ausência de um sistema estruturado de tokens e handoff representa um risco equivalente ao identificado na transição manual entre loops de validação: sem contrato explícito entre design e código, decisões visuais se perdem, há retrabalho de interface e a rastreabilidade "qual versão de componente está deployada em qual ambiente?" torna-se impossível de responder.

### 4.2 Design Tokens

Design tokens são as unidades atômicas do sistema: valores de cor, tipografia, espaçamento, elevação e estados de feedback que, ao serem centralizados e versionados, eliminam divergências entre o que o designer especifica e o que o engenheiro implementa.

#### 4.2.1 Estrutura de Tokens

Os tokens são organizados em três camadas hierárquicas:

| Camada | Descrição | Exemplo |
|---|---|---|
| **Primitivos** | Valores brutos sem semântica de uso | `color.green.500 = #22C55E` |
| **Semânticos** | Intenção de uso mapeada sobre primitivos | `color.status.success = color.green.500` |
| **Específicos de componente** | Aplicação restrita a um componente | `pipeline.stage.active.background = color.status.success` |

#### 4.2.2 Conjunto Mínimo de Tokens para o Dashboard Operacional

```json
{
  "color": {
    "status": {
      "success":   "#22C55E",
      "failure":   "#EF4444",
      "running":   "#3B82F6",
      "pending":   "#F59E0B",
      "skipped":   "#6B7280"
    },
    "loop": {
      "mil":  "#6366F1",
      "sil":  "#0EA5E9",
      "pil":  "#F97316",
      "hil":  "#10B981"
    },
    "surface": {
      "base":    "#0F172A",
      "raised":  "#1E293B",
      "overlay": "#334155"
    },
    "text": {
      "primary":   "#F8FAFC",
      "secondary": "#94A3B8",
      "disabled":  "#475569"
    }
  },
  "spacing": {
    "xs": "4px",
    "sm": "8px",
    "md": "16px",
    "lg": "24px",
    "xl": "40px"
  },
  "typography": {
    "mono": "'JetBrains Mono', monospace",
    "sans": "'IBM Plex Sans', sans-serif",
    "size": {
      "label": "11px",
      "body":  "14px",
      "title": "18px",
      "hero":  "28px"
    }
  },
  "radius": {
    "sm": "4px",
    "md": "8px",
    "lg": "12px"
  },
  "elevation": {
    "card":    "0 1px 3px rgba(0,0,0,0.4)",
    "modal":   "0 8px 32px rgba(0,0,0,0.6)",
    "tooltip": "0 2px 8px rgba(0,0,0,0.5)"
  }
}
```

Os tokens são mantidos no repositório em `/design/tokens/tokens.json` e exportados automaticamente via pipeline para os formatos consumidos pelo frontend (CSS Custom Properties, SCSS variables e, futuramente, constantes TypeScript).

#### 4.2.3 Versionamento de Tokens

Tokens seguem o mesmo esquema semântico do firmware: `V.x.y.z`.

- **x (major):** quebra de contrato — renomeação ou remoção de token existente.  
- **y (minor):** adição de novos tokens sem remoção.  
- **z (patch):** ajuste de valor sem alteração de nome ou semântica.

Toda alteração de token gera uma entrada no `CHANGELOG.md` via Conventional Commits (`feat(tokens):`, `fix(tokens):`, `BREAKING CHANGE:`), mantendo a mesma disciplina de rastreabilidade exigida no firmware (REQ-DEP-02).

### 4.3 Componentes e Inventário

O inventário de componentes é estruturado em três categorias, alinhadas às necessidades do dashboard operacional descrito nas seções 4.2 (TNF-08, TNF-09) e 5.3:

#### 4.3.1 Componentes de Status de Pipeline

| Componente | Função | Estados possíveis |
|---|---|---|
| `PipelineStage` | Representa um loop (MIL/SIL/PIL/HIL) | `pending`, `running`, `success`, `failure`, `skipped` |
| `BuildBadge` | Exibe SHA, branch e tag de um build | `stable`, `unstable`, `broken` |
| `PipelineTimeline` | Sequência visual MIL → SIL → PIL → HIL | — |
| `CycleTimeChart` | Gráfico de cycle time por etapa | — |

#### 4.3.2 Componentes de Rastreabilidade

| Componente | Função |
|---|---|
| `IssueLink` | Vincula commit/PR à Issue de origem (`#ID`) |
| `ProblemReportCard` | Exibe PR com versão que gerou a falha, loop e status de triagem |
| `TraceabilityMatrix` | Mapa Issue → Commit → Build → Tag → Loop → Problem Report |

#### 4.3.3 Componentes de Notificação

| Componente | Função | Gatilho |
|---|---|---|
| `FailureAlert` | Notifica falha de stage com link ao log | TNF-02: ≤5 min após evento |
| `RegressionBanner` | Alerta de regressão em `main` após merge | TNF-07 |
| `CredentialLeak` | Alerta de credencial detectada em log público | TNF-14 |

### 4.4 Handoff Design-to-Code

O handoff é o processo pelo qual especificações de design tornam-se implementação rastreável. O fluxo adotado elimina a etapa informal de "passar o Figma no Slack" e substitui por um ciclo auditável.

#### 4.4.1 Fluxo de Handoff

```markdown
Figma aprovado
→ Issue de UX no GitHub Projects (com link ao protótipo)
→ Branch: design/ISSUE-ID-componente
→ Implementação com tokens semânticos
→ PR com checklist preenchido
→ Review: designer (visual) + engenheiro (código)
→ Merge → Storybook atualizado via CI
```

**Checklist obrigatório no PR:**
- [ ] Apenas tokens semânticos (zero valores hardcoded)
- [ ] Testado em 1280, 1440 e 1920px
- [ ] Contraste ≥ 4.5:1 (WCAG AA) validado
- [ ] Issue de UX referenciada (`#ID`) e link ao Figma incluído
- [ ] Story no Storybook criada ou atualizada
- [ ] Problem Reports de UX pendentes tratados

### 4.4 CI/CD de Design

Stage adicional no GitHub Actions, acionado por mudanças em `/design/**` ou `/src/components/**`. Falha bloqueia merge do PR.

| Step | O que valida |
|---|---|
| `lint-tokens` | Zero valores hardcoded nos componentes |
| `storybook-build` | Build e publicação de preview por PR |
| `visual-regression` | Diff visual via Chromatic — aprovação obrigatória |

### 4.5 Definition of Done — Interface

Entrega concluída somente quando:

- CI passa (`lint-tokens`, `storybook-build`, `visual-regression`)
- Checklist de handoff aprovado no PR
- Issue de UX encerrada e vinculada ao merge
- Problem Reports de UX pendentes tratados
- WCAG AA validado (relatório anexado ao PR)

Responsável: Anna

## 5. Integração com a Esteira de CI/CD

`DESENVOLVIDO POR KAUÃ`

O DesignOps será integrado à esteira de CI/CD por meio de gates de qualidade antes que uma mudança de interface avance para validações mais caras ou dependentes de hardware. A documentação atual do projeto já identifica que a velocidade de feedback é o pior indicador do fluxo atual, com nota 1,5, porque o desenvolvedor pode esperar horas ou dias por validação. Portanto, a estratégia de DesignOps deve colocar verificações de UX e interface nas etapas mais baratas e automatizáveis da cadeia, principalmente antes de PIL/HIL.

No fluxo proposto, toda mudança de interface ou experiência deverá iniciar por uma issue com ID explícito. Essa issue deve conter o problema de usuário, hipótese de melhoria, link do protótipo, critério de aceite e impacto esperado na operação. O commit e o PR/MR devem referenciar a issue usando palavras-chave de fechamento ou vínculo, prática suportada por plataformas como GitHub para conectar pull requests e issues [11]. Quando o repositório estiver operando em GitLab, a mesma lógica deve ser mantida por merge requests, labels, aprovação e rastreabilidade por branch.

O pipeline deverá tratar o design como artefato operacional, não como material externo. Para isso, a esteira terá os seguintes pontos de automação:

| Etapa | O que será validado | Evidência esperada |
| --- | --- | --- |
| Issue de UX | Problema, usuário afetado, loop impactado e link do protótipo | Issue com template preenchido |
| Handoff | Protótipo marcado como pronto, tokens e comportamento documentados | Link do Figma/arquivo de design na issue |
| MR/PR | Vínculo com issue, checklist de UX e revisão técnica | Aprovação no MR/PR |
| CI inicial | Lint de estilo, build do dashboard, acessibilidade automatizada | Relatório anexado ao pipeline |
| Regressão visual | Comparação de telas críticas do dashboard | Relatório visual com status aprovado/reprovado |
| MIL/SIL | Validação automática sem bloquear por hardware | Resultado publicado no PR/MR e dashboard |
| PIL/HIL | Gate manual ou semiautomático com evidência rastreada | Registro de versão, hardware e Problem Reports |

A decisão de validar acessibilidade e regressão visual no pipeline deriva de duas fontes. Primeiro, o W3C organiza a WCAG em princípios de conteúdo perceptível, operável, compreensível e robusto, com critérios testáveis de conformidade [5]. Segundo, ferramentas como axe-core permitem automatizar parte das verificações de acessibilidade em interfaces web e HTML [6]. Essas automações não substituem teste com usuário real, mas reduzem falhas óbvias antes da Sprint Review e antes da validação em hardware.

Para regressão visual, o grupo poderá usar BackstopJS, Percy ou ferramenta equivalente. O critério operacional é simples: telas do dashboard que exibem status de pipeline, rastreabilidade de SC, Problem Reports e resultados MIL/SIL não podem mudar visualmente sem aprovação. Esse controle é importante porque o dashboard será o principal ponto de visibilidade do ciclo fechado requisitos -> testes -> resultados -> versão entregue.

O pipeline também deve refletir a estrutura de loops do projeto. Mudanças de interface do dashboard e de UX operacional devem ser verificadas em paralelo às validações MIL/SIL sempre que possível, porque GitLab CI/CD permite organizar pipelines por jobs e stages, com jobs paralelos dentro de um stage e progressão sequencial entre stages [10]. A etapa HIL continuará tendo restrições físicas por depender de hardware compartilhado, mas DesignOps evita que problemas de interface sejam descobertos apenas nesse ponto caro da cadeia.

Critérios mínimos para que uma mudança de UX avance na esteira:

- A issue de UX está vinculada ao MR/PR.
- O protótipo ou especificação visual está anexado.
- A mudança passou por revisão técnica de viabilidade.
- O build do dashboard ou componente de interface foi executado.
- A verificação de acessibilidade automatizada não encontrou falhas críticas.
- A regressão visual das telas críticas foi aprovada ou teve mudança justificada.
- O resultado do pipeline foi publicado no MR/PR e no dashboard.
- Se houver falha, um Problem Report foi aberto com vínculo à versão, commit e loop afetado.

Com isso, a rastreabilidade de design seguirá a mesma lógica de SCM já proposta para o firmware: Issue -> Commit -> MR/PR -> Build -> Tag/Release -> Deploy -> Problem Report -> Issue. A diferença é que o artefato rastreado não será apenas binário, modelo ou parâmetro embarcado, mas também decisão de interface, protótipo, token, componente e evidência de usabilidade.

Responsável: Kauã

## 6. Definition of Done de Design

`DESENVOLVIDO POR KAUÃ`

Para que uma entrega de UX/UI seja considerada pronta no projeto, ela deve atender simultaneamente critérios de design, engenharia e operação. Essa Definition of Done evita que uma interface seja considerada finalizada apenas porque "a tela ficou pronta" no protótipo ou porque "o código compilou". Em um sistema de drones, pronto significa que a decisão de design foi entendida, implementada, testada, rastreada e disponibilizada como evidência.

Checklist de pronto para mudanças de design:

- [ ] A issue descreve o problema de usuário, o contexto operacional e o loop impactado.
- [ ] O protótipo ou especificação está vinculado à issue.
- [ ] A decisão de design foi revisada por pelo menos uma pessoa de engenharia.
- [ ] Componentes, tokens ou assets alterados foram versionados no repositório.
- [ ] A interface usa linguagem consistente com o domínio do projeto: MIL, SIL, PIL, HIL, build, commit, Problem Report e release.
- [ ] Acessibilidade automatizada foi executada e falhas críticas foram corrigidas.
- [ ] O teste de regressão visual das telas críticas foi aprovado ou justificado.
- [ ] Métricas de UX relevantes foram registradas quando aplicável, como tempo de tarefa, taxa de erro ou sucesso de tarefa.
- [ ] O MR/PR contém evidência do pipeline e referência à issue.
- [ ] Problem Reports de UX relacionados foram triados ou encerrados.

Os critérios de linguagem, feedback e prevenção de erro são fundamentados nas heurísticas de usabilidade da Nielsen Norman Group, especialmente visibilidade do status do sistema, consistência, prevenção de erro e ajuda para reconhecer e recuperar erros [9]. Esses princípios se conectam diretamente ao dashboard operacional: uma interface que mostra status MIL/SIL/PIL/HIL precisa deixar claro o que está acontecendo, o que falhou, qual versão foi testada e qual ação o time deve executar.

Responsável: Kauã

## 7. Métricas, Impacto e Dashboard de UX

`DESENVOLVIDO POR KARINE`

A mensuração do DesignOps apresenta dificuldades conhecidas: falta de clareza sobre objetivos, equipes reduzidas e baixo suporte institucional para coleta de dados. Para contornar esses problemas no Grupo 05, este plano define métricas que conectam o desempenho do design à esteira de CI/CD, com foco inicial na experiência do time de engenharia ao utilizar o próprio sistema de validação.

### 7.1 Métricas de eficiência do DesignOps

| Métrica | Definição | Meta |
| --- | --- | --- |
| Cycle time de design | Tempo entre a abertura da issue de UX e a aprovação do protótipo. | ≤ 2 dias úteis |
| Lead time UX → deploy validado | Tempo entre a issue de UX e a entrega da mudança em versão testada (PIL/HIL). | ≤ 5 dias úteis |
| Taxa de retrabalho de interface | Percentual de MR/PR de UI que retornam para novos commits por falha em revisão. | < 15% |
| Problem Reports de UX por release | Número de reports abertos exclusivamente sobre problemas de interface ou experiência. | Decrescente |

Essas métricas são adaptadas dos indicadores de DesignOps propostos pelo Nielsen Norman Group e das métricas DORA para lead time e change failure rate [14][15].

### 7.2 Métricas de usabilidade da esteira

Uma vez que o dashboard ainda será construído como entregável do projeto, as métricas de usabilidade concentram-se na experiência do time de engenharia ao interagir com a esteira de CI/CD. A coleta ocorre por meio de observação direta e levantamentos periódicos com a equipe.

- **Sucesso de tarefa (%)**: percentual de engenheiros que conseguem, sem auxílio, identificar o status de uma validação (MIL/SIL/PIL/HIL) a partir dos artefatos disponíveis (logs, PR comments, issues).
- **Tempo de diagnóstico (segundos)**: tempo médio para localizar a causa de uma falha indicada no pipeline.
- **Taxa de erro operacional**: quantidade de ações incorretas por sessão (ex.: reexecutar pipeline desnecessariamente ou abrir Problem Report duplicado).
- **Satisfação subjetiva**: aplicação de questionário ao final de cada sprint sobre clareza do feedback fornecido pela esteira.

A coleta combina dados subjetivos e objetivos, conforme a abordagem *Value-Centered Design*, que recomenda a triangulação de múltiplas fontes de evidência. As práticas de mensuração seguem as recomendações de usabilidade do Nielsen Norman Group e a norma ISO 9241-210 para design centrado no ser humano [2][15].

### 7.3 Integração com métricas DORA

DesignOps e DevOps compartilham o objetivo de entrega contínua de valor. As métricas DORA são aplicadas também às mudanças de interface [14].

| Métrica DORA | Aplicação no DesignOps |
| --- | --- |
| Lead time for changes | Tempo entre o commit de uma mudança de interface (ex.: label de severidade) e sua presença no ambiente de homologação. |
| Change failure rate | Percentual de mudanças de UX que geram rollback ou Problem Report severo na validação PIL/HIL. |
| Time to recover (MTTR) | Tempo entre a abertura de um Problem Report de UX e o deploy da correção validada em staging. |

A extração ocorre via API do GitLab, utilizando as labels `ux` e `problem-report`.

### 7.4 Dashboard de UX

O dashboard operacional, previsto como entregável do projeto, incluirá um painel específico de DesignOps com quatro blocos:

1. **Fila de design**: issues de UX abertas por prioridade e cycle time por issue, com alerta quando a meta é excedida.
2. **Qualidade da release**: taxa de retrabalho de interface e Problem Reports de UX abertos/fechados.
3. **Rastreabilidade**: percentual de MR/PR de UI com link válido para issue e protótipo; commits de UI sem issue associada.
4. **Usabilidade da esteira**: evolução do tempo médio de diagnóstico e registro dos erros operacionais mais frequentes.

Cada elemento do painel deve permitir responder à pergunta de governança: *"Qual decisão de design está em qual versão e quais testes a validaram?"*. A estrutura do dashboard foi adaptada de práticas documentadas em DesignOps [13].

### 7.5 Plano de coleta e responsabilidades

| Métrica | Frequência | Responsável | Ferramenta |
| --- | --- | --- | --- |
| Cycle time / lead time de UX | Por sprint | Karine (UX) | GitLab Issues + API |
| Taxa de retrabalho de interface | Por sprint | Vinicius (Processos) | GitLab MR labels + CI status |
| Problem Reports de UX | Contínuo | André (Governança) | GitLab Issues |
| Sucesso / tempo de diagnóstico | Por sprint | Anna (Design) | Observação + planilha |
| Métricas DORA (UX) | Por release | Kauã (DevOps) | GitLab CI/CD |

**Responsável: Karine**

## 8. Riscos, Governança e Operação

`DESENVOLVIDO POR ANDRÉ`

A governança de DesignOps deste projeto precisa partir de uma limitação concreta do contexto: o produto será desenvolvido em apenas 10 semanas, todos os integrantes do grupo estão estagiando e a disponibilidade média real de desenvolvimento é de aproximadamente 1 hora por dia por pessoa. Nesse cenário, o principal risco não é tecnológico, mas operacional: o time pode priorizar apenas colocar a interface para funcionar e ir corrigindo em cima de feedback de professor, colega ou parceiro, sem prototipação suficiente, sem validação prévia, sem acompanhamento contínuo e sem métricas mínimas de UX. Isso enfraquece justamente a proposta de tratar DesignOps como o DevOps do Design.

Esse risco é coerente com o diagnóstico já presente no TAPI: ferramentas pouco integradas, rituais não padronizados, atividades manuais, ausência de pipeline consistente e baixa visibilidade do fluxo. Em um projeto curto, esse contexto favorece atalhos como implementar primeiro e estruturar depois, o que pode até acelerar uma entrega pontual, mas compromete rastreabilidade, consistência visual e capacidade de aprender com evidência. A Nielsen Norman Group define DesignOps como a disciplina que organiza pessoas, processos e artefatos para que o design produza valor de forma repetível e escalável. Se essa disciplina for negligenciada, o grupo tende a cair em decisões ad hoc, handoff incompleto e retrabalho recorrente ([1](https://www.nngroup.com/articles/design-operations-101/)).

Para evitar isso, a operação de DesignOps será governada por três regras simples:

1. Nenhuma mudança relevante de interface deve avançar sem vínculo com issue, responsável e evidência mínima de decisão.
2. Toda mudança visual que impacte dashboard, fluxo de usuário, componente compartilhado ou critério de acessibilidade deve ser revisada antes do merge.
3. A tabela de riscos desta seção deve ser revisitada em toda Sprint Retrospective, para reavaliar probabilidade, impacto, sinais de alerta e responsáveis por acompanhamento.

### 8.1 Matriz de riscos de DesignOps

| ID | Risco | Causa principal | Impacto no projeto | Sinal de alerta | Mitigação operacional | Responsável por acompanhar |
|---|---|---|---|---|---|---|
| R-01 | Negligência do processo de DesignOps por falta de tempo | Projeto de 10 semanas, estágio de todos os integrantes e baixa disponibilidade diária | Interface implementada sem protótipo, sem validação, sem métricas e com rastreio mínimo | Mudanças entram direto no código sem issue de UX, sem protótipo e sem checklist | Exigir issue, evidência mínima e checklist antes do merge; revisar esse risco toda retrospectiva | André |
| R-02 | Handoff e rastreabilidade incompletos | Atualização tardia de protótipo, links soltos e vínculo fraco entre issue, MR/PR, build e release | Retrabalho, perda de contexto e dificuldade para saber o que foi aprovado e entregue | PR com interface alterada sem protótipo atualizado, sem evidência ou sem vínculo com release | Tornar obrigatório o link do protótipo, checklist mínimo e trilha Issue -> MR/PR -> Build -> Release -> Problem Report | Responsável pela entrega da interface |
| R-03 | Mudança visual com qualidade insuficiente | Revisão fraca de UX e engenharia antes do merge | Interface ambígua, acessibilidade ruim, inconsistência visual e correções tardias | Labels confusas, contraste inadequado, feedback visual pouco claro ou revisão técnica tardia | Exigir revisão cruzada, checklist de acessibilidade e padrão mínimo de feedback visual antes do merge | Responsável pela revisão de UX |
| R-04 | Processo pesado demais para a capacidade do grupo | Excesso de etapas, documentos e validações para pouco tempo disponível | Abandono do processo, preenchimento superficial e uso simbólico do template | Checklist sempre incompleto ou ignorado | Manter apenas controles mínimos obrigatórios e simplificar o restante | André |
| R-05 | Governança frouxa sobre mudanças críticas de interface | Ausência de rotina clara para revisar riscos e mudanças | Decisões importantes entram sem acompanhamento e o processo se deteriora ao longo das sprints | Retrospectivas sem revisão da tabela, responsáveis indefinidos ou regras ignoradas | Revisitar a matriz em toda Sprint Retrospective e registrar ação corretiva quando um risco se materializar | André |

### 8.2 Política de governança

A governança desta frente será baseada em controle leve, mas obrigatório. O grupo não precisa simular uma operação corporativa completa, porém precisa definir um mínimo operacional que sobreviva à rotina real do semestre. Assim, ficam definidos os seguintes hábitos:

- Toda mudança de interface relevante deve nascer em uma issue, mesmo que curta.
- Todo MR/PR com impacto visual deve indicar responsável, evidência e status da revisão.
- Componentes ou telas críticas do dashboard não devem ser alterados sem revisão cruzada entre quem desenhou e quem implementou.
- Toda Sprint Retrospective deve incluir uma passada explícita pela matriz de riscos desta seção.
- Se um risco se concretizar, o grupo deve registrar uma ação corretiva objetiva para a sprint seguinte.

Esse modelo se aproxima da lógica de gestão baseada em risco descrita na ISO/IEC/IEEE 29119-2 para processos de teste, em que o esforço de validação deve ser direcionado aos pontos mais críticos do sistema. Também conversa com práticas de branch protection e aprovação formal em merge requests, usadas para impedir que mudanças cheguem ao fluxo principal sem revisão adequada. Fontes úteis para essa governança incluem GitLab Merge Request Approvals, GitLab Protected Branches e a própria Nielsen Norman Group ao tratar DesignOps como estrutura de sustentação do design ([1](https://www.nngroup.com/articles/design-operations-101/)), ([3](https://docs.gitlab.com/user/project/merge_requests/approvals/)) e ([4](https://docs.gitlab.com/user/project/repository/branches/protected/)).

### 8.3 Operação ao longo das retrospectivas

A retrospectiva não será usada apenas para discutir produtividade geral, mas também para verificar se o processo de DesignOps está sendo seguido ou abandonado. Em toda Sprint Retrospective, o grupo deve responder pelo menos estas perguntas:

1. Alguma interface foi implementada sem protótipo ou sem issue vinculada?
2. Houve mudança visual relevante que entrou sem revisão adequada?
3. Algum componente do dashboard ficou ambíguo ou gerou retrabalho por falta de padrão?
4. Os riscos da matriz aumentaram, diminuíram ou mudaram de responsável?
5. Existe alguma regra atual de DesignOps que o grupo não consegue cumprir e precisa simplificar?

O objetivo dessa revisão recorrente não é burocratizar a sprint, mas impedir que o plano vire documento morto. Se DesignOps não for revisitado de forma deliberada, a tendência mais provável em um projeto curto é sua erosão progressiva.

### 8.4 Critério operacional de sucesso desta seção

Esta seção será considerada operacionalmente válida se, ao final do projeto, o grupo conseguir demonstrar quatro coisas:

- quais riscos foram acompanhados durante as sprints;
- quais riscos realmente se materializaram;
- quais controles mínimos foram mantidos mesmo com restrição de tempo;
- como a retrospectiva ajudou a ajustar a operação de DesignOps ao longo do projeto.

Responsável: André

## 9. Referências

`REFERENCIAS PARTE KAUÃ`

[1] Nielsen Norman Group. "DesignOps 101". Disponível em: https://www.nngroup.com/articles/design-operations-101/  
[2] ISO. "ISO 9241-210:2019 - Human-centred design for interactive systems". Disponível em: https://www.iso.org/standard/77520.html  
[3] Nielsen Norman Group. "Design Systems 101". Disponível em: https://www.nngroup.com/articles/design-systems-101/  
[4] Design Tokens Community Group / W3C. "Design Tokens Format Module 2025.10". Disponível em: https://www.designtokens.org/TR/2025.10/format/  
[5] W3C WAI. "WCAG 2 Overview". Disponível em: https://www.w3.org/WAI/standards-guidelines/wcag/  
[6] Deque. "axe-core: Accessibility engine for automated Web UI testing". Disponível em: https://github.com/dequelabs/axe-core  
[7] Figma Developers. "Variables REST API". Disponível em: https://developers.figma.com/docs/rest-api/variables/  
[8] Nielsen Norman Group. "10 Usability Heuristics for User Interface Design". Disponível em: https://www.nngroup.com/articles/ten-usability-heuristics/  
[9] DORA. "DORA's software delivery performance metrics". Disponível em: https://dora.dev/guides/dora-metrics/  
[10] Nielsen Norman Group. "Usability Metrics". Disponível em: https://www.nngroup.com/articles/usability-metrics/  
[11] ISO. "ISO/IEC/IEEE 29119-2:2021 - Software testing - Test processes". Disponível em: https://www.iso.org/standard/79428.html  
[12] ISO. "ISO/IEC 10746-3:2009 - Open distributed processing - Reference model: Architecture". Disponível em: https://www.iso.org/standard/55724.html
