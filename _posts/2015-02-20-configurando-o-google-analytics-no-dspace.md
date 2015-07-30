<p>{
"title" : "Configurando o Google Analytics no DSpace",
"author":"Royopa",
"date":"20-02-2015",
"tag":"dspace",
"slug" : "configurando-o-google-analytics-no-dspace",
"category":"DSpace"
}</p>

<p>O <a href="http://www.google.com/analytics/">Google Analytics</a> é um serviço oferecido pelo Google no qual, ao ativar-se o serviço por intermédio de uma conta do Google, e ao cadastrar-se um site recebe-se um código para ser inserido na página cadastrada e, a cada exibição, estatísticas de visitação são enviadas ao sistema e apresentadas ao dono do site.</p>

<p>Temos um <a href="http://pt.slideshare.net/royopa1/mdulo-11-estatsticas-dspace-e-google-analytics">módulo no treinamento</a> para tratar dessa característica, os slides estão disponíveis <a href="http://pt.slideshare.net/royopa1/mdulo-11-estatsticas-dspace-e-google-analytics">aqui</a>.</p>

<p>O DSpace já vem pré-habilitado para utilização desse serviço, veremos adiante os passos para fazer essa configuração na interface XMLUI.</p>

<h3 id="crie-sua-conta-no-google-analytics">Crie sua conta no Google Analytics</h3>

<p>Crie uma conta no <a href="http://www.google.com/analytics/">Google Analytics</a> e cria uma entrada para o site do seu repositório. Você receberá um javascript para ser inserido em seu site, procure no javascript a chave do seu site, que deve ser parecida com isso "UA-XXXXXXX-X".</p>

<h3 id="configure-a-propriedade-no-arquivo-dspace.cfg">Configure a propriedade no arquivo dspace.cfg</h3>

<p>Abra o arquivo dspace.cfg e procure pela propriedade "xmlui.google.analytics.key" e preencha com o código do Google Analytics (UA-XXXXXXX-X) de seu repositório.</p>

<pre><code>xmlui.google.analytics.key=UA-XXXXXX-X
</code></pre>

<h3 id="reinicie-o-servidor-tomcat">Reinicie o servidor tomcat</h3>

<p>Reinicie o servidor tomcat e o código já estará lá para utilização.</p>

<pre><code>sudo service tomcat7 restart
</code></pre>

<h3 id="conclus%C3%A3o">Conclusão</h3>

<p>A partir daí o seu repositório já está sendo monitorado e você já poderá verificar as estatísticas no site do <a href="http://www.google.com/analytics/">Google Analytics</a>.</p>

<p>Não existe a necessidade de incluir o javascript completo em cada página pois os temas XMLUI já possuem o javascript que será carregado apenas se a propriedade xmlui.google.analytics.key tiver sido preenchida no DSpace.cfg.</p>

<p>Veja o trecho do arquivo responsável por isso (page-structure.xsl):</p>

<pre><code>&lt;!-- Add a google analytics script if the key is present --&gt;
&lt;xsl:if test="/dri:document/dri:meta/dri:pageMeta/dri:metadata[@element='google'][@qualifier='analytics']"&gt;
    &lt;script type="text/javascript"&gt;&lt;xsl:text&gt;
            var _gaq = _gaq || [];
            _gaq.push(['_setAccount', '&lt;/xsl:text&gt;&lt;xsl:value-of select="/dri:document/dri:meta/dri:pageMeta/dri:metadata[@element='google'][@qualifier='analytics']"/&gt;&lt;xsl:text&gt;']);
               _gaq.push(['_trackPageview']);

            (function() {
                var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
                ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
                var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
            })();
    &lt;/xsl:text&gt;&lt;/script&gt;
&lt;/xsl:if&gt;
</code></pre>

<h2 id="novas-funcionalidades-no-dspace-5">Novas funcionalidades no DSpace 5</h2>

<p>A partir da versão 5 do DSpace existem algumas novidades para a gravação e relatórios do Google Analytics.</p>

<h3 id="grava%C3%A7%C3%A3o-google-analytics">Gravação Google Analytics</h3>

<p>Até a versão 5.0 somente a atividade da interface com usuário podia ser gravada, ou seja, os downloads iniciados diretamente através de uma pesquisa no Google (ou qualquer outro mecanismo de busca) não eram registrados. A partir da versão 5.0 do DSpace os downloads são registrados como 'Eventos' Google, de modo que todas as visualizações de páginas e downloads de bitstreams passaram a ser registrados também.</p>

<h3 id="relat%C3%B3rios-do-google-analytics">Relatórios do Google Analytics</h3>

<p>É possível expor os dados gravados do Google Analytics dentro DSpace. Atualmente, só está disponível para sites que usem temas com base no XMLUI Mirage2. 
Os dados são recuperados a partir do Google usando a API <a href="https://developers.google.com/analytics/devguides/reporting/core/v3/">Google Analytics Reporting API v3</a>. Este recurso vem desabilitado por padrão. 
Para mais informações de como habilitar essa funcionalidade, siga as instruções contidas na <a href="https://wiki.duraspace.org/display/DSDOC5x/DSpace+Google+Analytics+Statistics">DSpace Wiki</a>.</p>
