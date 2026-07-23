# Roadmap — Evolução para Copiloto Financeiro com IA

> Tradução do "super prompt" (nível Copilot Money + ChatGPT + Power BI) em um plano executável e honesto.
> Regra de ouro deste documento: **separar o que já está no protótipo, o que dá para fazer no protótipo estático, e o que exige backend/LLM/infra** — sem prometer o que a arquitetura atual não sustenta.

---

## 1. Onde estamos (entregue no protótipo)

> **Sem modo demonstração.** O protótipo opera **sempre com os dados do próprio usuário** (localStorage). Todas as telas de PF (Dashboard, Orçamento, Metas, Investimentos, Relatórios, Calendário) e o Copiloto abrem vazias, com convite para inserir os dados — sem números fictícios. Exceção: a tela **Empresa (MEI)** ainda é um exemplo ilustrativo (marcado como tal), pois não tem entrada de dados reais.

| Área do super prompt | Estado |
|---|---|
| Dashboard financeiro | ✅ **Só "Meus dados"** (localStorage) — sem demonstração |
| Copiloto financeiro (conversa) | ✅ **v0 determinístico** — responde em PT com os números do usuário |
| Score financeiro 0–100 | ✅ Central de Segurança + resposta do copiloto (neutro “—” sem dados) |
| Insights / IA proativa | ✅ Guardião (3 níveis) + insights do agente no dashboard |
| Orçamento por categoria | ✅ Tela com ritmo do mês e envelope estrito |
| Metas + simulador | ✅ Slider recalcula data de conclusão |
| Investimentos | ✅ Carteira, alocação, benchmark CDI |
| Empresa (PJ/MEI) | ⚠️ Exemplo ilustrativo (DRE, ponto de equilíbrio, radar) — sem entrada de dados reais ainda |
| Segurança | ✅ Passkey, MFA adaptativo, dispositivos, Open Finance |
| Comandos rápidos | ✅ Ctrl+K + FAB de lançamento |
| Lançamento em segundos | ✅ Sheet receita/despesa persistida |

## 2. O que é o Copiloto hoje (e a honestidade sobre "IA")

O copiloto **v0** é um **motor de regras determinístico**: interpreta a pergunta por palavras-chave e **calcula a resposta a partir dos dados reais** do usuário (saldo, receitas, despesas, categorias, metas). Ele responde corretamente a: quanto posso gastar hoje, quanto economizei, para onde vai meu dinheiro, se estou gastando muito numa categoria, saúde financeira, e simulações de meta ("juntar R$ X até mês Y").

**Por que determinístico, e não um LLM?** Porque este protótipo é um HTML estático, sem servidor. Um copiloto de linguagem natural de verdade (entender qualquer frase, manter contexto, explicar) precisa da **Claude API** rodando num backend — exatamente o princípio já definido na pesquisa: *as regras calculam os números (barato, auditável, instantâneo), o LLM redige a explicação em cima deles*. Nunca deixar o LLM "inventar" saldo. Ver `stack-recomendada.md` e `pesquisa-inteligencia-financeira.md`.

---

## 3. Fila priorizada

### Fase A — Aprofundar no protótipo (sem backend, alto valor)

1. ✅ **Copiloto v1**: intenções de gasto/economia/categoria/score/meta/financiamento; "criar meta a partir da resposta" em um toque. **Foco PF**: agora responde também **contas a pagar** (o que já foi pago × o que falta, com aviso de vencidas), **entrada de caixa** (renda por origem) e **compras do mês** (por categoria), com atalho para Relatórios.
2. ✅ **Ligar Orçamento/Metas/Investimentos/Relatórios ao "Meus dados"** — modo real global; só Empresa/MEI segue demo.
3. **Dashboard personalizável** (drag & drop de widgets, layout salvo no localStorage). *(pendente)*
4. ✅ **Calendário financeiro** (grade mensal com contas, salários e lançamentos; navegação de mês; clique no dia lista os eventos).
5. ✅ **Score financeiro dinâmico** (topbar calcula da taxa de poupança, reserva, contas em dia, investimentos e saldo). Falta: histórico plotável.
13. ✅ **Previsão de saldo até o fim do mês** (determinística): saldo hoje − contas pendentes − gasto estimado no ritmo atual; card no dashboard + intenção no copiloto, com alerta de risco de ficar negativo.
14. ✅ **Alertas de contas vencendo/vencidas** (até 3 dias) em banner no topo do dashboard, com atalho para as contas do mês.
15. ✅ **Comparação mês a mês por categoria** (▲/▼ % vs. mês anterior) nos gastos do dashboard.
6. **Gamificação** (níveis, sequências, missões) sobre os dados reais. *(pendente)*
12. ✅ **Acessibilidade** — alto contraste, texto grande (zoom) e cores para daltonismo, persistidos; foco por teclado já existente.
7. ✅ **Exportação real** de relatório PDF (print) e CSV a partir dos dados do usuário.
8. ✅ **PWA** (instalável, ícone, offline via service worker).
9. ✅ **Planejar** — simuladores de financiamento (parcela/juros/total) e FIRE (número + tempo).
10. ✅ **Contas a pagar** — cadastro, pagas × pendentes, pagar gera despesa e baixa saldo.
11. ✅ **Comandos "/"** no Ctrl+K (/despesa, /meta, /planejar, /relatorio, /exportar…).

### Fase B — Exige backend + IA (o salto de produto)

9. **Copiloto com Claude API**: linguagem natural livre, memória de conversa, explicações — as regras da Fase A viram as *ferramentas* que o modelo chama para pegar números reais.
10. **Login real + multiusuário** (passkeys/WebAuthn, OAuth Google/Apple/Microsoft) e **sincronização entre dispositivos**.
11. **Open Finance** (via agregador tipo Pluggy): importar contas, cartões e extratos automaticamente — fim do lançamento 100% manual.
12. **OCR + IA** de notas/boletos/comprovantes → lançamento automático (needs backend de visão).
13. **IA preditiva**: previsão de fluxo de caixa, alerta "risco de ficar negativo dia 26", detecção de recorrências e anomalias.
14. **BI (estilo Power BI)**: drill-down/through, segmentações, heatmaps, forecast — sobre um data warehouse próprio.
15. **Criptografia ponta a ponta, backup, detecção de fraude, logs de auditoria** — no servidor.

### Como avaliar cada item antes de construir (do próprio super prompt)

Resolve problema real? · Intuitivo sem aprendizado? · Em ≤ 3 interações? · Valor mensurável? · Mantém a interface limpa? · Escalável/manutenível? · **Usa IA para automatizar, não só exibir?**

---

## 4. Recomendação de sequência

**Agora → próximas rodadas (Fase A):** copiloto v1, ligar as demais telas ao "Meus dados", dashboard personalizável, calendário, PWA. Tudo isso cabe no protótipo estático e já entrega uma experiência de "copiloto" convincente e usável de verdade.

**Depois (Fase B):** quando fizer sentido virar produto real, montar o backend (Expo/React + NestJS + Postgres + Pluggy + Claude API, por `stack-recomendada.md`) e migrar os motores determinísticos para trás das ferramentas do LLM. O protótipo atual vira a referência visual e de UX — nada se perde.

> Nenhuma funcionalidade entra só porque existe no concorrente. Cada uma precisa passar pelos critérios do §3 e resolver uma dor real do usuário.
