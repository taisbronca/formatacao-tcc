# Formatação de TCC para Codex e Claude

Skill não oficial para auditar documentos `.docx` de Projeto de Pesquisa, Resultados Preliminares e TCC dos cursos de especialização e MBA USP/Esalq. Pode ser utilizada no Codex, no Claude Code e, mediante upload, no Claude web ou desktop.

A auditoria compara o documento com o *Manual de Instruções e Normas para Trabalhos de Conclusão de Curso*, edição do 2º semestre de 2025. Ela apresenta os problemas ponto a ponto e não modifica o arquivo durante a primeira análise.

## O que compõe a skill

Uma skill mínima precisa apenas de uma pasta contendo `SKILL.md`. Este repositório inclui recursos adicionais porque a auditoria é mais complexa:

```text
formatacao-tcc/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── manual-rules.md
│   └── review-edit-workflow.md
└── scripts/
    └── audit_docx.py
```

- `SKILL.md`: ponto de entrada e regras de comportamento.
- `agents/openai.yaml`: nome, descrição e exemplo de prompt exibidos pelo Codex; o Claude ignora este arquivo.
- `references/manual-rules.md`: requisitos normativos organizados por assunto e página.
- `references/review-edit-workflow.md`: formato do relatório, níveis de gravidade e limites de edição.
- `scripts/audit_docx.py`: pré-auditoria estrutural reproduzível do DOCX.

`README.md`, `LICENSE`, `NOTICE.md` e `.gitignore` pertencem ao repositório, mas não são necessários para executar a skill.

## Como instalar e usar

Baixe ou instale somente a skill. O TCC não deve ser colocado neste repositório: anexe o `.docx` diretamente na conversa em que a auditoria será executada.

### Codex

Abra uma tarefa no Codex e peça a instalação:

```text
$skill-installer instale a skill deste repositório: https://github.com/taisbronca/formatacao-tcc
```

Se a skill não aparecer depois da instalação, abra uma nova tarefa ou reinicie o Codex. Para chamá-la explicitamente, digite:

```text
$formatacao-tcc
```

Como alternativa, é possível clonar o repositório diretamente na pasta de skills.

#### macOS ou Linux

```bash
git clone https://github.com/taisbronca/formatacao-tcc.git "${CODEX_HOME:-$HOME/.codex}/skills/formatacao-tcc"
```

#### Windows PowerShell

```powershell
git clone https://github.com/taisbronca/formatacao-tcc.git "$env:USERPROFILE\.codex\skills\formatacao-tcc"
```

### Claude web ou desktop

1. No GitHub, abra o menu **Code** e selecione **Download ZIP**.
2. No Claude, acesse **Settings > Features > Skills**.
3. Envie o arquivo ZIP baixado.
4. Abra uma nova conversa, anexe o TCC em `.docx` e mencione a skill `formatacao-tcc` no pedido.

O upload de skills personalizadas no Claude depende de um plano compatível e da execução de código habilitada.

### Claude Code

No macOS ou Linux, clone a skill na pasta pessoal do Claude Code:

```bash
git clone https://github.com/taisbronca/formatacao-tcc.git "$HOME/.claude/skills/formatacao-tcc"
```

No Windows PowerShell:

```powershell
git clone https://github.com/taisbronca/formatacao-tcc.git "$env:USERPROFILE\.claude\skills\formatacao-tcc"
```

Depois, abra o Claude Code e execute:

```text
/formatacao-tcc
```

No Claude Code, o script estrutural requer Python e `python-docx`. A renderização visual completa também pode depender de um editor compatível, como LibreOffice ou Microsoft Word. Por isso, o resultado pode variar de acordo com as ferramentas instaladas no computador.

## Uso

### Prompt para gerar o relatório de auditoria

Anexe o trabalho em `.docx` e use o prompt abaixo. Troque o valor de **Etapa** por `Projeto de Pesquisa`, `Resultados Preliminares` ou `TCC final`.

```text
Use a skill formatacao-tcc para auditar o documento anexado.

Etapa: TCC final
Curso: [nome do curso]
Idioma: português

Não edite o arquivo. Compare-o ponto a ponto com o manual e apresente:
- gravidade do problema;
- página ou localização;
- situação encontrada;
- exigência, item e página do manual;
- correção sugerida;
- possibilidade de correção automática;
- confiança da detecção.

Ao final, gere um relatório completo em PDF. Caso o ambiente não permita criar PDF, gere o mesmo relatório em Markdown.
```

Para garantir a chamada explícita, substitua a primeira linha conforme o ambiente:

- Codex: `Use $formatacao-tcc para auditar o documento anexado.`
- Claude Code: execute `/formatacao-tcc` e envie o restante do prompt.
- Claude web ou desktop: mantenha `Use a skill formatacao-tcc...`.

### Aplicar correções posteriormente

Primeiro leia o relatório. Se concordar com os ajustes, faça um segundo pedido:

```text
Aplique apenas as correções automáticas e seguras identificadas no relatório. Preserve o arquivo original, gere uma nova cópia e não reescreva conteúdo acadêmico sem minha autorização.
```

Também é possível autorizar somente itens específicos:

```text
Aplique somente as correções dos itens [informe os IDs]. Preserve o original e gere uma nova cópia.
```

## Compatibilidade

- O Codex chama skills explicitamente com `$nome-da-skill`.
- O Claude Code chama skills explicitamente com `/nome-da-skill`.
- O Claude web ou desktop permite o upload de skills personalizadas em formato ZIP nos planos compatíveis.
- O arquivo `agents/openai.yaml` é específico do Codex e não interfere no uso pelo Claude.

Documentação oficial:

- [Skills no Codex](https://developers.openai.com/codex/skills/)
- [Skills no Claude Code](https://docs.claude.com/en/docs/claude-code/skills)
- [Agent Skills no Claude](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)

## Auditoria estrutural opcional

O script pode ser executado separadamente quando Python e `python-docx` estiverem disponíveis:

```bash
python scripts/audit_docx.py caminho/para/trabalho.docx --stage tcc --json auditoria.json
```

Valores aceitos em `--stage`:

- `tcc`
- `resultados-preliminares`
- `projeto`

O JSON é apenas uma pré-auditoria. Contagem de páginas, quebras, cabeçalhos, rodapés, tabelas, figuras e equações precisam de renderização e inspeção visual.

## Privacidade

Não envie TCCs, relatórios de auditoria, documentos do comitê de ética ou o manual institucional para o repositório. O `.gitignore` bloqueia por padrão arquivos `.docx`, `.doc`, `.pdf` e saídas comuns de revisão.

## Escopo e responsabilidade

- Este é um projeto acadêmico independente e não oficial.
- A skill não substitui o orientador, a banca, o comitê de ética nem a coordenação do curso.
- O manual institucional não está incluído no repositório.
- As regras foram organizadas a partir da edição do 2º semestre de 2025. Edições posteriores devem ser revisadas antes do uso.
- Recomendações do manual são apresentadas como recomendações; não são convertidas em proibições.

## Contribuições

Correções devem indicar a regra, o item e a página da edição do manual utilizada. Evite adicionar documentos de estudantes, dados pessoais, grandes trechos do manual ou requisitos genéricos da ABNT que não estejam previstos pela instituição.
