[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: https://github.com/jakewaldron/PlexEmail
[hub]: https://hub.docker.com/r/linuxserver/plexemail/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# linuxserver/plexemail
[![](https://images.microbadger.com/badges/version/linuxserver/plexemail.svg)](https://microbadger.com/images/linuxserver/plexemail "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/linuxserver/plexemail.svg)](http://microbadger.com/images/linuxserver/plexemail "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/plexemail.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/linuxserver/plexemail.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io/linuxserver-plexemail)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io/job/linuxserver-plexemail/)

A script that aggregates all new TV and movie releases for the past x days then writes to your web directory and sends out an email.

[![plexemail](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/plexemail-icon.png)][appurl]

## Usage

```
docker create --name=plexemail \
-v <path to config>:/config \
-v <path to "Plex Media Server" folder>:/plex \
-e PGID=<gid> \
-e PUID=<uid>  \
-e TZ=<timezone> \
-p 8080:8080 \
linuxserver/plexemail
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 8080` - the port(s)
* `-v /config` - PlexEmail config folder
* `-v /plex` - "Plex Media Server" folder from Plex server
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` for timezone information, eg Europe/London

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it plexemail /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 

Edit the config.conf in the /config folder.

Update the /config/cron/plexemail as necessary, restart the container for any changes to take effect.

See [PlexEmail][appurl] for more information on configuration.

## Info

* To monitor the logs of the container in realtime `docker logs -f plexemail`.

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' plexemail`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/plexemail`

## Versions

+ **11.11.16:** Make web files persistent
+ **14.10.16:** Add version layer information.
+ **10.09.16:** Add layer badges to README.
+ **29.08.16:** Fix every 15 minutes cron.
+ **28.08.16:** Add badges to README.
+ **20.08.16:** Rebase to alpine
+ **10.11.15:** Initial Release 
