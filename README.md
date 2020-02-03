<!-- FILE VERSION DETAILS 
      TITLE: Player Kit Integration Guide
      Author: E Roberts (Perform)
      EDITING NOTES:
       - CMS UPLOAD PREP: Replace some HTML entities eg < to &amp;lt; + > to &amp;gt;
       - OFFLINE CSS: link rel="stylesheet" href="../../../css/main-opta.css"
       - STEPS - circled numbers: w3schools.com/charsets/ref_utf_dingbats.asp
       - STEPS - circled numbers: Either increment by 1 from, using &#x2780; or &#10112;
       - STEPS - paras: before 'p' tags were set to p class="quote-small"
      Version: 1.2.3
      Since: 2019-09-26
      Notes: 1.2.3 (2019-09-26): Added note that use of custom 1.2.2 (2018-12-18): Added screenshots of PKit and Content Player in Stats view and Stream-only mode. 1.2.1 (2018-12-06): Minor changes, added get/set to PK methods, renamed main h2 Appendix heading and checked other headings so they fit on screen using current font size (this is to be reviewed following request to portal team). v1.2 (2018-11-30): Updated with new format and examples, full Appendix, WebView, updated Player Kit API information, added referneces to living docs in repository. 2018-07-25 v1.1: Changes/additions as requested in WAB-2096. v1.0,2 (2017-11-30): Added note about using Player Kit with own player - no wrapper needed, just use API calls. Removed reference to third party player as it is NOT supported. v1.0.1 (2017-11-23): Removed API Overview, FAQs, Glossary (see separate guides). v1.0 (2017-11-17): DO NOT REMOVE lt gt amp character entities/refs as extra amps/ampersands needed to render via article API. v1.0: 11-17: Removed onclick from show/hide text, added a target to all href, removed JScript. 11-16: Removed elements with no-screendisplay class such as hyperlinks, print page breaks and 'continue on next page'. v0.6 (2017-11-10): Removed unused JS functions. v0.5 (2017-11-08): Added explicit note about player controls being optional and devs being unable to customise them if enabled, following Z's feedback.
-->
<meta charset="utf-8" />
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" />
<section class="tnr-container" id="top">
<div class="single-tech_docs" id="main-content-article">
<p class="lead-sm">Call stream and fixture schedules and authorise user requests through our APIs, and launch livestreams in a player</p>

<p>Our Player Kit is a JavaScript component which provides an <abbr title="Application Programming Interface">API</abbr> to playback Watch&amp;Bet video streams. Here, we explain how to complete the following steps:</p>
<!-- link to main guide #integrating-the-player-kit -->

<p class="margin-small"><span style="font-size:15px">➀</span><!-- number 1 &#x2780; --> <a href="#step-one"><strong>Integrate the Player Kit code and prepare your frontend environment</strong></a> - add the embed wrapper code to your pages and configure the API calls. <!-- <a href="#integrating-the-player-kit" target="_parent"></a> --><br />
<span style="font-size:15px">➁</span><!-- number 2 &#x2781; --> <strong><a href="#step-two">Call the Content Catalogue Stream Schedule feed</a> <span class="light-grey">(<kbd>/mcc</kbd>)</span></strong> - get fixture/event schedules and metadata, and fixture and livestream asset UUIDs. <!-- <a href="#wab-pkit-call-mcc-feed" target="_parent"></a> --><br />
<span style="font-size:15px">➂</span><!-- number 3 &#x2782; --> <a href="#step-three"><strong>Call the OAuth feed <span class="light-grey">(<kbd>/oauth</kbd>)</span> to get the access token</strong></a> - this is required to authorise each unique user session and livestream asset UUID request.<br />
<span style="font-size:15px">➃</span><!-- number 4 &#x2783; --> <a href="#step-four"><strong>Pass the access token and livestream UUID to the Player Kit embed code</strong></a> - complete any additional work, and launch and playback the livestream on your frontend site. <!-- <a href="#wab-pkit-access-stream-link" target="_parent"></a> --><br />
<span style="font-size:15px">➄</span><!-- number 5 &#x2784; --> <a href="#appendix"><strong>CORS configuration, Using the Player API, Setting the video size</strong></a> - configure your frontend to allow our player.</p>

<div class="pf-box-note"><strong>Note:</strong> Our recommended integration approach is to use the Content Player, our real-time web application, which combines Watch&amp;Bet video with access to prematch data, live visualisations and live data, such as match and player stats. If you choose to use the Player Kit, you will not be able to display data and visualisations. To keep a Player Kit integration without missing out on these features, you could have a separate integration which uses the Content Player in visualisation-only mode (without livestream video playback) - contact us for more details.<br />
<br />
Example - left: Player Kit with livestream; right: Content Player in visualisation/stats and stream-only mode (the default mode shows a header and footer)<br />
<img alt="Player Video only" perform:prop="uuid:6btfgnqb820n1wztqb29siamk;width:596;height:341" src="https://images.performgroup.com/di/library/Watch_and_Bet_Portal/a6/e6/player-video-only_1pi0f9t7btv2z1y2czncge3t8r.png?t=-1056446777" style="width: 300px; height: 172px;" />&nbsp;<img alt="Content Player in Stream-only mode showing Stats Data" perform:prop="uuid:15a2kp98tq12a148rle0mxsj9m;width:598;height:341" src="https://images.performgroup.com/di/library/Watch_and_Bet_Portal/23/6b/content-player-in-stream-only-mode-showing-stats-data_xkmt8vxb3sku104ycg0b0e07g.png?t=-1047439777" style="width: 300px" /></div>

<div class="pf-box-info">&bull; We use <code>{example/placeholder values}</code> in our guides, such as in request URL and body examples. Remember to remove/customise these placeholders in your own code.<br />
&bull; You will always find the <strong>latest Player Kit API documentation</strong> in our release repository. See <a href="https://docs.performgroup.io/wab-playerkit" target="_blank">Player Kit on GitHub</a> or contact your Account Manager for more information.</div>
<!-- end Introduction -->

<hr class="hrsmall" />
<div class="no-top-margin" id="wab-pkit-tutorial"><a id="step-one" name="step-one"></a>

<h2 class="no-top-margin" id="wab-pkit-embed">➀ Integrate the Player Kit and prepare your frontend environment</h2>

<p>The Player Kit provides an <abbr title="Software Developer's Kit">SDK</abbr> which we host on our servers to make it easy for you to access our API on your framework. It encapsulates the API calls to the Watch&amp;Bet platform and will handle communication with our platform, event management, and rights protection. The API also allows you to interact with and control playback of livestreams distributed by us.</p>

<p>There are three main ways to implement the Player Kit library and launch a livestream in your page/app. <a href="#pkit-implement-wrapper-embed-code" target="_parent">All use the wrapper embed code</a>, but options 2 and 3 require extra development work.<br />
Choose the option that best suits your requirements:</p>

<ul>
	<li><strong><span class="light-grey">option 1</span> Use our Player Kit with built-in player controls (wrapper embed code)</strong> - this is the easiest way to playback livestreams only from Watch&amp;Bet</li>
	<li><strong><span class="light-grey">option 2</span> Build/use your own JavaScript Player UI on top of our Player Kit (wrapper embed code + Player Kit API)</strong> - playback livestreams from Watch&amp;Bet (and other providers)</li>
	<li><strong><span class="light-grey">option 3</span> Embed in a mobile app with WebView (native apps only)</strong> - add the code to a separate HTML file, store on your public web server, and add its URL in an <code>&amp;lt;iframe&amp;gt;</code> embed</li>
</ul>

<div class="pf-box-note">&bull; <strong>Player Kit:</strong> You cannot style the built-in controls. If you load the latest release, any changes are reflected in your page automatically (or you can use a semantic version for some control).<br />
&bull; <strong>Using your own Player:</strong> If you choose to build/use your own JavaScript player application, you must configure its user interface, and apply the logic to load our Player Kit library and use its API calls. We can only support the use of our Player Kit or Content Player - we cannot provide support for using a third party player.</div>

<div class="pf-box-caution">
<ul>
	<li><strong>The use of custom user agents is not supported</strong> - this is because their use may cause issues - for example, playback issues (the stream may not play) or inaccurate reporting between the split of traffic between desktop/laptop and mobile devices.</li>
	<li><strong>Making successful calls:</strong> Calls to our platform must&nbsp;originate&nbsp;from&nbsp;an IP address or domain that you have supplied to us (for our whitelist). For pages containing the Player Kit, the header must include a domain we recognise. <a href="#wab-pkit-appendix-cors" target="_blank"><strong>You must enable CORS</strong></a> (Cross-Origin Resource Sharing) to access resources on our servers, such as JavaScript and livestreams. See: <a href="#wab-pkit-appendix-cors" target="_blank">Appendix</a></li>
</ul>
</div>

<div id="pkit-implement-wrapper-embed-code">&nbsp;</div>

<h3>Implement the wrapper embed code <span class="light-grey">(all options)</span></h3>
<!-- <p>The Player Kit library encapsulates the API calls to the Watch&amp;Bet platform and will handle communication with our platform, event management, and rights protection.<br />
The API also allows you to interact with and control playback of embedded livestreams that we distribute - to learn more, see <a href="#wab-pkit-parameters-to-launch-stream" target="_parent">step 4</a> and <a href="#wab-pkit-appendix-using-the-api" target="_parent">Appendix: Using the API</a> at the end of this guide.</p> -->

<p>The wrapper embed has <strong>3</strong> parts: 1) <strong>Player Kit URL</strong> (loads the library via a <strong><code>&amp;lt;script&amp;gt;</code></strong> tag); 2) <strong><code>&amp;lt;div&amp;gt;</code> tag</strong> (container for the video); 3) <strong><code>&amp;lt;script&amp;gt;</code> tag</strong> (where you add parameters for API calls).</p>

<h5><span class="light-grey">1)</span> Add the Player Kit URL and <span class="light-grey">&amp;lt;script&amp;gt;</span> tag to load the latest or a specific library version</h5>

<p>You can use the latest release channel or link to a specific version. Using the latest release means any updates to the Player Kit are propagated automatically, so no code changes are required to your site/app. If you specify a version you must manually update it in your code once it is deprecated. You can inspect the code of any version by entering its URL in your browser.</p>
<!-- The URL you should use depends on whether you want to always use the current version or use a specific (previous or beta) version. When the URL does not specify a version any changes and updates made to the Player Kit via the API will propagate without any code updates needed on your website. To inspect the code of any version, enter its URL in your browser -->

<p>Choose one of the following options:</p>

<p>&nbsp;&nbsp; <strong>a)</strong>&nbsp; load the latest release - always current (non-beta, does not specify a version)</p>

<table class="table-clear" style="max-width: 50%">
	<tbody>
		<tr>
			<td style="width:1%">&nbsp;</td>
			<td style="width:99%">
			<pre class="prettyprint small">
&amp;lt;script src=&quot;<span class="code-green">//player.performgroup.com/wab.kit.js</span>&quot;&amp;gt;&amp;lt;/script&amp;gt;</pre>
			</td>
		</tr>
	</tbody>
</table>

<p>&nbsp; &nbsp;<strong>b)</strong>&nbsp; specify a release (previous or beta version - replace <code>{VERSION]</code> with a specific number, eg <code>0.6.6</code>)</p>

<table class="table-clear" style="max-width: 50%">
	<tbody>
		<tr>
			<td style="width:1%">&nbsp;</td>
			<td style="width:99%">
			<pre class="prettyprint small">
&amp;lt;script src=&quot;//player.performgroup.com/<span class="code-green">wab.kit/releases/{VERSION}/wab.kit.js</span>&quot;&amp;gt;&amp;lt;/script&amp;gt;</pre>
			</td>
		</tr>
	</tbody>
</table>
&nbsp;

<p>Place the <code>&amp;lt;script&amp;gt;</code> tag containing the Player Kit library URL either in the <code>&amp;lt;head&amp;gt;</code> of your website or in a web page (anywhere before the closing <code>&amp;lt;/body&amp;gt;</code> tag), to suit your template/s.</p>

<table class="table-clear" style="max-width: 100%">
	<thead>
		<tr>
			<th style="width:1%">&nbsp;</th>
			<th style="width:49%">HEAD (website)</th>
			<th style="width:1%">&nbsp;</th>
			<th style="width:49%">BODY (web page)</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:1%">&nbsp;</td>
			<td style="width:49%;vertical-align: top">
			<pre class="prettyprint lang-html small">
&amp;lt;head&amp;gt;
  &amp;lt;script src=&quot;{playerUrl}&quot;&amp;gt;&amp;lt;/script&amp;gt;
&amp;lt;/head&amp;gt;
</pre>
			</td>
			<td style="width:1%">&nbsp;</td>
			<td style="width:49%;vertical-align: top">
			<pre class="prettyprint lang-html small">
&amp;lt;body&amp;gt;
  &amp;lt;script src=&quot;{playerUrl}&quot;&amp;gt;&amp;lt;/script&amp;gt;
&amp;lt;/body&amp;gt;
</pre>
			</td>
		</tr>
	</tbody>
</table>

<div class="pf-box-note">Do not specify a protocol in the URL (<code>//player</code>). This way, the browser can choose it, according to the protocol of the host page. It can also help performance, eg faster load times.</div>

<h5><span class="light-grey">2)</span> Add the <span class="light-grey">&amp;lt;div&amp;gt;</span> tag (livestream container)</h5>

<p>Place a <code>&amp;lt;div&amp;gt;</code> tag where you want the livestream to appear in the page <code>&amp;lt;body&amp;gt;</code> and assign an <code>id=&quot;{value}&quot;</code> to it. It acts as the player container and creates a player instance in the <abbr title="Document Object Model">DOM</abbr>.</p>

<table class="table-clear" style="max-width: 100%">
	<tbody>
		<tr>
			<td style="width:1%">&nbsp;</td>
			<td style="width:49%">
			<pre class="prettyprint small">
  &amp;lt;div id=&quot;{player-container}&quot;&amp;gt;&amp;lt;/div&amp;gt;
</pre>
			</td>
			<td style="width:1%">&nbsp;</td>
			<td style="width:49%">&nbsp;</td>
		</tr>
	</tbody>
</table>

<h5><span class="light-grey">3)</span> Add the <span class="light-grey">&amp;lt;script&amp;gt;</span> tag to contain the API calls</h5>

<p>To initialise the player, add a <code>&amp;lt;script&amp;gt;</code> tag. This must contain the required calls, parameters and values - supplied by you - to request a livestream session. When a user visits your page/app and once the Player Kit and container have loaded, the library will execute the API calls and launch the requested livestream in a player instance.</p>

<p>You have created the basic embed and completed step ➀ - great! Now, you must complete the following steps:</p>

<ul>
	<li style="list-style-type: none"><strong>➁ Get the fixture&#39;s livestream UUID from the Content Catalogue feed <span class="light-grey">(<kbd>/mcc</kbd>)</span></strong> - see: <a href="#wab-pkit-call-mcc-feed" target="_parent">step 2</a></li>
	<li style="list-style-type: none"><strong>➂ Get the OAuth access token and (authorise the stream and user) for the session <span class="light-grey">(<kbd>/oauth</kbd>)</span></strong> - see: <a href="#wab-pkit-call-oauth-feed" target="_parent">step 3</a></li>
	<li style="list-style-type: none"><strong>➃ Insert the access token, livestream UUID, and API calls, and configure the embed to launch the stream</strong> - configure the <code>&amp;lt;script&amp;gt;</code> tag and finish additional implemention work:
	<ul>
		<li>To learn about how to construct the API calls and specify parameters (including player controls on/off), see: <a href="#wab-pkit-parameters-to-launch-stream" target="_parent">step 4</a></li>
		<li>To complete the additional work (CORS, video size, additional API calls, WebView), see: <a href="#wab-pkit-appendix" target="_parent">Appendix</a></li>
	</ul>
	</li>
</ul>

<p><strong>Example:</strong></p>

<table class="table-clear" style="max-width: 100%">
	<tbody>
		<tr>
			<td style="width:1%">&nbsp;</td>
			<td style="width:49%">
			<pre class="prettyprint lang-html">
&amp;lt;!DOCTYPE html&amp;gt;
&amp;lt;html&amp;gt;
&amp;lt;head&amp;gt;
  <code class="table-green">&amp;lt;script src=&quot;//{playerUrl}&quot;&amp;gt;&amp;lt;/script&amp;gt;</code>
&amp;lt;/head&amp;gt;
&amp;lt;body&amp;gt;
  <code class="table-green">&amp;lt;div id=&quot;{player-container}&quot;&amp;gt;&amp;lt;/div&amp;gt;</code>
  <code class="table-green">&amp;lt;script&amp;gt;</code>
    // API calls here or link to your app .js in script src tag
  <code class="table-green">&amp;lt;/script&amp;gt;</code>
&amp;lt;/body&amp;gt;
&amp;lt;/html&amp;gt;
</pre>
			<!-- CHECK IF THIS NEEDS TO BE PRESENT: <script src="yourApp.js"></script> --></td>
			<td style="width:1%">&nbsp;</td>
			<td style="width:49%">
			<div class="pf-box-info">
			<p><code class="dl-small table-green">&amp;lt;script src=&quot;//{playerUrl}&quot;&amp;gt;</code> is in <code class="dl-small">&amp;lt;head&amp;gt;</code> in our example but you can place it in a page <code class="dl-small">&amp;lt;body&amp;gt;</code> and choose the current or a semantic <code class="dl-small">wab.kit.js</code> version. Only reference it once.</p>

			<p><code class="dl-small table-green">&amp;lt;div id=&quot;{}&quot;&amp;gt;</code> creates a player instance in the DOM. If using your own player UI you can choose a <code class="dl-small">div</code> or other element (not <code class="dl-small">video</code>) and where to put it.</p>

			<p><code class="dl-small table-green">&amp;lt;script&amp;gt;</code> will create all necessary DOM elements. You can place all <code class="dl-small">&amp;lt;script&amp;gt;</code> tags anywhere before the closing <code class="dl-small">&amp;lt;/body&amp;gt;</code> tag, near the footer (unless a script must run before the page loads). This ensures all <code class="dl-small">&amp;lt;div&amp;gt;</code> elements can load successfully and the DOM is ready to be manipulated. It can also aid page load speed and avoid issues such as conflicts with other scripts running on the page.</p>
			</div>
			</td>
		</tr>
	</tbody>
</table>

<hr class="hrsmall no-bottom-margin no-print" /><!-- link to main guide #wab-pkit-call-mcc-feed -->
<h3><a id="step-two" name="step-two"></a>➁ Get Fixture and Stream Schedules and UUIDs - call the Content Catalogue feed &nbsp; <span class="light-grey"><kbd>/mcc</kbd></span></h3>

<p>Call the Content Catalogue feed to get a schedule of upcoming fixtures, and get the fixture and livestream asset IDs you need to request an OAuth access token for the livestream session.</p>

<p>Each fixture listing is represented as an <code>mccItem</code> element in the response, and comprises <strong><code>fixture</code></strong> and <strong><code>asset</code></strong> elements. These contain the metadata, such as the unique fixture and asset IDs (UUIDs), scheduled start date/time, rights, and linked contestants, sport, and competition. All will have a &#39;fastdata&#39; <code>asset</code> but a &#39;livestream&#39; <code>asset</code> may not be available.</p>

<div class="quote-block"><!-- tableqpvalueexample -->
<table class="table-clear">
	<tbody>
		<tr>
			<td><code>fixture</code></td>
			<td><strong><code>type</code></strong> <code>&quot;urn:perform:mfl:<strong>fixture</strong>&quot;</code></td>
			<td><strong><code>id &quot;{fixtureUuid}&quot;</code></strong></td>
			<td><strong>fixture</strong> available (Player Kit/Content Player)</td>
			<td>all fixtures</td>
		</tr>
		<tr>
			<td colspan="5">&nbsp;</td>
		</tr>
		<tr>
			<td style="width: 8%"><code>asset</code></td>
			<td style="width: 25%"><strong><code>type</code></strong> <code>&quot;urn:perform:<strong>fastdata</strong>&quot;</code></td>
			<td style="width: 20%"><strong><code>id &quot;{assetUuid}&quot;</code></strong></td>
			<td style="width: 30%"><strong>visualisation data</strong> available (Content Player only)</td>
			<td style="width: 18%">all fixtures</td>
		</tr>
		<tr>
			<td style="width: 8%"><code>asset</code></td>
			<td><strong><code>type</code></strong> <code>&quot;urn:perform:<strong>livestream</strong>&quot;</code></td>
			<td><strong><code>id &quot;{streamUuid}&quot;</code></strong></td>
			<td><strong>livestream</strong> available (Player Kit/Content Player)</td>
			<td>NOT all fixtures</td>
		</tr>
	</tbody>
</table>
</div>

<p>You should use this metadata to do the following:</p>

<ul>
	<li><strong>get the livestream ID</strong> <em>(mandatory)</em> - the <code>id</code> value from an <code>asset</code> element that has a <code>type</code> attribute value of <code>&quot;urn:perform:livestream&quot;</code><br />
	Pay attention to the <code>lastUpdateTime</code> and <code>allowedCountryCodes</code> values. The <code>urn</code> attribute concatenates <code>id</code> and <code>type</code> values (<code>&quot;urn:perform:livestream:{uuid}&quot;</code>)</li>
	<li><strong>post the livestream ID in a call to <code>/oauth</code> feed to get an access token</strong> <em>(mandatory)</em> - then, pass the token in the Player Kit <code>&amp;lt;script&amp;gt;</code> wrapper/embed code</li>
	<li><strong>build a schedule of upcoming fixtures</strong> <em>(optional)</em> - for example in your frontend website/app</li>
</ul>

<h5>Example requests</h5>

<p>These examples specify an asset language of British English (<code>as.lang=en-gb</code>) and a sorting order of ascending, according to fixture date/time. This means the <code>mccItem</code> listings at the top of the feed are fixtures with the earliest start times that day. For example, fixtures scheduled to start at 10:00 (UTC) will be higher up than those starting at 11:00.</p>
<!-- div class="quote-block" -->

<div>
<p style="margin-left: 5px"><strong><span class="light-grey"><kbd>GET</kbd> fixtures -</span> for current day (default)</strong></p>

<table class="table-clear" style="margin-left:initial">
	<tbody>
		<tr>
			<td style="width: 5%"><em>n/a</em></td>
			<td style="width: 7%"><em>n/a</em></td>
			<td style="max-width: 88%">&nbsp;</td>
			<td>
			<pre class="small">
<code style="background-color: inherit">https://wab.performfeeds.com/mcc/{outletAuthKey}?_rt=b&amp;_fmt=json&amp;_fldGrp=mcc.a&amp;as.lang=en-gb&amp;_ord=fx.fDt&amp;_ordSrt=asc</code></pre>
			</td>
		</tr>
	</tbody>
</table>

<p style="margin-left: 5px"><strong><span class="light-grey"><kbd>GET</kbd> fixtures -</span> by number of days (fx.nDy)</strong></p>

<table class="table-clear" style="margin-left:initial">
	<tbody>
		<tr>
			<td style="width: 5%"><code>fx.nDy</code></td>
			<td style="width: 7%"><em>integer</em></td>
			<td style="max-width: 88%">
			<pre class="small">
<code style="background-color: inherit">https://wab.performfeeds.com/mcc/{outletAuthKey}?_rt=b&amp;_fmt=json&amp;_fldGrp=mcc.a&amp;as.lang={language}&amp;_ord=fx.fDt&amp;_ordSrt=asc&amp;<code>fx.nDy=14</code></code></pre>
			</td>
		</tr>
	</tbody>
</table>

<p style="margin-left: 5px"><strong><span class="light-grey"><kbd>GET</kbd> fixtures -</span> by start date to end date (fx.fDt)</strong></p>

<table class="table-clear" style="margin-left:initial">
	<tbody>
		<tr>
			<td style="width: 5%"><code>fx.fDt</code></td>
			<td style="width: 7%"><em>timestamp</em></td>
			<td style="max-width: 88%">&nbsp;</td>
			<td>
			<pre class="small">
<code style="background-color: inherit">https://wab.performfeeds.com/mcc/{outletAuthKey}?_rt=b&amp;_fmt=json&amp;_fldGrp=mcc.a&amp;_ord=fx.fDt&amp;_ordSrt=asc&amp;<code>fx.fDt=[2016-12-08T00:00:00Z TO 2016-12-20T00:00:00Z]</code></code></pre>
			</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong>Example response (JSON):</strong></p>

<p>The response body contains the <code>mccItem</code> objects for fixtures that match the criteria in your request. In this example, the <code>mccItem</code> represents <code>fixture</code> ID <code>54csvtpw8hip1eiv65bp4uuy9</code> and it has both livestream and data <code>asset</code> elements. To retrieve a livestream for a fixture, you need its livestream asset ID.</p>

<pre class="pre-scrollable prettyprint lang-json no-margin small">
{
   &quot;mccItem&quot;: [
      {
         &quot;name&quot;: &quot;Nice v Reims&quot;,
         &quot;type&quot;: &quot;urn:perform:mfl:fixture&quot;,
         &quot;fixture&quot;: {
            &quot;id&quot;: &quot;54csvtpw8hip1eiv65bp4uuy9&quot;,
            &quot;date&quot;: &quot;2016-04-22&quot;,
            &quot;time&quot;: &quot;19:00:00&quot;,
            &quot;sport&quot;: {
               &quot;id&quot;: &quot;289u5typ3vp4ifwh5thalohmq&quot;,
               &quot;value&quot;: &quot;Soccer&quot;
            },
            &quot;competition&quot;: {
               &quot;id&quot;: &quot;dm5ka0os1e3dxcp3vh05kmp33&quot;,
               &quot;code&quot;: &quot;FRA&quot;,
               &quot;value&quot;: &quot;Ligue 1&quot;
            },
            &quot;contestant&quot;: [
               {
                  &quot;id&quot;: &quot;3c3jcs7vc1t6vz5lev162jyv7&quot;,
                  &quot;value&quot;: &quot;Reims&quot;
               },
               {
                  &quot;id&quot;: &quot;bx0cdmzr2gwr70ez72dorx82p&quot;,
                  &quot;value&quot;: &quot;Nice&quot;
               }
            ]
         },
         &quot;asset&quot;: [
            {
               &quot;id&quot;: &quot;1kc73cm2uhwjw12fp59ark7lo9&quot;,
               &quot;name&quot;: &quot;Nice v Reims&quot;,
               &quot;type&quot;: &quot;urn:perform:livestream&quot;,
               &quot;urn&quot;: &quot;urn:perform:livestream:1kc73cm2uhwjw12fp59ark7lo9&quot;,
               &quot;lastUpdateTime&quot;: &quot;2016-04-11T13:25:06Z&quot;,
               &quot;allowedCountryCodes&quot;: &quot;AE AF AG AI AL AM&quot;
            },
            {
               &quot;id&quot;: &quot;ej9cnyc2lmczfeshz1np86ynd&quot;,
               &quot;type&quot;: &quot;urn:perform:fastdata&quot;,
               &quot;urn&quot;: &quot;urn:perform:fastdata:ej9cnyc2lmczfeshz1np86ynd&quot;,
               &quot;lastUpdateTime&quot;: &quot;2015-04-13T12:30:27Z&quot;
            }
         ]
      }
   ]
}
</pre>

<div class="pf-box-note">On success, the HTTP status code in the response header is <code>200</code> (OK). On error, the body contains an error code. For JSONP, successful and failed requests return status code <code>200</code>.<br />
To avoid errors, remember to use the correct case, syntax, and operators in URLs (such as <code>?</code> <code>_</code> <code>&amp;</code> <code>,</code> <code>=</code>). Your Outlet Authorisation Key, query parameters and values are case-sensitive. If you receive an error, see the <strong>Error codes</strong> section.</div>

<table class="tableqpvalueexample">
	<thead>
		<tr>
			<th>Element/Attribute</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><strong><code>&amp;lt;mccItem&amp;gt;</code></strong></td>
			<td><strong><em>parent element:</em> contains all fixture and asset metadata about a single fixture/MCC item</strong><br />
			<strong><em>attributes</em>:</strong> <code>&quot;name&quot;</code> <code>&quot;type&quot;</code> <code>&quot;id&quot;</code><br />
			<strong><em>elements</em>:</strong> <code>&amp;lt;fixture&amp;gt;</code> and <code>&amp;lt;assets&amp;gt;</code></td>
		</tr>
		<tr>
			<td>&nbsp; <code>name</code></td>
			<td><em>attribute</em>: name of the fixture (sometimes the participating contestants are named here)</td>
		</tr>
		<tr>
			<td>&nbsp; <code>type</code></td>
			<td><em>attribute</em>: type - always a fixture (<code>urn:perform:mfl:fixture</code>) for Watch&amp;Bet</td>
		</tr>
		<tr>
			<td>&nbsp; <code>id</code></td>
			<td><em>attribute</em>: unique ID (UUID) of the MCC item - the value matches the <code>fixture</code> element <code>&quot;id&quot;</code></td>
		</tr>
		<tr>
			<td><strong><code>&amp;lt;fixture&amp;gt;</code></strong></td>
			<td><strong><em>element</em>: contains all fixture metadata</strong><br />
			<em>attributes</em>: <code>&quot;id&quot;</code> <code>&quot;date&quot;</code> <code>&quot;time&quot;</code><br />
			<em>sub-elements</em>: <code>&amp;lt;sport&amp;gt;</code> <code>&amp;lt;competition&amp;gt;</code> <code>&amp;lt;contestants&amp;gt;</code></td>
		</tr>
		<tr>
			<td>&nbsp; <code>id</code></td>
			<td><em>attribute</em>: unique ID (UUID) of the fixture (also known as the MFL Fixture UUID) - the value matches the <code>mccItem</code> element <code>&quot;id&quot;</code></td>
		</tr>
		<tr>
			<td>&nbsp; <code>date</code></td>
			<td><em>attribute</em>: scheduled fixture start date (UTC format)</td>
		</tr>
		<tr>
			<td>&nbsp; <code>time</code></td>
			<td><em>attribute</em>: scheduled fixture start time (UTC format)</td>
		</tr>
		<tr>
			<td>&nbsp; <code>&amp;lt;sport&amp;gt;</code></td>
			<td><strong><em>sub-element</em>:</strong> contains <em>attribute values</em> <code>id</code> <code>name</code> of the Sport</td>
		</tr>
		<tr>
			<td>&nbsp; <code>&amp;lt;competition&amp;gt;</code></td>
			<td><strong><em>sub-element</em>:</strong> contains <em>attribute values</em> <code>id</code> <code>name</code> <code>country</code> of the Competition</td>
		</tr>
		<tr>
			<td>&nbsp; <code>&amp;lt;contestants&amp;gt;</code></td>
			<td><strong><em>sub-element</em>:</strong> contains a <code>&amp;lt;contestant&amp;gt;</code> sub-element and metadata for each Contestant in the fixture</td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>&amp;lt;contestant&amp;gt;</code></td>
			<td><strong><em>sub-element</em>:</strong> contains <em>attribute values</em> for the unique <code>id</code> and <code>name</code> of a Contestant</td>
		</tr>
		<tr>
			<td><strong><code>&amp;lt;assets&amp;gt;</code></strong></td>
			<td><strong><em>element</em>:</strong> contains <code>&amp;lt;asset&amp;gt;</code> sub-elements and metadata for each asset linked to the fixture</td>
		</tr>
		<tr>
			<td>&nbsp; <strong><code>&amp;lt;asset&amp;gt;</code></strong></td>
			<td><strong><em>sub-element</em>:</strong> contains <em>attribute values</em> for the asset <code>id</code> <code>name</code> <code>type</code> <code>urn</code> <code>lastUpdateTime</code> (and <code>allowedCountryCodes</code><code>blockedCountryCodes</code> for a livestream)</td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>id</code></td>
			<td><em>attribute</em>: unique ID (UUID) of the asset. <strong>In the OAuth call for a livestream:</strong> Specify the livestream/stream ID: <code>asset_id={streamUuid}</code> <!-- To authorise a user's request for a livestream in the OAuth service, you must specify it as the asset ID in the request --></td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>(value)</code></td>
			<td><em>string value</em>: name describing the asset</td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>type</code></td>
			<td><em>attribute</em>: type of asset - <code>urn:perform:livestream</code> (livestream) or <code>urn:perform:fastdata</code> (data). <strong>In the OAuth call for a livestream:</strong> Use <code>scope=livestream</code></td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>urn</code></td>
			<td><em>attribute</em>: URN of asset - it concatenates <code>type</code> and <code>id</code> values (<code>asset</code> element). Example: <code>urn:perform:livestream:1kc73cm2uhwjw12fp59ark7lo9</code></td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>lastUpdateTime</code></td>
			<td><em>attribute</em>: last update time of the asset (international date format: <code>YYYY-MM-DDTHH:MM:SSZ</code>)</td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>allowedCountryCodes</code></td>
			<td><em>attribute</em> (livestream): countries the stream <strong>is available</strong> to (as <a href="#wab-pkit-mcc-asset-allowed-blocked-country-codes" target="_parent">space-separated codes</a>, eg <code>AU GB</code>). Returned only when passing <strong><code>mcc.a</code></strong> to <code>_fldGrp</code> in the call</td>
		</tr>
		<tr>
			<td>&nbsp; &nbsp; <code>blockedCountryCodes</code></td>
			<td><em>attribute</em> (livestream): countries the stream <strong>is not available</strong> to (as <a href="#wab-pkit-mcc-asset-allowed-blocked-country-codes" target="_parent">space-separated codes</a>, eg <code>AU GB</code>). Returned only when passing <strong><code>mcc.b</code></strong> to <code>_fldGrp</code> in the call</td>
		</tr>
	</tbody>
</table>

<h4 id="wab-pkit-call-mcc-feed-parameters-values">Content Catalogue feed - Request Parameters and Values (/mcc)</h4>

<p>The example requests include the following elements, and <em>required</em> and key <em>optional</em> parameters.</p>

<div class="acc-container">
<div class="acc-accordion acc-light-grey"><button class="acc-btn-block acc-left-align"><strong>&nbsp; SHOW/HIDE </strong></button>

<div class="acc-accordion-content acc-container normal" id="wab-acc-mcc3">
<p class="margin-small"><strong>Basic URL:</strong> <code>https://wab.performfeeds.com/mcc/{outletAuthKey}/?{queryParameters}</code></p>

<table class="tableqpvalueexample">
	<thead>
		<tr>
			<th>Element/URL Parameter</th>
			<th>Description and accepted values</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code>https://</code><br />
			<code>wab.performfeeds.com/mcc</code></td>
			<td><em>Required</em>. <strong>Protocol, base domain and MCC endpoint (/mcc)</strong> to access fixture and livestream asset metadata in the Perform Feeds Master Content Catalogue. For Watch&amp;Bet, only <strong>https</strong> is supported.</td>
		</tr>
		<tr>
			<td><code>{outletAuthKey}</code></td>
			<td><em>Required</em>. <strong>Outlet Authentication Key</strong>. An alphanumeric UUID supplied to each outlet when we confirm your setup details. Requests made without a valid key will be rejected.</td>
		</tr>
		<tr>
			<td><code>{queryParameters}</code></td>
			<td><em>Required</em>. <strong>Query parameters</strong>. Query the feed and control the returned content, formatting and page size. Some require a prefix (<code>_</code>). You must specify all <em>required</em> parameters and valid values.</td>
		</tr>
		<tr>
			<td><code>_rt={mode}</code></td>
			<td><em>Required</em>. <strong>Operating mode / request type (query parameter)</strong>. Value: <code>b</code> (B2B). If you do not pass the <code>{mode}</code> value as <code>b</code>, the behaviour of the feed is B2C and results may not be as expected.</td>
		</tr>
		<tr>
			<td><code>_fmt={dataFormat}</code></td>
			<td><em>Recommended</em>. <strong>Data format (query parameter)</strong>. Values (<code>/mcc</code>): <code>xml</code>, <code>json</code>, and <code>jsonp</code> (requires <code>_clbk</code> query parameter). If you do not specify <code>_fmt</code>, it is XML (default).</td>
		</tr>
		<tr>
			<td><code>as.lang={language}</code></td>
			<td><em>Recommended</em>. <strong>Asset language (query parameter)</strong>. Values: one or more <a href="#wab-pkit-asset-language" target="_parent">asset language codes</a> or <code>*</code> (wildcard). If you do not use the <code>as.lang</code> parameter or pass a valid value to it, the settings of a user&#39;s browser will determine the language and returned content may not be as expected. Examples:<br />
			&bull; <code>as.lang=en-gb</code> (British English only)<br />
			&bull; <code>as.lang=en-gb,en-us,fr-fr</code> (&#39;OR&#39; logic: British English, US English, French)<br />
			&bull; <code>as.lang=*</code> (any language using wildcard: <code>*</code>)</td>
		</tr>
		<tr>
			<td><code>_fldGrp={fieldGroupMask}</code></td>
			<td><em>Required</em>. <strong>Field group/mask (query parameter)</strong>. Values: see the <a href="#wab-pkit-call-mcc-feed-step-three" target="_parent">example requests</a> and <a href="#wab-pkit-mcc-parameters-fldgrp" target="_parent"><code>_fldGrp</code></a>. Returns metadata associated with the <code>{fieldGroupMask}</code> specifier value, including details of fixtures (such as ID, date/time, sport and competition) and assets (such as type, livestream or fastdata, and ID). Example values:<br />
			&bull; <code>mcc.a</code> - returns all key fields including <strong>Allowed</strong> Country Codes (attribute <code>allowedCountryCodes</code>)<br />
			&bull; <code>mcc.b</code> - returns all key fields including <strong>Blocked</strong> Country Codes (attribute <code>blockedCountryCodes</code>)<br />
			&bull; <code>fx.idnt.{supplierName}</code> - returns <code>identifier</code> metadata (holds <code>supplierName</code> and own fixture <code>id</code>)<br />
			Example: <code>fx.idnt.RB</code> = RunningBall. If a fixture is not linked to the specified supplier, it is returned without identifier metadata. You cannot use the ID to get a livestream or data; it is supplied to help partners who have already mapped to the supplier to integrate their systems.</td>
		</tr>
		<tr>
			<td><code>_ord={fieldMask}</code></td>
			<td><em>Optional</em>. <strong>Ordering (query parameter)</strong> - the data field to use as the criteria by which to order the feed.<br />
			Values: abbreviation of a field mask, such as <code>fx.fDt</code> (fixture date and time).</td>
		</tr>
		<tr>
			<td><code>_ordSrt={sortOrder}</code></td>
			<td><em>Optional</em>. <strong>Sort order (query parameter)</strong> of the feed. It is only valid when passed with the <code>_ord</code> parameter and a field value in a request. Values: <code>asc</code> (ascending) or <code>desc</code> (descending).</td>
		</tr>
		<tr>
			<td><code>fx.fDt=[{date TO date}]</code></td>
			<td><em>Optional</em>. <strong>Fixture date and time range (query parameter)</strong>. Value: in XSD (YYYY-MM-DDTHH:MM:SSZ) or milliseconds (since the epoch). Examples:<br />
			&bull; <code>[xsd:dateTime TO xsd:dateTime]</code> or <code>[xsd:dateTime TO *]</code> or <code>[* TO xsd:dateTime]</code><br />
			&bull; <code>[milliseconds TO milliseconds]</code> or <code>[milliseconds TO *]</code> or <code>[* TO milliseconds]</code></td>
		</tr>
		<tr>
			<td><code>fx.nDy={numberDays}</code></td>
			<td><em>Optional</em>. <strong>Fixture date range - by day (query parameter)</strong> - number of days ahead of/before the request date. Value: integer in the range <code>[&amp;lt;-14,14&amp;gt;]</code> (<code>0</code> is invalid). Examples (using a current date of 2017-02-13):<br />
			&bull; <code>fx.nDy=1</code> (same day). Equivalent: <code>fx.fDt=[2017-02-13T00:00:00Z TO 2017-02-13T23:59:59Z]</code><br />
			&bull; <code>=2</code> (today to tomorrow). Equivalent: <code>fx.fDt=[2017-02-13T00:00:00Z TO 2017-02-14T23:59:59Z]</code><br />
			&bull; <code>=-1</code> (today to yesterday). Equivalent: <code>fx.fDt=[2017-02-12T00:00:00Z TO 2017-02-13T23:59:59Z]</code></td>
		</tr>
	</tbody>
</table>
</div>
<!-- end wab-acc-mcc3 --></div>
<!--end acc-accordion acc-light-grey --></div>
<!-- end acc-container -->

<div class="pf-box-caution"><strong>Caution:</strong><br />
If you do not pass any query parameters in the request URL, you may receive an error or limited data from the response. While some query parameters are optional, you should always specify the following query parameters (and valid values) to ensure that your requests are successful and return fixture and asset metadata, of the asset language(s) and in the response format, which you require: <code>_rt</code>, <code>_fmt</code>, <code>_fldGrp</code>, <code>as.lang</code></div>

<h4 id="wab-pkit-mcc-feed-errors">Content Catalogue feed - Error codes and explanations</h4>

<div class="acc-container">
<div class="acc-accordion acc-light-grey"><button class="acc-btn-block acc-left-align"><strong>&nbsp; SHOW/HIDE</strong></button>

<div class="acc-accordion-content acc-container normal" id="wab-acc-mcc5">
<table class="tableqpvalueexample">
	<thead>
		<tr>
			<th>Perform error code</th>
			<th>HTTP code</th>
			<th>Explanation</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>n/a</td>
			<td><code>200</code></td>
			<td>Successful request. No error.</td>
		</tr>
		<tr>
			<td><code>10000</code></td>
			<td><code>500</code></td>
			<td>An unexpected error has occurred. Verify that the URL, Outlet Authentication Key, query parameters and format are correct, and then try again. We supply your Outlet Authorisation Key to you when we setup your account. Contact us if you continue to receive the error.</td>
		</tr>
		<tr>
			<td><code>10001</code></td>
			<td><code>404</code></td>
			<td>Page not found. You may have requested a page that does not exist. Only <code>mccItems</code> with a fixture start date that is within a range of 14 days ahead of or before the current date will be returned.</td>
		</tr>
		<tr>
			<td><code>10010</code></td>
			<td><code>404</code></td>
			<td>Outlet does not exist. Incorrect or missing Outlet Authentication Key or mandatory character (<code>?</code> or <code>_</code>).<br />
			&bull; Make sure you have specified the correct Outlet Authentication Key. A unique key is supplied to each outlet when we first confirm your account is active. Contact us if the error continues.<br />
			&bull; Check for incorrect or missing characters. A <code>?</code> must be specified before the first query parameter in the URL: <code>?{queryParameters}</code>. The <code>_</code> (underscore) prefix is mandatory for some query parameters, such as <code>_rt</code> and <code>_fmt</code>.<br />
			<br />
			Make sure you have structured request URLs correctly. Example:<br />
			<code>{protocol}://{domain}/{resource}/{outletAuthKey}?{queryParameters}</code><br />
			Note: <code>{protocol}://{domain}</code> represents: <code>https://wab.performfeeds.com</code></td>
		</tr>
		<tr>
			<td><code>10100</code></td>
			<td><code>500</code></td>
			<td>Feed Unavailable. The service is not available at this time. Wait a short time, and then try again. If the error persists, contact Perform.</td>
		</tr>
		<tr>
			<td><code>10101</code></td>
			<td><code>500</code></td>
			<td>Feed Unavailable. The service is not available at this time. Wait a short time, and then try again. If the error persists, contact Perform.</td>
		</tr>
		<tr>
			<td><code>10102</code></td>
			<td><code>503</code></td>
			<td>Feed Unavailable. Temporary error - wait a short time and then try again. If the error persists, contact Perform.</td>
		</tr>
		<tr>
			<td><code>10200</code></td>
			<td><code>400</code></td>
			<td>Missing request parameter. Ensure all mandatory parameters are supplied and correct. If you specify the format as JSONP, you must also specify the callback function in the URI or Accept Header: <code>_fmt=jsonp&amp;_clbk={callbackFunction}</code></td>
		</tr>
		<tr>
			<td><code>10201</code></td>
			<td><code>562</code></td>
			<td>Unknown request parameter. A parameter has been supplied that does not exist in the <code>/mcc</code> feed. Check that the parameter and associated value exists for the feed, and that both are correct, and then try again. Note: The <code>/mcc</code> feed only supports requests in B2B mode (parameter-value of <code>_rt=b</code>. It does not support B2C mode.</td>
		</tr>
		<tr>
			<td><code>10202</code></td>
			<td><code>400</code></td>
			<td>Invalid request parameter value. You have passed invalid values to a valid query parameter, or multiple values to a parameter that only supports one value per request. Check the values and try again. Possible reasons:<br />
			&bull; Invalid value. Example: (<code>0</code>) in <code>fx.nDy=0</code> (invalid). Valid: Integer in <code>[&amp;lt;-14,14&amp;gt;]</code><br />
			&bull; Missing character. Example: (<code>.</code>) in <code>_fldGrp=mcca</code> (invalid). Valid: <code>mcc.a</code><br />
			&bull; Extra character. Example: (<code>+</code>) in <code>fx.nDy=+1</code> (invalid). Valid: <code>fx.nDy=1</code><br />
			&bull; Missing value. Example: If you specify the format as JSONP, you must also specify the callback function in the URI or Accept Header: <code>_fmt=jsonp&amp;_clbk={callbackFunction}</code></td>
		</tr>
		<tr>
			<td><code>10203</code></td>
			<td><code>400</code></td>
			<td>Ambiguous/Invalid request parameters. The request is invalid for one or more of the following reasons:<br />
			&bull; You have specified a parameter more than once in a request. Remove the duplicate parameter and try again.<br />
			&bull; You have used a combination of query parameters in the same request which cannot be used together (they do not make sense). Example: Using both <code>fx.nDy</code> and <code>fx.fDt</code> together.</td>
		</tr>
		<tr>
			<td><code>10204</code></td>
			<td><code>405</code></td>
			<td>Invalid request method. You are attempting to use <code>POST</code> with an endpoint that only accepts <code>GET</code>. Amend the method and try again.</td>
		</tr>
		<tr>
			<td><code>10210</code></td>
			<td><code>415</code></td>
			<td>Unsupported format: <code>_fmt</code> parameter. Check the supplied format value is valid for the feed. If using the <code>_fmt</code> parameter in the URI check that a valid value has been specified. If a value has not been specified, then default format for the feed is used to determine the response body format. Note: Perform Feeds does not support the Accept header method.</td>
		</tr>
		<tr>
			<td><code>10212</code></td>
			<td><code>400</code></td>
			<td>Unsupported mode: <code>_rt</code> parameter. The <code>/mcc</code> feed only supports requests in B2B mode (parameter-value of <code>_rt=b</code>. It does not support B2C mode (<code>c</code>). If a value is not supplied then B2C mode is used by default.</td>
		</tr>
		<tr>
			<td><code>10214</code></td>
			<td><code>400</code></td>
			<td>Unknown field requested : <code>_fld</code> / <code>_fldGrp</code> parameter. The field mask specifier you have supplied to the <code>_fld</code> or <code>_fldGrp</code> is not recognised for this feed. Most Perform Feeds share a common set of format query parameters that generally, but not always, offer the same behaviour and functionality. Query parameters, values and behaviour can differ, so make sure that the field mask value you specified is correct for the feed you are attempting to call, as given in our guide.</td>
		</tr>
		<tr>
			<td><code>10216</code></td>
			<td><code>400</code></td>
			<td>Invalid sorting: <code>_ord</code>, <code>_ordSrt</code> parameters. See <code>_ord</code> and <code>_ordSrt</code> for details.<br />
			&bull; <code>_ord</code> defines which field(s) are used to sort results; make sure you pass a valid value - this is the field mask&#39;s abbreviated name.<br />
			&bull; <code>_ordSrt</code> controls the sorting order of results in <code>asc</code> (ascending, default) or <code>desc</code> (descending) order, based on the field value passed to <code>_ord</code>; <code>_ordSrt</code> has no effect if <code>_ord</code> is not used.</td>
		</tr>
		<tr>
			<td><code>10217</code></td>
			<td><code>400</code></td>
			<td>Invalid pagination: <code>_pgNm</code>, <code>_pgSz</code> parameters. The page number or page size is not available. Page size is controlled by the operating mode (see <code>_rt</code> parameter). Page number is determined by the page size and number of results available - try the request without these parameters to find out the number of results in the first page. Note: Specify a value of &#39;1&#39; or higher; &#39;0&#39; is not valid. See the <code>_pgSz</code> and <code>_pgNm</code> for details.</td>
		</tr>
		<tr>
			<td><code>10219</code></td>
			<td><code>413</code></td>
			<td>Invalid size of parameter: <code>_pgSz</code> parameter. The specified page size exceeds the permitted maximum for the outlet. Page size is subject to a maximum of 1000 results per page (B2B requests). Otherwise, send the request without <code>_pgNm</code> or <code>_pgSz</code>.</td>
		</tr>
		<tr>
			<td><code>10300</code></td>
			<td><code>403</code></td>
			<td>Feed Access Denied. Check that the feed is available within your subscription, and that you have specified the correct settings for the access URI, B2B mode referer type (<code>_rt=b</code>), and domains/IP addresses (for whitelisting).</td>
		</tr>
		<tr>
			<td><code>10310</code></td>
			<td><code>401</code></td>
			<td>User not Authorised. You have not supplied a valid Outlet Authentication Key - your unique key is supplied to you when we first confirm your account is active. Make sure that a valid key is supplied in the HTTP request URI or in the Accept header. Contact us if you continue to receive this error or need to confirm your key.</td>
		</tr>
		<tr>
			<td><code>10320</code></td>
			<td><code>503</code></td>
			<td>Acceptable usage exceeded. Allow up to 30 minutes before sending further requests - if the error remains, contact Perform.</td>
		</tr>
		<tr>
			<td><code>10400</code></td>
			<td><code>404</code></td>
			<td>No data found. Try again. If the error continues, it may be that no MCC items (fixtures and assets) match the combination of query parameters and values - attempt a call using different search criteria (try a simple call for testing purposes).</td>
		</tr>
		<tr>
			<td><code>10401</code></td>
			<td><code>408</code></td>
			<td>Data request timeout. Request for specific data timed out. Try again. If the error continues, attempt a simple request to the feed (do not use many parameters and values).</td>
		</tr>
		<tr>
			<td><code>10402</code></td>
			<td><code>503</code></td>
			<td>Data corrupt. Try again. If the error continues try a request for simple and different search criteria.</td>
		</tr>
		<tr>
			<td><code>10501</code></td>
			<td><code>415</code></td>
			<td>Unsupported media format. Check the configuration.</td>
		</tr>
		<tr>
			<td><code>10502</code></td>
			<td><code>412</code></td>
			<td>Missing character encoding. Declare the character encoding. <strong>Note:</strong> The API expects data to be <code>UTF-8</code> encoded.</td>
		</tr>
		<tr>
			<td><code>10503</code></td>
			<td><code>412</code></td>
			<td>Invalid feed enclosed in body.</td>
		</tr>
	</tbody>
</table>
</div>
<!--end wab-acc-mcc5 --></div>
</div>

<hr class="hrsmall no-bottom-margin no-print" id="wab-pkit-call-oauth-feed" /><!-- link to main guide #wab-pkit-call-oauth-feed -->
<h3 id="wab-pkit-call-oauth-feed"><a id="step-three" name="step-three"></a>➂ Call the OAuth feed to get the access token and authorisation &nbsp; <span class="light-grey"><kbd>/oauth</kbd></span></h3>

<p>Your back-end server must make a <code>POST</code> request to <code>/oauth/token</code> over HTTPS, passing the livestream asset ID and type from the <code>/mcc</code> feed response, a username that uniquely identifies the end user, and a hashed password in the request body. This serves to authorise the end user and livestream, and returns a unique <code>access_token</code>. A new token is required for each user and livestream session. <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST" target="_blank"><code>POST</code> supports</a> an <a href="https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms" target="_blank">HTML form</a> (or similar) and other requests (such as XHR, <a href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest" target="_blank"><code>XMLHttpRequest</code></a>). You can implement your code and as you wish.</p>

<p><strong>Example request (JSON):</strong></p>

<p>In this example, we use an HTML form where data is included in the request entity body, not appended to the URL (as with XHR requests).</p>

<pre class="small prettyprint lang-json">
POST https://wab.performfeeds.com/oauth/token/{outletAuthKey}?_rt=b&amp;_fmt=json
Content-Type: application/x-www-form-urlencoded
</pre>

<pre class="small prettyprint lang-json">
grant_type=password    // value is always &#39;password&#39;
asset_id={livestreamUuid}    // from attribute id=&quot;&quot; (of mccItem &amp;gt; &#39;asset&#39; element of type &#39;livestream&#39; in the /mcc response. Example: 1kc73cm2uhwjw12fp59ark7lo9 from the example in step 2)
scope=livestream    // from attribute type=&quot;urn:perform:livestream&quot; (of mccItem &amp;gt; &#39;asset&#39; element in the /mcc response)
username={userId}    // any unique user identifier, allocated by you (see: OAuth Parameters)
password={hashedPassword}    // hashed password, generated by you (see: OAuth Parameters)
</pre>

<p><strong>Example response (JSON):</strong><br />
On success, the body contains a unique access token, which is valid only for the specified time (in seconds), the requesting user, livestream, and where the user has logged in from (eg browser instance or device). Your application should parse the <code>&quot;access_token&quot;</code> value and use it to request the livestream session within the token&#39;s <code>&quot;expires_in&quot;</code> time (this is usually 10 seconds).</p>

<pre class="small prettyprint lang-json">
{
  &quot;access_token&quot;:&quot;ABc1defGH23iJKLMNO4PQr56st7UV8w&quot;, 
  &quot;token_type&quot;:&quot;Bearer&quot;,
  &quot;uuid&quot;:&quot;{userId}&quot;,
  &quot;expires_in&quot;:10
}
</pre>

<div class="pf-box-caution">Access tokens cannot be refreshed or re-used. If a token expires before it is used by the Player Kit, error <code>10312</code> is returned. Your application must request a new access token if an access token or a session expires. For example, if a player&#39;s livestream session is terminated while an event is in progress and the user tries to relaunch it.</div>

<h5 id="wab-pkit-oauth-parameters">OAuth Parameters</h5>

<p>You must do the following to ensure the request URL is valid and avoid your requests being rejected:</p>

<ul>
	<li>use <code>POST</code> and <code>https</code> protocol (HTTP connections are not supported)</li>
	<li>replace <code>{outletAuthKey}</code> with your unique Outlet Authorisation Key</li>
	<li>specify <code>_rt=b</code> and <code>_fmt=json</code> or <code>_fmt=jsonp</code> (Note: <code>jsonp</code> <em>requires</em> <code>_fmt=jsonp&amp;_clbk={callbackFunction}</code> - replace <code>{callbackFunction}</code> with your callback)</li>
</ul>

<p>How you implement the calls and parameters depends on your programming language. For example, for an <a href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/setRequestHeader" target="_blank"><code>XMLHttpRequest</code></a> use method <code>setRequestHeader()</code> for each parameter-value pair to append the text to the header and merge the values into one single request header. Example: <code>xhr.setRequestHeader(&#39;grant_type&#39;, &#39;password&#39;);</code></p>

<table class="tableqpvalueexample">
	<thead>
		<tr>
			<th>Body Parameter</th>
			<th>Value</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code>grant_type</code></td>
			<td><code>password</code></td>
			<td><em>Required</em>. This field must contain the value <code>password</code> (as defined in the OAuth 2.0 specification).</td>
		</tr>
		<tr>
			<td><code>asset_id</code></td>
			<td><code>{livestreamUuid}</code></td>
			<td><em>Required</em>. The value of the <code>id</code> attribute for a livestream <code>asset</code> (<code>id=&quot;{livestreamUuid}&quot;</code>).</td>
		</tr>
		<tr>
			<td><code>scope</code></td>
			<td><code>livestream</code></td>
			<td><em>Required</em>. The value <code>livestream</code> from the <code>type</code> attribute for a livestream <code>asset</code> (<code>type=&quot;urn:perform:<strong>livestream</strong>&quot;</code>)</td>
		</tr>
		<tr>
			<td><code>username</code></td>
			<td><code>{userId}</code></td>
			<td><em>Required</em>. The value can be any user identifier allocated to your end user by you (the bookmaker, the &#39;Client&#39; in the authorisation flow). Make sure it is unique to your logged-in end user and corresponds with any existing username. It helps identify the user in any reports you request from our platform.</td>
		</tr>
		<tr>
			<td><code>password</code></td>
			<td><code>{hashedPassword}</code></td>
			<td><em>Required</em>. A Base64-encoded value which is a concatenation of username + Outlet Authorisation Key + stream UUID, hashed using SHA512 and generated by you (bookmaker, &#39;Client&#39;). It is unique to an end user and stream. SHA2-256 security is the minimum requirement. See: <strong>SHA512 Encryption</strong></td>
		</tr>
	</tbody>
</table>

<h5>SHA512 Encryption</h5>

<p>You must generate and supply a hashed password for an OAuth call. Here is an example of a concatenated password and returned, hashed password (do not use these values in your code).</p>

<pre class="prettyprint small">
var username =&quot;<span class="table-green">JBloggs</span>&quot;; 
var outletAuthKey = &quot;<span class="light-grey">outletAuthKey123</span>&quot;;
var streamUuid =&quot;<span class="table-magenta">1kc73cm2uhwjw12fp59ark7lo9</span>&quot;; 
// Use the CryptoJS library to generate hash var hash = CryptoJS.SHA512(username + outletAuthKey + streamUuid);
</pre>

<p>In this example, the concatenation of these values (username + Outlet Authorisation Key + livestream UUID) is: <code class="small">JBloggs<span class="light-grey">outletAuthKey123</span>1kc73cm2uhwjw12fp59ark7lo9</code> and the returned, hashed password is: <code>4e9893117c8e3f3489e81fcfa68d9d40b58d742ab3277b6856ded14401448fa13957fb7bdca64b14c8250df9abc3e6200abd89ef404404858d209633825c6811</code></p>

<pre class="prettyprint small">
Sha512(<span class="table-green">JBloggs</span><span class="light-grey">outletAuthKey123</span><span class="table-magenta">7ozy19tqjmun118qhuyzewcpb</span>) = 4e9893117c8e3f3489e81fcfa68d9d40b58d742ab3277b6856ded14401448fa13957fb7bdca64b14c8250df9abc3e6200abd89ef404404858d209633825c6811
</pre>

<div class="pf-box-note">You will need to add logic to your server-side applications to make secure requests for access tokens (for example, over HTTPS and not from the user&#39;s browser), assign a username that uniquely identifies the requesting end user, and generate a new, hashed password for each request. It also ensures that only your customers can access streams. This forms the &#39;Client to Authorisation Server Exchange&#39; in the <strong>Authorisation flow</strong> (you are the &#39;Client&#39; and our OAuth service is the &#39;Authorisation Server&#39;). Read more: <strong>Authorisation Guide</strong> and <a href="https://tools.ietf.org/html/rfc6749#section-4.3" target="_blank">RFC 6749</a> (4.3).</div>

<div class="pf-box-caution"><strong>Errors:</strong> On error, the OAuth service will not generate an access token and the response body contains an error code. See the <strong>Error codes</strong> section. Examples:<br />
&nbsp;&bull;&nbsp;HTTP <code>400</code> and error code <code>10202</code> - you have supplied an invalid value or parameter. Check each parameter-value and correct the problem.<br />
&nbsp;&bull;&nbsp;HTTP <code>400</code> and error code <code>10200</code> - you have not supplied a required parameter or have supplied a parameter without a value. Ensure each <em>required</em> parameter and its value is present.</div>

<div class="no-bottom-margin" id="wab-pkit-oauth-feed-errors-div">
<h4>OAuth feed - Error codes and explanations</h4>

<div class="acc-container">
<div class="acc-accordion acc-light-grey"><button class="acc-btn-block acc-left-align"><strong>&nbsp; SHOW/HIDE</strong></button>

<div class="acc-accordion-content acc-container normal" id="wab-acc-oauth2">
<table class="tableqpvalueexample">
	<thead>
		<tr>
			<th>Perform error code</th>
			<th>HTTP code</th>
			<th>Explanation</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>n/a</td>
			<td><code>200</code></td>
			<td>Success (no error). The response contains an access token, valid for the requested user and stream, and 10 seconds (expiry time).</td>
		</tr>
		<tr>
			<td><code>10000</code></td>
			<td><code>500</code></td>
			<td>An unexpected error has occurred. Verify that the URL, Outlet Authentication Key, query parameters and format are correct, and then try again. We supply your Outlet Authorisation Key to you when we setup your account. Contact us if you continue to receive the error.</td>
		</tr>
		<tr>
			<td><code>10010</code></td>
			<td><code>404</code></td>
			<td>Outlet does not exist. Incorrect or missing Outlet Authentication Key or mandatory character (<code>?</code> or <code>_</code>).<br />
			&bull; Make sure you have specified the correct Outlet Authentication Key. A unique key is supplied to each outlet when we first confirm your account is active. Contact us if the error continues.<br />
			&bull; Check for incorrect or missing characters. A <code>?</code> must be specified before the first query parameter in the URL: <code>?{queryParameters}</code>. The <code>_</code> (underscore) prefix is mandatory for some query parameters, such as <code>_rt</code> and <code>_fmt</code>.<br />
			<br />
			Make sure you have structured request URLs correctly. Example:<br />
			<code>{protocol}://{domain}/{resource}/{outletAuthKey}?{queryParameters}</code><br />
			Note: <code>{protocol}://{domain}</code> represents: <code>https://wab.performfeeds.com</code></td>
		</tr>
		<tr>
			<td><code>10200</code></td>
			<td><code>400</code></td>
			<td>Missing request parameter. A mandatory parameter has not been supplied. Ensure all required parameters are present and correct.<br />
			&bull; If you specify the format as JSONP, you must also specify the callback function in the URI or Accept Header: <code>_fmt=jsonp&amp;_clbk={callbackFunction}</code><br />
			&bull; A maximum of five parameter-value pairs are required: <code>grant_type</code>, <code>asset_id</code>, <code>scope</code>, <code>username</code>, <code>password</code>. The <code>username</code> parameter takes the form <code>username=&quot;{userId}&quot;</code>. The <code>{userId}</code> is a unique identifier you allocate to the end user.</td>
		</tr>
		<tr>
			<td><code>10201</code></td>
			<td><code>562</code></td>
			<td>Unknown request parameter. A parameter has been supplied that does not exist in the <code>/mcc</code> feed. Check that the parameter and associated value exists for the feed, and that both are correct, and then try again. Note: The <code>/mcc</code> feed only supports requests in B2B mode (parameter-value of <code>_rt=b</code>. It does not support B2C mode.</td>
		</tr>
		<tr>
			<td><code>10202</code></td>
			<td><code>400</code></td>
			<td>Invalid request parameter value. You have passed invalid values to a valid query parameter, or multiple values to a parameter that only supports one value per request. On error, no access token is returned. Check the values and try again. Possible reasons:<br />
			&bull; Invalid number of parameters (Forbidden): A request <em>requires</em> the five parameters (parameter-value pairs): <code>grant_type</code>, <code>asset_id</code>, <code>scope</code>, <code>username</code>, <code>password</code><br />
			&bull; Invalid <code>grant_type</code> value: <em>Requires</em> a name-value pair of <code>grant_type=password</code><br />
			&bull; Invalid <code>asset_id</code> value: <em>Requires</em> a name-value pair of <code>asset_id={streamUuid}</code><br />
			&bull; Invalid <code>scope</code> value: <em>Requires</em> a name-value pair of <code>scope=livestream</code><br />
			&bull; Invalid <code>password</code> value: <em>Requires</em> <code>password={password}</code> - the <code>{password}</code> is a unique value (Base64-encoded) that you must generate by concatenating the end-user&#39;s username, your Outlet Authorisation Key and the livestream ID, hashed using SHA512 (Note: Minimum hashing requirements are SHA2-256).<br />
			&bull; Extra character. Example: (<code>+</code>) in <code>_fmt=json+p</code> (invalid). Valid: <code>_fmt=jsonp</code><br />
			&bull; Missing character or value. Example: You have not passed a value to the <code>_fmt</code> parameter.</td>
		</tr>
		<tr>
			<td><code>10203</code></td>
			<td><code>400</code></td>
			<td>Ambiguous/Invalid request parameters. The request is invalid for one or more of the following reasons:<br />
			&bull; You have specified a parameter more than once in a request. Remove the duplicate parameter and try again.<br />
			&bull; You have used a combination of query parameters in the same request which cannot be used together (they do not make sense). Example: If you specify the format as JSONP, you must also specify the callback function in the URI or Accept Header: <code>_fmt=jsonp&amp;_clbk={callbackFunction}</code>. You cannot use the <code>_clbk</code> parameter with XML.</td>
		</tr>
		<tr>
			<td><code>10204</code></td>
			<td><code>405</code></td>
			<td>Invalid request method. You are attempting to use <code>GET</code> with an endpoint that only accepts <code>POST</code>. Amend the method.</td>
		</tr>
		<tr>
			<td><code>10210</code></td>
			<td><code>415</code></td>
			<td>Unsupported format: <code>_fmt</code> parameter. Check the supplied format value is valid for the feed. If using the <code>_fmt</code> parameter in the URI check that a valid value has been specified. If a value has not been specified, then default format for the feed is used to determine the response body format. Note: Perform Feeds does not support the Accept header method.</td>
		</tr>
		<tr>
			<td><code>10212</code></td>
			<td><code>400</code></td>
			<td>Unsupported mode: <code>_rt</code> parameter. The <code>/oauth</code> feed only supports requests in B2B mode (parameter-value of <code>_rt=b</code>. It does not support B2C mode (<code>c</code>). If a value is not supplied then B2C mode is used by default.</td>
		</tr>
		<tr>
			<td><code>10300</code></td>
			<td><code>403</code></td>
			<td>Feed Access Denied. Check that the feed is available within your subscription, and that you have specified the correct settings for the access URI, B2B mode referer type (<code>_rt=b</code>), and domains/IP addresses (for whitelisting).</td>
		</tr>
		<tr>
			<td><code>10310</code></td>
			<td><code>401</code></td>
			<td>User not Authorised. You have not supplied a valid Outlet Authentication Key - your unique key is supplied to you when we first confirm your account is active. Make sure that a valid key is supplied in the HTTP request URI or in the Accept header. Contact us if you continue to receive this error or need to confirm your key.</td>
		</tr>
		<tr>
			<td><code>10320</code></td>
			<td><code>503</code></td>
			<td>Acceptable usage exceeded. Allow up to 30 minutes before sending further requests - if the error remains, contact Perform.</td>
		</tr>
		<tr>
			<td><code>10401</code></td>
			<td><code>408</code></td>
			<td>Data request timeout. Request for specific data timed out. Try again. If the error continues, attempt a simple request to the feed (do not use many parameters and values).</td>
		</tr>
		<tr>
			<td><code>10402</code></td>
			<td><code>503</code></td>
			<td>Data corrupt. Try again. If the error continues try a request for simple and different search criteria.</td>
		</tr>
		<tr>
			<td><code>10501</code></td>
			<td><code>415</code></td>
			<td>Unsupported media format. Check the configuration.</td>
		</tr>
		<tr>
			<td><code>10502</code></td>
			<td><code>412</code></td>
			<td>Missing character encoding. Declare the character encoding. <strong>Note:</strong> The API expects data to be <code>UTF-8</code> encoded.</td>
		</tr>
	</tbody>
</table>
</div>
<!--end wab-acc-oauth2 --></div>
<!-- end acc-accordion acc-light-grey --></div>
<!-- end acc-container -->

<p><a class="no-print" href="#wab-pkit-oauth-feed-errors" target="_parent">Back to Top</a></p>
</div>
<!-- end wab-api-oauth-feed-errors -->

<hr class="hrsmall no-bottom-margin no-print" /><!-- link to main guide #wab-pkit-access-stream-link -->
<div class="no-bottom-margin" id="wab-pkit-access-stream-link">
<h3 id="wab-pkit-parameters-to-launch-stream"><a id="step-four" name="step-four"></a>➃ Supply the access token and livestream UUID in your embed code to launch the stream in a player</h3>

<p>Now that you have the basic <a href="#wab-pkit-embed" target="_blank">Player Kit embed</a>, <a href="#wab-pkit-call-mcc-feed" target="_blank">livestream UUID</a> and <a href="#wab-pkit-call-oauth-feed" target="_blank">OAuth access token</a> (steps<a href="#wab-pkit-embed" target="_blank"><strong> 1</strong></a>,<a href="#wab-pkit-call-mcc-feed" target="_blank"><strong> 2</strong></a>,<a href="#wab-pkit-call-oauth-feed" target="_blank"><strong> 3</strong></a>), you must instantiate the player and livestream. You must supply these values and the user ID, container ID, your Outlet Authorisation Key, and any optional parameters to the Player Kit embed. You should also see the <a href="#wab-pkit-appendix" target="_blank">Appendix</a> for <a href="#wab-pkit-appendix-cors" target="_blank">CORS</a> and <a href="#wab-pkit-appendix-video-size" target="_blank">video size</a> information.</p>

<p>You will also need to complete any extra work - this depends on how you have chosen to implement the Player Kit library (as discussed in step 1):</p>

<ul>
	<li><strong><span class="light-grey">(option 1)</span> Use Player Kit with built-in player controls/overlay:</strong> Specify key-value pair <code>controls: &#39;true&#39;</code> and other parameters and values.</li>
	<li><strong><span class="light-grey">(option 2)</span> Build/use your own JavaScript Player UI over Player Kit + additional API calls:</strong> Keep our Player Kit&#39;s built-in controls disabled (specify <code>controls: &#39;false&#39;</code> or omit the &#39;controls&#39; parameter). You must instantiate the Player Kit within your own player and use additional API calls for binding (see <a href="#wab-pkit-appendix-using-the-api" target="_blank">Appendix: Using the API</a>).</li>
	<li><strong><span class="light-grey">(option 3)</span> Embed in a mobile app with WebView - load the Player Kit code in an external HTML file in an iframe (native apps only):</strong> Follow the instructions for your chosen approach (option 1 - use our Player Kit&#39;s built-in controls, or option 2 - use your own Player UI) and configure WebView (see <a href="#wab-pkit-appendix-using-webview" target="_blank">Appendix: Using WebView</a>).</li>
</ul>

<div class="pf-box-note"><strong>Player Kit:</strong> You cannot style the built-in controls. If you load the latest release, any changes are reflected in your page automatically (or you can use a semantic version for some control).</div>

<h4>Player Kit - Constructor</h4>

<p>The Player Kit, when instantiated, takes a single key-value pair object, which configures the player. <em>Required parameters</em> are needed to request and retrieve the stream; <em>Optional parameters</em> allow you to implement playback settings and corresponding player controls. After you instantiate the Player Kit, you will have access to three namespaces. From there you can either invoke their methods or listen to their events with <code>on()</code> method. For a full user guide, see <a href="#wab-pkit-appendix-using-the-api" target="_blank">Appendix: Using the API</a></p>

<ul>
	<li><code>wab</code> - the Player Kit itself (as below - start here)</li>
	<li><code>wab.player</code> - the Player controller (everything related to actual video playback)</li>
	<li><code>wab.event</code> - the Event controller (everything related to the live sports event/fixture)</li>
</ul>
<!-- It will register itself in the JavaScript variable <code>WAB</code> on the document window and allow you to embed the livestream... allow you to embed the livestream using JavaScript -->

<p><strong>Example:</strong> This sample contains example values from step 3, such as the livestream asset ID and access token. You must supply your own values to retrieve the livestream.</p>

<pre class="prettyprint">
  &amp;lt;script&amp;gt;
    const wab = new WAB({
      outlet: &#39;{outletAuthKey}&#39;,   // <em>Required</em>. Specify your unique {outletAuthKey}
      stream: &#39;{livestreamUuid}&#39;,   // <em>Required</em>. Livestream asset ID of the mccItem. Must match &#39;asset_id&#39; value in /oauth request (eg &#39;1kc73cm2uhwjw12fp59ark7lo9&#39;)
      user: &#39;{userId}&#39;,    // <em>Required</em>. Unique end-user identifier. Must match userId value from the /oauth request
      key: &#39;{accessToken}&#39;,   // <em>Required</em>. Unique access token value from the /oauth request response (eg &#39;ABc1defGH23iJKLMNO4PQr56st7UV8w&#39;)
      container: &#39;{player-container}&#39;,   // <em>Required</em>. Must match &amp;lt;div id=&quot;&quot;&amp;gt; value in your embed
      controls: &amp;lt;true|false&amp;gt;,   // <em>Optional</em>. Set player controls on/off (use built-in/use own)
      audioLang: &amp;lt;string&amp;gt;   //<em>Optional</em>. Implement livestream audio language selection (for example, &#39;en-gb&#39; = English - UK)
      });
  &amp;lt;/script&amp;gt;
</pre>

<p>Our WebSocket servers will check that the request is valid. Playback of the livestream will then commence, providing that the time-limited access token is still valid and the user does not start an additional playback request on a device. The livestream session ends if the user starts a new session as it will trigger the user eviction mechanism.</p>
<!-- 

<pre class="prettyprint">
  &amp;lt;script&amp;gt;
    var wab = new WAB({
      outlet: '{outletAuthKey}',   // <em>Required</em>. Specify your unique {outletAuthKey}
      stream: '{livestreamUuid}',   // <em>Required</em>. Livestream asset ID of the mccItem. Must match 'asset_id' value in /oauth request (eg '1kc73cm2uhwjw12fp59ark7lo9')
      user: '{userId}',    // <em>Required</em>. Unique end-user identifier. Must match userId value from the /oauth request
      key: '{accessToken}',   // <em>Required</em>. Unique access token value from the /oauth request response (eg 'ABc1defGH23iJKLMNO4PQr56st7UV8w')
      container: '{player-container}',   // <em>Required</em>. Must match &amp;lt;div id=""&amp;gt; value in your embed
      controls: &amp;lt;true|false&amp;gt;,   // <em>Optional</em>. Set player controls on/off (use built-in/use own)
      audioLang: &amp;lt;string&amp;gt;   //<em>Optional</em>. Implement livestream audio language selection (for example, 'en-gb' = English - UK)
      });
  &amp;lt;/script&amp;gt;
</pre>
  <div class="pf-box-caution"><strong>Avoiding and understanding errors:</strong>
    <br />&nbsp;&bull; Do not include example values or <code>// comments</code> in your code; otherwise, the Player Kit will not work. You must pass parameter values that are correct for the unique livestream session.
    <br />&nbsp;&bull; The Player Kit will return an error when something is not working correctly, which will be displayed in the browser console - you can test this in Google Chrome using its Developer Tools.
    <br />&nbsp;&bull; If you receive an error, see the following section: <strong>Player Kit: Error messages and explanations</strong>
  </div>
-->

<div class="pf-box-warning"><strong>Important:</strong><br />
&nbsp;&bull; Do not expose your Outlet Authorisation Key where possible, for example in the user interface of your front-end apps/websites; this will allow you to maintain a certain level of secrecy. While it is possible for a user to view the key if they open the browser console (as the key is included in the script tag), we have security features in place to prevent unauthorised use of the key being successful.<br />
&nbsp;&bull; Do not include <code>{example}</code> values or <code>// comments</code> in your code; otherwise, the Player Kit will not work. The parameter-key values must be correct for the unique livestream session.<br />
&nbsp;&bull; In most browsers the <code>script</code> element defaults to <code>text/javascript</code> so it is safe to omit this line (<code>type</code> is optional in HTML5).<br />
&nbsp;&bull; The Player Kit will return an error when something is not working correctly, which will be displayed in the browser console - you can test this in Google Chrome using its Developer Tools.<br />
&nbsp;&bull; If you receive an error, see the following section: <strong>Player Kit: Error messages and explanations</strong></div>

<h4>Player Kit - Parameters and values</h4>

<table class="tableqpvalueexample" style="width:100%">
	<thead>
		<tr>
			<th style="width:10%">Parameter</th>
			<th style="width:17%">Value</th>
			<th style="width:73%">Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code class="table">outlet</code></td>
			<td><code class="table"><span class="code-green">{outletAuthKey}</span></code></td>
			<td><em>Required</em>. <strong>Outlet Authentication Key</strong>. The alphanumeric UUID that is unique to each outlet - we supply this to you upon activation. Our platform requires this key to authenticate an outlet. Requests made without a valid authentication key will be rejected. If you have more than one outlet, use the key that belongs to the outlet making the request; otherwise, an authentication error occurs and the requested data is not retrieved.</td>
		</tr>
		<tr>
			<td><code class="table">stream</code></td>
			<td><code class="table"><span class="code-green">{livestreamUuid}</span></code></td>
			<td><em>Required</em>. <strong>livestream asset ID</strong> - for your chosen fixture. Returned from a successful request to the <code>/mcc</code> (Content Catalogue) feed with its parent element <code>mccItem</code> (a fixture) in the response. It is the <code>id</code> of an <code>asset</code> sub-element with attribute <code>type=&quot;urn:perform:livestream&quot;</code> (<a href="#wab-pkit-call-mcc-feed" target="_parent">step 2</a>).<br />
			<strong>Example:</strong> Below, the livestream <code>asset id</code> value is <code>1kc73cm2uhwjw12fp59ark7lo9</code> (for <code>mccItem</code> and <code>fixture id</code> 54csvtpw8hip1eiv65bp4uuy).
			<pre class="small prettyprint">
&amp;lt;<strong>mccItem</strong> id=&quot;54csvtpw8hip1eiv65bp4uuy&quot; name=&quot;Nice v Reims&quot; type=&quot;urn:perform:mfl:fixture&quot;&amp;gt;
  &amp;lt;<strong>fixture</strong> id=&quot;54csvtpw8hip1eiv65bp4uuy&quot; date=&quot;2016-04-22&quot; time=&quot;19:00:00&quot;&amp;gt;
  &nbsp; &amp;lt; object metadata truncated &amp;gt; &nbsp; 
  &amp;lt;/fixture&amp;gt;
  &amp;lt;assets&amp;gt;
    &amp;lt;asset <span class="code-green"><strong>id=&quot;1kc73cm2uhwjw12fp59ark7lo9&quot;</strong></span> name=&quot;Nice v Reims&quot; <span class="code-green"><strong>type=&quot;urn:perform:livestream&quot;</strong></span> urn=&quot;urn:perform:livestream:1kc73cm2uhwjw12fp59ark7lo9&quot; lastUpdateTime=&quot;2016-04-11T13:25:06Z&quot; allowedCountryCodes=&quot;AE AF AG AI AL AM&quot;/&amp;gt;
    &amp;lt;asset id=&quot;ej9cnyc2lmczfeshz1np86ynd&quot; type=&quot;urn:perform:fastdata&quot; urn=&quot;urn:perform:fastdata:ej9cnyc2lmczfeshz1np86ynd&quot; lastUpdateTime=&quot;2015-04-13T12:30:27Z&quot;/&amp;gt;
  &amp;lt;/assets&amp;gt;
&amp;lt;/mccItem&amp;gt;
</pre>
			</td>
		</tr>
		<tr>
			<td><code class="table">user</code></td>
			<td><code class="table-green">{userId}</code></td>
			<td><em>Required</em>. <strong>User ID</strong>. The <code>&quot;uuid&quot;</code> value returned in the response from a successful OAuth request (<a href="#wab-pkit-call-oauth-feed" target="_parent">step 3</a>). It is valid only for the unique request (username, hashed password, and livestream asset ID).</td>
		</tr>
		<tr>
			<td><code class="table">key</code></td>
			<td><code class="table-green">{accessToken}</code></td>
			<td><em>Required</em>. <strong>Access token</strong>. The <code>&quot;access_token&quot;</code> value returned in the response from a successful OAuth request. It is valid for only one use, within 10 seconds, for the unique request (username, hashed password, and livestream asset ID). <strong>Example:</strong> Below is an example response (see <a href="#wab-pkit-call-oauth-feed" target="_parent">step 3</a>).
			<pre class="small prettyprint">
{
  &quot;access_token&quot;:&quot;ABc1defGH23iJKLMNO4PQr56st7UV8w&quot;, 
  &quot;token_type&quot;:&quot;Bearer&quot;,
  &quot;uuid&quot;:&quot;{userId}&quot;,
  &quot;expires_in&quot;:10
}
</pre>
			</td>
		</tr>
		<tr>
			<td><code class="table">container</code></td>
			<td><code class="table-green">{player-container}</code></td>
			<td><em>Required</em>. <strong>Container ID</strong>. The <code>id</code> value of the <code>&amp;lt;div&amp;gt;</code> container (<code>id=&quot;{player-container}&quot;</code>).</td>
		</tr>
		<tr>
			<td><code class="table">controls</code></td>
			<td><code class="table-magenta">&amp;lt;true|false&amp;gt;</code></td>
			<td><em>Optional</em>. <strong>Basic player controls UI</strong>. Use basic controls or your own player interface. <em>Boolean:</em> <code>true</code> (with controls) or <code>false</code> (no controls - default)</td>
		</tr>
		<tr>
			<td><code class="table">audioLang</code></td>
			<td><code class="table-magenta">{string}</code></td>
			<td><em>Optional</em>. <strong>Audio language</strong>. Implement audio language selection (if you will supply/offer a choice of tracks). <em>String</em> of values. Example: <code>x-it</code></td>
		</tr>
		<tr>
			<td><code class="table">autoplay</code></td>
			<td><code class="table-magenta">&amp;lt;true|false&amp;gt;</code></td>
			<td><em>Optional</em>. <strong>Autoplay</strong>. Controls livestream autoplay - see <a href="https://docs.performgroup.io/wab-playerkit/docs/autoplay.notes.md" target="_blank">guide</a>. <em>Boolean</em>: <code>true</code> (autoplay enabled) or <code>false</code> (autoplay disabled - default)</td>
		</tr>
		<tr>
			<td><code class="table">autoplayHybrid</code></td>
			<td><code class="table-magenta">&amp;lt;true|false&amp;gt;</code></td>
			<td><em>Optional</em>. <strong>Autoplay Hybrid</strong>. Attempts livestream autoplay with sound on a desktop - see <a href="https://docs.performgroup.io/wab-playerkit/docs/extra-behaviours/autoplay.hybrid.md" target="_blank">guide</a>. <em>Boolean</em>: <code>true</code> (enabled) or <code>false</code> (disabled - default)</td>
		</tr>
		<tr>
			<td><code class="table">muted</code></td>
			<td><code class="table-magenta">&amp;lt;true|false&amp;gt;</code></td>
			<td><em>Optional</em>. <strong>Mute (audio off)</strong>. Audio setting upon playback. <em>Boolean</em>: <code>true</code> (no audio, mute enabled) or <code>false</code> (allow audio, mute disabled - default) <!-- Set livestream audio when playback starts --></td>
		</tr>
		<tr>
			<td><code class="table">maxWidth</code></td>
			<td><code class="table-magenta">{number}</code></td>
			<td><em>Optional</em>. <strong>Max width</strong>. Constrains the maximum width of the player.</td>
		</tr>
		<tr>
			<td><code class="table">maxHeight</code></td>
			<td><code class="table-magenta">{number}</code></td>
			<td><em>Optional</em>. <strong>Max height</strong>. Constrains the maximum height of the player.</td>
		</tr>
		<tr>
			<td><code class="table">poster</code></td>
			<td><code class="table-magenta">{urlString}</code></td>
			<td><em>Optional</em>. <strong>Poster</strong>. Choose an image/thumbnail (its URL) to be displayed immediately before playback starts.</td>
		</tr>
	</tbody>
</table>

<div class="pf-box-note"><strong>Enabling Autoplay:</strong> You should set <code>autoplay: true</code> in the config but sometimes this is not enough - most browsers also require you to mute the video (set using <code>muted: true</code>). However, Google Chrome allows video to autoplay with sound under certain circumstances - to utilise this, use and set <code>autoplayHybrid: true</code>, <code>autoplay: true</code>, and <code>muted: false</code> (equivalent to audio = true).</div>

<h4 id="wab-pkit-wrapper-errors">Player Kit: Error messages and explanations</h4>

<div class="acc-container">
<div class="acc-accordion acc-light-grey"><button class="acc-btn-block acc-left-align"><strong>&nbsp; SHOW/HIDE</strong></button>

<div class="acc-accordion-content acc-container normal" id="wab-acc-pkit-errors">
<p>The Player Kit script will return an error when something is not working correctly, which will be displayed in the browser console - you can test this in Google Chrome and its Developer Tools. The errors are detailed in table below.</p>

<table class="tableqpvalueexample">
	<thead>
		<tr>
			<th>Error code</th>
			<th>Explanation</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td colspan="2"><strong>wab errors</strong></td>
		</tr>
		<tr>
			<td><code>001</code></td>
			<td>Failed to load a component of the player. Check the Player Kit reference and try a different version if the error continues; otherwise, contact Perform for support.</td>
		</tr>
		<tr>
			<td colspan="2"><strong>player errors</strong></td>
		</tr>
		<tr>
			<td><code>101</code></td>
			<td>Specified container <code>id &quot;&quot;</code> does not exist. Check the container ID is correct, and then try again.</td>
		</tr>
		<tr>
			<td colspan="2"><strong>wab.event errors</strong></td>
		</tr>
		<tr>
			<td><code>201</code></td>
			<td>XML parse error, with error message.</td>
		</tr>
		<tr>
			<td><code>202</code></td>
			<td>Metadata parse error, error message.</td>
		</tr>
		<tr>
			<td><code>203</code></td>
			<td>Feed data parse error, feed URL: <code>{this.feedUrl}</code></td>
		</tr>
		<tr>
			<td><code>204</code></td>
			<td>Stream load error, feed URL: <code>{this.feedUrl}</code> (and details).</td>
		</tr>
		<tr>
			<td><code>205</code></td>
			<td>No purchase order found.</td>
		</tr>
		<tr>
			<td><code>206</code></td>
			<td>Event over.</td>
		</tr>
		<tr>
			<td><code>207</code></td>
			<td>Event has not started.</td>
		</tr>
		<tr>
			<td><code>208</code></td>
			<td>Geo-blocked. Access to the stream is not permitted from the location of the user/the user&#39;s device.</td>
		</tr>
		<tr>
			<td><code>209</code></td>
			<td>User is not logged-in.</td>
		</tr>
		<tr>
			<td><code>210</code></td>
			<td>Invalid partner id.</td>
		</tr>
		<tr>
			<td><code>211</code></td>
			<td>Invalid event id.</td>
		</tr>
		<tr>
			<td><code>212</code></td>
			<td>No encoder.</td>
		</tr>
		<tr>
			<td><code>213</code></td>
			<td>Security error.</td>
		</tr>
		<tr>
			<td><code>214</code></td>
			<td>Out of range.</td>
		</tr>
		<tr>
			<td><code>215</code></td>
			<td>Georestricted purchase.</td>
		</tr>
		<tr>
			<td><code>216</code></td>
			<td>Fair use breach.</td>
		</tr>
		<tr>
			<td><code>217</code></td>
			<td>DVR event expired.</td>
		</tr>
		<tr>
			<td><code>218</code></td>
			<td>Wrong configuration.</td>
		</tr>
		<tr>
			<td><code>219</code></td>
			<td>Invalid key.</td>
		</tr>
		<tr>
			<td colspan="2"><strong>wab.advert errors</strong></td>
		</tr>
		<tr>
			<td><code>301</code></td>
			<td>DFP advert error.</td>
		</tr>
		<tr>
			<td colspan="2"><strong>Other errors</strong></td>
		</tr>
		<tr>
			<td><code>401</code></td>
			<td>General error.</td>
		</tr>
		<tr>
			<td><code>402</code></td>
			<td>Fullscreen error. The Player does not allow streaming in fullscreen mode for normal use-cases.</td>
		</tr>
		<tr>
			<td><code>403</code></td>
			<td>Unsupported format error.</td>
		</tr>
	</tbody>
</table>
</div>
</div>
</div>
<!-- end error messages --></div>
<!-- end step 4 wab-pkit-parameters-to-launch-stream -->

<p class="no-print"><a class="no-print" href="#top" target="_parent">Back to top</a></p>

<hr class="hrline" />
<h2 id="wab-pkit-appendix"><a id="appendix" name="appendix"></a>Appendix: CORS, Video size, Player Kit API, WebView</h2>
<!-- &#x2784; was: Appendix: CORS configuration, Setting the video size, Using the Player API, Using WebView or Appendix: Configuring CORS, the player controls and API, and video size -->

<ul>
	<li><a href="#wab-pkit-appendix-cors" target="_blank">CORS configuration and HTTP access control</a></li>
	<li><a href="#wab-pkit-appendix-video-size" target="_blank">Setting the video size</a></li>
	<li><a href="#wab-pkit-appendix-using-the-api" target="_blank">Using the Player API</a></li>
	<li><a href="#wab-pkit-appendix-using-webview" target="_blank">Using WebView</a></li>
</ul>

<h3 id="wab-pkit-appendix-cors"><span class="light-grey">Appendix:</span> CORS configuration and HTTP access control</h3>

<p>In order to make successful requests to our platform,&nbsp;they&nbsp;must&nbsp;be made over <code>https</code>, from&nbsp;a&nbsp;recognised&nbsp;IP address/range and domain, and CORS must be enabled.</p>

<ul>
	<li>Player Kit requests&nbsp;-&nbsp;originate&nbsp;from&nbsp;the client (browser/app/frontend)</li>
	<li>Requests to /mcc and /oauth feeds - originate from your backend servers</li>
</ul>

<p>When&nbsp;using&nbsp;HTTP&nbsp;access&nbsp;control&nbsp;(CORS)&nbsp;your&nbsp;header&nbsp;must&nbsp;include&nbsp;the&nbsp;domain you&nbsp;supplied&nbsp;to&nbsp;us - we add this domain to our whitelist. This ensures that your server can request restricted resources from our servers, such as JavaScript, CSS, iframes, plugins, embedded fonts and videos. Your HTTP access control settings must allow Origin, Credentials, GET, POST, and file types related to the Player Kit wab.kit.js, such as .css .jpg .png .mp4 .m3u8 .m3u .ts .xml</p>

<p>In the case of the OAuth feed (step 2), the <code>POST</code> method supports a cross-domain (CORS) request via HTTP(S) as an HTML form or XHR (<code>XMLHttpRequest</code>)</p>

<p><strong>Background technical information</strong><br />
CORS (Cross-Origin Resource Sharing) defines a set of HTTP headers that allow the browser and server to communicate about which requests are and are not allowed. This mechanism allows JavaScript on a web page to make request to a different domain or the domain that the JavaScript originated from. Such &quot;cross-domain&quot; requests would otherwise be forbidden by web browsers.</p>

<p>For example, we can identify an HTTP Header (called Origin) and determine if the site that created the request (Origin header) is allowed to make requests. Then, we can return the response with another Header (Access-Control-Allow-Origin) which tells the site if it can or cannot access our services (in the browser). It is more useful than only allowing &quot;same-origin&quot; requests, but it is more secure than simply allowing <strong>all</strong> such &quot;cross-origin&quot; requests.</p>

<p><a class="no-print" href="#wab-pkit-appendix" target="_parent">Back to Top - Appendix</a></p>

<hr class="hrsmall" />
<h3 id="wab-pkit-appendix-setting-the-video-size"><span class="light-grey">Appendix:</span> Setting the video size</h3>

<p>The livestream video uses a 16:9 aspect ratio - this means that if you set a width of 600px, then you should set a height of 338px. If you want to set a width for your video container you must set a height which conforms to the 16:9 aspect ratio. To calculate the height, use the formula <code>height = width&divide;1.777777777777778</code> (because <code>16&divide;9 = 1.777777777777778</code>).</p>

<p>In our example, if we set the width to 500 pixels (<code>500px</code>) then in order to maintain the 16:9 aspect ratio, the height should be 281 pixels (<code>281px</code>).</p>

<h5>Intrinsic ratio</h5>

<p>For responsive players, there is also a solution to maintain aspect ratio which uses intrinsic ratio, a CSS technique, to fluidly constrain a child element to a ratio set in their parent element. To do this, we add a <code>&amp;lt;div&amp;gt;</code> container around the video player and set the <code>padding-top</code> property with the desired aspect ratio (a percentage) for the video. To calculate this percentage for a video with a 16:9 aspect ratio, you must divide 9 by 16 (ie 9/16 = .5625) to get 56.25%. For a 16:9 video, the height should be 9/16ths of the width.</p>

<p>The <code>padding</code> property styles a box with an intrinsic ratio, setting it as a percentage of the width of the containing block - and padding styles are supported in all common web browsers.</p>

<p>Example HTML:</p>

<pre class="prettyprint lang-html">
  &amp;lt;div class=&quot;player-wrapper&quot;&amp;gt;
    &amp;lt;div id=&quot;{player-container}&quot;&amp;gt;
      // your code goes here
    &amp;lt;/div&amp;gt;
  &amp;lt;/div&amp;gt;
</pre>

<p>Example CSS: You can experiment with this if you find the fluid display of the embedded video isn&#39;t optimal for common viewports.<br />
For example, with the properties for <code>.player-wrapper</code> (which contains the player DIV, (<code>{player-container}</code>), you could try switching the values for <code>padding-top</code> and <code>padding-bottom</code>, setting a <code>max-width</code> and <code>max-height:auto</code>, changing the height to <code>height:auto</code>, and so on.<br />
For the properties of the player DIV (<code>{player-container}</code>), you could try changing <code>position:absolute</code> to <code>position:relative</code>.</p>

<pre class="prettyprint lang-html">
.player-wrapper {
    padding-top: 
	position:relative;
	padding-top:56.25%;
	padding-bottom:30px;
	height:0;
	overflow:hidden;
}

#player-container {
	position:absolute;
	top:0;
	left:0;
	width:100%;
	height:100%;
}
</pre>

<p><a class="no-print" href="#wab-pkit-appendix" target="_parent">Back to Top - Appendix</a></p>

<hr class="hrsmall" />
<h3 id="wab-pkit-appendix-using-the-api"><span class="light-grey">Appendix:</span> Player Kit API: Using the API and controlling the player</h3>
<!-- <p>Find out about the Endpoints, Events, Promises, and Methods, and see usage examples.</p> -->

<div class="pf-box-info"><strong>Getting the latest updates and information:</strong><br />
While we provide a guide below, you will always find the <strong>latest</strong> Player Kit API documentation and usage examples in our <a href="https://docs.performgroup.io/wab-playerkit" target="_blank">release repository</a>. You can also track releases there - this is also useful if you wish to specify a semantic version or use the latest release (not specify a version) and want to differentiate between them. See <a href="https://docs.performgroup.io/wab-playerkit" target="_blank">Player Kit on GitHub</a> or contact your Account Manager for more information. Remember: You can also inspect the code at any time by copying and pasting the URL into your browser.</div>

<h4>Supported browsers:</h4>

<ul>
	<li><strong>Desktop:</strong> Internet Explorer 9+, Microsoft Edge, Mozilla Firefox (last 5 versions), Google Chrome (last 5 versions), Safari 7.1+</li>
	<li><strong>Mobile:</strong> Android 4.4.3+, iOS 8+</li>
</ul>

<h4>Namespaces and endpoints</h4>

<p>After you instantiate the Player Kit, you will have access to three namespaces. From there you can either invoke their methods or listen to their events with the <code>on()</code> method.</p>
<!-- The Player Kit has separate endpoints for a sports <code>event</code> (fixture/match) and specific events and methods to control the <code>player</code>: -->

<table class="tableqpvalueexample">
	<tbody>
		<tr>
			<td><code>wab</code></td>
			<td>The Player Kit itself (as above - start here).</td>
			<td><a href="#wab-pkit-using-the-api-wab-guide" target="_blank">Guide</a></td>
		</tr>
		<tr>
			<td><code>wab.player</code></td>
			<td>Player controller (everything related to actual video playback)<br />
			Use this property to access the documented methods and events to control the player and video playback.</td>
			<td><a href="#wab-pkit-using-the-api-wabplayer-guide" target="_blank">Guide</a></td>
		</tr>
		<tr>
			<td><code>wab.event</code></td>
			<td>Event controller (everything related to the live sports event/fixture)<br />
			Use this property to access event/fixture information. Details about an ongoing live or future event is loaded asynchronously. It provides an interface for handling events.</td>
			<td><a href="#wab-pkit-using-the-api-wabevent-guide" target="_blank">Guide</a></td>
		</tr>
	</tbody>
</table>

<div class="pf-box-info"><strong>Tip:</strong> You will always find the latest documented parameters and values, events, and methods in our <a href="https://docs.performgroup.io/wab-playerkit/docs/" target="_blank">Player Kit GitHub release repository</a>.</div>

<h5>Events vs Promises</h5>

<p class="margin-small">We support both callbacks and promises. Callbacks are commonly used; Promises are useful for multiple async calls. A Promise object represents a value that may not be available yet, but will be resolved at some point in the future. You should decide on the approach that best suits your environment.<br />
You can use one of the following:</p>

<pre class="prettyprint lang-javascript small">
wab.event.on(&#39;meta&#39;, meta =&amp;gt; console.log(meta))</pre>

<pre class="prettyprint lang-javascript small">
wab.event.on(&#39;meta&#39;).then(meta =&amp;gt; console.log(meta))</pre>

<p class="margin-small">The <code>.then()</code> function returns a <strong>promise</strong>. Promises will be fired only once.</p>

<div class="pf-box-info"><strong>Tip:</strong> Promises make it easier to call more than one function. Functions can be added anywhere in the code, subject only to the Promise being within scope. Functions added after a deferred/promise has been resolved/rejected will fire immediately. For more information, see MDN&#39;s guide: <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises" target="_blank">Using promises</a></div>

<h5>Chaining/Transformation</h5>

<p class="margin-small">Chaining allows you to run multiple methods on the same element within a single statement. This is called a Promise chain, where each subsequent operation starts when the previous operation succeeds, with the result from the previous step.<br />
You can add the <code>.then</code> function after an operation to return a new promise that is different to the original. You can attach callbacks to returned promises to form a chain, and use arrow functions. For more information, see MDN&#39;s guide: <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises" target="_parent">Promises and Chaining</a></p>

<hr class="hrsmall" />
<h4 id="wab-pkit-using-the-api-wab-guide"><span class="light-grey">Namespaces and endpoints |</span> wab</h4>

<h5>Constructor</h5>

<p>The constructor takes a single key-value pair object, which configures the player; <code>options</code> will take the properties (required/optional parameters) detailed in <a href="#wab-pkit-parameters-to-launch-stream" target="_blank">step 4</a> and section <strong>Player Kit - Parameters and values</strong>.</p>

<pre class="prettyprint">
const wab = new WAB({options})</pre>

<div class="pf-box-note">&nbsp;</div>

<h5>Instance methods <span class="light-grey">| wab</span></h5>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:25%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:25%;vertical-align: text-top"><code>wab.on(eventName[, callback])</code></td>
			<td style="vertical-align: text-top">
			<p><code>eventName: <em>string</em></code><br />
			<code>callback: <em>function</em></code><br />
			<code>WAB|Promise:</code> <em>reference to Player Kit for chaining OR a Promise</em></p>

			<p>The <code>.on</code> method subscribes a callback handler to an event. The <em>callback</em> argument is optional; if omitted, it will return a <em>Promise</em> object. If a callback is specified, then it will return itself allowing chainability.</p>
			Example:

			<pre class="prettyprint small">
// listening to an event through regular callback
wab.on(&#39;error&#39;, errorInfo =&amp;gt; {
	console.log(errorInfo)
})
</pre>

			<pre class="prettyprint small">
// listening to an event with a promise
wab.on(&#39;error&#39;).then(errorInfo =&amp;gt; {
	console.log(errorInfo)
})
</pre>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.</span><strong>off</strong>(eventName[, callback])</code></td>
			<td style="vertical-align: text-top">
			<p><code>eventName: <em>string</em></code><br />
			<code>callback: <em>function</em></code><br />
			<code>WAB|Promise:</code> <em>reference to Player Kit for chaining</em></p>

			<p>The <code>.off</code> method removes a callback handler from the listeners. This will also return itself for chainability.</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.</span><strong>destroy</strong>()</code></td>
			<td>Every instance of <code>WAB</code> implements a <code>destroy()</code> method that allows you to destroy the player and clean up event listeners.</td>
		</tr>
	</tbody>
</table>

<h5>Events <span class="light-grey">| wab</span></h5>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><span class="light-grey"><strong>error</strong></span></td>
			<td><code>errorInfo: object</code> with error details. Triggers with errors from all Player Kit modules, such as events and playback. The <code>data</code> object contains two properties:
			<ul>
				<li><code>code: <em>string</em></code></li>
				<li><code>msg: <em>string</em></code></li>
			</ul>
			Errors emitted by a <code>wab</code> instance can be caught with a simple listener:

			<pre class="prettyprint small">
const wab = new WAB(options)
wab.on(&#39;error&#39;, data =&amp;gt; {
	// your error handler here
})
</pre>
			See the list of possible errors is described in the Player Kit <strong>Error codes and explanations</strong> guide or its <a href="https://docs.performgroup.io/wab-playerkit/docs/error.codes.md" target="_blank">GitHub repository</a>.</td>
		</tr>
	</tbody>
</table>
<!-- 
Information about the ongoing or future event is loaded asynchronously and can be accessed through events provided by the <code>wab.event</code> property. It provides an interface for handling events. 

The player offers various events and methods to allow controls over the underlying video playback. The publicly available methods and events can be accessed through the <code>wab.player</code> property.
<ul>
	<li><code>wab.event</code></li>
	<li><code>wab.player</code></li>
</ul>
<p>The Player Kit has separate endpoints for a fixture (sports event) and specific events and methods to control the player.</p> 
<p>Information about the ongoing or future event is loaded asynchronously and can be accessed through events provided by the <code>wab.event</code> property. It provides a jQuery-like interface for handling events.</p>
-->

<hr class="hrsmall" />
<h4 id="wab-pkit-using-the-api-wabplayer-guide"><span class="light-grey">Namespaces and endpoints |</span> wab.player</h4>
<!--  <br /><p class="lead-sm" id="wab-pkit-using-the-api-wabplayer-guide"><span class="light-grey">Namespaces and endpoints |</span> <strong>wab.player</strong></p> -->

<h5>Controlling the player</h5>

<p>The player offers various events and methods to allow controls over the underlying video playback. The available methods and events can be accessed through the <code>wab.player</code> property.</p>

<pre class="prettyprint lang-javascript small">
const wab = new WAB(options)
wab.player.on(&#39;playing&#39;, () =&amp;gt; {
	console.log(&#39;stream started playing&#39;)
})
</pre>

<h5>Instance methods <span class="light-grey">| wab.player</span></h5>
<!-- <h5 class="margin-small"><strong><span class="light-grey">wab.player</span>.on</strong></h5> -->

<ul>
	<li><strong>getters:</strong> duration, playing</li>
	<li><strong>getters and setters:</strong> currentTime, muted, volume, fullscreen</li>
	<li><strong>methods:</strong> play, pause, resume (Note: destroy is for <code>wab</code>)</li>
</ul>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>on</strong>(eventName[, callback])</code></td>
			<td style="vertical-align: text-top">
			<p><code>eventName: <em>string</em></code><br />
			<code>callback: <em>function</em></code><br />
			<code>PlayerController|Promise:</code> <em>reference to</em> <code>player</code> <em>for chaining OR a Promise</em><br />
			The <code>.on</code> method subscribes a callback handler to an event. The <em>callback</em> argument is optional; if omitted, it will return a <em>Promise</em> object. If a callback is specified, then it will return itself allowing chainability.</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>off</strong>(eventName[, callback])</code></td>
			<td style="vertical-align: text-top">
			<p><code>eventName: <em>string</em></code><br />
			<code>callback: <em>function</em></code><br />
			<code>PlayerController|Promise:</code> <em>reference to</em> <code>player</code> <em>for chaining</em><br />
			The <code>.off</code> method removes a callback handler from the listeners. This will also return itself for chainability.</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>duration</strong>()</code> <strong>(get)</strong></td>
			<td>
			<p><em><strong>returns</strong></em> <code><em>number</em>|null</code> - length <code>(<em>number in seconds</em></code>) of content played or <code>null</code> if called before player is ready</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>playing</strong>()</code> <strong>(get)</strong></td>
			<td>
			<p><em><strong>returns</strong></em> boolean: <code>true|false|null</code> - <code>true</code> if the video is playing | <code>false</code> if the video is not playing | <code>null</code> if called before player is ready</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>currentTime</strong>([newTime])</code> <strong>(get/set)</strong></td>
			<td style="vertical-align: text-top">
			<p><em><strong>returns</strong></em> <code><em>number</em>|null</code> - <code>(<em>number in seconds</em></code>) of current position of video, or <code>null</code> if called before player is ready<br />
			<code>newTime: <em>number</em></code> - optional, <strong>set/change</strong> the current time of the video. Values: <code><em>number</em></code> value in seconds, between <code>0.0</code> and whatever the total duration value is. Example: A video duration of 3 mins gives value range <code>0.0-180.0</code> secs</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>muted</strong>([newMuted])</code> <strong>(get/set)</strong></td>
			<td style="vertical-align: text-top">
			<p><em><strong>returns</strong></em> <code><em>boolean</em>|null</code> - <code>(<em>boolean: true|false|null</em></code>) as current state of muted property in the video element, or <code>null</code> if called before player is ready<br />
			<code>newMuted: <em>boolean</em></code> - <em>optional</em>, <strong>set/change</strong> the muted property. Values: <code>true</code> muted | <code>false</code> unmuted</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>volume</strong>([newVolume])</code> <strong>(get/set)</strong></td>
			<td style="vertical-align: text-top">
			<p><em><strong>returns</strong></em> <code><em>number</em>|null</code> - <code>(<em>number</em></code>) current volume in the range <code>0&amp;lt;-&amp;gt;1</code>, or <code>null</code> if called before player is ready</p>

			<p><code>newVolume: <em>number</em></code> - <em>optional</em>, set the new playback volume of the video/audio. Value: number in <code>0&amp;lt;-&amp;gt;1</code> - between 0.0 (silent/mute) and 1.0 (default - highest volume = 100%). Example: 0.2 = 20%</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>fullscreen</strong>([inOut])</code> <strong>(get/set)</strong></td>
			<td style="vertical-align: text-top">
			<p><em><strong>returns</strong></em> <code>null</code> if called before player is ready<br />
			<code>inOut: <em>boolean</em></code> - <em>optional</em>. Values: <code>true</code> enters fullscreen | <code>false</code> exits fullscreen, and an undefined value will change the current state to the other (eg toggle between the two states)</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>play</strong>()</code> <strong>(set)</strong></td>
			<td style="vertical-align: text-top">
			<p>Starts playing the video - only use this initially</p>
			</td>
		</tr>
	</tbody>
</table>
<!-- <p class="margin-small"><span class="light-grey">wab.player.</span><strong>pause</strong></p> -->

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>pause</strong>()</code> <strong>(set)</strong></td>
			<td style="vertical-align: text-top">
			<p>Pauses playback of the video</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.player.</span><strong>resume</strong>()</code> <strong>(set)</strong></td>
			<td style="vertical-align: text-top">Resumes (unpauses) playback of the video - only works if the player content is paused</td>
		</tr>
	</tbody>
</table>

<h5 id="wab-pkit-controlling-the-player-events-player">Events <span class="light-grey">| wab.player</span></h5>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th style="width:70%">&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><code class="table">destroy</code></td>
			<td>Triggered just before the player is destroyed</td>
		</tr>
		<tr>
			<td><code class="table">end</code></td>
			<td>Triggered when the video ends</td>
		</tr>
		<tr>
			<td><code class="table">error</code></td>
			<td>Triggered when an error occurs. It can occur during playback with a video element. Inspect the additional <code>data</code> object:
			<pre class="prettyprint small">
wab.player.on(&#39;error&#39;, data =&amp;gt; {
	console.error(&#39;ERROR&#39;, data)
})
</pre>
			</td>
		</tr>
		<tr>
			<td><code class="table">fullscreen</code></td>
			<td>Triggered when the player goes into fullscreen mode - fullscreen is not available by default, only in certain use cases</td>
		</tr>
		<tr>
			<td><code class="table">fullscreenerror</code></td>
			<td>Triggered when the player fails to go into fullscreen mode</td>
		</tr>
		<tr>
			<td><code class="table">mutedchange</code></td>
			<td>Triggered when the video is muted or unmuted</td>
		</tr>
		<tr>
			<td><code class="table">offline</code></td>
			<td>Triggered when the network connection is lost</td>
		</tr>
		<tr>
			<td><code class="table">pause</code></td>
			<td>Triggered when the video gets paused by calling <code class="code">wab.player.pause()</code></td>
		</tr>
		<tr>
			<td><code class="table">paused</code></td>
			<td>Triggered when the video stops playing</td>
		</tr>
		<tr>
			<td><code class="table">play</code></td>
			<td>Triggered when playback is initiated by calling <code class="table">wab.player.play()</code></td>
		</tr>
		<tr>
			<td><code class="table">playing</code></td>
			<td>Triggered when the video starts playing</td>
		</tr>
		<tr>
			<td><code class="table">timeupdate</code></td>
			<td>Triggered constantly while the video is playing</td>
		</tr>
		<tr>
			<td><code class="table">unsupportedformat</code></td>
			<td>Triggered when the player fails to load the content because of an unsupported media format</td>
		</tr>
		<tr>
			<td><code class="table">volumechange</code></td>
			<td>Triggered when the volume is changed</td>
		</tr>
	</tbody>
</table>

<hr class="hrsmall" />
<p class="lead-sm" id="wab-pkit-using-the-api-wabevent-guide"><span class="light-grey">Namespaces and endpoints |</span> <strong>wab.event</strong></p>

<h5>Controlling the player</h5>

<p>Information about an ongoing live or future sports event/fixture is loaded asynchronously and can be accessed via the <code>wab.event</code> property. It provides an interface for handling events. Start listening for the <code>meta</code> event, as described below.</p>
<!-- wab.event namespace provides access to information about live event -->

<pre class="prettyprint lang-javascript small">
const wab = new WAB(options)
wab.event.on(&#39;meta&#39;, data =&amp;gt; {
	console.log(`event: ${data.description}`)
	console.log(&#39;start:&#39;, data.start)
	console.log(&#39;finish:&#39;, data.finish)
})
</pre>

<h5>Instance methods <span class="light-grey">| wab.event</span></h5>
<!-- <h5 class="margin-small"><strong><span class="light-grey">wab.event</span>.on</strong></h5> -->

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.event.</span><strong>on</strong>(eventName[, callback])</code></td>
			<td style="vertical-align: text-top">
			<p>Subscribes a callback handler to an event<br />
			<code>eventName: <em>string</em></code><br />
			<code>callback: <em>function</em></code><br />
			<strong><em>returns</em></strong> <code>WAB|Promise:</code> <em>reference to</em> the Player Kit <em>for chaining OR a Promise</em><br />
			The <code>.on</code> method subscribes a callback handler to an event. The <em>callback</em> argument is optional; if omitted, it will return a <em>Promise</em> object. If a callback is specified, then it will return itself allowing chainability.</p>
			</td>
		</tr>
	</tbody>
</table>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><code><span class="light-grey">wab.event.</span><strong>off</strong>(eventName[, callback])</code></td>
			<td style="vertical-align: text-top">
			<p><code>eventName: <em>string</em></code><br />
			<code>callback: <em>function</em></code><br />
			<strong><em>returns</em></strong> <code>WAB:</code> <em>reference to</em> the Player Kit <em>for chaining</em><br />
			The <code>.off</code> method removes a callback handler from the listeners. It will also return itself for chainability.</p>
			</td>
		</tr>
	</tbody>
</table>

<h5>Events <span class="light-grey">| wab.event</span></h5>

<table class="table-clear" style="text-align: left;font-size: 14px">
	<thead>
		<tr>
			<th style="width:30%">&nbsp;</th>
			<th>&nbsp;</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="width:30%;vertical-align: text-top"><span class="light-grey"><strong>meta</strong></span></td>
			<td>
			<p>Triggered when the stream information is received from the server<br />
			<code>data: object</code> with the following properties:</p>

			<ul>
				<li><code>description: <em>string</em></code> - name of the event</li>
				<li><code>start: <em>number</em></code> - starting time of the event (when it began or will begin) represented in Unix timestamp format, converted to the local timezone (milliseconds)</li>
				<li><code>finish: <em>number</em></code> - ending time of the event, represented in Unix timestamp format, converted to the local timezone (milliseconds)</li>
			</ul>
			</td>
		</tr>
	</tbody>
</table>

<p>Example:</p>

<pre class="prettyprint small">
// listening to an event through regular callback
wab.event.on(&#39;meta&#39;, eventInfo =&amp;gt; {
	console.log(eventInfo)
})
</pre>

<pre class="prettyprint small">
// listening to an event with a promise
wab.event.on(&#39;meta&#39;).then(eventInfo =&amp;gt; {
	console.log(eventInfo)
})
</pre>

<pre class="prettyprint small">
// calculating the event state
wab.event.on(&#39;meta&#39;, eventInfo =&amp;gt; {
	const current = Date.now()

	let status = &#39;currently running&#39;
	if (current &amp;lt; eventInfo.start) {
		status = &#39;not started yet&#39;
	} else if (current &amp;gt; eventInfo.finish) {
		status = &#39;already finished&#39;
	}
	console.log(&#39;Event status: ${status}&#39;)
})
</pre>

<p><a class="no-print" href="#wab-pkit-appendix" target="_parent">Back to Top - Appendix</a></p>

<hr class="hrsmall" />
<h3 id="wab-pkit-appendix-using-webview"><span class="light-grey">Appendix:</span> Using WebView</h3>

<h5>External HTML file</h5>

<p>The HTML file used in the WebView component must be hosted externally. It cannot be a local HTML file that is loaded directly from the project. We require this in order to validate a Player Kit instance against the site rather the local HTML. Additionally, local HTML files are opened with <code>file:///</code> protocol, from which the Player Kit cannot fetch some required external resources.</p>

<h5>Can be loaded via iframe</h5>

<p>In Cordova apps, the main entry HTML file will always be hosted locally. In this case, you can create an <code>&amp;lt;iframe&amp;gt;</code> element which points to an external HTML file containing the Player Kit.</p>

<h5>Cordova test app example</h5>

<p>You can see a basic example of a Cordova test app with the Player Kit on our GitHub repository. See the README file there for more information.</p>

<ul>
	<li><a href="https://github.com/performgroup/wab-cordova-example" target="_blank">Cordova example</a></li>
	<li><a href="https://docs.performgroup.io/wab-playerkit/" target="_blank">Player Kit on GitHub</a></li>
</ul>

<div class="pf-box-info"><strong>Getting the latest updates and information:</strong><br />
While we provide a guide below, you will always find the <strong>latest WebView guide</strong>, <strong>Player Kit API documentation</strong> and usage examples in our release repository. You can also track releases there - this is also useful if you wish to specify a semantic version or use the latest release (not specify a version) and want to differentiate between them. See <a href="https://docs.performgroup.io/wab-playerkit" target="_blank">Player Kit on GitHub</a> or contact your Account Manager for more information. Remember: You can also inspect the code at any time by copying and pasting the URL into your browser.</div>

<p class="no-print"><a class="no-print" href="#top" target="_parent">Back to top</a></p>
</div>
</div>
<!-- end single-tech_docs --></section>
