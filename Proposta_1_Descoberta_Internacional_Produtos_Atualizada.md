# Proposta 1 — Descoberta internacional de produtos

**Versão:** consolidada com as últimas decisões de Erick.  
**Status:** fluxo aprovado pelo usuário; integração e desempenho ainda precisam de validação técnica.  
**Finalidade:** briefing funcional para desenvolvimento da ferramenta própria.

## 1. Objetivo

Descobrir automaticamente produtos anunciados no exterior, agrupá-los a partir dos anúncios encontrados e apresentar candidatos com presença publicitária relevante ou crescimento observado. Entregar evidências, lojas de destino e identificação de plataforma, quando verificável, para que Erick avalie uma oferta própria no Brasil.

**O produto é uma saída da análise, não uma entrada obrigatória.** Erick não precisa escolher previamente um produto, palavra-chave, imagem ou concorrente. Isso não impede pesquisas manuais posteriores: depois de receber um candidato, ele poderá procurar ofertas brasileiras desse produto na Biblioteca de Anúncios da Meta.

“Escalado” é uma motivação de pesquisa, não uma classificação financeira comprovada. Contagens de anúncios não serão apresentadas como prova de aumento de investimento, vendas, receita ou lucratividade.

## 2. Decisões aprovadas e limites

| Aspecto | Definição |
|---|---|
| Fonte inicial de anúncios | API oficial da Biblioteca de Anúncios da Meta. |
| Mercado de descoberta | Internacional, começando pela União Europeia e pela cobertura efetivamente validada. Reino Unido poderá integrar a coleta após teste. |
| Mercado de adaptação da oferta | Brasil. Não é filtro obrigatório da coleta inicial. |
| Escolha prévia de produto | Não exigida do usuário. |
| Descoberta e análise | Módulos próprios, separados da integração com a fonte de dados. |
| Ofertas brasileiras | Pesquisa manual de Erick após a descoberta; não depende de automação no MVP. |
| Lojas/plataformas | Identificar destinos e evidências de Shopify, Yampi e Cartpanda; não excluir automaticamente outras plataformas. |
| AdSpy | Fora do escopo. |
| ScrapeCreators | Não é dependência da primeira versão. |
| Outros coletores | Nenhuma coleta não oficial ou contratação será adicionada silenciosamente como alternativa. |
| Cobertura | Amostra observada e documentada; sem promessa de examinar todos os anúncios mundiais. |

A documentação da Meta descreve acesso a anúncios de qualquer tipo veiculados na União Europeia e no Reino Unido no último ano. Essa cobertura não equivale a um catálogo global irrestrito. Há uma divergência entre a apresentação geral e parte da nota de países; a integração deverá testar a cobertura utilizada, começando pela União Europeia. [1]

## 3. Fluxo principal

**Consultas geradas pelo sistema → coleta oficial → identificação dos produtos → agrupamento e eliminação de duplicações → análise de volume/histórico → identificação das lojas → fichas de candidatos → pesquisa manual no Brasil → avaliação de oferta própria.**

A automação deverá responder: “Quais produtos merecem a atenção de Erick dentro dos anúncios que conseguimos analisar?” Ela não dependerá de ele responder antes: “Qual produto devo procurar?”

## 4. Módulos próprios

### 4.1 Descoberta de anúncios

O sistema deverá construir consultas internas a partir de categorias amplas e termos comerciais em diferentes idiomas. Os primeiros resultados poderão fornecer novos termos e páginas anunciantes para ampliar a exploração. Países, idiomas, período e orçamento de consultas serão configurações da execução, não uma obrigação de escolher produtos.

A API documenta pesquisas por termos e por identificadores de páginas. A lógica de exploração e priorização será desenvolvida na ferramenta. [1]

Cada execução deverá registrar consultas, filtros, páginas de resultados processadas, horários, erros e limites atingidos. Uma parte das consultas deverá explorar áreas ainda pouco pesquisadas, evitando que a base se limite aos primeiros produtos encontrados.

A coleta não será apresentada como aleatória, representativa de todo o mercado ou exaustiva sem evidências disso.

### 4.2 Identificação e agrupamento de produtos

Usar textos, títulos, descrições, páginas de venda acessíveis e referências visuais disponíveis por meios permitidos. Reconhecimento visual será complementar, sujeito à disponibilidade e às condições de uso dos ativos; não será prometido um reconhecimento perfeito de imagens ou vídeos.

Distinguir o mesmo produto, suas variantes e produtos apenas semelhantes. Registrar confiança e evidências do agrupamento. Permitir revisão humana de correspondências incertas, sem exigir seleção manual de um produto para iniciar a descoberta.

Anúncios genéricos de loja, coleções, carrosséis ou páginas com múltiplos produtos poderão ficar sem atribuição a um produto específico. Não atribuir automaticamente todo o volume de uma loja a um único item. Não inventar especificações nem reconstruir características que os dados não mostrem.

### 4.3 Contagens e histórico

Contar anúncios distintos por identificador, evitando duplicação quando o mesmo anúncio aparecer em vários países, termos ou execuções. Separar quantidade de anúncios de quantidade de criativos visivelmente diferentes.

Registrar, separadamente, o início de veiculação informado, a primeira observação pela ferramenta, a última observação e o status da consulta. Não confundir anúncio antigo recém-descoberto com anúncio recém-criado.

A primeira coleta apresentará volume observado. Crescimento dependerá de observações comparáveis. Relatórios distinguirão:

- Mudança entre anunciantes/produtos já acompanhados.
- Ampliação da cobertura, com lojas e anúncios antes desconhecidos.
- Falha ou incompletude de coleta.

Se um anúncio não aparecer em uma consulta, marcar “não observado nesta consulta”, e não automaticamente “desativado”. Se a coleta estiver incompleta, sinalizar a limitação e não calcular variação como se os dados fossem completos.

### 4.4 Identificação de destinos e plataformas

Registrar a página anunciante, o link de visualização do anúncio e, quando identificável por meios permitidos, a página de destino e o domínio final da loja. O campo `ad_snapshot_url` exibe o anúncio arquivado e não deve ser tratado como endereço final da loja. [1]

Examinar páginas públicas acessíveis para obter evidências técnicas da plataforma. Não realizar compras, preencher cadastros, acionar pagamentos, contornar autenticação ou tentar vencer bloqueios para identificar um checkout.

Manter campos separados para plataforma da loja e plataforma do checkout. Priorizar a detecção de Shopify, Yampi e Cartpanda, registrando evidência, confiança e data. “Não identificado” é um resultado válido. Não decidir pela aparência do site.

A ausência de identificação da loja ou do checkout não deverá apagar um candidato útil; a ficha deverá mostrar o dado faltante. Domínios diferentes não serão automaticamente tratados como empresas independentes.

### 4.5 Priorização e apresentação

Ordenar candidatos por indicadores verificáveis, com possibilidade de alternar entre volume atual e crescimento observado. Qualidade da correspondência, persistência e distribuição dos anúncios entre lojas serão informações complementares.

Não usar pesos opacos nem rotular candidatos como “vencedores” com base apenas em contagens. A expressão padrão será “anúncios ativos encontrados na base analisada”, acompanhada de data e cobertura.

A documentação não disponibiliza o campo de gasto como medida geral para anúncios comerciais comuns; portanto o MVP não prometerá identificar escala financeira. [1]

## 5. Ficha de cada candidato

| Campo | Conteúdo esperado |
|---|---|
| Identidade | Nome descritivo, função provável e referência visual, quando disponível e permitida. |
| Confiança | Evidências da associação dos anúncios ao produto e eventuais dúvidas. |
| Anúncios | Quantidade de anúncios distintos encontrados como ativos, com links verificáveis. |
| Histórico | Evolução em consultas comparáveis; “histórico insuficiente” quando necessário. |
| Lojas e anunciantes | Nomes das páginas, domínios identificados e distribuição dos anúncios. |
| Tecnologia | Plataforma da loja e checkout, com evidências ou indicação de ausência. |
| Contexto | Países pesquisados, idiomas, período, última coleta e limitações de cobertura. |
| Oferta estrangeira | Link da página e informações observadas, quando disponíveis; não converter promessas do vendedor em fatos comprovados. |
| Apoio à pesquisa brasileira | Sugestões de nomes e termos em português, sinônimos e link de acesso à Biblioteca. |
| Pesquisa manual | Links encontrados por Erick, observações e status da verificação no Brasil. |

A quantidade de candidatos apresentada dependerá dos resultados válidos. Não preencher uma lista mínima com produtos inventados, duplicados ou sem evidência.

## 6. Pesquisa manual de ofertas no Brasil

Depois de receber os candidatos, Erick poderá abrir a Biblioteca de Anúncios da Meta, selecionar Brasil, pesquisar pelos termos sugeridos e comparar os resultados com o produto identificado no exterior.

O painel deverá permitir salvar os links e observações dessa checagem. Estados sugeridos:

- Não pesquisado no Brasil.
- Pesquisa manual realizada — oferta encontrada.
- Pesquisa manual realizada — oferta não encontrada nesta busca.
- Identidade do produto ainda precisa de revisão.

“Oferta não encontrada” não significará ausência comprovada de concorrência. A automação dessa investigação brasileira não será requisito de lançamento e não será prometida como funcionalidade já aprovada.

A análise de candidatos serve para decidir o que investigar. Não comprova viabilidade, conformidade do produto nem resultado de vendas no Brasil. Desenvolvimento de oferta, imagens e textos deverá ser próprio, sem copiar marcas, peças ou promessas de terceiros.

## 7. Escopo da primeira versão funcional

Incluído: coleta oficial, descoberta por consultas internas, agrupamento de produtos, eliminação de duplicações, contagens e histórico, tentativa de identificação dos destinos e plataformas, painel de candidatos, links de evidências, campos para a checagem manual no Brasil e exportação em CSV.

Fora do escopo: AdSpy; dependência obrigatória de ScrapeCreators; promessa de cobertura mundial; automação completa da pesquisa brasileira; busca automática de fornecedores; criação ou publicação automática de anúncios; compras; estimativas inventadas de faturamento, vendas, gasto ou lucro; classificação de saturação do Brasil sem pesquisa; coleta não oficial adicionada sem discussão.

Uma busca manual opcional por um produto já descoberto não muda o objetivo da ferramenta, mas não será necessária para obter as primeiras sugestões.

## 8. Proposta técnica inicial — ainda sujeita à validação

Estrutura sugerida: Python para os módulos, SQLite para o histórico inicial e Streamlit para uma interface local. Essa combinação é uma proposta de implementação, não uma obrigação aprovada separadamente. A fonte de dados será desacoplada da lógica de produtos e do painel.

Separar descoberta, cliente da API, agrupamento, métricas, análise de destinos, detecção de plataforma, revisão humana e exportação. Credenciais ficarão no ambiente local, fora do código, dos relatórios e dos arquivos exportados.

O repositório oficial da Meta contém exemplos e uma interface de linha de comando, não um radar completo de produtos. É referência de integração; o código e a versão da API deverão ser revisados antes de qualquer uso. [2]

Não depender de uma API paga de coleta não significa custo total zero. Infraestrutura e eventual processamento por modelos serão dimensionados depois do teste e sujeitos a um limite de custo aprovado.

## 9. Validação antes do desenvolvimento completo

**Acesso:** concluir as etapas de autorização da Meta e testar a coleta com uma conta autorizada. A documentação exige confirmação de identidade/localização, conta de desenvolvedor e aplicativo. As credenciais não devem ser enviadas em mensagens nem publicadas no repositório. [1]

**Descoberta:** executar um piloto sem nomes de produtos ou concorrentes fornecidos por Erick. O piloto deverá produzir candidatos com evidências reais; nenhum conjunto ilustrativo será apresentado como resultado de coleta.

**Qualidade:** revisar uma amostra para medir erros de agrupamento, duplicações e taxa de identificação dos destinos. Os limites de aprovação serão definidos após observar a primeira amostra, não inventados como desempenho garantido.

**Histórico:** repetir a coleta para comprovar a distinção entre anúncios novos, anúncios recém-descobertos e falhas de cobertura.

**Uso real:** permitir que Erick escolha um candidato apresentado, pesquise manualmente no Brasil e registre o resultado sem precisar de um serviço pago de coleta.

## 10. Critério de conclusão do MVP

A ferramenta deverá apresentar produtos descobertos automaticamente, sustentar as contagens com anúncios verificáveis, explicar o alcance e as limitações da coleta, identificar destinos/plataformas quando houver evidência e apoiar a validação manual no Brasil.

**Descoberta automatizada de candidatos no exterior; seleção, checagem brasileira e decisão comercial humanas.**

## Referências técnicas

[1] Meta. API da Biblioteca de Anúncios — cobertura, acesso, parâmetros e campos. Consultada em 01/10/2026. https://pt-br.facebook.com/ads/library/api

[2] Meta / facebookresearch. Ad-Library-API-Script-Repository — exemplos de integração. Consultado em 01/10/2026. https://github.com/facebookresearch/Ad-Library-API-Script-Repository
