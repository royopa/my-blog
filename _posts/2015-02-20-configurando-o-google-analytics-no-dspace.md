{
"title" : "Configurando o Google Analytics no DSpace",
"author":"Royopa",
"date":"20-02-2015",
"tag":"dspace",
"slug" : "configurando-o-google-analytics-no-dspace",
"category":"DSpace"
}

O [Google Analytics][1] é um serviço oferecido pelo Google no qual, ao ativar-se o serviço por intermédio de uma conta do Google, e ao cadastrar-se um site recebe-se um código para ser inserido na página cadastrada e, a cada exibição, estatísticas de visitação são enviadas ao sistema e apresentadas ao dono do site. 

Temos um [módulo no treinamento][2] para tratar dessa característica, os slides estão disponíveis [aqui][2].

O DSpace já vem pré-habilitado para utilização desse serviço, veremos adiante os passos para fazer essa configuração na interface XMLUI.

###Crie sua conta no Google Analytics

Crie uma conta no [Google Analytics][1] e cria uma entrada para o site do seu repositório. Você receberá um javascript para ser inserido em seu site, procure no javascript a chave do seu site, que deve ser parecida com isso "UA-XXXXXXX-X".

###Configure a propriedade no arquivo dspace.cfg

Abra o arquivo dspace.cfg e procure pela propriedade "xmlui.google.analytics.key" e preencha com o código do Google Analytics (UA-XXXXXXX-X) de seu repositório.

    xmlui.google.analytics.key=UA-XXXXXX-X

###Reinicie o servidor tomcat

Reinicie o servidor tomcat e o código já estará lá para utilização.
    
    sudo service tomcat7 restart

###Conclusão

A partir daí o seu repositório já está sendo monitorado e você já poderá verificar as estatísticas no site do [Google Analytics][1].

Não existe a necessidade de incluir o javascript completo em cada página pois os temas XMLUI já possuem o javascript que será carregado apenas se a propriedade xmlui.google.analytics.key tiver sido preenchida no DSpace.cfg. 

Veja o trecho do arquivo responsável por isso (page-structure.xsl):

    <!-- Add a google analytics script if the key is present -->
    <xsl:if test="/dri:document/dri:meta/dri:pageMeta/dri:metadata[@element='google'][@qualifier='analytics']">
        <script type="text/javascript"><xsl:text>
                var _gaq = _gaq || [];
                _gaq.push(['_setAccount', '</xsl:text><xsl:value-of select="/dri:document/dri:meta/dri:pageMeta/dri:metadata[@element='google'][@qualifier='analytics']"/><xsl:text>']);
                   _gaq.push(['_trackPageview']);

                (function() {
                    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
                    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
                    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
                })();
        </xsl:text></script>
    </xsl:if>

Novas funcionalidades no DSpace 5
-----

A partir da versão 5 do DSpace existem algumas novidades para a gravação e relatórios do Google Analytics.

###Gravação Google Analytics

Até a versão 5.0 somente a atividade da interface com usuário podia ser gravada, ou seja, os downloads iniciados diretamente através de uma pesquisa no Google (ou qualquer outro mecanismo de busca) não eram registrados. A partir da versão 5.0 do DSpace os downloads são registrados como 'Eventos' Google, de modo que todas as visualizações de páginas e downloads de bitstreams passaram a ser registrados também.

###Relatórios do Google Analytics

É possível expor os dados gravados do Google Analytics dentro DSpace. Atualmente, só está disponível para sites que usem temas com base no XMLUI Mirage2. 
Os dados são recuperados a partir do Google usando a API [Google Analytics Reporting API v3][3]. Este recurso vem desabilitado por padrão. 
Para mais informações de como habilitar essa funcionalidade, siga as instruções contidas na [DSpace Wiki][4].

[1]: http://www.google.com/analytics/
[2]: http://pt.slideshare.net/royopa1/mdulo-11-estatsticas-dspace-e-google-analytics
[3]: https://developers.google.com/analytics/devguides/reporting/core/v3/
[4]: https://wiki.duraspace.org/display/DSDOC5x/DSpace+Google+Analytics+Statistics