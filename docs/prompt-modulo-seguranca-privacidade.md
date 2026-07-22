# PROMPT — Módulo de Segurança e Privacidade (Assistente Financeiro IA)

> Prompt de design/arquitetura reformulado com base em padrões e benchmarks de 2026.
> Complementa o módulo principal em [`prompt-plataforma-financeira.md`](./prompt-plataforma-financeira.md).
> Principal mudança em relação a versões anteriores: **passkeys como método de autenticação primário**, MFA adaptativo (step-up) no lugar de MFA em todo login, e segurança visível como princípio de UX.

---

## 1. Papel e Objetivo

Você é um **designer de produto sênior + arquiteto de segurança** especializado em apps que lidam com dados sensíveis (financeiros, bancários, patrimoniais e de investimentos). Projete o **Módulo de Segurança e Privacidade** de uma plataforma financeira mobile-first e web que armazena dados pessoais, saldos, extratos, transações, cartões, investimentos e metas financeiras.

O módulo deve atingir o padrão de confiança de apps bancários líderes (Revolut, N26, Nubank, Cash App), aplicando confidencialidade, integridade, disponibilidade, autenticidade e privacidade — dando ao usuário **controle total** sobre dados, dispositivos, permissões e histórico de acessos.

**Regra de ouro:** segurança nunca custa usabilidade. Proteção invisível costuma parecer ausência de proteção — então torne a segurança visível e legível. Toda tela deixa claras três coisas: estado atual da conta, estado do sistema, e o que acontece a seguir.

---

## 2. Diretrizes de Design (benchmarks 2026)

- **Progressive disclosure:** simplicidade por padrão, avançado sob demanda. Não sobrecarregue dashboards nem trate prompts de segurança como afterthought.
- **Trust cues visíveis:** cadeados, badges "Criptografado", carimbo "Verificado", estados "Processando → Concluído". Sinais visuais como opções biométricas, ícones de cadeado e microcopy tranquilizadora ("Seus dados são criptografados de ponta a ponta") mostram a proteção funcionando para o usuário.
- **Microcopy honesto e não-técnico:** "Digite o código do seu app autenticador", nunca "Forneça o TOTP". Sempre explique *por que* um dado é pedido.
- **Zero dark patterns:** consentimento transparente, sem pré-marcação enganosa.
- **Light + dark mode nativos.** Em 2026, suportar ambos os modos não é mais um diferencial — é esperado.
- **Paleta deliberada:** fundações neutras, dados de alto contraste, cores de destaque usadas com precisão.
- **Design system modular:** tokens consistentes = fluência cognitiva = percepção de segurança.
- **Acessibilidade (WCAG 2.2):** alternativas à biometria, contraste adequado, leitores de tela.

**Proposta nova — "Central de Segurança" com Security Score:** tela-hub que dá uma nota de 0–100 à saúde da conta ("Sua conta está 80% protegida") com checklist acionável (ative passkey, revise dispositivos, configure recuperação). Transforma segurança em algo gamificado e proativo em vez de reativo.

---

## 3. Especificação Funcional por Tela

### 3.1 Onboarding & Autenticação

**Passkeys como método primário (mudança central vs. 2025).** Passkeys usam criptografia de chave pública; as chaves privadas nunca deixam os dispositivos do usuário, então mesmo que todo serviço sofra uma violação simultânea, as contas permanecem seguras.

- **Fluxo "Identifier-First":** o usuário digita só o e-mail; o sistema detecta se há passkey e dispara a autenticação automaticamente. Essa é a abordagem mais eficaz — significativamente melhor que um botão separado "Entrar com passkey", que pesquisas mostram levar a baixa adoção.
- **Login social** (Google One-tap / Sign in with Apple / Microsoft) como alternativa: pedir **apenas** nome, e-mail e foto; suporte a Apple Private Relay para ocultar e-mail.
- **Estratégia híbrida obrigatória.** O padrão de adoção mais forte em 2026 é híbrido: passkeys como opção primária, mantendo senha ou recuperação alternativa disponível durante a migração. Nunca corte senhas da noite para o dia.

### 3.2 Autenticação Biométrica

**Regra dos 3:**
1. Ofereça biometria **após** o primeiro login, nunca durante o cadastro.
2. Opt-in em toque único.
3. Fallback visível para PIN/senha sempre.

- Processamento **100% local** via Android Keystore / Apple Keychain. Deixe explícito: "Seus dados biométricos nunca saem do aparelho."
- **Proposta:** distinga "biometria de acesso" (desbloquear o app) de "biometria de aprovação" (confirmar ações críticas — transferências e Pix acima de limite configurável, exportar dados financeiros, revogar dispositivo, excluir conta), com fricção proporcional ao risco.

### 3.3 PIN de Segurança

- Configurável (4/6+ dígitos), alterável, com recuperação segura.
- Bloqueio automático por inatividade configurável (imediato / 1 / 5 min).
- Rate limiting após tentativas falhas, com mensagens claras.

### 3.4 MFA Adaptativo (Step-up)

O ponto onde 2026 mais diverge de prompts antigos: **MFA não é mais pedido em todo login.** Sistemas modernos invocam autenticação step-up apenas quando necessário, adaptando-se ao contexto.

- Dispare MFA só em: novo dispositivo, troca de senha/e-mail, exclusão de conta, exportação de dados financeiros, transferências/Pix acima do limite configurado, alteração de limites.
- **Push MFA em vez de SMS.** Em 2026, push MFA é preferível ao SMS por ser muito mais seguro e amigável — autentica o dispositivo confiável diretamente, e o usuário confirma com biometria se necessário.
- **Reconhecimento de dispositivo confiável.** Implemente reconhecimento de dispositivos confiáveis para que o login permaneça fluido.
- UX do campo OTP (para o fallback por e-mail): auto-focus, autofill, auto-submissão, validade 30–60s com aviso de expiração, sempre com caminho de backup.

### 3.5 Criptografia (3 camadas com badges de status)

- **Em trânsito:** TLS em toda comunicação app↔servidor.
- **At rest:** dados criptografados nos servidores.
- **Local:** dados offline criptografados (proteção contra perda/roubo).
- Chaves via Keystore / Keychain. **Proposta:** para relatórios financeiros e exportações (PDF/Excel/CSV), considere criptografia ponta-a-ponta com chave derivada do dispositivo, e comunique isso como badge "E2EE".

### 3.6 Backup Seguro em Nuvem

Painel de status: última data, tamanho, dispositivo, sincronização, espaço usado.

- Automático em background + manual; histórico de versões, restauração seletiva, sync incremental, **criptografia antes do envio**.

### 3.7 Gerenciamento de Dispositivos

Cards por sessão: nome, SO, local aproximado, último acesso, IP, status. Ações: encerrar/remover/revogar/bloquear remotamente. Destaque para dispositivo atual e sessões incomuns.

- **Proposta crítica para passkeys:** inclua fluxo explícito de **dispositivo perdido/trocado**. As equipes que têm sucesso com passkeys tratam o suporte como parte do design — desenhe a recuperação (device sync quebrado, celular novo) como tela de primeira classe, não como ticket de suporte.

### 3.8 Log de Segurança (Timeline)

Eventos: logins novos, tentativas falhas, alteração de senha/e-mail/PIN, transferências, exportações, compartilhamentos, conexões Open Finance, restaurações. Notificação imediata em atividade incomum.

### 3.9 Controle de Privacidade

Toggles granulares: visibilidade de saldos e patrimônio ("modo discreto"); compartilhamento de relatórios; permissões para contador, planejador financeiro ou cônjuge (acesso somente-leitura ou por módulo) com revogação a qualquer momento; gestão de consentimentos Open Finance (quais instituições estão conectadas, quais dados compartilham, validade de cada consentimento).

### 3.10 Conformidade (LGPD / GDPR / Open Finance)

Consentimento explícito; direito de acesso, correção e exclusão; exportação completa; registro de consentimentos; minimização de coleta; conformidade com as regras de consentimento do Open Finance Brasil (prazo, renovação e revogação). Não pule a consultoria de compliance no design.

### 3.11 IA para Segurança (transparente)

Detecção de padrões incomuns de login, acessos simultâneos distantes, dispositivos desconhecidos, transações fora do padrão de gastos do usuário; sugestão de troca de senha; ajuste automático do nível de proteção conforme risco.

- **Transparência obrigatória — evite "creepy personalization".** Explique claramente por que um alerta aparece, adicione níveis de confiança ("Temos 85% de certeza de que este acesso é incomum") e dê ao usuário toggles de controle.

---

## 4. Integração com o Ecossistema

Segurança como **camada transversal** da plataforma financeira:

- **Dashboard:** ocultar saldos e patrimônio em ambiente público ("modo discreto" em um toque).
- **Metas:** proteção contra alteração acidental ou não autorizada.
- **Contas e cartões:** conexões Open Finance com consentimento visível e revogável.
- **Transferências/Pix:** limites configuráveis + biometria de aprovação acima do limite.
- **Investimentos:** histórico e posições protegidos.
- **Relatórios:** exportações com autenticação adicional e criptografia.
- **Backup:** restauração íntegra.

---

## 5. Entregáveis

1. User flows por tela (incluindo os fluxos de erro/recuperação de passkey).
2. Wireframes / especificação de UI mobile-first, light + dark.
3. Design tokens (cores, tipografia, espaçamento, ícones, estados).
4. Microcopy completo em português (erros, tooltips, consentimentos).
5. Estados de tela: vazio, carregando, sucesso, erro, offline.
6. Stack recomendada de front-end com justificativa.

---

## Resumo das principais melhorias desta versão

1. **Passkeys viram o método primário** via fluxo identifier-first, com estratégia híbrida de migração.
2. **MFA vira adaptativo/step-up**, com push MFA no lugar de SMS e reconhecimento de dispositivo confiável.
3. **"Central de Segurança" com security score** (0–100) e o fluxo de recuperação de dispositivo perdido/trocado como tela de primeira classe.
4. **Contexto financeiro:** biometria de aprovação para Pix/transferências acima de limite, gestão de consentimentos Open Finance, modo discreto para saldos e compartilhamento granular com contador/planejador.
