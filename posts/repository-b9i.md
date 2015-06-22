<!-- 
.. title: Repository b9i
.. slug: repository-b9i
.. date: 2014/02/18 11:49:34
.. tags: Admin, Centos
.. link: 
.. description: 
.. type: text
-->

Mein eigenes Repo
-------------

In diesem sind verschiedene kleine Tools drin, die ich fuer Backup bzw Administration immer wieder brauche:
so ist das Repo auf dem Server einzurichten `/etc/yum.repo.d/b9i.repo`

	[b9i]
	name=own Repository
	baseurl=http://b9i.de/repo/Centos/6/$basearch
	enabled=1