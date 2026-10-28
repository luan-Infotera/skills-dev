---
name: pr
description: Gera a descrição do pull request a partir dos commits da branch (só devolve o texto, não cria o PR)
argument-hint: "[branch [base] | stage | local | arquivo <caminho>]"
---

Leia `CLAUDE.md` e `.claude/ESCOPO.md` antes de analisar. **Nunca executar commit, push, merge, criar PR ou publicar.** Esta skill é somente leitura e não executa testes.

Gere a descrição do pull request da branch atual para o dev colar no "New pull request".

**Não crie o PR, não faça push, não rode `gh pr create` e não altere arquivos.** Só devolva o texto.

## De onde tirar as informações

Argumento recebido: `$ARGUMENTS`

1. Use os escopos de `.claude/ESCOPO.md`. Sem argumento, use `branch`; um nome de base direto continua aceito.
2. Em `branch`, leia os commits, o resumo e o diff em relação à base resolvida. Em outros escopos, use o diff correspondente e identifique o texto como rascunho baseado em alterações locais.
3. Procure os usos dos métodos alterados quando necessário para entender o impacto.
4. Se não houver alterações no escopo escolhido, avise e pare.
5. Em `branch`, se houver alterações locais relevantes, avise fora do bloco que elas não fazem parte da descrição. Não as incorpore como se estivessem commitadas.

## Como escrever

- Em português, simples e direto, sem enrolação.
- "O que foi feito": 1 ou 2 frases explicando o problema e a solução, depois uma lista curta dos arquivos/classes principais com o motivo de cada um. Agrupe arquivos parecidos (ex.: "Telas (xhtml): reserva, circuito, voo").
- "Como testar": de 2 a 4 passos práticos, do ponto de vista de quem usa a tela.
- "Impacto": quem é afetado e o que muda. Se algo mudar de comportamento sem configuração, diga isso. Só afirme que um comportamento permanece igual quando houver evidência no código.
- Não invente. Se não souber o motivo de uma mudança, escreva o que mudou e não o porquê.
- Separe validações comprovadamente executadas de testes sugeridos. Não atribua execução de teste a partir da existência de um teste ou de uma mensagem de commit; use evidências disponíveis na conversa ou resultados verificados. Sem evidência, escreva "Não executadas nesta análise".
- Não adicione assinatura nem rodapé.

## Saída

Devolva o bloco abaixo, dentro de um bloco de código markdown, pronto para colar. Antes dele, informe brevemente o escopo, a base e eventuais exclusões ou limitações:

```markdown
## 📝 Analise / O que foi feito
<problema e solução em 1-2 frases>

- `Classe`: <o que mudou>

## Validações executadas
<evidências disponíveis ou "Não executadas nesta análise">

## 🧪 Como testar (sugestões)
1. <passo>
2. <passo>
3. <passo>

## 📝 Impacto
- <quem é afetado e o que muda>
- <comportamento preservado, somente quando verificado>
```
