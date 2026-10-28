# skills-dev

Skills de análise para o Claude Code, originadas no Infotravel (Java, JSF/XHTML e Hibernate).

## Instalação em cada projeto

Na raiz do projeto que será analisado, copie a pasta `.claude` deste repositório e o arquivo `CLAUDE.md`. A estrutura final deve ser:

```text
seu-projeto/
├── CLAUDE.md
└── .claude/
    ├── ESCOPO.md
    ├── REVIEW.md
    └── skills/
        ├── analisar-chamado/SKILL.md
        ├── impacto/SKILL.md
        ├── pr/SKILL.md
        ├── revisar/SKILL.md
        ├── seguranca/SKILL.md
        └── testes/SKILL.md
```

A grafia é `skills`, não `skiils`. Não basta copiar só a pasta `skills`: as instruções também dependem de `CLAUDE.md`, `ESCOPO.md` e `REVIEW.md`.

Se o projeto já tiver esses arquivos ou skills com os mesmos nomes, compare e mescle o conteúdo antes de copiar. Preserve instruções, configurações e skills existentes. Se já houver `CLAUDE.md`, incorpore as regras deste modelo nele, sem substituir as regras específicas do projeto.

Para um projeto que ainda não tenha `.claude` nem `CLAUDE.md`, estes comandos podem ser executados manualmente a partir da raiz dele (ajuste o caminho da origem):

```bash
cp -R /caminho/skills-dev/.claude ./.claude
cp /caminho/skills-dev/CLAUDE.md ./CLAUDE.md
```

Abra o Claude Code na raiz do projeto, usando `claude` no terminal, ou inicie uma nova sessão na integração que você utiliza. Digite `/` e procure os nomes das skills; depois invoque uma delas. Não é necessário instalar dependências deste pacote.

## Como usar

Os comandos abaixo são digitados na conversa do Claude Code, não no terminal:

| Comando | Resultado |
| --- | --- |
| `/analisar-chamado <texto do chamado>` | Localiza a regra, consulta o histórico e recomenda ajustes sem editar código. |
| `/revisar local` | Revisa alterações locais, incluindo arquivos novos pertinentes. |
| `/revisar stage` | Revisa apenas o que o desenvolvedor já colocou no stage. |
| `/seguranca local` | Procura riscos nas alterações locais, incluindo Java e XHTML. |
| `/impacto metodo isStModuloCorporativo` | Investiga os usos e o impacto do símbolo informado. |
| `/testes local` | Sugere checklist de testes; não executa testes. |
| `/pr branch origin/main` | Gera somente o texto da descrição, considerando a base informada. |

Também são aceitos `branch [base]` e `arquivo <caminho>` nas skills de alterações. `tudo` é alias de `local`. A referência base precisa existir localmente: as skills não executam fetch automaticamente.

Sem argumentos: `/revisar` usa `stage`; `/seguranca` e `/pr` usam `branch`; `/impacto` e `/testes` analisam `branch` e `local` separadamente. Consulte `.claude/ESCOPO.md` para as regras completas.

## Adaptação para outros projetos

Antes de usar em outra arquitetura, adapte `.claude/REVIEW.md` e as instruções de domínio das skills. Caminhos como `faces/`, `service/`, `dao/`, entidades Hibernate, regras de plano e isolamento entre empresas são específicos do Infotravel. Não assuma que descrevem outro sistema.

Mantenha as regras de escopo, exigência de evidências, preservação do trabalho existente e proibição de ações de entrega. Para incluir uma nova skill, crie `.claude/skills/<nome>/SKILL.md` com metadados `name` e `description` entre delimitadores YAML `---`, seguidos das instruções. Use as existentes como exemplo.

## Limites

**Nunca executar commit, push, merge, criar PR, publicar ou fazer deploy**, inclusive por scripts, APIs e automações. Não alterar o stage nem preparar entregas pelo Git. A skill `/pr` só fornece texto quando solicitada.

Estas seis skills são somente leitura: não alteram arquivos nem executam testes. A regra não impede implementação e validação local quando solicitadas fora dessas skills, respeitando os limites de entrega.

Esses arquivos definem instruções de comportamento, não um bloqueio técnico de ferramentas. O pacote não inclui regras de permissões nem hooks para impedir comandos.

## Se não aparecerem

- Confira se o caminho é `.claude/skills/<nome>/SKILL.md`, com o nome do arquivo correto.
- Confira os metadados no início do arquivo e se você abriu o projeto correto.
- Inicie uma nova sessão após copiar os arquivos.
- Verifique conflitos com skills de mesmo nome e políticas do ambiente que possam desabilitá-las.

Documentação oficial: [Skills no Claude Code](https://code.claude.com/docs/en/skills).
