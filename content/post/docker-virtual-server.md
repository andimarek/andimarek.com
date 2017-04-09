+++
date = "2014-10-16T10:50:35+02:00"
title = "'Docker on a virtual server' or 'A virtual server is not a real root server'"
draft = false

+++
For this blog I decided to setup a complete new server with [Docker](https://www.docker.com/) running my services (e.g. Ghost).

I was already customer at a German hoster and I decided to  rent a virtual Server there. (With Ubuntu 14.04 as OS)

I installed Docker and everything seemed fine, until I tried to start the docker daemon.

The daemon failed to start with the following error message: `init_networkdriver() [...] package not installed`.

After some searching I learned that my kernel didn't have the `bridge` kernel module. No problem just execute `sudo modprobe bridge`, but it failed again with `Module bridge not found`. And now it got interesting: 

Only then I learned, that a virtual server is not a real root server. I always thought that a virtual server maybe a bit slower than a root server (dedicated server) and RAM and other resources are shared with others, but that I had full rights to do with my system whatever I want. 
As it turned out, there a different virtualization solutions and my virtual server was a [OpenVZ](http://en.wikipedia.org/wiki/OpenVZ) virtual server. The kernel is shared between the different virtual servers and I'm not able to modify the kernel.

Then I tried a workaround described [here](http://slopjong.de/2014/09/03/install-docker-on-a-debian-based-vps/), but it didn't work for me. (The daemon started, but running a container resulted in a error message: `fork/exec /var/lib/docker/init/dockerinit-1.2.0: operation not permitted`.)

After some more research I found some hints, that [Xen](http://en.wikipedia.org/wiki/Xen) virtualization didn't suffer from the same kernel `bridge` problem. I decided to rent a server at [Linode](https://blog.linode.com/2008/03/28/linodes-in-xen/) and Docker is running smoothly out of the box without any extra modifications. (I didn't dig into the details why it works, I'm just happy it does. :-))

To sum it up: I failed to run Docker (1.2.0) on a OpenVZ server, but on Xen it worked and there is really a difference between a virtual server and a root/dedicated server.

P.S.: I used [Ansible](http://www.ansible.com/) to setup the server and the initial investment paid off directly after I switched ther servers. Highly recommended even
for small server installations. More infos about my Ansible setup including sources [here](https://andimarek.com/ghost-server-via-ansible/).





