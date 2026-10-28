---
name: testes
description: Gera o checklist de testes (para o dev ou QA) a partir do que foi alterado na branch
argument-hint: "[stage | local | branch [base] | arquivo <caminho>]"
---

Leia `CLAUDE.md` e `.claude/ESCOPO.md` antes de analisar. **Nunca executar commit, push, merge, criar PR ou publicar.** Esta skill é somente leitura e não executa testes.

Gere um checklist de testes a partir das alterações. **Não altere nenhum arquivo e não rode testes.**

Argumento recebido: `$ARGUMENTS`

## De onde tirar as informações

1. Use os escopos de `.claude/ESCOPO.md`. Sem argumento, analise `branch` e `local`, identificando cada conjunto separadamente.
2. Informe o escopo, a base quando aplicável e arquivos relevantes excluídos.
3. Para cada alteração, descubra a tela ou o fluxo onde ela aparece:
   - `.xhtml` alterado → qual tela é (pelo caminho e pelo menu).
   - `.java` alterado → procure quem chama até chegar numa tela, num serviço (`ws/`) ou numa rotina.

## Como escrever

- Linguagem de quem usa o sistema: "Abrir a reserva e clicar em Clonar", não "chamar `iniciarClonar()`".
- Agrupe por tela ou fluxo.
- Para cada caso, diga o que deve acontecer (resultado esperado).
- Inclua o **teste de regressão**: quem não deveria ser afetado (outro plano, outro perfil, outra empresa) e confirme que continua igual.
- Se o teste depender de cadastro ou configuração (plano, parâmetro, perfil), diga qual junto ao cenário ou ao grupo correspondente.
- Priorize os cenários de maior risco e mantenha o checklist curto; ultrapasse 10 linhas quando necessário para cobrir riscos distintos. Não omita isolamento entre empresas, permissões ou cálculo de valores apenas para caber no limite.
- Identifique o checklist como testes sugeridos, não executados. Se o resultado esperado depender de regra de negócio não confirmada, marque essa dependência em vez de inventá-lo.

## Saída

```
## Testes sugeridos (não executados)

### <Tela ou fluxo>
**Pré-requisito:** <empresa/plano/parâmetro/perfil necessário para este grupo>
- [ ] <ação> → <resultado esperado>

### Regressão
- [ ] <cenário que não deve mudar> → continua igual
```
