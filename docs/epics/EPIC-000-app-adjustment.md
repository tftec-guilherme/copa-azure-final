# EPIC-000 — App Adjustment for TFTEC Event

> **Owner:** Morgan (PM) · **Status:** ✅ **DONE** (validação visual aprovada pelo owner em 2026-05-07) · **Created:** 2026-05-07
> **Precedes:** [EPIC-001 (parked)](parked/EPIC-001-vm-to-webapp-modernization.md)
> **QA gate report:** [docs/qa/gates/2026-05-07-EPIC-000-qa-gate.md](../qa/gates/2026-05-07-EPIC-000-qa-gate.md)
> **Live:** https://fifa2026-web.azurewebsites.net

---

## Motivação

A aplicação **FIFA 2026 Tickets** vai ser usada no evento "Copa do Mundo Azure" da TFTEC. Antes de roteirizar a jornada didática para os alunos (EPIC-001, parked), precisamos **ajustar a aplicação** para que ela esteja pronta para o evento.

**Princípio de ambiente desta fase:** trabalhar em **Azure Web Apps (PaaS)**, porque o ciclo de build+deploy é minutos (vs ~30min de provisionar VM com IIS+iisnode). Iteração rápida é o que importa enquanto há ajustes pendentes. VMs ficam para a validação final / conteúdo do evento.

## Estado de partida

- ✅ Codebase canônica (frontend Lovable + backend fifa2026-api)
- ✅ Schema canônico extraído do `.bacpac`
- ✅ IaC pronto em `infra/` (Bicep + scripts az CLI)
- ✅ CI/CD pronto em `.github/workflows/`
- ✅ Build do frontend parametrizado por `BACKEND_URL`
- ❌ App **ainda não está deployada** em nenhum ambiente Azure
- ❌ Ajustes funcionais/conteúdo **ainda não foram aplicados**

## Estado-alvo

- ✅ App rodando 100% em Azure PaaS (`fifa2026-web` + `fifa2026-back` + Azure SQL)
- ✅ Todos os ajustes de conteúdo aplicados (definidos em stories filhas)
- ✅ Ciclo de build+deploy automatizado via GitHub Actions
- ✅ Smoke test passando ponta-a-ponta

Quando este epic fechar, **EPIC-001 (parked)** pode ser ativado para roteirizar o conteúdo do evento (jornada VM→PaaS para o aluno).

## Escopo (in scope)

- Provisionar Azure (Bicep) + importar bacpac + deployar app — **Story 0.1**
- Ajustes de **conteúdo** na aplicação (textos, dados de seed, labels) — **Stories 0.2+** (a definir com o usuário)
- Manter o layout pattern intacto (constraint inegociável)
- Manter o domínio (seleções, estádios, ingressos) inalterado

## Fora de escopo

- Refatoração arquitetural (já feita)
- Provisionar VMs (vai para EPIC-001 quando ativado)
- Funcionalidades novas (a app está funcionalmente completa)
- Mudança de design/layout

## Stories

| # | ID | Título | Status | Estimativa |
|---|---|---|---|---|
| S1 | 0.1 | [Deploy inicial em Azure PaaS](../stories/0.1.story.md) | ✅ Done | 30 min |
| S2 | 0.2 | [Remover todas as referências Lovable](../stories/0.2.story.md) | ✅ Done | 20 min |
| S3 | 0.3 | [Footer com disclaimer TFTEC](../stories/0.3.story.md) | ✅ Done | 15 min |
| S4 | 0.4 | [Admin Dashboard com dados reais](../stories/0.4.story.md) | ✅ Done | 45 min |
| S5 | 0.5 | [Polish — TD-3 + TD-5 + TD-6](../stories/0.5.story.md) | ✅ Done (CONCERNS) | 30 min |

> Total estimado: ~140 min. Stories 0.1-0.4 fecharam o escopo principal; Story 0.5 resolve tech debts do QA gate. Stories adicionais (0.6+) podem ser criadas se surgirem novos ajustes.

## Success criteria

| # | Critério | Verificação |
|---|---|---|
| SC-1 | App acessível publicamente em `https://fifa2026-web.azurewebsites.net` | Smoke test do navegador |
| SC-2 | Backend privado (não responde direto da Internet, exceto Azure outbound IPs) | `curl https://fifa2026-back.azurewebsites.net/api/health` ainda funciona, mas Access Restriction limita |
| SC-3 | Banco com dados reais do bacpac (16 seleções, 9 estádios, 12 jogos, 84 categorias) | `/api/admin/stats` |
| SC-4 | Todos os ajustes de conteúdo aprovados pelo usuário | Revisão visual + smoke test |

## Dependências

Nenhuma. Pode iniciar imediatamente — toda a infra/CI já foi preparada nas frentes anteriores.

## Stakeholders

- **Owner:** Raphael Andrade (rapha.rss@gmail.com)
- **Squad:** @pm (Morgan) → @sm (River) → @po (Pax) → @dev (Dex) → @qa (Quinn)
- **Final audience:** alunos TFTEC (mas só consome este epic via app já pronta)
