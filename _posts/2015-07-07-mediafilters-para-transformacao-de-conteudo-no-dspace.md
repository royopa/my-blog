<p>{
"title" : "Mediafilters para transformação de conteúdo no DSpace",
"author":"Royopa",
"date":"07-07-2015",
"tag":"DSpace, Mediafilters",
"slug" : "mediafilters-para-transformacao-de-conteudo-no-dspace",
"category":"DSpace"
}</p>

<p>O DSpace pode aplicar diversos filtros para arquivos/bitstreams criando novos conteúdos.
Os filtros foram incluídos para extrair textos numa busca de texto completo 
(full-text searching) e criar thumbnails para os itens que contém imagens.  Os 
media filters são controlados pelo script dspace <strong>filter-media</strong> que vai até
asset store e chama todas as classes de MediaFilter ou FormatFilter configurados
nos arquivos/bitstreams (veja a seção <a href="https://wiki.duraspace.org/display/DSDOC4x/Configuration+Reference#ConfigurationReference-ConfiguringMediaFilters">"Configuring Media Filters"</a>
para mais informações de como eles são configurados).</p>

<h2 id="mediafilters-dispon%C3%ADveis">MediaFilters disponíveis</h2>

<p>A tabela abaixo lista todos os Media Filters disponíveis atualmente no DSpace e 
o que eles realmente fazem:</p>

<table>
<thead>
<tr>
  <th>Nome</th>
  <th>Classe Java</th>
  <th>Função</th>
  <th>Habilitado por Padrão?</th>
</tr>
</thead>
<tbody>
<tr>
  <td>HTML Text Extractor</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/HTMLFilter.java">org.dspace.app.mediafilter.HTMLFilter</a></td>
  <td>Extrai o texto completo de documentos HTML para indexação de texto completo (usa o Swing HTML Parser)</td>
  <td>true</td>
</tr>
<tr>
  <td>JPEG Thumbnail</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/JPEGFilter.java">org.dspace.app.mediafilter.JPEGFilter</a></td>
  <td>Cria thumbnail de imagens GIF, JPEG e PNG</td>
  <td>true</td>
</tr>
<tr>
  <td>Branded Preview JPEG</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/BrandedPreviewJPEGFilter.java">org.dspace.app.mediafilter.BrandedPreviewJPEGFilter</a></td>
  <td>Cria uma imagem de pré-visualização para imagens GIF, JPEG e PNG</td>
  <td>false</td>
</tr>
<tr>
  <td>PDF Text Extractor</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/PDFFilter.java">org.dspace.app.mediafilter.PDFFilter</a></td>
  <td>Extrai o texto completo de documentos PDF (somente se o PDF for baseado em texto ou OCR) para indexação de texto completo. (usa a ferramenta Apache PDFBox)</td>
  <td>true</td>
</tr>
<tr>
  <td>XPDF Text Extractor</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/XPDF2Text.java">org.dspace.app.mediafilter.XPDF2Text</a></td>
  <td>Extrai o texto completo de documentos PDF (somente se o PDF for baseado em texto ou OCR) para indexação de texto completo. (usa a <a href="http://www.foolabs.com/xpdf/">ferramenta de linha de comando XPDF</a> disponível para *nix). Veja <a href="https://wiki.duraspace.org/display/DSDOC4x/Configuration+Reference#ConfigurationReference-XPDFFilter">XPDF Filter Configuration</a> para detalhes de como habilitar/instalar essa ferramenta.</td>
  <td>false</td>
</tr>
<tr>
  <td>Word Text Extractor</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/WordFilter.java">org.dspace.app.mediafilter.WordFilter</a></td>
  <td>Extrai o texto completo de documentos Word ou texto simples para indexação de texto completo. (Usa a ferramenta <a href="http://code.google.com/p/text-mining/">Microsoft Word Text Mining</a> )</td>
  <td>true</td>
</tr>
<tr>
  <td>PowerPoint Text Extractor</td>
  <td><a href="https://github.com/DSpace/DSpace/blob/master/dspace-api/src/main/java/org/dspace/app/mediafilter/PowerPointFilter.java">org.dspace.app.mediafilter.PowerPointFilter</a></td>
  <td>Extrai o texto completo de slides e notas em documentos Power Point e Power Point XML para indexação de texto completo. (usa as ferramentas <a href="http://poi.apache.org/">Apache POI</a> )</td>
  <td>true</td>
</tr>
</tbody>
</table>

<p>Note que o script filter-media irá atualizar automaticamente os índices de busca 
do DSpace por padrão (veja <a href="https://wiki.duraspace.org/display/DSDOC4x/Legacy+methods+for+re-indexing+content">Legacy methods for re-indexing content</a>).
Essa é a forma recomendada de rodar esses scripts. Mas se desejar desabilitar isso,
basta passar o parâmetro -n para o script (veja a seção <a href="https://wiki.duraspace.org/display/DSDOC4x/Mediafilters+for+Transforming+DSpace+Content#MediafiltersforTransformingDSpaceContent-Executing(viaCommandLine)">Executing via Command Line</a>.</p>

<h2 id="habilitando%2Fdesabilitando-mediafilters">Habilitando/desabilitando MediaFilters</h2>

<p>A seção de configuração <strong>filter.plugins</strong> no dspace.cfg contém uma lista com todos
os media/format filters plugins habilitados (veja <a href="https://wiki.duraspace.org/display/DSDOC4x/Configuration+Reference#ConfigurationReference-ConfiguringMediaFilters">Configuring Media Filters</a> para mais informações). Modificando o valor de <strong>filter.plugins</strong> 
você consegue habilitar ou desabilitar os plugins MediaFilter.</p>

<h2 id="executing-via-command-line">Executing (via Command Line)</h2>

<p>O sistema de media filter é feito para rodar em linha de comando (ou regularmente
como uma cron task):</p>

<pre><code class="sh">[dspace]/bin/dspace filter-media
</code></pre>

<p>Sem opções, o script irá percorrer o asset store, aplicará os media filters para
os bitstreams e irá ignorar os bitstreams que já foram filtrados.</p>

<p>Mais informações podem ser encontradas na <a href="https://wiki.duraspace.org/display/DSDOC4x/Mediafilters+for+Transforming+DSpace+Content">Wiki do DSpace</a>.</p>
