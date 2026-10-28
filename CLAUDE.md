# Regras do projeto para o Claude Code

## Limites de atuação

- **Nunca executar commit, push ou merge, criar PR, publicar ou fazer deploy.** Isso inclui comandos Git, CLIs, APIs, ferramentas, scripts e automações que realizem essas ações indiretamente.
- Não executar `git add`, alterar o stage, fazer rebase, reset ou checkout para preparar entregas. A preparação e a entrega pelo Git ficam com o desenvolvedor.
- Mensagens de commit, títulos e descrições de PR podem ser sugeridos **somente como texto e quando solicitados**. A skill `/pr` apenas redige a descrição; não cria PR.
- Não interpretar pedidos de análise, revisão ou preparação de descrição como autorização para executar ações de entrega. Se o pedido exigir uma ação proibida, explique o limite e forneça somente a análise ou o texto permitido.
- Nas skills de análise, não alterar arquivos nem executar testes. Em tarefas de implementação solicitadas fora dessas skills, editar código e validar localmente conforme o pedido, respeitando os limites acima.
- Preservar alterações existentes do desenvolvedor. Não descartar nem sobrescrever trabalho alheio.

## Análise e comunicação

- Responder em português e sustentar conclusões com referências ao código.
- Distinguir achados confirmados, hipóteses e dúvidas de negócio. Não inventar justificativas nem resultados de testes.
- Para os escopos de análise das skills, seguir `.claude/ESCOPO.md`.
- Para revisão de código, seguir também `.claude/REVIEW.md`.
