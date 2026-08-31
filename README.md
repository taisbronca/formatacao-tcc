# Formatação de TCC para Codex

Skill não oficial para auditar documentos `.docx` de Projeto de Pesquisa, Resultados Preliminares e TCC dos cursos de especialização e MBA USP/Esalq.

A auditoria compara o documento com o *Manual de Instruções e Normas para Trabalhos de Conclusão de Curso*, edição do 2º semestre de 2025. Ela apresenta os problemas ponto a ponto e não modifica o arquivo durante a primeira análise. Alterações são feitas somente mediante solicitação explícita e sempre em uma cópia.

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
- `agents/openai.yaml`: nome, descrição e exemplo de prompt exibidos pelo Codex.
- `references/manual-rules.md`: requisitos normativos organizados por assunto e página.
- `references/review-edit-workflow.md`: formato do relatório, níveis de gravidade e limites de edição.
- `scripts/audit_docx.py`: pré-auditoria estrutural reproduzível do DOCX.

`README.md`, `LICENSE`, `NOTICE.md` e `.gitignore` pertencem ao repositório, mas não são necessários para executar a skill.

## Instalação

Copie a URL fornecida pelo GitHub e clone o repositório diretamente na pasta de skills.

### macOS ou Linux

```bash
git clone URL_COPIADA_DO_GITHUB "${CODEX_HOME:-$HOME/.codex}/skills/formatacao-tcc"
```

### Windows PowerShell

```powershell
git clone URL_COPIADA_DO_GITHUB "$env:USERPROFILE\.codex\skills\formatacao-tcc"
```

Depois da instalação, abra uma nova tarefa no Codex para que a skill seja descoberta.

## Uso

Primeiro, peça somente a auditoria:

```text
Use $formatacao-tcc para auditar este TCC final do MBA em Gestão de Projetos. Mostre os problemas ponto a ponto, mas não edite o arquivo.
```

Para outras etapas, informe o contexto:

```text
Use $formatacao-tcc para revisar este Projeto de Pesquisa. O texto ainda descreve atividades futuras.
```

Depois de avaliar o relatório, autorize somente as mudanças desejadas:

```text
Aplique as correções de formatação classificadas como automáticas e seguras. Preserve o original e gere uma cópia revisada.
```

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
