# Fluxo de auditoria e edição

## Dependência de documentos

Use a skill de documentos disponível no ambiente para leitura, edição e renderização de `.docx`. Resolva o runtime empacotado e use o `render_docx.py` canônico. Antes da primeira edição, cumpra o marcador de operação exigido por essa skill. Auditoria somente leitura não deve disparar marcador de edição.

## Auditoria

1. Calcule SHA-256 do original para comprovar preservação.
2. Extraia estrutura do DOCX com `python-docx`/OOXML e execute:

   ```bash
   python scripts/audit_docx.py trabalho.docx --json relatorio-estrutural.json
   ```

3. Renderize o documento em diretório temporário e examine toda página.
4. Valide as observações do script. Transforme em não conformidade somente quando a evidência e a regra forem claras.
5. Use a seguinte gravidade:
   - `crítica`: risco ético, privacidade, plágio aparente, arquivo corrompido ou ausência de parte obrigatória central;
   - `alta`: regra obrigatória/proibição com impacto institucional, como limite de páginas, estrutura, citações diretas, margens ou ausência de referências;
   - `média`: formatação obrigatória localizada, legenda, fonte, recuo, alinhamento, espaçamento;
   - `baixa`: inconsistência pequena, sem prejuízo de leitura;
   - `recomendação`: item que o manual apresenta como preferência ou boa prática.
6. Para cada achado, registre:

   | Campo | Conteúdo |
   |---|---|
   | ID | identificador estável, como `FMT-003` |
   | Gravidade | crítica/alta/média/baixa/recomendação |
   | Natureza | obrigatório/proibido/condicional/recomendado |
   | Localização | página renderizada e parágrafo/tabela/figura quando possível |
   | Observado | evidência objetiva |
   | Esperado | regra resumida |
   | Fonte | item e página do manual |
   | Sugestão | correção concreta |
   | Autoajuste | sim/não/depende |
   | Confiança | alta/média/baixa |

## Classificação de correções

### Normalmente seguras após autorização geral para formatar

- A4/retrato e margens de 2,5 cm;
- Arial 11/preto no corpo e exceções explícitas;
- alinhamento, espaçamento e recuo;
- estilos de títulos/subtítulos, remoção de numeração de seções;
- paginação em campo Word, cabeçalho/rodapé, desde que curso/ano/logo já estejam disponíveis;
- posição de legenda, fonte e nota sem alterar seu texto factual;
- bordas, alinhamentos e repetição de cabeçalho de tabelas;
- remoção de instruções residuais inequívocas do template, depois de mostrar ao usuário o que será removido.

### Exigem confirmação específica ou edição intelectual

- abreviar/reformular o título;
- reescrever Resumo, Introdução, Resultados, Discussão ou Conclusão;
- converter voz/pessoa/tempo verbal em massa;
- remover/alterar citação, referência, número, fórmula ou resultado;
- anonimizar participante/empresa ou inserir informação ética;
- escolher entre Conclusão e Considerações Finais;
- escolher a variante de curso ou limite de páginas;
- criar texto que esteja ausente.

## Edição

1. Gere uma cópia; não sobrescreva.
2. Faça alterações mínimas e rastreáveis. Preserve seções, tabelas, imagens, campos, comentários e alterações controladas que não façam parte do pedido.
3. Se a mudança puder afetar o sentido, use comentário ou alteração controlada quando disponível, em vez de substituir silenciosamente.
4. Não aplique um preset visual genérico: os tokens do manual são o sistema de design.
5. Prefira estilos nomeados e OOXML válido. Não use espaços/tabs para layout, bullets falsos ou números digitados no rodapé.
6. Reexecute a auditoria estrutural e compare o antes/depois.
7. Renderize todas as páginas da cópia final e verifique:
   - contagem e sequência de páginas;
   - margens, cabeçalho e rodapé;
   - ausência de corte, sobreposição e páginas vazias acidentais;
   - títulos junto ao conteúdo pertinente;
   - tabelas sem estouro e com cabeçalhos repetidos;
   - figuras legíveis e legendas/fontes próximas;
   - equações e símbolos intactos;
   - referências sem quebras ou recuos inconsistentes.

## Resultado

No modo auditoria, entregue o relatório com totais por gravidade e uma proposta de lote de correções. No modo edição, entregue a cópia final e uma tabela de mudanças associada aos IDs do relatório. Informe separadamente tudo o que continuou pendente por exigir decisão ou conteúdo do estudante.
