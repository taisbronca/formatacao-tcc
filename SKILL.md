---
name: formatacao-tcc
description: Audita documentos DOCX de TCC, Resultados Preliminares e Projeto de Pesquisa conforme o manual institucional coberto pela skill; produz diagnóstico ponto a ponto e, somente após autorização explícita, cria uma cópia corrigida e visualmente verificada. Não use como revisor ABNT genérico nem sem confirmar que o trabalho segue esse manual.
---

# Formatação e revisão de TCC

Use esta skill para comparar um `.docx` com o manual institucional e, em um segundo momento, aplicar as correções autorizadas. O manual é a autoridade; preferências estéticas genéricas não podem substituí-lo.

## Antes de começar

1. Confirme que o arquivo é `.docx`. Para `.doc`, peça uma versão `.docx` ou converta sem alterar o original.
2. Identifique, pelo pedido ou pelo próprio arquivo:
   - etapa: Projeto de Pesquisa, Resultados Preliminares ou TCC final;
   - curso;
   - se é um TCC de Data Science e Analytics ou Engenharia de Software sobre implementação de algoritmo de Machine Learning;
   - idioma e eventual adesão ao Plano Internacional.
3. Se uma dessas informações afetar a regra e não puder ser inferida com segurança, registre a pendência; não aplique a exceção por suposição.
4. Leia [references/manual-rules.md](references/manual-rules.md) para qualquer auditoria ou edição.
5. Leia [references/review-edit-workflow.md](references/review-edit-workflow.md) antes de produzir o relatório ou modificar o DOCX.

## Autoridade e interpretação

- Base normativa: *Manual de Instruções e Normas para Trabalhos de Conclusão de Curso*, MBA USP/Esalq, 2º semestre de 2025, 67 páginas.
- Classifique cada item como `obrigatório`, `proibido`, `condicional` ou `recomendado` conforme o manual. Não transforme recomendações em reprovações.
- Cite no relatório o item e a página do manual. Se houver diferença entre um exemplo e uma regra textual, registre a ambiguidade e prefira a regra textual explícita.
- Não trate textos instrucionais do template como conteúdo do estudante. Ao contrário, sinalize-os para remoção antes da entrega.
- Não invente exigências da ABNT. Este manual declara normas próprias da instituição.

## Modo padrão: auditoria sem edição

Uma solicitação para “revisar”, “conferir”, “avaliar” ou “ver o que está errado” autoriza somente leitura e diagnóstico.

1. Preserve o arquivo original.
2. Execute `scripts/audit_docx.py` para obter observações estruturais reproduzíveis. Use o runtime de documentos disponibilizado pelo ambiente.
3. Inspecione diretamente o conteúdo e a estrutura OOXML para confirmar as observações. O script é auxiliar; não é a decisão final.
4. Renderize o DOCX e examine todas as páginas em PNG a 100%. A paginação, quebras, cabeçalhos, rodapés, figuras, tabelas e equações só podem ser julgados definitivamente após a renderização.
5. Compare o trabalho ponto a ponto com a referência normativa.
6. Entregue primeiro um resumo executivo e depois uma matriz de não conformidades com:
   - gravidade: `crítica`, `alta`, `média`, `baixa` ou `recomendação`;
   - natureza da regra;
   - localização precisa no DOCX;
   - situação observada;
   - exigência do manual, com item e página;
   - mudança sugerida;
   - `correção automática segura`: sim, não ou depende de confirmação;
   - confiança da detecção.
7. Separe o resultado em: estrutura e limites; página e estilos; seções; figuras/tabelas/equações; citações e referências; redação e integridade; pendências que exigem informação do estudante.
8. Não altere o arquivo nesta etapa.

## Segundo modo: edição autorizada

Edite somente se o usuário pedir explicitamente para aplicar as correções.

1. Preserve o original e crie uma nova cópia com sufixo claro, por exemplo `-formatado-usp-esalq.docx`.
2. Corrija automaticamente apenas itens de formatação inequívocos e autorizados: tamanho/orientação de página, margens, estilos, fonte, cor, alinhamento, espaçamento, recuos, paginação, hierarquia, posicionamento de títulos/legendas/fontes e geometria de tabelas.
3. Não reescreva afirmações científicas, resultados, conclusões, citações, autoria, nomes institucionais ou justificativas éticas sem autorização específica. Sugira a redação e marque a pendência.
4. Não altere dados numéricos, fórmulas, URLs, DOI, nomes próprios ou referências sem evidência suficiente.
5. Use estilos reais do Word, numeração/campos reais e tabelas editáveis. Não simule estrutura com espaços, quebras manuais, imagens de texto ou caracteres de tabulação.
6. Após cada lote relevante, rode novamente `scripts/audit_docx.py`, renderize e inspecione todas as páginas. Corrija regressões até não restarem defeitos visuais.
7. Entregue a cópia editada junto de um registro conciso: corrigido, mantido por decisão do usuário, e ainda pendente.

## Limites de detecção

- Contagem de páginas exige renderização; a estrutura OOXML isolada não é conclusiva.
- Tempo verbal, terceira pessoa, qualidade da discussão, plágio e adequação científica exigem leitura humana. Heurísticas devem aparecer como “revisar”, nunca como violação confirmada.
- Correspondência entre citações e referências pode ser aproximada; confirme manualmente falsos positivos, autores corporativos e variantes de sobrenome.
- O conteúdo do manual não substitui avaliação do orientador, comitê de ética ou banca.

## Saída esperada

No modo auditoria, entregue o relatório e indique quantos itens são seguramente automatizáveis. No modo edição, entregue somente a nova cópia e o registro de mudanças; nunca sobrescreva ou oculte o original.
