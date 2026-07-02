# Instagram Analytics

Uma skill do Claude que transforma os dados do seu Instagram em um diagnóstico estratégico com recomendações acionáveis e um relatório visual interativo. Em vez de olhar métricas soltas, você recebe a leitura do que está funcionando, o que trava o crescimento e o que fazer a seguir.

A arquitetura é hub-and-spoke: um relatório mestre para a visão completa e seis lentes para dúvidas específicas.

---

## O que ela responde

- Meu perfil está crescendo? O que está travando?
- Quais foram meus melhores conteúdos e por quê?
- Qual formato performa melhor (Reels, carrossel, imagem)?
- Qual o melhor horário para postar?
- Por que meus Reels não seguram a audiência?
- Qual formato mais converte (visitas ao perfil, cliques no link)?
- Como chego em quem ainda não me segue?
- O que fazer na próxima semana, em ordem de prioridade?

---

## Requisitos

- Conta **Instagram Business ou Creator**, vinculada a uma **Página do Facebook**. Conta pessoal não expõe métricas.
- Uma das duas fontes de dados:
  - **Conector Windsor.ai** conectado ao seu Claude (recomendado, dados automáticos), ou
  - **Dados colados** manualmente (exportados do painel nativo do Instagram ou de uma planilha).
- Os dados são **orgânicos**. Anúncios (Instagram Ads) não são cobertos.

---

## Instalação

**No Claude Code (pasta de skills):**
1. Copie a pasta `instagram-analytics/` para o diretório de skills do seu usuário:
   `~/.claude/skills/instagram-analytics/`
2. Reinicie a sessão do Claude. A skill passa a ser reconhecida automaticamente.

**Pelo arquivo `.skill`:**
1. Abra o arquivo `instagram-analytics.skill` no app do Claude.
2. Clique em **Save skill** no card que aparece (disponível quando sua organização permite criar skills).

Não há comando de barra a decorar: a skill dispara sozinha quando você descreve uma tarefa de análise de Instagram.

---

## Como usar (chamadas de exemplo)

Basta escrever seu pedido em linguagem natural. Exemplos que disparam a skill:

**Relatório completo (com Windsor):**
> Analisa meu perfil @seuperfil dos últimos 30 dias e monta o relatório completo, com o visual.

**Relatório completo (sem Windsor, colando dados):**
> Não uso Windsor. Vou colar meus dados do Instagram aqui, faz o relatório completo.

Depois cole os dados no formato que a skill pedir (uma linha por publicação: tema/legenda, formato, data, alcance, impressões, curtidas, comentários, salvamentos, compartilhamentos), mais a evolução de seguidores.

**Lentes de aprofundamento (perguntas específicas):**
> Em quais dias eu mais cresci esse mês e o que eu postei nesses dias?

> Quais temas geraram mais salvamento e compartilhamento? Me dá 3 ideias novas.

> Qual o melhor horário pra eu postar?

> Quais Reels seguraram mais a audiência e o que eles têm em comum?

> Qual formato me traz mais visitas ao perfil e cliques no link?

> Quais posts me trouxeram mais gente que ainda não me segue e como eu replico isso?

Dica: você não precisa citar "Windsor" nem "relatório". Descrever o objetivo já aciona a skill (por exemplo: "quero crescer no Instagram e não sei o que está travando").

---

## O que tem dentro (conteúdo da skill)

| Prompt | Para que serve | Dado que usa |
|---|---|---|
| **Mestre (Windsor / Manual)** | Relatório completo em 9 seções, mensal ou por qualquer período | Alcance, views, engajamento, seguidores, top conteúdos |
| **Lente: Crescimento diário** | Em que dias você cresceu e o que puxou seguidores | Seguidores por dia e publicações |
| **Lente: Saves e Shares + ideias** | O que o algoritmo mais valoriza, com 3 ideias novas | Salvamentos e compartilhamentos por post |
| **Lente: Melhor horário** | As melhores janelas para postar | Horários de atividade do público |
| **Lente: Retenção de Reels** | Que Reel segurou a audiência e por quê | Tempo médio de visualização e plays |
| **Lente: Formato que converte** | Onde focar energia (visitas ao perfil, cliques) | Engajamento, visitas ao perfil, cliques no link |
| **Lente: Descoberta** | Como chegar em quem não te segue | Alcance por não-seguidores |

O **relatório mestre** cobre: visão geral do período, crescimento de seguidores, engajamento por formato, top conteúdos (com o link ou tema de cada post), conteúdos de baixo desempenho, formatos e horários, diagnóstico geral, recomendações priorizadas e roteiros prontos baseados nos seus melhores vídeos.

---

## Relatório visual

O relatório mestre termina gerando um **dashboard interativo** (um artifact HTML) com cards de indicadores, gráficos, navegação por seções, recomendações e roteiros. É a **entrega única e completa** — tudo (inclusive as recomendações) fica no painel, não repetido como um segundo relatório no chat. A única coisa que também aparece no chat é um **bloco de links clicáveis** dos seus top conteúdos: isso é proposital, porque o preview do artifact bloqueia links externos clicados dentro do painel, então os links no chat garantem que você consiga abrir os posts. As lentes respondem em texto por padrão; peça o visual se quiser.

---

## Como os dados chegam, e o que acontece se falharem

A skill segue um protocolo em três camadas, pensado para você não se perder:

1. Ela tenta puxar os dados do Windsor.
2. Se nada vier, ela trata como **problema de conexão** (não como falta de dados) e pede para você conferir, nesta ordem: se a conta é Business/Creator vinculada a uma Página do Facebook, se o conector Windsor está autorizado e ativo, e se não há instabilidade na plataforma Windsor. Só depois disso ela sugere colar os dados manualmente.
3. Se a conexão está boa mas uma métrica específica falta, ela marca "Dado indisponível" e, quando possível, usa uma estimativa (modo proxy) ou indica onde pegar o número no app.

Ela nunca inventa números.

---

## Limitações (leia antes de cobrar demais)

- **Anúncios não entram.** Só métricas orgânicas. Para Ads, use outra integração.
- **Algumas métricas o Instagram/Meta não fornece oficialmente pela API** e por isso aparecem marcadas como **"Não exposto pela API"** (não como "Dado indisponível", que seria uma falha). É o caso de **duração de Reel**, **retenção % exata**, **horários de atividade do público** (lente Melhor horário) e **alcance por não-seguidores** (lente Descoberta). Quando isso acontece, a skill avisa — no texto e no relatório visual — que é limitação da plataforma, não erro seu nem da análise, e oferece a alternativa (dado manual do app ou modo proxy) quando existe.
- **Dois rótulos, causas diferentes.** "Não exposto pela API" = a plataforma não dá o número. "Dado indisponível" = o dado só não foi enviado/coletado e você pode preencher. A skill nunca inventa número em nenhum dos casos.
- **"Impressions" está em transição para "views"** nas contas mais novas. A skill pede as duas e usa a que estiver disponível.
- A qualidade da análise depende da qualidade dos dados que você fornece. Quanto mais completo o export, melhor o diagnóstico.

---

## Solução de problemas

- **"Não consegui acessar o Windsor.ai".** É conexão, não falta de dados. Confira: conta Business/Creator + Página do Facebook, conector autorizado no Claude, e o status da fonte no windsor.ai. Depois rode de novo.
- **Uma seção veio como "Dado indisponível".** O dado só não chegou na fonte (export incompleto ou você não colou). Cole o número do app nativo e rode de novo.
- **Algo veio como "Não exposto pela API".** Não é erro: é uma métrica que o Instagram/Meta não fornece oficialmente (duração de Reel, retenção % exata, horários de atividade, alcance por não-seguidores). A skill segue a análise com o que dá e, quando possível, oferece o dado manual do app ou o modo proxy.
- **A skill não disparou.** Descreva o objetivo com mais clareza (por exemplo, cite "meu Instagram", "meu perfil", ou a métrica que quer analisar), ou peça diretamente o relatório.

---

## Boas práticas

- Rode o **mestre uma vez por mês** para acompanhar a evolução, e use as **lentes no dia a dia** quando bater uma dúvida específica.
- Informe o **objetivo de negócio** (crescer seguidores, gerar leads, vender) no pedido: as recomendações ficam mais afiadas.
- Traga a **legenda ou o link dos posts** nos dados: assim o ranking de top conteúdos identifica exatamente qual publicação foi.

---

## Estrutura de arquivos

```
instagram-analytics/
├── SKILL.md            (regras de roteamento e uso)
├── README.md           (este arquivo)
└── references/
    ├── mestre-windsor.md
    ├── mestre-manual.md
    ├── lente-crescimento-diario.md
    ├── lente-saves-shares.md
    ├── lente-melhor-horario.md
    ├── lente-retencao-reels.md
    ├── lente-formato-converte.md
    └── lente-descoberta.md
```
