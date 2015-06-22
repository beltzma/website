<!-- 
.. title: Configure loopback device
.. slug: configure-loopback-device
.. date: 2014/02/05 13:44:21
.. tags: Centos, Admin
.. link: 
.. description: 
.. type: text
-->

Ein Tool ueber das ich beim letzten Servercrash gestolpert bin, ist `fallocate`

Immer wenn man temporaer ein Festplattenimage erstellen moechte habe ich bisher `dd` verwendet. Dauert jedoch bei mehreren GB recht lange. `fallocate` ist in diesem Fall die LSG

	[root@centos tmp]# fallocate -l 3G disk1.img

Dieses File ist sofort erstellt und kann dann entsprechend verwendet werden:

	[root@centos tmp]# losetup /dev/loop0 /tmp/disk1.img
	[root@centos tmp]# losetup --show /dev/loop0
	/dev/loop0: [fd00]:266586 (/tmp/disk1.img)

	