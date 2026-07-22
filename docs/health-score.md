# Financial Health Score — Composição e Pesos

> Entregável nº 6 do [`prompt-guardiao-inteligente.md`](./prompt-guardiao-inteligente.md).
> Indicador único de 0–100 que consolida a saúde **financeira** e a postura de **segurança**.
> Todo ponto perdido é rastreável a um item acionável — o score nunca é caixa-preta.

---

## 1. Estrutura geral

```
SCORE (0–100) = PILAR FINANCEIRO (0–60) + PILAR SEGURANÇA (0–40)
```

O pilar financeiro pesa mais porque muda todo dia e é onde o usuário age com mais frequência; o pilar de segurança tem teto menor, mas seus débitos são mais agudos (um único item grave derruba muito).

---

## 2. Pilar Financeiro (60 pts)

| Componente | Peso | Como pontua |
|---|---|---|
| **Orçamento & gastos** | 15 | % de categorias dentro do limite no mês (proporcional). Estouro confirmado: −3 por categoria (máx −9). Assinatura fantasma ativa: −4. Padrão de impulso recorrente: −2. |
| **Reserva de emergência** | 15 | `min(15, 15 × meses_cobertura / meta_meses)`. Meta padrão: 6 meses. Reserva zerada trava o componente em 0 e dispara alerta F3 nível 2. |
| **Endividamento** | 15 | Começa em 15. Rotativo ativo: −8. Comprometimento de renda com parcelas > 30%: −4 (> 50%: −8). Conta em atraso: −3 cada (máx −6). Sem dívida cara: 15. |
| **Metas & consistência** | 10 | Aportes em dia em todas as metas ativas: 10. Meta sem aporte há 2+ períodos: −3 cada (máx −6). Streak de economia ≥ 30 dias: +2 bônus (respeitando o teto de 10). |
| **Previsibilidade de fluxo** | 5 | Projeção de fim de mês positiva: 5. Projeção negativa (overspending preditivo F2): 0. |

## 3. Pilar Segurança (40 pts)

| Componente | Peso | Como pontua |
|---|---|---|
| **Autenticação** | 15 | Passkey ativa: 8. MFA por push ativo: 4. Biometria/PIN no app: 3. Só senha: 0 no componente e alerta S1 recorrente. |
| **Dispositivos & sessões** | 10 | Sem sessões incomuns/esquecidas pendentes: 10. Acesso incomum não revisado: −5. Sessão inativa > 30 dias mantida: −2 cada (máx −4). |
| **Backup & recuperação** | 8 | Backup ≤ 7 dias: 5. Contato/método de recuperação configurado: 3. Backup > 30 dias: 0 no sub-item e alerta S4. |
| **Privacidade & terceiros** | 7 | Permissões de terceiros revisadas ≤ 90 dias: 4. Consentimentos Open Finance válidos e dentro do prazo: 3. |

---

## 4. Regras de cálculo

1. **Rastreabilidade:** cada débito aparece na UI ao lado do item que o causou ("−4 pts · assinatura fantasma"), com o CTA que o recupera. Resolver o item devolve os pontos no mesmo dia.
2. **Sem dupla punição:** um mesmo fato debita em um único componente. O estouro de orçamento não debita também em previsibilidade; o alerta F2 só pontua se o estouro ainda não ocorreu.
3. **Suavização:** variação máxima de ±8 pts/dia por acúmulo de fatores financeiros, para o score não chicotear com um único fim de semana. Exceção: eventos de segurança graves (S5 — acesso impossível) aplicam o débito integral na hora.
4. **Piso de pânico:** o score nunca exibe abaixo de 5 — score 0 desmotiva em vez de mobilizar. Abaixo de 20, a UI troca a régua de "melhorar" para "plano de resgate" com os 3 itens de maior recuperação de pontos.
5. **Bandas de exibição:** 80–100 **Protegido** (verde) · 60–79 **Atenção** (âmbar) · 20–59 **Em risco** (laranja) · < 20 **Crítico** (vermelho). A cor da banda segue a paleta de status, nunca o acento da marca.
6. **Recálculo:** financeiro a cada sincronização de transações; segurança em tempo real por evento. O histórico do score é gravado por dia e plotável (mesma linha do tempo da evolução patrimonial).

---

## 5. Exemplo — o usuário do protótipo (score 76)

| Pilar | Componente | Pontos |
|---|---|---:|
| Financeiro | Orçamento & gastos (Lazer em 85% + assinatura fantasma −4) | 9/15 |
| Financeiro | Reserva de emergência (4,3 de 6 meses) | 11/15 |
| Financeiro | Endividamento (sem rotativo, parcelas 18% da renda) | 15/15 |
| Financeiro | Metas & consistência (aportes em dia, streak 90d +2) | 10/10 |
| Financeiro | Previsibilidade (projeção positiva) | 5/5 |
| **Subtotal financeiro** | | **50/60** |
| Segurança | Autenticação (passkey 8 + push 4 + biometria 3) | 15/15 |
| Segurança | Dispositivos (acesso incomum de Curitiba não revisado −5) | 5/10 |
| Segurança | Backup & recuperação (backup ok 5, sem contato de recuperação 0) | 5/8 |
| Segurança | Privacidade & terceiros (contador revisado −, OF válidos) | 4/7 |
| **Subtotal segurança** | | **29/40** |
| **TOTAL** | | **76/100 — banda Atenção** |

Os três CTAs de maior recuperação para este usuário: revisar o dispositivo de Curitiba (+5), configurar contato de recuperação (+3), cancelar a assinatura fantasma (+4) → levariam o score a 88 (banda Protegido).
