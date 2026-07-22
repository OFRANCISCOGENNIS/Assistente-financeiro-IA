# Biblioteca de Microcopy — Guardião Inteligente

> Entregável nº 4 do [`prompt-guardiao-inteligente.md`](./prompt-guardiao-inteligente.md).
> Todos os textos seguem a fórmula **fato + consequência + próximo passo**, sem julgamento moral.
> Variáveis entre `{chaves}` são preenchidas pelo motor de alertas.
> Nível 1 = banner/nudge no dashboard · Nível 2 = push + card com CTA · Nível 3 = bottom sheet com confirmação consciente.
> Nem todo gatilho tem os 3 níveis: o nível 3 existe apenas quando há ação grave/irreversível a interceptar.

---

## RADAR FINANCEIRO

### F1 — Estouro de orçamento

| Nível | Texto | CTA |
|---|---|---|
| 1 | Atenção: você já usou {pct}% do orçamento de {categoria} e ainda faltam {dias} dias no mês. | Ver orçamento |
| 2 | Você estourou o orçamento de {categoria} em {pct_acima}% ({valor_acima}). No ritmo atual, isso consome {dias_reserva} dias da sua reserva. Ajustar agora evita que o mês feche no vermelho. | Ajustar limites · Ver gastos |
| 3 | Esta compra de {valor} estoura o orçamento de {categoria} em {pct_acima}% e deixa sua reserva abaixo do mínimo que você definiu. Quer registrar mesmo assim? | Confirmar mesmo assim · Repensar |

### F2 — Overspending preditivo

| Nível | Texto | CTA |
|---|---|---|
| 1 | No ritmo atual, {categoria} fura o orçamento em {dias} dias — antes do fim do mês. | Ver projeção |
| 2 | Projeção do mês: seus gastos vão superar sua renda em {valor} se o ritmo se mantiver. Reduzir {categoria_maior} em {sugestao_pct}% resolve. | Simular ajuste · Ver detalhes |

### F3 — Reserva de emergência crítica

| Nível | Texto | CTA |
|---|---|---|
| 1 | Sua reserva cobre {dias} dias de despesas — abaixo da sua meta de {meta_meses} meses. | Ver reserva |
| 2 | Sua reserva de emergência caiu para {valor} e cobre só {dias} dias. Um imprevisto agora viraria dívida. Um aporte de {aporte_sugerido}/mês recompõe em {meses} meses. | Programar aporte · Adiar 7 dias |
| 3 | Este resgate de {valor} zera sua reserva de emergência. Sem ela, qualquer imprevisto vira dívida no rotativo. Confirme se quer mesmo continuar. | Resgatar mesmo assim · Cancelar |

### F4 — Dívida se acumulando

| Nível | Texto | CTA |
|---|---|---|
| 1 | O parcelado do cartão já compromete {pct}% da sua renda dos próximos {meses} meses. | Ver parcelas |
| 2 | Você pagou o mínimo da fatura. O saldo restante de {valor} entra no rotativo a {juros}% a.m. — em 3 meses vira {valor_projetado}. Quitar agora custa {valor} e evita {custo_juros} em juros. | Quitar fatura · Simular parcelamento |
| 3 | Pagar só o mínimo vai custar {custo_juros} em juros nos próximos {meses} meses — o cartão mais caro que você tem. Confirme que entendeu o custo antes de continuar. | Pagar mínimo assim mesmo · Ver alternativas |

### F5 — Contas a vencer

| Nível | Texto | CTA |
|---|---|---|
| 1 | {conta} de {valor} vence em {dias} dias. | Ver conta |
| 2 | {conta} de {valor} vence {quando} e não há saldo suficiente na conta {conta_bancaria}. Atraso gera multa de {multa} + juros por dia. | Pagar agora · Transferir saldo |

### F6 — Assinaturas fantasma

| Nível | Texto | CTA |
|---|---|---|
| 1 | {assinatura} ({valor}/mês) não é usada há {meses} meses. | Revisar |
| 2 | Sua assinatura {assinatura} ({valor}/mês) não é usada há {meses} meses. Cancelar economizaria {valor_ano}/ano — o equivalente a {comparacao_meta}. (Confiança: {confianca}%) | Cancelar assinatura · Manter |

### F7 — Meta abandonada

| Nível | Texto | CTA |
|---|---|---|
| 1 | A meta "{meta}" está sem aporte há {periodos} meses. | Ver meta |
| 2 | "{meta}" parou em {pct}%: sem aporte há {periodos} meses, a data prevista já escorregou de {data_antiga} para {data_nova}. Retomar com {aporte}/mês recupera o prazo. | Retomar aportes · Redefinir meta |

### F8 — Padrão de impulso

| Nível | Texto | CTA |
|---|---|---|
| 1 | Suas compras de madrugada somaram {valor} este mês — {pct}% delas você categorizou depois como arrependimento. | Ver padrão |
| 3 | São {hora} e este é o seu {n}º gasto em {categoria} hoje — o padrão que você me pediu para vigiar. Respira: se ainda fizer sentido em 10 minutos, eu registro sem perguntar de novo. | Esperar 10 min · Registrar agora |

---

## RADAR DE SEGURANÇA

### S1 — Sem MFA ativado

| Nível | Texto | CTA |
|---|---|---|
| 1 | Sua conta ainda está protegida só por senha. | Reforçar |
| 2 | Sua conta ainda não tem verificação em duas etapas. Isso deixa seus dados financeiros expostos se a senha vazar. Ativar agora leva 40 segundos. | Ativar verificação · Lembrar amanhã |

*Recorrente até resolver; cooldown progressivo (1d → 3d → 7d), nunca silenciável por definitivo.*

### S2 — Sem biometria/PIN

| Nível | Texto | CTA |
|---|---|---|
| 1 | Qualquer pessoa com seu celular desbloqueado vê seus saldos. | Proteger app |
| 2 | O app está sem biometria nem PIN. Se seu celular for perdido ou emprestado, todo o seu histórico financeiro fica aberto. Configurar leva 30 segundos. | Ativar biometria · Criar PIN |

### S3 — Senha fraca, reutilizada ou antiga

| Nível | Texto | CTA |
|---|---|---|
| 1 | Sua senha não muda há {meses} meses. | Revisar |
| 2 | Sua senha aparece em {n} outros serviços que você conectou. Se um deles vazar, sua conta financeira cai junto. Uma passkey elimina esse risco de vez. | Criar passkey · Trocar senha |

### S4 — Backup desatualizado

| Nível | Texto | CTA |
|---|---|---|
| 1 | Seu último backup foi há {dias} dias. | Fazer backup |
| 2 | Sem backup há {dias} dias: se este aparelho for perdido, você perde {n_lancamentos} lançamentos e {n_anexos} comprovantes. O backup é criptografado antes de sair do aparelho. | Fazer backup agora · Ativar automático |

### S5 — Dispositivo desconhecido / login incomum

| Nível | Texto | CTA |
|---|---|---|
| 2 | Novo acesso de {dispositivo} em {local}, {quando} — um local incomum para você. Se não foi você, revogue agora e troque sua senha. (Confiança: {confianca}%) | Fui eu · Revogar acesso |
| 3 | Este acesso de {local} está a {distancia} km do seu login de {tempo} atrás — fisicamente impossível. Bloqueamos ações sensíveis até você confirmar sua identidade. | Confirmar com biometria |

### S6 — Sessões esquecidas ativas

| Nível | Texto | CTA |
|---|---|---|
| 1 | {n} sessões abertas em aparelhos que você não usa há mais de {dias} dias. | Revisar sessões |
| 2 | O aparelho "{dispositivo}" mantém acesso à sua conta desde {data}, sem uso. Sessões esquecidas são porta de entrada — encerrar não afeta seus dados. | Encerrar sessão · Manter |

### S7 — Permissões de terceiros não revisadas

| Nível | Texto | CTA |
|---|---|---|
| 1 | O acesso de {pessoa} à sua conta não é revisado há {meses} meses. | Revisar acesso |
| 2 | {pessoa} tem acesso a {escopo} desde {data} e você nunca revisou. Se a parceria mudou, o acesso deveria mudar junto. Revisar leva 1 minuto. | Revisar agora · Está correto |

### S8 — Exportação/compartilhamento sensível

| Nível | Texto | CTA |
|---|---|---|
| 3 | Você está prestes a exportar todo o seu histórico financeiro ({periodo}, {n_lancamentos} lançamentos). Esse arquivo sai da proteção do app. Confirme com biometria para continuar. | Confirmar com biometria · Cancelar |

---

## REFORÇO POSITIVO

| Gatilho | Texto |
|---|---|
| Streak de economia (7 dias) | 7 dias dentro do orçamento. É assim que hábito se constrói. |
| Streak de economia (30 dias) | Um mês inteiro no controle — seu melhor mês desde {mes_referencia}. |
| Streak de economia (90 dias) | 90 dias de consistência. Sua reserva cresceu {valor} nesse período. |
| Streak de economia (365 dias) | Um ano de disciplina: {valor_ano} economizados. Pouca gente chega aqui. |
| Meta de economia batida | Você economizou {valor} este mês — {pct}% acima da meta. Mantenha o ritmo. |
| Meta concluída | "{meta}" concluída: {valor} em {meses} meses. Qual é a próxima? |
| Health score subiu | Seu score subiu para {score} ({+pts} pts) depois que você {acao}. |
| Postura de segurança completa | Passkey, biometria e backup em dia. Sua conta está no nível máximo de proteção. |

---

## REGRAS DE APLICAÇÃO

1. **Fato + consequência + próximo passo** — nunca só a bronca, nunca só o dado.
2. **Números concretos sempre:** percentuais, valores, dias e datas reais no lugar de "muito", "pouco", "em breve".
3. **Consequência tangível no score:** todo alerta de nível 2 mostra o impacto ("−{pts} pts no seu score enquanto isso não for resolvido").
4. **Nunca vocabulário de julgamento:** proibidos "desperdiçou", "irresponsável", "de novo?", "você deveria". Permitidos "estourou", "expõe", "custa", "resolve".
5. **CTA primário resolve o problema em um toque;** o secundário nunca é "ignorar para sempre" em riscos graves — no máximo "adiar".
6. **Transparência da IA:** alertas baseados em inferência (assinatura fantasma, login incomum, impulso) exibem o nível de confiança e o porquê.
7. **Anti-fadiga:** um mesmo gatilho nunca dispara duas vezes dentro do cooldown; três ignorados seguidos → cooldown dobra e o alerta migra para o resumo semanal.
