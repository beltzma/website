<!-- 
.. title: SSMTP mit SMTP Authentication
.. slug: ssmtp-mit-smtp-authentication
.. date: 2014/02/18 11:44:55
.. tags: Admin, Centos, SMTP, SMTP Auth
.. link: 
.. description: 
.. type: text
-->

Manchmal ist es mit Kanonen auf Spatzen geschossen einen kompletten Mailserver einzurichten, dh. ein kleines Tool um nur mal die root Emails zu verschicken, ist noetig.

Die LSG: ssmtp (kann sogar SMTP Auth)

Einfache Konfiguration:

`yum install ssmtp`

`alternatives --config mta` dort die Nummer waehlen, wo auch `/usr/sbin/sendmail.ssmtp` steht

Minimalste Konfig `/etc/ssmtp/ssmtp.conf`:

	root=postmaster

	# The place where the mail goes. The actual machine name is required
	# no MX records are consulted. Commonly mailhosts are named mail.domain.com
	# The example will fit if you are in domain.com and your mailhub is so named.
	mailhub=<adresse des Mailservers>
	AuthUser=<usernamen fuer SMTP Auth>
	AuthPass=<password fuer SMTP Auth>

Update
---

etwas erweiterte Konfig in Verbindung mit z.B. Apache + PHP:

`/etc/ssmtp/ssmtp.conf`:

	root=postmaster
	FromLineOverride=YES
	rewriteDomain=<absender domain>

	# The place where the mail goes. The actual machine name is required
	# no MX records are consulted. Commonly mailhosts are named mail.domain.com
	# The example will fit if you are in domain.com and your mailhub is so named.
	mailhub=<adresse des Mailservers>
	AuthUser=<usernamen fuer SMTP Auth>
	AuthPass=<password fuer SMTP Auth>

`/etc/ssmtp/revaliases`:

	apache:<emailadress von absender domain>

