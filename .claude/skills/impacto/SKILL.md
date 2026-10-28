---
name: impacto
description: Mostra quem usa um método/campo (ou o que foi alterado no diff) e o que muda em cada lugar
argument-hint: "[metodo <nome> | stage | local | branch [base] | arquivo <caminho>]"
---

Leia `CLAUDE.md` e `.claude/ESCOPO.md` antes de analisar. **Nunca executar commit, push, merge, criar PR ou publicar.** Esta skill é somente leitura e não executa testes.

Mostre ao dev o impacto de uma alteração. **Não altere nenhum arquivo.**

Argumento recebido: `$ARGUMENTS`

## O que analisar

Use os escopos de `.claude/ESCOPO.md`. Sem argumento, analise `branch` e `local`, identificando cada conjunto separadamente. Para um símbolo, aceite `metodo <nome>` ou o nome direto quando não for caminho nem escopo; deixe claro quando a análise for de usos atuais, sem diff para comprovar antes/depois.

## Como analisar

1. Para cada método ou campo, procure quem usa em `.java` e `.xhtml`.
   - No `.xhtml` o nome aparece sem prefixo (`isStX()` → `stX`, `getNomeX()` → `nomeX`).
   - Se o método é usado por outro método (ex.: `EmpresaPlanoPermissao`), siga também quem usa esse outro.
2. Em cada lugar, leia o contexto e classifique:
   - **Muda sozinho**: o comportamento muda sem ninguém configurar nada.
   - **Só libera opção**: depende de parâmetro, cadastro ou perfil para ter efeito.
   - **Sem efeito confirmado**: código comentado ou condição comprovadamente sem mudança de resultado.
   - **Impacto não confirmado**: usos indiretos, reflexão, EL/JSF ou configuração não esclarecidos. Não encontrar referências não prova que o código não é usado.
3. Dê atenção especial a valores (markup, comissão, tarifa), status, e-mails/notificações, permissões e fluxo corporativo.

## Saída

```
## Impacto: <método/arquivo>

### Muda sozinho
- `Arquivo.java:linha`: <tela/fluxo> — antes: <...> / agora: <...>

### Só libera opção
- `Arquivo.java:linha`: <tela/fluxo> — depende de <parâmetro/perfil/cadastro>

### Sem efeito confirmado
- `Arquivo.java:linha`: <motivo>

### Impacto não confirmado
- `Arquivo.java:linha`: <incerteza e o que falta verificar>

## Resumo
<quebra algo? o que precisa ser avisado/testado? em 1-3 frases>
```

Omita as seções vazias. Seja simples e direto.
