<!-- 
.. link: 
.. description: 
.. tags: Centos, Admin, SMTP, SMTP Auth
.. date: 2014/01/10 12:37:50
.. title: Postfix mit SMTP Authentication
.. slug: postfix-mit-smtp-authentication
-->

Nehmen wir an der Postfix eines CentOS Linux Servers soll alle Emails via einen speziellen Host (smarthost.firma.zz) per SMTP mit Authentication (Username: senduser mit dem Passwort supersecret) über den Port 587 verschicken.

In /etc/postfix/main.cf fügen wir folgende Zeile hinzu:

    relayhost = smarthost.firma.zz:587
    smtp_sasl_auth_enable = yes
    smtp_sasl_password_maps = hash:/etc/postfix/smtp_auth
    smtp_sasl_security_options = noanonymous

Dann erstellen wir die Datein /etc/postfix/smtp_auth mit folgenden Angaben:

    smarthost.firma.zz       senduser:supersecret

Aus der erstellen Datei müssen wir noch eine “lookup table” erstellen. Dies geht ganz einfach mit

    postmap /etc/postfix/smtp_auth

Die Datei /etc/postfix/smtp_auth.db wird dadurch erstellt. Wenn wir in Zukunft änderungen an der Datei smtp_auth machen, müssen wir auch den entsprechenden postmap Befehl ausführen. Ansonsten bekommt Postfix die Änderungen nicht mit.

Nun Postfix mit

    /etc/init.d/postfix reload
die neue Konfiguration laden lassen und das wars auch schon.

