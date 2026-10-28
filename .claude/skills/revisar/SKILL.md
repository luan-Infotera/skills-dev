---
name: revisar
description: Revisa o ajuste antes do commit (quebras, lazy, NullPointer, simplificação)
argument-hint: "[stage | local | branch [base] | arquivo <caminho>]"
---

Leia `CLAUDE.md` e `.claude/ESCOPO.md` antes de analisar. **Nunca executar commit, push, merge, criar PR ou publicar.** Esta skill é somente leitura e não executa testes.

Revise o ajuste do dev seguindo as regras de `.claude/REVIEW.md`. Leia esse arquivo primeiro.

## Qual diff revisar

Argumento recebido: `$ARGUMENTS`

Use os escopos de `.claude/ESCOPO.md`. Sem argumento, use `stage`; `tudo` continua sendo alias de `local` e um caminho direto seleciona o arquivo. Informe o escopo analisado e eventuais mudanças relevantes excluídas.

## Como revisar

1. Rode o comando git acima e identifique as linhas adicionadas/alteradas.
2. Para cada ponto suspeito, leia o contexto necessário (entidade, chamadores, DAO, `.xhtml`) para confirmar antes de perguntar.
3. Comente apenas o que está no diff (veja "Escopo" no `REVIEW.md`).
4. Não altere nenhum arquivo. Apresente achados confirmados com evidência; faça perguntas apenas para dúvidas reais de negócio. O dev decide se ajusta.

## Saída

```
## Revisão: <n> ponto(s)

<ícone> **[Alta/Média/Baixa] Arquivo.java:linha**: <problema ou dúvida de negócio>
   Cenário: <quando ocorre e qual a consequência>
   Evidência: <o que você verificou, com referência a arquivo:linha>
   Sugestão: <ajuste, quando sustentado pelo contexto>

🔍 **O que testar**
- [ ] <fluxo/tela>
```

Priorize bugs e regressões. Simplificações são opcionais e só devem aparecer quando trouxerem benefício concreto, separadas dos problemas. Se não houver achados, diga "Não encontrei problemas relevantes no escopo analisado" e informe limitações, sem garantir ausência de bugs.

Se o dev responder a uma pergunta, avalie a resposta: se fizer sentido, aceite. Se não, explique uma vez e deixe a decisão com ele.
