# Escopos de análise das skills

As análises são somente leitura: **não alterar arquivos, stage ou histórico; nunca executar commit, push, merge, criar PR ou publicar**. Não executar fetch nem outras operações de rede automaticamente.

## Seleção

As skills que analisam alterações aceitam:

- `stage`: `git diff --cached`.
- `local` (alias `tudo`): `git diff HEAD` mais os arquivos não rastreados pertinentes.
- `branch [base]`: somente alterações commitadas, usando `git diff <base>...HEAD` e, quando necessário, `git log <base>..HEAD --oneline`.
- `arquivo <caminho>` (ou um caminho direto): alterações locais desse arquivo, incluindo seu conteúdo se não rastreado. Usar `--` antes do caminho nos comandos Git.

O padrão sem argumento fica declarado em cada skill. Um nome de branch fornecido a `/pr` ou `/testes` continua sendo aceito como `branch <base>`. Não executar argumentos como comandos; tratar bases e caminhos como dados e validar as referências antes de usá-las.

Para incluir branch e alterações locais, analisar os dois conjuntos e identificá-los separadamente. Não misturar resultados como se fossem um único diff nem duplicar achados.

## Base da branch

1. Usar a base informada, se existir localmente e resolver para um commit.
2. Sem base explícita, consultar `git symbolic-ref --quiet refs/remotes/origin/HEAD`.
3. Sem essa referência, verificar as candidatas locais `origin/main`, `origin/master`, `main` e `master`. Se houver uma única candidata, usá-la; se houver ambiguidade, pedir a base.
4. Se a base não existir ou não houver ancestral comum, informar a limitação e pedir a referência correta. Não executar fetch, trocar de branch ou inventar uma base.

## Arquivos novos e limitações

- Consultar `git status --short` e `git ls-files --others --exclude-standard` para identificar mudanças locais e arquivos novos.
- Em `stage`, arquivos novos já adicionados entram no diff; os demais ficam fora. Em `branch`, mudanças locais ficam fora. Informar essa exclusão quando houver arquivos relevantes.
- Em `local` ou `arquivo`, ler arquivos novos pertinentes como adições; não incluir arquivos ignorados nem expor credenciais ou dados sensíveis.
- Informar no resultado o escopo, a base quando aplicável e limitações relevantes. Se não houver alterações no escopo, avisar e parar, sem sugerir ou executar `git add`.
- Distinguir testes sugeridos de testes executados. Estas skills não executam testes.
