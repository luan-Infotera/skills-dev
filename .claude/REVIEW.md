# Padrões de revisão do Infotravel

Regras usadas pelo `/revisar` (local) e pelo robô de PR.

## Escopo

- Analise APENAS as linhas adicionadas/alteradas no diff.
- Você pode ler outros arquivos para entender o contexto (entidades, chamadores, DAOs), mas NÃO comente neles.
- Problemas pré-existentes fora do diff: ignore.
  - Exceção: se a linha nova depende diretamente de um problema existente, comente na linha nova.

## Formato

- Escreva em português.
- Descreva bugs confirmados de forma direta, com cenário, consequência e evidência. Reserve perguntas para dúvidas reais de regra de negócio; diferencie hipóteses de achados confirmados.
- Informe severidade conforme o impacto e as condições necessárias para a falha. Priorize bugs e regressões; simplificações são sugestões opcionais e ficam separadas.
- No máximo 5 pontos, os mais relevantes primeiro.
- Cite arquivo e linha (`Arquivo.java:123`). Quando possível, cite um exemplo do próprio Infotravel que segue o padrão sugerido.
- Use os ícones: 🔍 quebra/impacto, 💤 lazy, ⚠️ NullPointer, 💡 simplificação.
- Se não houver nada relevante, diga que não encontrou problemas no escopo analisado e informe limitações aplicáveis. Não invente ponto para preencher nem garanta ausência de bugs.

## Arquitetura do projeto

- `faces/` (JSF, ManagedBeans) → `service/` (`*Srv`, EJB `@Stateless`) → `dao/` → `entity/` (JPA/Hibernate).
- Transação: o padrão dos `*Srv` é `@TransactionAttribute(REQUIRED)`. Não deduza a transação apenas pelo nome da classe: confira anotações, overrides, chamada via proxy EJB e configuração. Faces, threads, servlets e `ws/` podem estar fora dela; métodos `@Asynchronous` de EJB podem iniciar sua própria transação conforme os atributos configurados.

## 🔍 Quebras / impacto

- Para cada método, campo ou assinatura alterados, procure quem usa (`.java` e `.xhtml`).
- Aponte os fluxos/telas afetados e sugira o que testar.
- Atenção a mudanças em: cálculo de valores (markup, comissão, tarifa, aprovação), status, notificações/e-mail, permissões.
- Mudança de comportamento em método compartilhado: pergunte se todos os chamadores esperam o novo comportamento.

## 💤 Lazy (Hibernate)

- Acesso a relacionamento `FetchType.LAZY` na linha nova: confira na entidade.
- Verifique o contexto transacional real do acesso e a sessão que carregou a entidade, inclusive em chamadas assíncronas; não classifique apenas pelo tipo da classe.
- Verifique se a query que carregou a entidade faz `JOIN FETCH` do relacionamento (padrão muito usado no projeto).
- Confira também se o relacionamento já foi inicializado e como a entidade chega ao acesso. Só aponte falha confirmada quando o caminho sustentar a entidade desconectada com relacionamento não inicializado; caso contrário, informe a incerteza.

## ⚠️ NullPointer

- Cadeias `getX().getY().getZ()` em relacionamento opcional precisam de verificação de null.
- Retornos de `buscarPor...()` / `find...()` podem ser null: verifique antes de usar.
- Wrappers (`Boolean`, `Integer`, `Double`, `BigDecimal`) podem ser null: atenção a unboxing (`if (obj.getStAlgo())`, comparações, aritmética).
- Siga o padrão de verificação já usado no mesmo arquivo.

## 💡 Simplificação

- `if` com muitas condições (`&&`/`||`): sugira separar ou extrair para variáveis com nome (`boolean ehOperadorComReserva = ...`).
- Evite criar classes internas dentro de `*Srv` para carregar dados quando já existe entidade/DTO que resolve.
  - Pode fazer sentido quando o dado só existe naquele método. Depende do contexto, então pergunte.
- Antes de criar utilitário novo, verifique se já existe em `util/` (`*Utils`, `*Util`).
- Siga o estilo do arquivo alterado (nomes, comentários, formatação).
