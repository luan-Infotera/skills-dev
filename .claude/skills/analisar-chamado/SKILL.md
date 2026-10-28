---
name: analisar-chamado
description: Analisa um chamado/pedido do suporte, acha a regra no código e no histórico e devolve as opções de ajuste com recomendação
argument-hint: "<texto do chamado>"
---

Leia `CLAUDE.md` e `.claude/ESCOPO.md` antes de analisar. **Nunca executar commit, push, merge, criar PR ou publicar.** Esta skill é somente leitura e não executa testes.

Analise o chamado abaixo e diga ao dev o que deve ser feito. **Não altere nenhum arquivo.** Só analise e recomende.

Chamado: `$ARGUMENTS`

Se o chamado vier em imagem ou texto colado na conversa, use esse conteúdo.

## Como analisar

1. **Entenda o pedido.** Resuma em 1 frase o que o suporte/cliente quer. Separe o pedido do cliente, o comportamento atual comprovado e as hipóteses. Se o pedido estiver ambíguo, anote a dúvida para o final; não trate hipótese como regra confirmada.
2. **Ache a regra no código.** Procure as classes, métodos e telas (`.java` e `.xhtml`) envolvidos.
   - Lembre que no `.xhtml` o método aparece sem o prefixo (`isStPlanoDiamante()` → `stPlanoDiamante`, `getNmEmpresa()` → `nmEmpresa`).
   - Conte em quantos arquivos e lugares a regra aparece.
3. **Veja o histórico.** Procure se já houve ajuste parecido e siga o padrão do time:
   - `git log --oneline -S "<método ou texto>" -- src/main`
   - `git log --oneline --grep="<palavra-chave>"`
   - Abra os commits relevantes (`git show <hash>`) e veja como foi resolvido.
4. **Monte as opções reais.** Não invente alternativas para preencher o formato. Quando só houver um caminho sustentado pelas evidências, apresente apenas ele. Para cada opção: o que mudar, quantos arquivos, prós e contras. Considere também resolver por cadastro ou configuração, sem código, quando fizer sentido.
5. **Recomende uma.** Priorize, nessa ordem: seguir o padrão já usado no projeto, código claro para o próximo dev e diff pequeno.

## Saída

```
## Chamado
<o que foi pedido, em 1 frase>

## Comportamento atual e hipóteses
<o que o código comprova; hipóteses e dúvidas separadas, se houver>

## Onde está no código
- `Arquivo.java:linha`: <o que faz>

## Histórico
- `<hash>` <mensagem>: <como resolveram>  (ou "nenhum ajuste parecido encontrado")

## Opções
1. **<nome>** (<n> arquivos): <o que fazer>
   - Prós: ...
   - Contras: ...

## Recomendação
<qual opção e por quê, em 2-3 frases>

## Confirmar antes de fazer
- <dúvidas para o suporte/cliente, se houver>
```

Seja simples e direto. Não invente: se não achar algo, diga que não achou.
