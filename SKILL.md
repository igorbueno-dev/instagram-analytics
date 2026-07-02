---
name: instagram-analytics
description: >-
  Diagnóstico e análise de desempenho de perfil de Instagram a partir de dados reais (conector Windsor.ai ou dados colados), gerando relatório estratégico com recomendações e, quando fizer sentido, um relatório visual interativo. Use SEMPRE que o usuário quiser analisar, diagnosticar ou dar um raio-X de um perfil ou conta de Instagram; avaliar métricas como alcance, impressões, engajamento, crescimento de seguidores, salvamentos, compartilhamentos, retenção de Reels, melhores horários, qual formato converte mais, ou alcance entre não-seguidores; entender por que um conteúdo performou; ou montar um relatório de resultados de Instagram. Dispare mesmo que não citem Windsor nem a palavra relatório, ou só descrevam o objetivo (ex.: quero crescer no Instagram e não sei o que trava, por que meus Reels não engajam, qual o melhor horário pra postar, meus dados do Insta estão no Windsor). Também quando mencionarem Windsor.ai com Instagram.
---

# Instagram Analytics

Analisa o desempenho de um perfil de Instagram a partir de dados reais e devolve diagnóstico acionável. A skill é hub-and-spoke: um **relatório mestre** (visão completa, recorrente) e **6 lentes** de aprofundamento para dúvidas específicas. Cada prompt-fonte completo fica em `references/`; sua função aqui é escolher o certo, seguir o protocolo de dados e conduzir a entrega.

## Requisitos (verifique cedo, sem travar)
- A conta precisa ser **Instagram Business ou Creator** vinculada a uma **Página do Facebook**. Conta pessoal não expõe métricas.
- Dados via **conector Windsor.ai** no Claude (permissão "sempre permitir") **ou** colados manualmente pelo usuário (export do app nativo / planilha).
- Só dados **orgânicos** (Ads exigem outra integração). Se o usuário pedir análise de anúncios, avise que a skill não cobre Ads e ofereça focar na análise orgânica no lugar.

## Como escolher o prompt certo (roteamento)

Primeiro decida **relatório completo** vs **dúvida pontual**:

- **Relatório completo / mensal / geral / "raio-X" / "analisa meu perfil"** → use o **Mestre**.
  - Windsor conectado (ou o usuário quer que você puxe os dados) → `references/mestre-windsor.md`.
  - Sem Windsor, ou o usuário vai colar os dados → `references/mestre-manual.md`.
- **Dúvida específica** → use a **lente** correspondente:

| Se o usuário quer saber... | Lente |
|---|---|
| Em que dias cresceu e o que puxou seguidores | `references/lente-crescimento-diario.md` |
| O que o algoritmo amou (salvamentos/compartilhamentos) e ideias novas | `references/lente-saves-shares.md` |
| Qual o melhor horário para postar | `references/lente-melhor-horario.md` |
| Que Reel segurou a audiência e por quê (retenção) | `references/lente-retencao-reels.md` |
| Qual formato converte mais (visitas ao perfil, cliques) | `references/lente-formato-converte.md` |
| Como chegar em quem não segue (descoberta) | `references/lente-descoberta.md` |

Na dúvida entre Mestre e lente, pergunte em uma linha: "Quer o relatório completo ou só essa análise específica?". Se o pedido for amplo ("me ajuda a crescer"), comece pelo Mestre.

**Leia o arquivo de referência escolhido e siga o prompt dele à risca.** Ele já traz persona, protocolo, tarefa, formato e regras. Não reescreva o prompt; execute-o com os dados.

## Protocolo de dados em 3 camadas (vale para todos)

A regra de ouro é nunca inventar número e não confundir o usuário sobre a causa de uma falha:

1. **Tente os dados** (puxar do Windsor ou ler o que o usuário colou).
2. **Se nada vier via Windsor, trate como falha de CONEXÃO, não como dado inexistente.** Avise que não conseguiu acessar o Windsor.ai e peça para o usuário verificar, nesta ordem: (a) conta Business/Creator vinculada a Página do Facebook; (b) conector Windsor autorizado e ativo no Claude; (c) possível instabilidade da plataforma Windsor (testar/reconectar em windsor.ai). Só avance para o passo manual depois que ele confirmar que a conexão está OK. Isso evita que o usuário saia puxando dados à mão quando o problema era só a integração.
3. **Se a conexão está OK mas uma métrica específica falta**, distinga a causa com o rótulo certo (nunca invente número):
   - **"Dado indisponível"** → o usuário não colou aquele dado ou ele não veio na fonte por lacuna do export. É contornável: explique a lacuna e, quando o prompt oferecer, use o **modo proxy** (estimativa rotulada como tal) ou oriente o caminho manual exato no app.
   - **"Não exposto pela API"** → a métrica **não é fornecida oficialmente pela API do Instagram/Meta**. NÃO use "Dado indisponível" aqui, porque parece erro da skill ou do usuário. Métricas nesse caso incluem **duração de Reel, retenção % exata, horários de atividade do público e alcance por não-seguidores**. Rotule como "Não exposto pela API" e **avise o usuário, no texto E no relatório visual**, que é limitação da plataforma (não falha da análise nem dado que faltou ele enviar), oferecendo a alternativa quando existir (dado manual do app ou modo proxy).

Regra de notificação: sempre que algo aparecer como "Não exposto pela API", inclua uma **nota curta visível** (uma linha no texto e uma legenda/rodapé no artifact) explicando o que a Meta não expõe. O usuário precisa entender que foi a plataforma que não deu o número, não a skill que falhou.

## Insights por cruzamento (não fique refém da API)

Uma métrica que a Meta não expõe **não é um beco sem saída**. Explore e cruze os dados que existem — horário × desempenho, formato × alcance, tema × engajamento, dia da semana × crescimento, tempo médio × formato — para gerar leituras que uma métrica isolada não daria (ex.: o melhor horário sai de quando os seus posts realmente performaram, não do gráfico de "seguidores online" que a API esconde). Duas regras acompanham isso, sempre:

- **Honestidade prevalece.** Toda conclusão inferida por cruzamento é uma **TENDÊNCIA baseada em dado real**, não uma certeza. Rotule como tendência, aponte a força da amostra (poucos pontos enfraquecem) e diga que o ideal é validar testando. Nunca apresente inferência como fato oficial.
- **Nunca invente número.** Cruzar dados reais para achar um padrão é encorajado; fabricar um valor para preencher uma célula não é. Inferência é leitura qualitativa/relativa ou estimativa rotulada como tal, jamais um número inventado.

## Relatório visual

O **Mestre** termina gerando um **relatório visual** (dashboard HTML interativo), que é a **entrega única e completa**: as recomendações e os roteiros vão DENTRO do painel, não repetidos como um segundo relatório no chat. No chat, entregue só uma abertura curta (2-3 linhas) + o **bloco de links clicáveis** dos top conteúdos (necessário porque o preview do artifact bloqueia links externos clicados dentro do painel). O Claude tem **liberdade de design**; o prompt-fonte lista as regras inegociáveis: autocontido e CSP-safe (tudo inline, gráficos em SVG, sem recursos externos), legível (alto contraste, nunca texto escuro em fundo escuro, cores explícitas sem herdar do host), responsivo (celular, sem rolagem horizontal, nada vazando dos cards), sem emojis, baseado só nos dados reais. Estilo: não force dark — um tema claro bem resolvido é a opção segura; se usar fundo escuro, nenhum texto pode ficar preto. Layout de referência (mantenha o esqueleto): app-shell com barra lateral fixa (marca + card de perfil/período + navegação por seções) e área principal rolável; a navegação da sidebar deve levar à seção certa e destacar a ativa sem pular para outra. Siga o prompt-fonte.

As **lentes** são respostas rápidas e por padrão saem em texto (tabela + bullets). Ofereça a versão visual só se o usuário pedir ou se o volume de dados justificar.

## Estilo (aplica-se a toda saída, texto e visual)
- **Idioma: PT-BR.**
- **Não use emojis.**
- Seja objetivo e baseado em dados; conecte cada recomendação a uma métrica concreta.

## Fluxo resumido
1. Identifique se é relatório completo ou dúvida pontual e escolha o prompt (tabela acima).
2. Confirme a fonte de dados (Windsor vs. colar) e os campos de contexto mínimos (perfil/nicho, período, objetivo).
3. Leia o arquivo de `references/` e execute o prompt seguindo o protocolo de dados.
4. Entregue a análise; no Mestre, gere o relatório visual; nas lentes, ofereça-o como opção.
