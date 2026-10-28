---
name: seguranca
description: Verifica se o ajuste abriu brecha de segurança (SQL injection, XSS, permissão, acesso a dados de outra empresa, dados sensíveis)
argument-hint: "[stage | local | branch [base] | arquivo <caminho>]"
---

Leia `CLAUDE.md` e `.claude/ESCOPO.md` antes de analisar. **Nunca executar commit, push, merge, criar PR ou publicar.** Esta skill é somente leitura e não executa testes.

Verifique se as alterações abriram alguma brecha de segurança. **Não altere nenhum arquivo.** Aponte achados com evidências e diferencie dúvidas de riscos confirmados.

Argumento recebido: `$ARGUMENTS`

## Qual diff analisar

Use os escopos de `.claude/ESCOPO.md`. Sem argumento, use `branch`. Informe o escopo e as limitações.

Analise **apenas as linhas adicionadas ou alteradas**. Pode ler outros arquivos para entender o contexto, mas não comente problema antigo fora do diff.
Se a alteração depender diretamente de uma falha existente e expuser um novo caminho de exploração, aponte na linha nova e explique essa relação.

## O que procurar

1. **SQL injection.** O projeto monta muita query concatenando string (`query += "... " + valor`).
   - Valor efetivamente tipado como número reduz o risco de injeção naquele trecho, mas não comprova autorização, isolamento entre empresas ou validade do identificador. Confirme origem e tipo; prefira parâmetros.
   - `String` vinda do usuário concatenada: risco. Confira a implementação atual de `Dao.ajustaLike()` e seus usos; não presuma que ele torne concatenação segura. Se apenas remove `'` e corta em 50 caracteres, isso não comprova segurança; prefira parâmetros também em `LIKE`.
   - Prefira `setParameter(...)`, que já é usado no projeto.
2. **XSS.** `escape="false"`, `h:outputText` com HTML ou montagem de HTML/JS com dado digitado pelo usuário (nome, observação, descrição). Verifique a implementação de `Utils.ajustaTagsHtml` ou equivalente e a adequação ao contexto de saída (HTML, atributo, JavaScript ou URL); o nome da função não comprova proteção.
3. **Permissão.** Funcionalidade nova ou liberada:
   - Está protegida só pelo `rendered` da tela? Ações sensíveis (valor, cancelamento, emissão, aprovação) também deveriam validar no Face ou Srv.
   - A regra de plano, perfil ou `permissaoMenu` está igual nos dois lugares (tela e back-end)?
4. **Dados de outra empresa ou cliente (IDOR).** Busca por id vindo da tela, URL ou `ws/` sem filtrar pela empresa, unidade ou cliente da sessão. Atenção redobrada em `stMultiEmpresa` e no fluxo corporativo (`idClienteBase`, `stCorp`).
5. **Dados sensíveis.** Senha, token, cartão (número/CVV) ou documento em log, mensagem de erro, e-mail ou resposta de API. Credencial ou chave fixa no código.
6. **Endpoints (`ws/`, servlets).** Endpoint novo ou alterado sem autenticação ou sem validar a entrada.

## Formato

- Em português, descreva o problema confirmado de forma direta. Use pergunta apenas quando houver dúvida de negócio; hipótese sem caminho de exploração demonstrado deve ser identificada como incerteza.
- No máximo 5 pontos, os mais graves primeiro.
- Cite `Arquivo.java:linha` e explique o cenário de ataque em 1 frase (quem faz o quê e o que consegue).
- Não invente risco: comprove origem, validação e caminho de exploração antes de apontar uma vulnerabilidade; avalie autorização separadamente do risco de injeção.

## Saída

```
## Segurança: <n> ponto(s)

🔒 **[Alta/Média/Baixa] Arquivo.java:linha**: <problema confirmado ou dúvida>
   Cenário: <como alguém exploraria>
   Contexto: <o que você verificou>
```

Se não houver nada relevante, informe as limitações aplicáveis e responda: "Não identifiquei vulnerabilidades no escopo analisado."
