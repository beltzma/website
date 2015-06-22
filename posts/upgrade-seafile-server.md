<!-- 
.. link: 
.. description: 
.. tags: seafile, Centos, Admin
.. date: 2013/11/08 10:40:07
.. title: Upgrade Seafile Server
.. slug: upgrade-seafile-server
-->

Ein "Minor" Update ist herausgekommen (2.0.1 -> 2.0.3), der vor allem das Handling bzw den Sync vieler kleiner Dateien verbessern soll.

Worauf muss man achten:

* Benutzerrechte
* Update des Startscriptes
* Einpflegen der Aenderungen

<!-- TEASER_END -->

Benutzerrechte
--------------

Bei mir laeuft Seafile als unpriviligierter User, da ich mich als solcher nicht am Server anmelden kann. Wird die neue Version als "root" User entpackt! Leider kann aber so der Server vom init Script nicht gestartet werden:

`chown -R seafile seafile-server-2.0.3`

Startscript <a name="seafile-script"></a>
-----------

Ich habe folgendes Start/init- Script geschrieben: 

    ### BEGIN INIT INFO
    # Provides: seafile-server
    # Required-Start: $network $local_fs $remote_fs $mysqld
    # Should-Start: $syslog
    # Short-Description: start the seafile-server
    # Description: start the seafile-server with fastcgi
    ### END INIT INFO

    # Source function library.
    . /etc/init.d/functions

    # Source networking configuration.
    . /etc/sysconfig/network

    user=seafile
    script_path=/srv/seafile/b9i/seafile-server-2.0.3

    start() {
            sudo -u ${user} ${script_path}/seafile.sh start > /tmp/seafile.init.log 2>&1
            sudo -u ${user} ${script_path}/seahub.sh start-fastcgi > /tmp/seahub.init.log 2>&1

            return $RETVAL
    }


    stop() {
            sudo -u ${user} ${script_path}/seafile.sh stop
            sudo -u ${user} ${script_path}/seahub.sh stop
    }

    # See how we were called.
    case "$1" in
      start)
            start
            ;;
      stop)
            stop
            ;;
      restart|force-reload)
            stop
            start
            ;;
      *)
            echo $"Usage: $0 {start|stop|restart|force-reload}"
            exit 2
    esac


Wichtig ist das der Wert in der Variablen `${script_path} ` angepasst wird.

Aenderungen einpflegen
--------

Alles was im alten System geeaendert wurde oder hinzugefuegt muss wieder ins neue mit rein, speziell "logos" usw im Verzeichnis "media"

