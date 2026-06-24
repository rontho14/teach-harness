# Learning Harness

Um tutor reutilizável construído sobre qualquer harness de agente. Clone esta pasta; ela se torna seu
**organizador** (organizer). O organizador não ensina — ele planeja assuntos e cria
**salas de aula** (classrooms - uma subpasta por assunto), cada uma sendo um curso autocontido com
lições, exercícios pesquisados, revisões e exames periódicos, projetos pessoais
do mundo real, recordação socrática ao vivo e um motor de repetição espaçada FSRS que prevê
o que você provavelmente está esquecendo.

## Início rápido

1. **Planeje um assunto** — a partir desta pasta organizadora, inicie o Claude e execute:
   ```
   /teach Quero aprender sobre <assunto e o motivo> (Dica: personalize seu pedido ao máximo )
   ```
   Ele fará uma entrevista com você para entender o objetivo real, escolherá o seu idioma, pesquisará fontes confiáveis e comunidades (blogs, comunidades no Reddit, etc)
   e elaborará um rascunho de plano em `.claude/plans/<assunto>.md`.
2. **(Opcional) Refine o plano**
   ```
   /grill-me
   ```
   Refinamento implacável do plano, uma pergunta de cada vez.
3. **Construa a sala de aula**
   ```
   /new-classroom <assunto>
   ```
   Estrutura `./<assunto>/` com seu próprio `.claude/`, semeia MISSION/NOTES/RESOURCES,
   inicializa o cronograma e escreve sua **primeira lição + exercício**.
4. **Aprenda** — `cd <assunto>` e inicie um novo chat lá. O hook de sessão carrega
   o contexto do seu tutor automaticamente. Depois:
   - abra o HTML da lição (o tutor lhe dará um comando de uma linha),
   - faça o exercício, clique em **Finish** → um arquivo `…-submission.zip` será baixado,
   - diga **"revisar minhas respostas"** — o tutor irá avaliar, dar feedback e atualizar
     seu cronograma,
   - diga **"next"** para a próxima lição, ou **"quiz me"** (`/recall`) para um treino de fixação.

## Como o ensino funciona

- **Lições** ensinam uma única coisa vinculada à sua missão; cada uma inclui **5 perguntas
  pesquisadas** (~5–10 min cada) — nunca inventadas, sempre citadas.
- **A cada 5 lições** → uma **revisão** cumulativa (recordação ativa, casos de uso mistos).
- **A cada 10 lições** → um **exame** modelado em exames reais anteriores do MIT OCW / nível Ivy League
  (médio→muito difícil, ~20–30 min por pergunta, várias partes, focado em transferência de conhecimento).
- **Projetos pessoais** — uma entrega do mundo real vinculada a um objetivo da missão (enviar um
  repositório para o GitHub, construir e defender seu próprio programa de treinamento, correr uma maratona). Solicite
  um a qualquer momento, ou o tutor proporá um quando um marco da missão estiver ao alcance.
  Ele roda **em paralelo** com suas lições ao longo de semanas, com check-ins periódicos
  onde você envia evidências reais — o pilar "testar na prática" (Sabedoria / Wisdom).
- **FSRS** (`.claude/scripts/fsrs.py`) rastreia cada *subconceito* que você aprende,
  agenda revisões para quando se *prevê* que sua recordação caia (uma estimativa do modelo, não uma
  medição — previsões com poucas repetições são marcadas como incertas) e aposenta um tópico
  depois que ele se consolidar (~3 semanas). Longo intervalo entre as sessões? O tutor faz uma revisão primeiro.
  Depois de registrar revisões avaliadas suficientes (~50), o comando `fsrs.py optimize` reajusta os
  pesos do agendador ao *seu* histórico (mantendo o novo ajuste apenas se ele previr
  melhor), para que seus padrões — treinados em flashcards — se adaptem aos seus subconceitos.

## Estrutura (Layout)

```
<organizer>/.claude/
  skills/        teach (intake), grill-me (refine plan), new-classroom (scaffold)
  plans/         rascunhos de admissão, um por assunto
  templates/classroom/.claude/   o pacote copiado para cada sala de aula:
                 skills/ (teach, teach-init, recall), scripts/fsrs.py,
                 rules/ (pedagogy, question/exam research, grading), templates/ (html)
<organizer>/<subject>/           uma sala de aula (criada por /new-classroom)
  MISSION.md NOTES.md GLOSSARY.md RESOURCES.md PROGRESS.md course.json
  lessons/ exercises/ reviews/ exams/ projects/ answers/ learning-records/ reference/
  .claude/   (o pacote; o hook de sessão carrega automaticamente o contexto do seu tutor)
```

O motor FSRS é Python puro, sem dependências (`python .claude/scripts/fsrs.py
selftest` para fazer um teste de sanidade). Todo o resto é Markdown e HTML legível —
abra, leia e ajuste como quiser.
