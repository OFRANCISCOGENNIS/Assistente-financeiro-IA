# Pesquisa — Inteligência Financeira Pessoal e Empresarial

> Pesquisa multi-fonte para fundamentar o que implementar no sistema (features, indicadores com fórmulas e gatilhos do Guardião), separando evidência científica de prática de mercado.
>
> **Método e status de verificação:** 5 frentes de busca paralelas → 15 fontes lidas → 43 alegações extraídas, 25 selecionadas para verificação adversarial. A etapa de verificação em painel falhou por limite de infraestrutura (não por refutação) — portanto as alegações abaixo estão marcadas como **extraídas de fonte primária, sem contra-checagem independente**. Nenhuma foi refutada. Grau de evidência: **[A]** experimento controlado/meta-análise revisados por pares · **[B]** framework institucional (regulador, Sebrae, CFPB) · **[C]** fonte de mercado/marketing (usar como benchmark, não como verdade).

---

## PARTE 1 — Finanças Pessoais

### 1.1 Frameworks de score de saúde financeira (o que calibrar no nosso)

**CFPB Financial Well-Being Scale (EUA)** [B]
- Define bem-estar financeiro em 4 elementos: controle das finanças do dia a dia; capacidade de absorver um choque; estar no rumo das metas; liberdade de escolha. ([CFPB Quick Guide](https://files.consumerfinance.gov/f/documents/201701_cfpb_FinancialWell-Being_Quick-Guide.pdf))
- Instrumento de 10 perguntas com **versão curta de 5 perguntas cujos escores são comparáveis à versão completa** — dá para embutir no onboarding sem atrito.
- Converte para **escore 0–100** — a mesma régua que já usamos, permitindo calibração direta.
- **Existe versão validada em português brasileiro** (tradução formal + back-translation + equivalência semântica), com alta consistência interna (α de Cronbach = 0,89) e estrutura unidimensional confirmada por análise fatorial — um escore único composto é psicometricamente defensável. ([PMC8317546](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8317546/))

**I-SFB — Índice de Saúde Financeira do Brasileiro (BCB/Febraban)** [B]
- Framework institucional **brasileiro**, derivado da escala CFPB e do FinHealth Score: 15 perguntas, escore 0–100, faixas de classificação (0–36 muito baixo até 83–100 excelente) e **dados populacionais brasileiros** — a referência ideal para dizer ao usuário como ele se compara com a população. ([Relatório 2024](https://pefmbddiag.blob.core.windows.net/cdn/downloads/A_Saude_Financeira_do_Brasileiro_2024.pdf))

**Financial Health Network (CFSI) — 8 indicadores objetivos** [B]
4 pilares (Spend, Save, Borrow, Plan) em 8 indicadores: gastar menos que a renda; pagar contas em dia e integralmente; reserva líquida suficiente; poupança/ativos de longo prazo; carga de dívida sustentável; score de crédito saudável; seguros adequados; planejar despesas futuras. ([CFSI Metrics PDF](https://s3.amazonaws.com/cfsi-innovation-files-2018/wp-content/uploads/2016/05/09212818/Consumer-FinHealth-Metrics-FINAL_May.pdf))
Limiares prontos para virarem cortes do nosso score e gatilhos:

| Indicador | Verde | Amarelo | Vermelho |
|---|---|---|---|
| Reserva de emergência | ≥ 6 meses de despesas | 1–5 meses | < 1 mês |
| Comprometimento de renda (DTI = dívida recorrente mensal ÷ renda bruta mensal) | < 36% | 36–43% | > 43% |

**Score híbrido é defensável cientificamente** [A] — estudo na *Financial Planning Review* construiu 4 categorias ordenadas a partir de **razões financeiras objetivas** (distressed → fragile → stable → flourishing) e mostrou consistência empírica com a escala subjetiva do CFPB: escores CFPB mais altos acompanham categorias objetivas mais altas. Ou seja: um score calculado de dados transacionais (Open Finance) aproxima bem o bem-estar percebido — e o ideal é combinar os dois. ([Wiley cfp2.1194](https://onlinelibrary.wiley.com/doi/full/10.1002/cfp2.1194))

### 1.2 Metodologias de orçamento — o que a evidência diz

| Metodologia | Evidência | Veredito para o produto |
|---|---|---|
| **Pay-yourself-first / poupança automática** | [A] Save More Tomorrow (Thaler & Benartzi, *JPE* 2004): pré-compromisso + automação elevou taxa média de poupança de 3,5% para 13,6% em 40 meses, com adesão de 78%. Avaliado independentemente pelo clearinghouse CLEAR (Dept. of Labor). | **A mais forte de todas.** Implementar aporte automático no dia do salário + oferta de aumentar o % quando a renda subir. |
| **Envelope / orçamento por categoria** | [A] Mental budgeting (particionar dinheiro em categorias e monitorar contra elas) é associado positiva e significativamente ao bem-estar financeiro (PLOS One 2023, PLS-SEM: literacia, mental budgeting e autocontrole, todos significativos). Regra do envelope: quando esgota, o gasto na categoria para até o próximo mês — traduzível em trava/alerta por categoria (UPenn). | Já temos orçamento por categoria; adicionar **modo envelope estrito opcional** (hard stop + alerta nível 3 ao estourar). |
| **Zero-based (YNAB)** | [B] Definição institucional: todo real recebe uma função até sobrar zero; alegação de que previne compra por impulso é **sem evidência citada** (UPenn). | Oferecer como modo avançado; não prometer efeitos não comprovados. |
| **50/30/20** | [B] Definição: 50% necessidades / 30% desejos / 20% poupança sobre a renda **líquida** (UPenn). **Popularidade de praticantes, sem evidência de eficácia revisada por pares comparável** — um achado em si. | Usar como *template inicial amigável* de orçamento, não como recomendação "cientificamente comprovada". |

### 1.3 Finanças comportamentais aplicadas a alertas (validação direta do Guardião)

- **Alertas just-in-time antes do evento funcionam** [A]: experimento de campo do regulador britânico (FCA OP40; versão peer-reviewed: Grubb et al., *Journal of Finance* 2025) com 1M+ clientes — SMS antes de entrar no cheque especial reduziu tarifas em **21–25%**, corrigindo desatenção ao saldo. Virou alerta obrigatório por regulação (CMA 2018/FCA 2019). É a validação mais direta do nosso alerta preditivo pré-evento (níveis 1–2).
- **Lembretes atrelados a metas** [A]: RCTs em 3 países (Karlan et al., *Management Science*) — lembretes simples aumentaram poupança tanto quanto produtos de compromisso; mensagens citando **a meta + um incentivo** funcionaram melhor; enquadramento de perda vs. ganho foi **equivalente**. → Nosso microcopy deve citar a meta pelo nome ("2 meses da meta Chile"), como já fazemos.
- **Alertas direcionados por IA** [A]: estudo em *Management Science* sobre alertas preditivos personalizados (prever quando o usuário vai incorrer em tarifa e avisar antes) reduziu mensuravelmente tarifas de desatenção — o análogo acadêmico mais próximo da nossa IA guardiã: **preditivo, personalizado e com timing**, não notificação genérica.
- **⚠️ Efeito "licença para gastar"** [B]: mostrar "ainda restam R$X no orçamento" perto do fim do período pode **aumentar** o gasto (Behavioral Economics Institute). → Regra de microcopy: enquadrar por acumulado e meta ("você já usou 85%"), nunca por sobra ("ainda tem R$120") no fim do ciclo.
- **Educação financeira funciona, com nuance** [A]: meta-análise de 76 RCTs, 160 mil+ participantes, 33 países (Kaiser, Lusardi et al., *JFE* 2022): efeitos causais positivos e economicamente relevantes sobre conhecimento e comportamento, mais fortes em orçamento, poupança e crédito. Mas [A] RCT austríaco (Journal of Behavioral & Experimental Economics): app de orçamento **aumentou o monitoramento** (checar saldo com mais frequência), sem aumentar conhecimento; exercício web isolado não teve efeito algum. → **Apps mudam comportamento de monitoramento, não literacia por osmose**: nossa trilha de educação deve ser estruturada (estilo curso), e o app deve apostar nos alertas como motor principal de mudança.

---

## PARTE 2 — Empresarial (PME e MEI no Brasil)

### 2.1 Por que o módulo PJ importa (dados Sebrae) [B]

- Mortalidade em até 5 anos por porte: **MEI 29%, microempresa 21,6%, EPP 17%** — quanto menos estruturado, maior a mortalidade (Pesquisa Sobrevivência de Empresas, Sebrae/DataSebrae, base Receita Federal; ~57,7% dos MEI seguem ativos após 5 anos).
- Entre empresas encerradas, **22% apontaram falta de capital de giro como causa primordial** (41% citaram a pandemia no ciclo pesquisado) — mortalidade diretamente ligada à gestão de caixa.
- Sebrae/PB: falta de planejamento e má gestão financeira são as principais causas de mortalidade, atingindo MEIs com mais força. **Erro nº 1: misturar dinheiro PF com PJ** — impede saber o que o negócio realmente gera. Outros erros recorrentes: confundir faturamento com lucro, não controlar fluxo de caixa, precificar errado, operar sem capital de giro.

### 2.2 Indicadores PJ com fórmulas (prontos para o dashboard empresarial)

Fórmulas recomendadas pelo Sebrae [B]:

- **Margem de contribuição** = vendas − (custos variáveis + despesas variáveis)
- **Ponto de equilíbrio** = (custos fixos + despesas fixas) ÷ margem de contribuição (em %)

Indicadores contábeis padrão para completar o conjunto (prática consolidada):

- **Capital de giro líquido** = ativo circulante − passivo circulante
- **PMR / PMP** (prazo médio de recebimento/pagamento) = (contas a receber ÷ vendas) × dias · (fornecedores ÷ compras) × dias
- **Ciclo de caixa** = PME (estoque) + PMR − PMP → se positivo e crescendo, o caixa está financiando clientes
- **CMV** = estoque inicial + compras − estoque final
- **DRE simplificada**: receita − deduções − CMV = lucro bruto − despesas operacionais = resultado operacional − pró-labore/retiradas = resultado líquido
- **Liquidez corrente** = ativo circulante ÷ passivo circulante
- **Pró-labore fixo** como mecanismo prático de separação PF/PJ

---

## PARTE 3 — Benchmarks de produto [C]

| Produto | Metodologia/feature central | Lição para nós |
|---|---|---|
| **YNAB** | Zero-based manual ("give every dollar a job") — exige mudança ativa de hábito | Modo avançado opcional; alto atrito, alta fidelidade |
| **PocketGuard** | **"In My Pocket" (safe-to-spend)** calculado automaticamente: quanto sobra após contas, metas e recorrências | **Lacuna nossa** — indicador de "disponível de verdade" no dashboard (com o cuidado do efeito licença-para-gastar no fim do ciclo) |
| **Monarch** | "Flex budgeting" automation-first + dashboard consolidado de patrimônio | Confirma nossa direção de dashboard 360° + score |
| **Goodbudget** | Envelope digital | Confirma o modo envelope estrito como demanda real |
| **Mobills / Organizze** (BR, PF) | Importação via Open Finance, multi-cartões, alertas de contas; usados também por autônomos/MEI | Público autônomo/MEI transita entre PF e PJ no mesmo app — nossa oportunidade de fazer isso de forma estruturada |
| **Conta Azul / Nibo / Omie** (BR, PJ) | DRE "em poucos cliques", conciliação bancária, fluxo de aprovação de pagamentos, **conexão com o contador** | O acesso do contador que já temos vira ponte natural para o modo PJ; DRE automática é o benchmark de mercado |

---

## PARTE 4 — O que implementar (priorizado)

### P0 — Recalibrar o que já existe (baixo esforço, alta fundamentação)

1. **Adotar os limiares CFSI no health score** (`health-score.md`): reserva verde ≥ 6 meses / amarela 1–5 / vermelha < 1; DTI < 36% / 36–43% / > 43% no componente endividamento. Fórmula DTI explícita: dívida recorrente mensal ÷ renda bruta.
2. **Regra anti-"licença para gastar" no microcopy** (`microcopy-guardiao.md`): no último terço do ciclo, alertas F1/F2 sempre por acumulado ("você já usou X%"), nunca por sobra.
3. **Citar a meta pelo nome nos lembretes** (já fazemos — formalizar como regra 8 da biblioteca, com base em Karlan).

### P1 — Novas features PF

4. **Quiz de bem-estar (5 perguntas, CFPB-BR validado)** no onboarding: camada subjetiva do score híbrido (objetivo via Open Finance + percebido via escala validada — arquitetura defendida pelo estudo da Wiley), com comparação às faixas do I-SFB brasileiro.
5. **"Disponível de verdade" (safe-to-spend)** no dashboard: saldo − contas do ciclo − aportes de metas − recorrências.
6. **Aporte automático pay-yourself-first**: agendado no dia do salário + oferta de escalar o % quando a renda subir (mecânica Save More Tomorrow, a de evidência mais forte).
7. **Modo envelope estrito opcional** por categoria: hard stop com alerta nível 3 consultivo ("esta compra fura o envelope de Lazer — confirmar mesmo assim?").
8. **Templates de orçamento** com rótulo honesto: 50/30/20 como ponto de partida amigável (sem alegar comprovação científica), zero-based como modo avançado.

### P2 — Módulo PJ/MEI (novo terceiro radar do Guardião)

9. **Perfil PJ/MEI** com separação PF/PJ estruturada (contas marcadas, pró-labore fixo, transferências PF↔PJ rastreadas).
10. **Dashboard empresarial**: fluxo de caixa projetado, DRE simplificada automática (benchmark Conta Azul), margem de contribuição, ponto de equilíbrio, ciclo de caixa, capital de giro.
11. **Radar Empresarial do Guardião** — gatilhos fundamentados nos erros fatais do Sebrae:

| Gatilho | Base | Exemplo de alerta (nível 2) |
|---|---|---|
| E1 — Mistura PF/PJ | Erro nº 1 Sebrae | "Você pagou 4 despesas pessoais com a conta da empresa este mês. Sem separação, você não sabe o que o negócio gera — o erro mais associado a fechamento de empresas." |
| E2 — Capital de giro crítico | 22% das mortes de PME | "Seu capital de giro cobre 11 dias de operação. Empresas fecham mais por falta de giro do que por falta de lucro." |
| E3 — Faturamento ≠ lucro | Erro Sebrae | "Faturou R$ 18.400, mas após custos e pró-labore sobraram R$ 1.230 (margem 6,7%). Cuidado ao assumir compromissos sobre o faturamento." |
| E4 — Ciclo de caixa esticando | PMR−PMP | "Seus clientes pagam em 42 dias e você paga fornecedores em 18 — seu caixa financia 24 dias de venda." |
| E5 — Preço abaixo do equilíbrio | Fórmula Sebrae | "No preço atual, você precisa vender 340 unidades/mês para empatar; vendeu 210. Revisar precificação." |
| E6 — Pró-labore não definido | Separação PF/PJ | "Você retirou 5 valores diferentes da empresa este mês. Definir um pró-labore fixo protege o caixa e o seu planejamento pessoal." |

12. **Educação financeira estruturada** (trilhas estilo curso, com base na meta-análise de Kaiser/Lusardi), focada nos temas de maior efeito: orçamento, poupança e crédito — sem prometer que "usar o app ensina sozinho".

---

## Fontes

| # | Fonte | Tipo | Qualidade |
|---|---|---|---|
| 1 | [CFPB — Financial Well-Being Scale Quick Guide](https://files.consumerfinance.gov/f/documents/201701_cfpb_FinancialWell-Being_Quick-Guide.pdf) | Institucional | Primária |
| 2 | [CFSI/Financial Health Network — Eight Ways to Measure Financial Health](https://s3.amazonaws.com/cfsi-innovation-files-2018/wp-content/uploads/2016/05/09212818/Consumer-FinHealth-Metrics-FINAL_May.pdf) | Institucional | Primária |
| 3 | [I-SFB — A Saúde Financeira do Brasileiro 2024 (BCB/Febraban)](https://pefmbddiag.blob.core.windows.net/cdn/downloads/A_Saude_Financeira_do_Brasileiro_2024.pdf) | Institucional BR | Primária (não extraída em detalhe nesta rodada) |
| 4 | [Versão brasileira validada da escala CFPB (PMC8317546)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8317546/) | Peer-reviewed | Primária |
| 5 | [Perceptions or behavior? (Financial Planning Review)](https://onlinelibrary.wiley.com/doi/full/10.1002/cfp2.1194) | Peer-reviewed | Primária |
| 6 | [Thaler & Benartzi — Save More Tomorrow (JPE 2004)](https://www.journals.uchicago.edu/doi/10.1086/380085) + [avaliação CLEAR/DoL](https://clear.dol.gov/study/save-more-tomorrow%E2%84%A2-using-behavioral-economics-increase-employee-saving-thaler-benartzi-2004) | Peer-reviewed + clearinghouse | Primária |
| 7 | [Mental budgeting e bem-estar (PLOS One 2023)](https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0294466) | Peer-reviewed | Primária |
| 8 | [RCT austríaco — Smart tools? (JBEE)](https://www.sciencedirect.com/science/article/abs/pii/S2214804318301642) | Peer-reviewed | Primária |
| 9 | [Karlan et al. — Reminders Increase Saving (Mgmt Science)](https://pubsonline.informs.org/doi/10.1287/mnsc.2015.2296) | Peer-reviewed | Primária |
| 10 | [FCA OP40 — Overdraft Alerts](https://www.fca.org.uk/publications/occasional-papers/occasional-paper-no-40-time-act-field-experiment-overdraft-alerts) (peer-reviewed: Grubb et al., J. Finance 2025) + [FCA Prompts & Alerts Design](https://www.fca.org.uk/publication/research/fca-prompts-and-alerts-design-behavioural-evidence.pdf) | Regulador | Primária |
| 11 | [Kaiser, Lusardi et al. — meta-análise educação financeira (JFE 2022)](https://www.sciencedirect.com/science/article/abs/pii/S0304405X21004281) | Peer-reviewed | Primária |
| 12 | [UPenn — Popular Budgeting Strategies](https://srfs.upenn.edu/financial-wellness/browse-topics/budgeting/popular-budgeting-strategies) | Institucional | Primária |
| 13 | [Sebrae — Sobrevivência de Empresas](https://datasebrae.com.br/wp-content/uploads/2021/06/Apresenta%C3%A7%C3%A3o-Sobreviv%C3%AAncia_2020_Web_Final.pdf) + [Agência Sebrae MEI](https://agenciasebrae.com.br/economia-e-politica/tres-em-cada-10-mei-fecham-as-portas-em-ate-cinco-anos-de-atividade-no-brasil/) + [erros de gestão](https://pb.agenciasebrae.com.br/cultura-empreendedora/investir-na-gestao-financeira-diminui-chances-de-mortalidade-empresarial-conheca-os-principais-erros/) + [ponto de equilíbrio](https://sebrae.com.br/sites/PortalSebrae/artigos/artigoshome/ponto-de-equilibrio,67ca5415e6433410VgnVCM1000003b74010aRCRD) | Institucional BR | Primária |
| 14 | [Behavioral Economics Institute — The Budgeting App Trap](https://www.behavioraleconomics.com/the-budgeting-app-trap-when-spending-information-backfires/) | Secundária | Usar com cautela |
| 15 | Benchmarks: [Ramsey comparison](https://www.ramseysolutions.com/budgeting/budgeting-apps-comparison), [PocketGuard vs YNAB](https://pocketguard.com/blog/pocketguard-vs-ynab/), [Monarch vs YNAB](https://www.monarch.com/compare/ynab-alternative), [mybest BR](https://br.my-best.com/18262), [comparativo ERPs PJ](https://multise.com.br/conta-azul-omie-nibo-ou-bling-comparativo-entre-os-erps-mais-usados-por-pmes/), [Nibo blog](https://www.nibo.com.br/blog/nibo-x-conta-azul-x-omie-qual-e-a-melhor-gestao-financeira-para-sua-empresa-de-servicos) | Mercado/marketing | Benchmark [C] |
