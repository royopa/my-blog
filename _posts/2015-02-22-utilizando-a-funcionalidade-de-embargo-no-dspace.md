<p>{
"title" : "Utilizando a funcionalidade de embargo no DSpace",
"author":"Royopa",
"date":"22-02-2015",
"tag":"dspace",
"slug" : "utilizando-a-funcionalidade-de-embargo-no-dspace",
"category":"DSpace"
}</p>

<p>https://wiki.duraspace.org/display/DSDOC4x/Embargo</p>

<p>Um <a href="https://wiki.duraspace.org/display/DSDOC5x/Embargo">Embargo</a> é uma restrição de acesso temporária colocada em metadados ou bitstreams. Sua abrangência ou duração pode variar, mas a expiração é o que a distingue de outras restrições de conteúdo. Por exemplo, é comum conteúdos no DSpace terem restrições de acesso permanente para grupos de usuários. Essas restrições são gerenciadas usando a ferramenta administrativa padrão do DSpace, incluindo políticas para coleções, itens, bitstreams, etc. 
A funcionalidade Embargo foi introduzida no DSpace 1.6 permitindo embargos sobre itens e aplicada a <strong><em>todos os bitstreams do item</em></strong>. Na versão 3.0 do DSpace, essa funcionalidade foi estendida para a interface XMLUI, permitindo fazer embargo sobre <strong><em>bistreams individuais</em></strong>.</p>

<p>Existem dois tipos de embargo no DSpace: o simples e o avançado, veremos adiante como cada um funciona.</p>

<h4 id="embargo-simples">Embargo Simples</h4>

<p>Quando habilitado, permite ao submitter de um novo item a possibilidade de aplicar restrição de acesso para os metadados e qualquer bitstream carregado.</p>

<p>Um novo step "Access" no processo de submissão permite que o submitter escolha todos os metadados do item como "private". Dessa forma, os metadados não serão pesquisáveis e o item não aparecerá nos índices de busca. Somente administradores tem acesso a esses items privados.</p>

<h5 id="accessstep">AccessStep</h5>

<p>O formulário do step "AccessStep" no Embargo Simples exibe três opções para o usuário:</p>

<ul>
<li><strong><em>Private item</em></strong>: para esconder os metadados de um item de todos os índices de pesquisa e navegação, bem como interfaces extrnas como o OAI_PMH.</li>
<li><strong><em>Embargo Access until Specific Date</em></strong>: para indicar a data até a qual o item será embargado.</li>
<li><strong><em>Reason</em></strong>: para elaborar o motivo específico para o item estar sob embargo.</li>
</ul>

<p>Quando o Embargo é definido, ele se aplica para usuários anônimos ou para qualquer outro grupo que tenha o acesso padrão de leitura para essa coleção específica.</p>

<p>Veja na figura abaixo como o AccessStep é exibido:</p>

<p><img src="http://www.royopa.url.ph/themes/royopa-blog/assets/img/embargo-simples.png" alt="" /></p>

<h5 id="uploadwithembargostep">UploadWithEmbargoStep</h5>

<p>O formulário do step "UploadWithEmbargoStep" no Embargo Simples exibe dois novos campos para o usuário:</p>

<ul>
<li><strong><em>Embargo Access until Specific Date</em></strong>: para indicar a data até a qual o bitstream será embargado. Se ficar vazio, nenhum embargo será aplicado.</li>
<li><strong><em>Reason</em></strong>: para elaborar o motivo específico para o bitstream estar sob embargo.</li>
</ul>

<p>Veja na figura abaixo como o UploadWithEmbargoStep é exibido:</p>

<p><img src="http://www.royopa.url.ph/themes/royopa-blog/assets/img/embargo-simples-UploadWithEmbargoStep.png" alt="" /></p>

<h4 id="embargo-avan%C3%A7ado">Embargo Avançado</h4>

<p>O Embargo Simples é limitado no sentido de que afeta apenas usuários anônimos e todos os grupos que têm acesso de leitura padrão.  Já o Embargo Avançado é mais poderoso, pois permite definir os embargos a grupos específicos de usuários. É aconselhável que esta funcionalidade só esteja habilitada para os usuários que conheçam quais os tipos de grupos que foram definidos no repositório, a fim de fazer um correto julgamento de quais embargos devem ser aplicados para cada grupo.</p>

<h3 id="habilitando-o-embargo-no-dspace">Habilitando o Embargo no DSpace</h3>

<p>Veja abaixo as configurações para habilitar a funcionalidade no DSpace 3.0+.</p>

<h5 id="embargo-simples-ou-avan%C3%A7ado">Embargo simples ou avançado</h5>

<h6 id="dspace.cfg">dspace.cfg</h6>

<p>Para escolher entre os dois tipos de embargo, você deve preencher a propriedade <strong>webui.submission.restrictstep.enableAdvancedForm</strong> no arquivo <strong>[dsapce]/config/dspace.cfg</strong>. O valor padrão é false, que habilita o modo simples enquanto o valor true habilita o embargo avançado.</p>

<pre><code>webui.submission.restrictstep.enableAdvancedForm=false
</code></pre>

<h5 id="processo-de-submiss%C3%A3o">Processo de submissão</h5>

<h6 id="item-submission.xml">item-submission.xml</h6>

<p>Para habilitar o embargo, o arquivo <strong>[dspace]/config/item-submission.xml</strong> também deverá ser alterado. Esse arquivo determina os steps que são executados na submissão de um novo item.
Dois novos steps são introduzidos no arquivo. Por padrão eles ainda não estão ativados:</p>

<ul>
<li><strong>AccessStep</strong>: step no qual o usuário define o embargo no nível de item, restringindo o acesso aos metadados do item.</li>
<li><strong>UploadWithEmbargoStep</strong>: step no qual o usuário define o embargo no nível de bitstream. Se esse step estiver habilitado, o antigo <em>UploadStep</em> deve estar desabilitado. Manter os dois steps habilitados resultará numa falha de sistema.</li>
</ul>

<p>Veja um trecho do arquivo alterado:</p>

<ul>
<li>Step 3 - AccessStep*</li>
</ul>

<pre><code class="xml">&lt;step&gt;
    &lt;heading&gt;submit.progressbar.access&lt;/heading&gt;
    &lt;processing-class&gt;org.dspace.submit.step.AccessStep&lt;/processing-class&gt;
    &lt;jspui-binding&gt;org.dspace.app.webui.submit.step.JSPAccessStep&lt;/jspui-binding&gt;
    &lt;xmlui-binding&gt;org.dspace.app.xmlui.aspect.submission.submit.AccessStep&lt;/xmlui-binding&gt;
    &lt;workflow-editable&gt;true&lt;/workflow-editable&gt;
&lt;/step&gt;
</code></pre>

<ul>
<li>Step 4 - UploadWithEmbargoStep*</li>
</ul>

<pre><code class="xml">&lt;step&gt;
    &lt;heading&gt;submit.progressbar.access&lt;/heading&gt;
    &lt;processing-class&gt;org.dspace.submit.step.AccessStep&lt;/processing-class&gt;
    &lt;jspui-binding&gt;org.dspace.app.webui.submit.step.JSPAccessStep&lt;/jspui-binding&gt;
    &lt;xmlui-binding&gt;org.dspace.app.xmlui.aspect.submission.submit.AccessStep&lt;/xmlui-binding&gt;
    &lt;workflow-editable&gt;true&lt;/workflow-editable&gt;
&lt;/step&gt;
</code></pre>

<p>Mais informações sobre a funcionalidade de embargo podem ser vistas na <a href="https://wiki.duraspace.org/display/DSDOC5x/Embargo">Wiki do DSpace</a></p>
