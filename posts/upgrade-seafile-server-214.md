<!-- 
.. title: Upgrade Seafile Server -> 2.1.4
.. slug: upgrade-seafile-server-214
.. date: 2014/02/03 14:01:06
.. tags: seafile, Centos, Admin
.. link: 
.. description: 
.. type: text
-->

Ein "Major" Update ist herausgekommen (2.1.4)
* es gelten wieder die gleiche Dinge beim Upgarde wie in meinem frueheren [Post](./upgrade-seafile-server.html) Ich habe auch kleine Fehler im frueheren Post behoben.
* Es gibt ein Upgrade Script das im `seafile-server-2.1.4/upgarde/upgrade_2.0_2.1.sh`, dies wird einfach nur ausgefuehrt und sollte alle Aenderungen an der Datenbank vornehmen.

<!-- TEASER_END -->

Benutzerrechte
--------------

Bei mir laeuft Seafile als unpriviligierter User, da ich mich als solcher nicht am Server anmelden kann. Wird die neue Version als "root" User entpackt! Leider kann aber so der Server vom init Script nicht gestartet werden:

`chown -R seafile seafile-server-2.1.4`

Startscript
-----------

Wichtig ist das der Wert in der Variablen `${script_path} ` im File [`/etc/init.d/seafile-server`](./upgrade-seafile-server.html#seafile-script) angepasst wird.

Auch muss im Apache Config File der Path bei `Alias` angepasst werden: 

	<VirtualHost *:80>
	  ServerName file.b9i.de
	  DocumentRoot /srv/seafile/b9i/web
	  Alias /media  /srv/seafile/b9i/seafile-server-2.1.4/seahub/media
	  ErrorLog logs/file.b9i.de-error_log
	  CustomLog logs/file.b9i.de-access_log common

	  RewriteEngine On

	  #
	  # seafile httpserver
	  #
	  ProxyPass /seafhttp http://127.0.0.1:8082
	  ProxyPassReverse /seafhttp http://127.0.0.1:8082
	  RewriteRule ^/seafhttp - [QSA,L]

	  #
	  # seahub
	  #
	  RewriteRule ^/(media.*)$ /$1 [QSA,L,PT]
	  RewriteCond %{REQUEST_FILENAME} !-f
	  RewriteRule ^(.*)$ /seahub.fcgi$1 [QSA,L,E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
	</VirtualHost>

Aenderungen wie Logo etc.
-----------------

er hat sich im in der neuen Version etwas geaendert, was die LOGO's usw angeht:

im Verzeichnis `<media>`
`mkdir custom`

dort hin das Logo kopieren und vielleicht auch noch ein neues CSS-File

`vim seahub/media/custom/custom.css`

	#logo {
    	margin: auto;
    	width: 53px;
    	height: 53px;
	}