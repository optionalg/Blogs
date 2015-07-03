---
ID: 346
post_title: 'Retour sur la soirée &quot;Young Blood&quot;'
author: xdetant
post_date: 2014-01-27 10:30:00
post_excerpt: '<p>Le 14 janvier dernier le Paris JUG  a organisé une soirée «Young Blood». L’idée est simple&nbsp;: proposer à des personnes n’ayant jamais fait de présentation de se lancer. C’est ainsi que 7 nouveaux orateurs ont vu le jour. Nous avons eu le plaisir d’assister à ces conférences et nous vous proposons ici un résumé.</p>'
layout: post
permalink: http://blog.zenika-offres.com/?p=346
published: true
---
<p>Le 14 janvier dernier le Paris JUG  a organisé une soirée «Young Blood». L’idée est simple&nbsp;: proposer à des personnes n’ayant jamais fait de présentation de se lancer. C’est ainsi que 7 nouveaux orateurs ont vu le jour. Nous avons eu le plaisir d’assister à ces conférences et nous vous proposons ici un résumé.</p>
<!--more-->
<h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/2014-01-04ParisJUG-PackagingDebianpourJava.pdf">«apt-get myapp !» présenté par Emmanuel Bourg</a></h4> <p>Emmanuel nous a rappelé les avantages d’utiliser des paquets Debian lors de l’installation d’une solution (vision de l’administrateur systèmes) ainsi que l’intérêt de proposer des paquets Debian (vision du vendeur). Suite à cela, il nous a présenté deux manières de créer un paquet Debian pour une application Java&nbsp;:</p> <ul> <li>avec les outils Debian</li> <li>avec <a href="https://github.com/tcurdt/jdeb">jdeb</a></li> </ul> <p>La première solution est assez difficile à maîtriser mais nécessaire pour proposer son paquet aux archives officielles de Debian. La seconde est plus simple et permet d'intégrer la construction de paquet Debian à son outil de build (maven ou ant par exemple). Bien que cette méthode ne permet pas d’être candidat à l’ajout aux dépots officiels, elle permet, alliée à un auto-hébergement de serveur d’archives, de profiter de tous les avantages des paquets Debian pour son application. Si vous voulez que votre application soit distribuée largement, rapidement et simplement, <a href="https://github.com/tcurdt/jdeb">jdeb</a> est une solution à regarder de très près comme peut en <a href="https://github.com/elasticsearch/elasticsearch/issues/1525">témoigner l’équipe d’ElasticSearch</a>.</p> <h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/jvmtools.pdf">«JVM Tools: The hard way» présenté par Brice Leporini.</a></h4> <p>Durant cette présentation, Brice nous a présenté les outils de monitoring built-in de la JDK après nous avoir démontré que la ligne de commande est toujours un outil indispensable malgré les outils graphiques dont nous disposons aujourd’hui. En effet, l’argumentaire est très simple&nbsp;: les serveurs ne tournent jamais avec une interface graphique et il est quasi impossible d’avoir les droits pour se connecter au port de monitoring sur un serveur de production. Nous avons alors pu voir une démonstration de quelques outils, notamment&nbsp;:</p> <ul> <li>jps</li> <li>jinfo</li> <li>jstack</li> <li>jmap</li> <li>jstat</li> </ul> <p>Brice nous a brillamment démontré que ces outils sont très puissants et que leur maîtrise est essentielle. Le public a visiblement partagé notre avis puisque cette présentation a été élue meilleur présentation de la soirée.</p> <h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/JavaFxTour.pdf">«Les IHM riches en Java ne sont pas mortes&nbsp;: tour d'horizon de Java FX&nbsp;» présenté par Simon Basle</a></h4> <p>Simon nous a présenté <a href="http://docs.oracle.com/javafx/2/overview/jfxpub-overview.htm">Java FX</a>, la nouvelle API Java qui a pour vocation de remplacer Swing. Nous avons ainsi eu le plaisir de découvrir un comparatif entre ce que nous avons l’habitude de faire avec Swing et ce que permet <a href="http://docs.oracle.com/javafx/2/overview/jfxpub-overview.htm">Java FX</a>. Nous découvrons ainsi que <a href="http://docs.oracle.com/javafx/2/overview/jfxpub-overview.htm">Java FX</a> propose&nbsp;:</p> <ul> <li>l’utilisation de l'accélération matérielle,</li> <li>s’appuie sur le langage CSS,</li> <li>permet d’externaliser les layouts</li> <li>apporte un support pour le multimédia (MP3, MPEG-4, …),</li> <li>apporte un support pour les écrans tactiles (rotate, zoom, swipe, scroll),</li> <li>est capable de produire un rendu 3D,</li> <li>etc.</li> </ul> <p>Le constat est sans appel, <a href="http://docs.oracle.com/javafx/2/overview/jfxpub-overview.htm">Java FX</a> est plus simple, plus performant, plus élégant, plus riche. Cerise sur le gâteau, Simon nous explique que <a href="http://docs.oracle.com/javafx/2/overview/jfxpub-overview.htm">Java FX</a> peut-être intégré à du Swing (et inversement à partir de Java 8). Que vous démarriez une nouvelle application lourde ou que vous soyez en plein milieu de votre projet, pensez à <a href="http://docs.oracle.com/javafx/2/overview/jfxpub-overview.htm">Java FX</a>.</p> <h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/Desrecommandationsauservicedubusiness-15minJUG.pdf">«Des recommandations au service du business» présenté par Loïc Knuchel.</a></h4> <p>Il s’agit là d’un type de présentation qu’il est rare de voir à un JUG&nbsp;: une présentation fonctionnelle. Ainsi Loïc s’est appliqué à nous expliquer pourquoi les clients veulent un système de recommandations et non pas comment on les réalisait. Il nous a ainsi été rappelé qu’il ne sert à rien de mettre en place un moteur de recommandations si nous ne savons pas pourquoi nous le voulons. La mise en place de ces solutions ont cependant tous une même origine&nbsp;: servir le business du site. En effet, soit nous guidons l’utilisateur vers ce qu’il aime soit nous le guidons vers ce qu’il pourrait aimer mais dans les deux cas, l’objectif est de faire consommer notre contenu. Enfin, Loïc nous a expliqué qu’en fonction des objectifs définis par le métier, notre façon de faire de la recommandation changerait&nbsp;: le site peut prendre des initiatives en proposant des produits dont le stock est trop important, ou les recommandations peuvent être très personnalisées pour correspondre exactement aux goûts de chaque utilisateur, ou elles peuvent être contextualisées pour correspondre aux produits en cours de consultation,… Au final, Loïc nous a élégamment rappelé que la technique est au service du business et non pas le contraire. De plus, il nous a aussi précisé que le respect des données personnelles est très important et qu’il faut vite fixer des limites à ne pas transgresser. Nous retiendrons notamment ceci&nbsp;: “Si je dis publiquement ce que je fais, est-ce que ça me gène”&nbsp;?</p> <h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/PredictionIO5.pdf">«Recommandation avec PredictionIO» présenté par Ludwine Probst</a></h4> <p>Ludwine nous a elle aussi fait une présentation sur les recommandations mais en se concentrant cette fois ci sur la technique. Nous avons ainsi eu le plaisir de découvrir <a href="http://prediction.io/">PredictionIO</a>, une solution open-source permettant de faire de la prédiction (et donc de la recommandation). Cette solution s’appuie notamment sur scala, play!, hadoop, mahout et mongodb et est utilisable via de nombreux langages dont Java. Le principe de <a href="http://prediction.io/">PredictionIO</a> est classique&nbsp;: construire un modèle de recommandation en fonction des données collectées. Bien que sur le papier, cela paraisse très simple, en pratique, la mise en place se révèle complexe. C’est là toute la puissance de <a href="http://prediction.io/">PredictionIO</a> qui offre une solution clef en main à ce problème en s’appuyant sur les algorithmes de mahout pour construire le modèle qui est ensuite stocké dans mongodb avant d’être requêté par votre application. <a href="http://prediction.io/">PredictionIO</a> est une solution que nous suivront de près et nous remercions fortement Ludwine pour nous l’avoir fait découvrir.</p> <h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/prez-disruptor-young-blood-2014.pdf">«Pimp my Inter Thread Communication (aka Inter-Thread Messaging Architecture)» présenté par Hichame El Khalfi</a></h4> <p>Durant cette présentation, Hichame nous a présenté des méthodes de communication entre threads. Dans un premier temps, il nous a été rappelé comment fonctionne le pattern de producteur - consommateur utilisé régulièrement via un système de queue. Cette solution est simple et efficace tant que le nombre de communications est limité et que le nombre de consommateurs est strictement égal au nombre de producteur, mais dès qu’un de ces paramètres n’est plus maîtrisé tout se complique et l’application devient vite impossible à maintenir. Heureusement, Hichame nous a présenté le pattern <a href="http://lm
ax-exchange.github.io/disruptor/">Disruptor</a> qui permet de palier à cette montée en complexité. Le principe de producteur - consommateur est conservé mais cette fois ci, grâce à l’utilisation d’un ring buffer, la gestion des messages est mieux centralisée (et donc plus simple à maintenir) et plusieurs consommateurs peuvent lire le même message (ce qui simplifie grandement les communications). Le pattern <a href="http://lmax-exchange.github.io/disruptor/">Disruptor</a> est donc une alternative très intéressante à l’utilisation classique de queues dans le domaine de communication inter-thread et a pour vocation à être utilisé dès que les échanges se multiplient.</p> <h4><a href="http://www.parisjug.org/xwiki/bin/download/Meeting/20140114/parisjug-clojure-overtone.pdf">«Apéritif dinatoire avec Clojure et Overtone» présenté par Mathieu Gandin</a></h4> <p>Après toutes ces présentations bien sérieuses, Mathieu nous a proposé de nous détendre avec un peu de musique et nous a ainsi présenté <a href="http://overtone.github.io/">Overtone</a>. <a href="http://overtone.github.io/">Overtone</a> est un outil qui associe SuperCollider avec le language Clojure. Bien que de solides connaissances en mathématiques et en physique semblent requises pour pouvoir utiliser cette solution, son côté ludique est indéniable. En effet, nous avons eu le plaisir de découvrir du code dans une nouvelle dimension&nbsp;: la dimension sonore. Ainsi, en plus de voir les lignes de code que nous a présentées Mathieu, nous les entendions. Mathieu s’est ensuite lancé dans une session de live-coding où nous entendions la musique produite par son code. La moindre erreur de frappe se transforme alors vite en fausse note mais Mathieu s’en est brillamment sorti.</p>