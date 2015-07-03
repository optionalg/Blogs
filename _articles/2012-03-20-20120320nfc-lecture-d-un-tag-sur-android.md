---
ID: 440
post_title: 'NFC II &#8211; Lecture d&#039;un Tag sur Android'
author: Guillaume Gerbaud
post_date: 2012-03-20 08:33:00
post_excerpt: |
  <p>Dans cet article, nous allons voir comment lire un Tag <a href="/index.php?tag/nfc">NFC</a> sur Android. L'article sera illustré d'exemples issus de l'application Zen Wifi NFC. L'application Zen Wifi NFC permet de connecter automatiquement un appareil Android compatible NFC à un réseau Wifi, lors de la lecture d'un Tag spécifique.</p>
layout: post
permalink: http://blog.zenika-offres.com/?p=440
published: true
slide_template:
  - ""
---
<p>Dans cet article, nous allons voir comment lire un Tag <a href="/index.php?tag/nfc">NFC</a> sur Android. L'article sera illustré d'exemples issus de l'application Zen Wifi NFC. L'application Zen Wifi NFC permet de connecter automatiquement un appareil Android compatible NFC à un réseau Wifi, lors de la lecture d'un Tag spécifique.</p>
<!--more-->
<h2>Tag Dispatch</h2> <p><br />
La puce NFC des appareils Android n'est active que si l'utilisateur en décide ainsi dans ses paramètres et si l'écran de l'appareil est déverrouillé. Lorsqu'un Tag est détecté, Android va tenter de découvrir le type MIME de la donnée et aussi son URI. Ces informations, ainsi que le Tag, sont ensuite enveloppés dans un Intent.<br /></p> <p>En fonction du type de message contenu dans le Tag, l'action de l'Intent envoyé sera différente. <br /></p> <p>Ainsi, si le Tag contient un message NDEF correctement formatté, l'action sera <a href="http://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED">NDEF_DISCOVERED</a>. <br />
Si le Tag ne contient pas de message NDEF, ou si à partir de celui-ci aucun type MIME ou URI ne peut être identifié, ou si aucune application ne possède de filtre pour l'action NDEF_DISCOVERED, alors l'action de l'Intent envoyé sera <a href="http://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED">TECH_DISCOVERED</a>.<br />
Enfin, si aucune Activity ne possède le filtre adéquat, alors l'Intent est envoyé avec l'action <a href="http://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED">TAG_DISCOVERED</a>.</p> <p>Le schéma ci dessous résume le mécanisme de dispatch. <br />
<br />
<a href="http://developer.android.com/guide/topics/nfc/nfc.html#dispatching"><img src="http://developer.android.com/images/nfc_tag_dispatch.png" alt="Tag Dispatch System" style="display:block; margin:0 auto;" title="Tag Dispatch System" /></a> <br /></p> <h3>Configurer son application</h3> <p>Plusieurs points de configuration sont nécessaires au niveau du fichier AndroidManifest.xml.</p> <h4>Demander la permission</h4> <p>Pour pouvoir utiliser le NFC, votre application doit demander la permission.</p> <pre class="xml code xml" style="font-family:inherit"><span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;uses-permission</span> <span style="color: #000066;">android:name</span>=<span style="color: #ff0000;">&quot;android.permission.NFC&quot;</span> <span style="color: #000000; font-weight: bold;">/&gt;</span></span></pre> <h4>Préciser la version minimum du SDK</h4> <p>Il est théoriquement possible de mettre 9 (Gingerbread) pour la propriété <code>minSdkVersion</code>, mais l'Api NFC de cette version est plus que limitée. Je conseille donc de mettre 10 (Gingerbread MR1), voire 14 pour l'utilisation de Beam (Beam sera abordé dans un futur billet).</p> <pre class="xml code xml" style="font-family:inherit"><span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;uses-sdk</span> <span style="color: #000066;">android:minSdkVersion</span>=<span style="color: #ff0000;">&quot;10&quot;</span><span style="color: #000000; font-weight: bold;">/&gt;</span></span></pre> <h4>Cibler uniquement les appareils compatibles</h4> <p>Le Tag <code>uses-feature</code> permet de n'afficher l'application dans le market que pour les appareils qui possèdent cette fonctionnalité. Cependant, si le NFC n'est pas une fonctionnalité essentielle de votre application, il est possible de cibler plus large et de vérifier la disponibilité du NFC au runtime (ceci est également vrai concernant le minSdkVersion).</p> <pre class="xml code xml" style="font-family:inherit"><span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;uses-feature</span> <span style="color: #000066;">android:name</span>=<span style="color: #ff0000;">&quot;android.hardware.nfc&quot;</span> <span style="color: #000066;">android:required</span>=<span style="color: #ff0000;">&quot;true&quot;</span> <span style="color: #000000; font-weight: bold;">/&gt;</span></span></pre> <h3>Utiliser le bon filtre</h3> <p>Il est important que le ou les <code>intent-filter</code> placés sur vos Activity soit paramétrés avec la plus grande attention. Votre application ne doit pas être proposée à l'utilisateur si elle n'est pas intéressée par le Tag qui vient d'être découvert (e.g. le Tag contient du texte brut, et votre application ne s’intéresse qu'aux Tag contenant une Url).<br /></p> <p>De même, il est important de filtrer sur la bonne action. En filtrant sur l'action TAG_DISCOVERED, votre application ne sera appelée que si aucune autre n'est en mesure de gérer le Tag, ce qui est peu probable.</p> <h4>Zen Wifi NFC</h4> <p>L'application Zen Wifi NFC utilise un type de données spécifique, ce n'est pas un type standard. <br />
La norme du NFC Forum permet de définir ses propres types en utilisant le TNF (Type Name Format) External Type.<br /></p> <p>Le type de notre donnée sera <q>zenwifi</q>. L'Urn de ce type, tel que défini par le NFC Forum sera <br /><code>urn:nfc:ext:zenika.com:zenwifi</code>.<br /></p> <p>A la lecture d'un Tag de tel type, Android transforme l'Urn en l'Uri suivante<br /> <code>vnd.android.nfc://ext/zenika.com:zenwifi</code>.<br /></p> <p>Nous utiliserons par conséquent le filtre suivant.<br /></p> <pre class="xml code xml" style="font-family:inherit"><span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;activity</span> <span style="color: #000066;">android:name</span>=<span style="color: #ff0000;">&quot;.WifiReaderActivity&quot;</span><span style="color: #000000; font-weight: bold;">&gt;</span></span> 	<span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;intent-filter<span style="color: #000000; font-weight: bold;">&gt;</span></span></span> 		<span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;action</span> <span style="color: #000066;">android:name</span>=<span style="color: #ff0000;">&quot;android.nfc.action.NDEF_DISCOVERED&quot;</span> <span style="color: #000000; font-weight: bold;">/&gt;</span></span> 		<span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;category</span> <span style="color: #000066;">android:name</span>=<span style="color: #ff0000;">&quot;android.intent.category.DEFAULT&quot;</span> <span style="color: #000000; font-weight: bold;">/&gt;</span></span> 		<span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;data</span> <span style="color: #000066;">android:scheme</span>=<span style="color: #ff0000;">&quot;vnd.android.nfc&quot;</span></span> <span style="color: #009900;">        		<span style="color: #000066;">android:host</span>=<span style="color: #ff0000;">&quot;ext&quot;</span></span> <span style="color: #009900;">        		<span style="color: #000066;">android:path</span>=<span style="color: #ff0000;">&quot;/zenika.com:zenwifi&quot;</span><span style="color: #000000; font-weight: bold;">/&gt;</span></span> 	<span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;/intent-filter<span style="color: #000000; font-weight: bold;">&gt;</span></span></span> <span style="color: #009900;"><span style="color: #000000; font-weight: bold;">&lt;/activity<span style="color: #000000; font-weight: bold;">&gt;</span></span></span></pre> <h3>Foreground Dispatch</h3> <p>Une Activity au premier plan peut court-circuiter le dispatch par Intent grâce au foreground dispatch. La mise en place se fait dans l'Activity. On commence par obtenir une instance du <code>NfcAdapter</code>.</p> <pre class="java code java" style="font-family:inherit"><span style="color: #7F0055; font-weight: bold;">public</span> <span style="color: #7F0055; font-weight: bold;">class</span> ForegroundDispatchTest <span style="color: #7F0055; font-weight: bold;">extends</span> Activity <span style="color: #000000;">&#123;</span> &nbsp; 	<span style="color: #7F0055; font-weight: bold;">private</span> NfcAdapter	mNfcAdapter; &nbsp; 	@Override 	<span style="color: #7F0055; font-weight: bold;">protected</span> <span style="color: #7F0055; font-weight: bold;">void</span> onCreate<span style="color: #000000;">&#40;</span>Bundle savedInstanceState<span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 		<span style="color: #7F0055; font-weight: bold;">super</span>.<span style="color: #000000;">onCreate</span><span style="color: #000000;">&#40;</span>savedInstanceState<span style="color: #000000;">&#41;</span>; 		setContentView<span style="color: #000000;">&#40;</span>R.<span style="color: #000000;">layout</span>.<span style="color: #000000;">main</span><span style="color: #000000;">&#41;</span>; 		mNfcAdapter = NfcAdapter.<span style="color: #000000;">getDefaultAdapter</span><span style="color: #000000;">&#40;</span>getApplicationContext<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span>; 		<span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span>mNfcAdapter == <span style="color: #7F0055; font-weight: bold;">null</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 			<span style="color: #808080; font-style: italic;">// NFC is not available</span> 			finish<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 			<span style="color: #7F0055; font-weight: bold;">return</span>; 		<span style="color: #000000;">&#125;</span> 		<span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">!</span>mNfcAdapter.<span style="color: #000000;">isEnabled</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 			<span style="color: #808080; font-style: italic;">// NFC is disabled</span> 			finish<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 			<span style="color: #7F0055; font-weight: bold;">return</span>; 		<span style="color: #000000;">&#125;</span> 	<span style="color: #000000;">&#125;</span> <span style="color: #000000;">&#125;</span></pre> <p>Il est obligatoire que l'Activity soit au premier plan, on active donc le foreground dispatch dans onResume et on le désactive dans le onPause.</p> <pre class="java code java" style="font-family:inherit"><span style="color: #7F0055; font-weight: bold;">public</span> <span style="color: #7F0055; font-weight: bold;">class</span> ForegroundDispatchTest <span style="color: #7F0055; font-weight: bold;">extends</span> Activity <span style="color: #000000;">&#123;</span> &nbsp; 	<span style="color: #7F0055; font-weight: bold;">private</span> NfcAdapter	mNfcAdapter; &nbsp; 	<span style="color: #000000;">&#91;</span>...<span style="color: #000000;">&#93;</span> &nbsp; 	@Override 	<span style="color: #7F0055; font-weight: bold;">protected</span> <span style="color: #7F0055; font-weight: bold;">void</span> onResume<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 		<span style="color: #7F0055; font-weight: bold;">super</span>.<span style="color: #000000;">onResume</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 		<span style="color: #000000;">&#91;</span>...<span style="color: #000000;">&#93;</span> 		mNfcAdapter.<span style="color: #000000;">enableForegroundDispatch</span><span style="color: #000000;">&#40;</span><span style="color: #7F0055; font-weight: bold;">this</span>, pendingIntent, filters, techListsArray<span style="color: #000000;">&#41;</span>; 	<span style="color: #000000;">&#125;</span> &nbsp; 	@Override 	<span style="color: #7F0055; font-weight: bold;">protected</span> <span style="color: #7F0055; font-weight: bold;">void</span> onPause<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 		mNfcAdapter.<span style="color: #000000;">disableForegroundDispatch</span><span style="color: #000000;">&#40;</span><span style="color: #7F0055; font-weight: bold;">this</span><span style="color: #000000;">&#41;</span>; 		<span style="color: #7F0055; font-weight: bold;">super</span>.<span style="color: #000000;">onPause</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 	<span style="color: #000000;">&#125;</span> &nbsp; <span style="color: #000000;">&#125;</span></pre> <p>La méthode <code>enableForegroundDispatch</code> prend en paramètre l'Activity cible du dispatch, un <a href="http://developer.android.com/reference/android/app/PendingIntent.html">PendingIntent</a>, un tableau d'IntentFilter et un tableau à double entrée de String, qui représente les technologies qui doivent être supportées
par le Tag.<br />
Pour qu'un Tag soit dispatché, il faut qu'il supporte toutes les technologies indiquées dans une des entrées de premier niveau du dernier paramètre (ET logique entre les éléments de second niveau, OU logique entre les éléments de premier niveau).<br />
Il est possible de passer null pour les 2 derniers paramètres&nbsp;; tous les Tag seront alors reçus.</p> <h4>Zen Wifi NFC</h4> <p>On utilisera la même Activity pour déclencher le foreground dispatch et pour analyser les Tag captés. On utilise donc <code>getClass</code> pour préciser la classe de l'Intent, mais pour ne pas superposer plusieurs fois le même écran, on ajoute un flag qui permet de ne pas redémarrer l'Activity si elle est déjà au premier plan. <br /></p> <p>On utilise le même filtre que dans le manifest. <br />
Au niveau des technologies, on demande à ce que le Tag supporte les messages NDEF. <br />
<em>A titre personnel, je trouve le dernier paramètre redondant lorsqu'on filtre l'action NDEF_DISCOVERED, mais il est obligatoire.</em></p> <pre class="java code java" style="font-family:inherit">@Override <span style="color: #7F0055; font-weight: bold;">protected</span> <span style="color: #7F0055; font-weight: bold;">void</span> onResume<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 	<span style="color: #7F0055; font-weight: bold;">super</span>.<span style="color: #000000;">onResume</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; &nbsp; 	PendingIntent pendingIntent = PendingIntent.<span style="color: #000000;">getActivity</span><span style="color: #000000;">&#40;</span>mContext, 0, 			<span style="color: #7F0055; font-weight: bold;">new</span> Intent<span style="color: #000000;">&#40;</span>mContext, getClass<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span>.<span style="color: #000000;">addFlags</span><span style="color: #000000;">&#40;</span>Intent.<span style="color: #000000;">FLAG_ACTIVITY_SINGLE_TOP</span><span style="color: #000000;">&#41;</span>, 0<span style="color: #000000;">&#41;</span>; &nbsp; 	IntentFilter ndefFilter = <span style="color: #7F0055; font-weight: bold;">new</span> IntentFilter<span style="color: #000000;">&#40;</span>NfcAdapter.<span style="color: #000000;">ACTION_NDEF_DISCOVERED</span><span style="color: #000000;">&#41;</span>; 	ndefFilter.<span style="color: #000000;">addDataScheme</span><span style="color: #000000;">&#40;</span><span style="color: #888888;">&quot;vnd.android.nfc&quot;</span><span style="color: #000000;">&#41;</span>; 	ndefFilter.<span style="color: #000000;">addDataAuthority</span><span style="color: #000000;">&#40;</span><span style="color: #888888;">&quot;ext&quot;</span>, <span style="color: #7F0055; font-weight: bold;">null</span><span style="color: #000000;">&#41;</span>; 	ndefFilter.<span style="color: #000000;">addDataPath</span><span style="color: #000000;">&#40;</span><span style="color: #888888;">&quot;/zenika.com:zenwifi&quot;</span>, PatternMatcher.<span style="color: #000000;">PATTERN_LITERAL</span><span style="color: #000000;">&#41;</span>; &nbsp; 	IntentFilter<span style="color: #000000;">&#91;</span><span style="color: #000000;">&#93;</span> filters = <span style="color: #000000;">&#123;</span>ndefFilter<span style="color: #000000;">&#125;</span>; &nbsp; 	<span style="color: #000000;">String</span><span style="color: #000000;">&#91;</span><span style="color: #000000;">&#93;</span><span style="color: #000000;">&#91;</span><span style="color: #000000;">&#93;</span> techs = <span style="color: #000000;">&#123;</span><span style="color: #000000;">&#123;</span>Ndef.<span style="color: #7F0055; font-weight: bold;">class</span>.<span style="color: #000000;">getName</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#125;</span><span style="color: #000000;">&#125;</span>; &nbsp; 	mNfcAdapter.<span style="color: #000000;">enableForegroundDispatch</span><span style="color: #000000;">&#40;</span><span style="color: #7F0055; font-weight: bold;">this</span>, pendingIntent, filters, techs<span style="color: #000000;">&#41;</span>; <span style="color: #000000;">&#125;</span></pre> <h2>Lire le contenu du Tag</h2> <p><br />
Si notre application est active, et le foreground dispatch effectif, puisque l'Activity ne sera pas redémarrée, c'est la méthode <code>onNewIntent</code> qui sera appelée. Dans les autres cas, l'Intent sera passé à notre Activity via <code>onCreate</code>.</p> <pre class="java code java" style="font-family:inherit">@Override <span style="color: #7F0055; font-weight: bold;">protected</span> <span style="color: #7F0055; font-weight: bold;">void</span> onNewIntent<span style="color: #000000;">&#40;</span>Intent intent<span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 	<span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span>NfcAdapter.<span style="color: #000000;">ACTION_NDEF_DISCOVERED</span>.<span style="color: #000000;">equals</span><span style="color: #000000;">&#40;</span>intent.<span style="color: #000000;">getAction</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 		<span style="color: #000000;">&#91;</span>...<span style="color: #000000;">&#93;</span> 	<span style="color: #000000;">&#125;</span> <span style="color: #000000;">&#125;</span></pre> <p><br />
L'Intent reçu contient obligatoirement le Tag découvert dans l'extra <code>EXTRA_TAG</code></p> <pre class="java code java" style="font-family:inherit">Tag tag = intent.<span style="color: #000000;">getParcelableExtra</span><span style="color: #000000;">&#40;</span>NfcAdapter.<span style="color: #000000;">EXTRA_TAG</span><span style="color: #000000;">&#41;</span>;</pre> <p>De plus, si l'action de l'Intent est <code>NDEF_DISCOVERED</code>, on peut récupérer directement les messages NDEF à partir de l'extra <code>EXTRA_NDEF_MESSAGES</code>.<br /></p> <p>Pour obtenir le premier enregistrement du premier message, on procède de la manière suivante.</p> <pre class="java code java" style="font-family:inherit">Parcelable<span style="color: #000000;">&#91;</span><span style="color: #000000;">&#93;</span> rawMsgs = intent.<span style="color: #000000;">getParcelableArrayExtra</span><span style="color: #000000;">&#40;</span>NfcAdapter.<span style="color: #000000;">EXTRA_NDEF_MESSAGES</span><span style="color: #000000;">&#41;</span>; <span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span>rawMsgs <span style="color: #000000;">!</span>= <span style="color: #7F0055; font-weight: bold;">null</span> <span style="color: #000000;">&amp;&amp;</span> rawMsgs.<span style="color: #000000;">length</span> <span style="color: #000000;">&gt;</span> 0<span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 	NdefMessage message = <span style="color: #000000;">&#40;</span>NdefMessage<span style="color: #000000;">&#41;</span> rawMsgs<span style="color: #000000;">&#91;</span>0<span style="color: #000000;">&#93;</span>; 	NdefRecord<span style="color: #000000;">&#91;</span><span style="color: #000000;">&#93;</span> records = message.<span style="color: #000000;">getRecords</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 	<span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span>records.<span style="color: #000000;">length</span> <span style="color: #000000;">&gt;</span> 0<span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 		NdefRecord record = records<span style="color: #000000;">&#91;</span>0<span style="color: #000000;">&#93;</span>; 		<span style="color: #000000;">&#91;</span>...<span style="color: #000000;">&#93;</span> 	<span style="color: #000000;">&#125;</span> <span style="color: #000000;">&#125;</span></pre> <h3>Déchiffrage du payload</h3> <p>On obtient le payload (contenu de notre enregistrement) sous forme de tableau de <code>byte</code> de la manière suivante.</p> <pre class="java code java" style="font-family:inherit"><span style="color: #7F0055; font-weight: bold;">byte</span><span style="color: #000000;">&#91;</span><span style="color: #000000;">&#93;</span> payload = record.<span style="color: #000000;">getPayload</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>;</pre> <h4>Zen Wifi NFC</h4> <p>Le payload est la concaténation du SSID du réseau Wifi et de sa clé d'authentification, tous deux sous forme de chaînes de caractères entourées de double quote.</p> <pre> &quot;example_SSID&quot;&quot;password&quot; </pre> <p>Les doubles quotes sont nécessaires pour l'Api de configuration du Wifi sur Android. Voici comment on procède.</p> <pre class="java code java" style="font-family:inherit"><span style="color: #000000;">String</span> ssidPwd = <span style="color: #7F0055; font-weight: bold;">new</span> <span style="color: #000000;">String</span><span style="color: #000000;">&#40;</span>record.<span style="color: #000000;">getPayload</span><span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span>; <span style="color: #7F0055; font-weight: bold;">int</span> idx = ssidPwd.<span style="color: #000000;">indexOf</span><span style="color: #000000;">&#40;</span><span style="color: #888888;">&quot;<span style="color: #000099; font-weight: bold;">&quot;</span><span style="color: #000099; font-weight: bold;">&quot;</span>&quot;</span><span style="color: #000000;">&#41;</span>; <span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span>idx <span style="color: #000000;">&gt;</span> 0<span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 	<span style="color: #000000;">String</span> ssid = ssidPwd.<span style="color: #000000;">substring</span><span style="color: #000000;">&#40;</span>0, idx + <span style="color: #cc66cc;">1</span><span style="color: #000000;">&#41;</span>; 	<span style="color: #000000;">String</span> pwd = ssidPwd.<span style="color: #000000;">substring</span><span style="color: #000000;">&#40;</span>idx + <span style="color: #cc66cc;">1</span><span style="color: #000000;">&#41;</span>; 	WifiConfiguration conf = <span style="color: #7F0055; font-weight: bold;">new</span> WifiConfiguration<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 	conf.<span style="color: #000000;">SSID</span> = ssid; 	conf.<span style="color: #000000;">preSharedKey</span> = pwd; 	WifiManager wManager = <span style="color: #000000;">&#40;</span>WifiManager<span style="color: #000000;">&#41;</span> getSystemService<span style="color: #000000;">&#40;</span><span style="color: #000000;">Context</span>.<span style="color: #000000;">WIFI_SERVICE</span><span style="color: #000000;">&#41;</span>; 	<span style="color: #7F0055; font-weight: bold;">int</span> id = wManager.<span style="color: #000000;">addNetwork</span><span style="color: #000000;">&#40;</span>conf<span style="color: #000000;">&#41;</span>; 	<span style="color: #7F0055;font-weight: bold;">if</span><span style="color: #000000;">&#40;</span>wManager.<span style="color: #000000;">enableNetwork</span><span style="color: #000000;">&#40;</span>id, <span style="color: #7F0055; font-weight: bold;">false</span><span style="color: #000000;">&#41;</span><span style="color: #000000;">&#41;</span> <span style="color: #000000;">&#123;</span> 		finish<span style="color: #000000;">&#40;</span><span style="color: #000000;">&#41;</span>; 	<span style="color: #000000;">&#125;</span> 	<span style="color: #7F0055;font-weight: bold;">else</span> <span style="color: #000000;">&#123;</span> 		<span style="color: #808080; font-style: italic;">//...</span> 	<span style="color: #000000;">&#125;</span> <span style="color: #000000;">&#125;</span></pre> <h2>Conclusion</h2> <p><br />
Pour le développeur, la lecture d'un Tag <a href="/index.php?tag/nfc">NFC</a> se fait en 2 étapes&nbsp;: la gestion du dispatch et la récupération du contenu. <br />
Le mécanisme de dispatch avec Intent permet d'utiliser la puissance des IntentFilter et de ne recevoir que les Tag qui nous intéressent. <br />
Ensuite, le contenu du Tag dépend du besoin. Quelques types de données sont spécifiés par le <a href="http://www.nfc-forum.org/specs/spec_list/" hreflang="en">NFC Forum</a>, et le format du payload doit être conforme à la spécification, mais il est également possible d'utiliser ses propres formats.<br /></p> <p>Dans le prochain article, nous verrons comment écrire des données sur un Tag.</p>