# Stack Recomendada — Assistente Financeiro IA

> Entregável nº 7 do [`prompt-guardiao-inteligente.md`](./prompt-guardiao-inteligente.md).
> Recomendação com justificativa; as alternativas avaliadas estão ao final.

---

## Recomendação: React Native (Expo) + TypeScript ponta a ponta

### Front-end mobile + web

| Camada | Escolha | Por quê |
|---|---|---|
| Framework | **React Native com Expo** (Expo Router) | Uma base de código para iOS, Android **e web** (react-native-web) — o produto pede mobile-first com dashboard web, e manter três bases (Swift/Kotlin/React) triplicaria o custo de cada tela do guardião. |
| Linguagem | **TypeScript** | Tipos compartilhados entre app e backend (o motor de alertas usa os mesmos contratos nos dois lados); menos bugs de contrato em um domínio onde errar número é inaceitável. |
| UI/Design system | Tokens próprios (já definidos no protótipo) + **Tamagui** ou StyleSheet puro | Tokens de tema claro/escuro como fonte única; Tamagui compila para CSS na web e estilos nativos no mobile. |
| Gráficos | **Victory Native XL / React Native Skia** | Renderização a 60fps das linhas, barras e anéis do dashboard; Skia dá o controle fino usado no protótipo (crosshair, tooltips). |
| Biometria & passkeys | **expo-local-authentication** + **react-native-passkeys** (WebAuthn) | Face ID/digital via Keystore/Keychain 100% local; passkeys identifier-first conforme o doc de segurança. |
| Estado & dados | **TanStack Query** + Zustand | Cache de sincronização bancária com revalidação; estado de UI (modo discreto, tema) simples e testável. |

### Backend

| Camada | Escolha | Por quê |
|---|---|---|
| Runtime/API | **NestJS (Node + TypeScript)** | Mesmo ecossistema do front; módulos claros para cada radar do guardião; injeção de dependência facilita testar as regras de pontuação do score isoladamente. |
| Banco | **PostgreSQL** + Prisma | Transações financeiras pedem ACID; row-level security para o acesso somente-leitura de terceiros (contador); Prisma gera os tipos compartilhados. |
| Open Finance | **Pluggy** (ou Belvo) como agregador | Conectores prontos e homologados para os bancos brasileiros — construir conexão direta ao Open Finance Brasil por conta própria custa meses de certificação. |
| Autenticação | **WebAuthn/passkeys** (SimpleWebAuthn) + OAuth Google/Apple | Implementa o fluxo identifier-first do doc de segurança; sessões com reconhecimento de dispositivo confiável. |
| Jobs & alertas | **BullMQ** (Redis) | Cooldowns e anti-fadiga do guardião são jobs agendados por gatilho; push via FCM/APNs. |
| Criptografia | TLS 1.3 + AES-256 at rest + campo-a-campo para dados sensíveis | As 3 camadas do doc de segurança; chaves em KMS. |

### Camada de IA (o guardião e o assistente)

- **Claude API** (Anthropic) — modelo de última geração para o assistente conversacional, explicação de conceitos e diagnósticos; um modelo rápido/econômico (tier Haiku) para categorização de transações em lote e detecção de recorrência.
- **Regras determinísticas primeiro, LLM depois:** os 16 gatilhos do guardião e o score são código puro (auditável, barato, instantâneo); o LLM entra para *redigir* a explicação personalizada sobre os números já calculados e para o chat. Nunca deixar o LLM calcular saldo.
- Microcopy da biblioteca ([`microcopy-guardiao.md`](./microcopy-guardiao.md)) como templates com variáveis — o LLM só personaliza dentro do template, mantendo o tom aprovado.

---

## Alternativas avaliadas

| Opção | Veredito | Motivo |
|---|---|---|
| **Flutter** | Forte segundo lugar | Excelente performance de UI e tema claro/escuro; perde na web (renderiza em canvas — pesado para dashboard com SEO/acessibilidade) e o ecossistema Dart é menor para passkeys/Open Finance no Brasil. Contratação de devs React é mais fácil no mercado brasileiro. |
| **SwiftUI + Kotlin Compose (nativo puro)** | Rejeitado para o estágio atual | Melhor integração possível com biometria/Keychain, mas duas bases de código + uma web triplicam o custo de iteração — fatal para um produto que vai evoluir o guardião toda semana. Reavaliar módulos nativos pontuais (widget de tela inicial, App Intents) depois do product-market fit. |
| **PWA pura (Next.js)** | Rejeitado | Sem acesso confiável a biometria de aprovação, push limitado no iOS e sem presença nas lojas — confiança percebida importa em fintech. |
| **Supabase como backend completo** | Válido para MVP | Auth + Postgres + RLS prontos aceleram semanas; migrar o motor do guardião para NestJS quando os jobs de alerta ficarem complexos. Caminho recomendado se o objetivo for validar em < 2 meses. |

---

## Ordem de construção sugerida (MVP → completo)

1. **MVP (6–8 semanas):** Expo + Supabase + Pluggy sandbox. Login social + passkey, dashboard com 4 indicadores e 2 gráficos, lançamento manual de despesas, 3 gatilhos do guardião (F1, F5, S1) com push.
2. **v1:** score completo com composição na Central de Segurança, todos os gatilhos financeiros, modo discreto, backup criptografado, exportação com bloqueio consultivo.
3. **v2:** radar de segurança completo, chat com IA (Claude), forecasting/what-if, educação financeira, relatórios PDF/Excel.
