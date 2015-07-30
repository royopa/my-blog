{
"title" : "Mediafilters para transformação de conteúdo no DSpace",
"author":"Royopa",
"date":"07-07-2015",
"tag":"DSpace, Mediafilters",
"slug" : "mediafilters-para-transformacao-de-conteudo-no-dspace",
"category":"DSpace"
}

O DSpace pode aplicar diversos filtros para arquivos/bitstreams criando novos conteúdos.
Os filtros foram incluídos para extrair textos numa busca de texto completo 
(full-text searching) e criar thumbnails para os itens que contém imagens.  Os 
media filters são controlados pelo script dspace **filter-media** que vai até
asset store e chama todas as classes de MediaFilter ou FormatFilter configurados
nos arquivos/bitstreams (veja a seção ["Configuring Media Filters"](https://wiki.duraspace.org/display/DSDOC4x/Configuration+Reference#ConfigurationReference-ConfiguringMediaFilters)
para mais informações de como eles são configurados).

MediaFilters disponíveis
------------------------

A tabela abaixo lista todos os Media Filters disponíveis atualmente no DSpace e 
o que eles realmente fazem:

| Nome | Classe Java | Função | Habilitado por Padrão? |
|---|---|---|---|
| HTML Text Extractor | [org.dspace.app.mediafilter.HTMLFilter](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/HTMLFilter.java) | Extrai o texto completo de documentos HTML para indexação de texto completo (usa o Swing HTML Parser) | true |
| JPEG Thumbnail | [org.dspace.app.mediafilter.JPEGFilter](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/JPEGFilter.java) | Cria thumbnail de imagens GIF, JPEG e PNG | true |
| Branded Preview JPEG | [org.dspace.app.mediafilter.BrandedPreviewJPEGFilter](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/BrandedPreviewJPEGFilter.java) | Cria uma imagem de pré-visualização para imagens GIF, JPEG e PNG | false |
| PDF Text Extractor | [org.dspace.app.mediafilter.PDFFilter](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/PDFFilter.java) | Extrai o texto completo de documentos PDF (somente se o PDF for baseado em texto ou OCR) para indexação de texto completo. (usa a ferramenta Apache PDFBox) | true |
| XPDF Text Extractor | [org.dspace.app.mediafilter.XPDF2Text](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/XPDF2Text.java) | Extrai o texto completo de documentos PDF (somente se o PDF for baseado em texto ou OCR) para indexação de texto completo. (usa a [ferramenta de linha de comando XPDF](http://www.foolabs.com/xpdf/) disponível para *nix). Veja [XPDF Filter Configuration](https://wiki.duraspace.org/display/DSDOC4x/Configuration+Reference#ConfigurationReference-XPDFFilter) para detalhes de como habilitar/instalar essa ferramenta. | false |
| Word Text Extractor | [org.dspace.app.mediafilter.WordFilter](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/WordFilter.java) | Extrai o texto completo de documentos Word ou texto simples para indexação de texto completo. (Usa a ferramenta [Microsoft Word Text Mining](http://code.google.com/p/text-mining/) ) | true |
| PowerPoint Text Extractor | [org.dspace.app.mediafilter.PowerPointFilter](https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/PowerPointFilter.java) | Extrai o texto completo de slides e notas em documentos Power Point e Power Point XML para indexação de texto completo. (usa as ferramentas [Apache POI](http://poi.apache.org/) ) | true |

Note que o script filter-media irá atualizar automaticamente os índices de busca 
do DSpace por padrão (veja [Legacy methods for re-indexing content](https://wiki.duraspace.org/display/DSDOC4x/Legacy+methods+for+re-indexing+content)).
Essa é a forma recomendada de rodar esses scripts. Mas se desejar desabilitar isso,
basta passar o parâmetro -n para o script (veja a seção [Executing via Command Line](https://wiki.duraspace.org/display/DSDOC4x/Mediafilters+for+Transforming+DSpace+Content#MediafiltersforTransformingDSpaceContent-Executing(viaCommandLine)).

Habilitando/desabilitando MediaFilters
--------------------------------------

A seção de configuração **filter.plugins** no dspace.cfg contém uma lista com todos
os media/format filters plugins habilitados (veja [Configuring Media Filters](https://wiki.duraspace.org/display/DSDOC4x/Configuration+Reference#ConfigurationReference-ConfiguringMediaFilters) para mais informações). Modificando o valor de **filter.plugins** 
você consegue habilitar ou desabilitar os plugins MediaFilter.

Executing (via Command Line)
----------------------------

O sistema de media filter é feito para rodar em linha de comando (ou regularmente
como uma cron task):

```sh
[dspace]/bin/dspace filter-media
```

Sem opções, o script irá percorrer o asset store, aplicará os media filters para
os bitstreams e irá ignorar os bitstreams que já foram filtrados.

Mais informações podem ser encontradas na [Wiki do DSpace](https://wiki.duraspace.org/display/DSDOC4x/Mediafilters+for+Transforming+DSpace+Content).
