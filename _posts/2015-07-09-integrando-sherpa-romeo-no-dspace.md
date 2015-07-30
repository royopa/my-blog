<p>{
"title" : "Integrando SHERPA/RoMEO no DSpace",
"author":"Royopa",
"date":"09-07-2015",
"tag":"DSpace, SherpaRomeo",
"slug" : "integrando-sherpa-romeo-no-dspace",
"category":"DSpace"
}</p>

<p>O <a href="http://www.sherpa.ac.uk/romeo">portal SHERPA/RoMEO</a> reúne políticas de 
arquivamento de diversas editoras internacionais. Esta ferramenta permite 
aos usuários e gestores de repositórios consultar as políticas de copyright e 
auto-arquivamento das revistas e editoras sobre o depósito das publicações em 
repositórios de acesso aberto.</p>

<h2 id="cores-do-sherpa%2Fromeo">Cores do SHERPA/RoMEO</h2>

<p>O serviço SHERPA/RoMEO utiliza um simples <a href="http://www.sherpa.ac.uk/romeo/definitions.php?la=pt&amp;fIDnum=|&amp;mode=simple&amp;version=#colours">código de cores</a> para ajudar a consultar as
políticas de arquivamento das editoras. Existem quatro categorias de cores, que
são descritas abaixo:</p>

<table>
<thead>
<tr>
  <th align="left">Cor</th>
  <th align="left">Política de arquivamento</th>
</tr>
</thead>
<tbody>
<tr>
  <td align="left"><a href="http://www.sherpa.ac.uk/romeo/browse.php?colour=green&amp;la=pt&amp;fIDnum=|&amp;mode=simple">Verde</a></td>
  <td align="left">Pode arquivar a versão preprint e postprint ou Versão/PDF do editor</td>
</tr>
<tr>
  <td align="left"><a href="http://www.sherpa.ac.uk/romeo/browse.php?colour=blue&amp;la=pt&amp;fIDnum=|&amp;mode=simple">Azul</a></td>
  <td align="left">Pode arquivar a versão postprint (ex: o rascunho final após o peer-review) ou Versão/PDF do editor</td>
</tr>
<tr>
  <td align="left"><a href="http://www.sherpa.ac.uk/romeo/browse.php?colour=yellow&amp;la=pt&amp;fIDnum=|&amp;mode=simple">Amarelo</a></td>
  <td align="left">Pode arquivar a versão preprint (ex: antes do peer-review)</td>
</tr>
<tr>
  <td align="left"><a href="http://www.sherpa.ac.uk/romeo/browse.php?colour=white&amp;la=pt&amp;fIDnum=|&amp;mode=simple">Branco</a></td>
  <td align="left">O arquivo não é suportado formalmente</td>
</tr>
</tbody>
</table>

<h2 id="integra%C3%A7%C3%A3o-do-sherpa%2Fromeo-no-dspace">Integração do SHERPA/RoMEO no DSpace</h2>

<p>O DSpace 5 acrescenta a funcionalidade de lookup da política SHERPA/RoMEO para a 
interface XMLUI. Essa funcionalidade evita o risco de quebrar as restrições de
publicação de forma involuntária.</p>

<p>Habilitando o lookup SHERPA/RoMEO no DSpace adicionará informações nos steps
de submissão. Depois de selecionar um determinado editor ou jornal no formulário
de submissão, o DSpace automaticamente inclui informações sobre permissões de
publicação e restrições ao lado da opção de seleção de arquivos.</p>

<h2 id="m%C3%A3o-na-massa%21">Mão na massa!</h2>

<p>Crie sua chave de acesso para a API SHERPA/RoMEO, para ter os benefícios abaixo:</p>

<ul>
<li>As aplicações registradas podem exceder o limite de 500 requisições por dia.</li>
<li>Os usuários registrados recebem avisos antecipados das mudanças planejadas para
a API.</li>
<li>Estatísticas de uso da API para a sua chave ficam disponíveis mediante solicitação.</li>
</ul>

<p>Para criar a chave de acesso, use esse link: 
<a href="http://www.sherpa.ac.uk/romeo/apiregistry.php">http://www.sherpa.ac.uk/romeo/apiregistry.php</a></p>

<p>Primeiro configure o controle de autoridade do plugin SHERPA/RoMEO no arquivo
[dspace]/config/dspace.cfg conforme abaixo:</p>

<pre><code class="cfg">#####  SHERPA/Romeo Integration Settings ####
# the SHERPA/RoMEO endpoint
sherpa.romeo.url = http://www.sherpa.ac.uk/romeo/api29.php

# to disable the sherpa/romeo integration
# uncomment the follow line 
# webui.submission.sherparomeo-policy-enabled = false

# please register for a free api access key to get many benefits
# http://www.sherpa.ac.uk/news/romeoapikeys.htm
sherpa.romeo.apikey = YOUR-API-KEY
</code></pre>

<p>E então reinicie o servidor tomcat com o comando abaixo:</p>

<pre><code class="sh">sudo service tomcat7 restart
</code></pre>

<h2 id="testando">Testando</h2>

<p>Para testar a funcionalidade, primeiro pesquisei um registro na <a href="http://www.sherpa.ac.uk/romeo/search.php">página de busca do SHERPA/RoMEO</a>
com o <strong>ISSN 1210-8510</strong>, que é um Jornal com a cor <strong>branca</strong>, conforme mostrado na 
imagem abaixo:</p>

<p><img src="http://www.royopa.url.ph/themes/royopa-blog/assets/img/sherpa-romeo-consulta-registro-issn.jpg" alt="" /></p>

<p>E então inclui um novo item no DSpace e usei o ISSN 1210-8510, como mostrado na
figura abaixo:</p>

<p><img src="http://www.royopa.url.ph/themes/royopa-blog/assets/img/sherpa-romeo-upload-inclusao-de-issn.jpg" alt="" /></p>

<p>E então quando eu chego no step para fazer o upload do arquivo, veja que as
informações do registro no SHERPA/RoMEO (Publisher Information) são exibidas:</p>

<p><img src="http://www.royopa.url.ph/themes/royopa-blog/assets/img/sherpa-romeo-upload-arquivos.jpg" alt="" /></p>

<p>Veremos num próximo artigo como incluir um controle de autoridades usando a mesma 
API do SHERPA/RoMEO, até lá!</p>
