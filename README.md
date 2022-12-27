# HomeServer
This repo contains the information of the application layer built on top of my personal `Home Server`.

My server is a [Synology 420+](https://amzn.to/3vgVTok), for that reason the docker volumes are pointing to `/volumes1`.

The only configuration you should change to make it work on your environment is the local route for example, `/volumes1/Documents:/srv` change it for `C:\users\{user}\Documents:/srv` or the similar in Linux `/home/testuser/Documents:/srv`



On the server you can find:

## Portainer
For the administration of the containers I use Portainer, because it is easier for me to manage the containers from there rather than manually modifiying the server using ssh.
- [Guide how to use portainer, in Spanish](https://www.netmentor.es/entrada/introduccion-portainer-contenedores)

I have installed it natively in the synology Nas, but you can find the configuration in the `docker-compose.old.yaml` file.


## File system - Web Client - Removed from now
Originally I used a web client, but I find it easier to access using the explorer with `\\{server-name}`

As I have a synology device I do also use synology drive.


## Media server
the content in the `docker-compose.multimedia.yaml` file 

[Plex](https://www.netmentor.es/entrada/servidor-multimedia-casero) is a global streaming media service and a client–server media player platform.

Which will allow me to stream content from the server into any Plex client (like the one in the Smart TV, Phones, etc).
You can access using the browser on `http://localhost:32400`; I have it installed it natively on the synology nas.


Along with `Plex`, a few other apps are included on the compose file:
- [Sonarr](https://www.netmentor.es/entrada/sonarr-biblioteca-series) to manage TvShows. `http://localhost:8989`
- [Radarr](https://www.netmentor.es/entrada/radarr-biblioteca-peliculas) to manage Movies. `http://localhost:7878`
- [prowlarr](https://www.netmentor.es/entrada/introduccion-prowlarr) to manage Torrent trackers. `http://localhost:9696`
- [Transmission](https://www.netmentor.es/entrada/cliente-bittorrent-transmission) Torrent client. `http://localhost:9091`
- [Overseer](https://www.netmentor.es/entrada/overseerr-biblioteca-contenido) Content manager for media library. `http://localhost:5055`
- [Bazaar](https://www.netmentor.es/entrada/bazar-biblioteca-administrar-subtitulos) subtitle manager. `http://localhost:6767`
- [Lidarr](https://www.netmentor.es/entrada/biblioteca-musica-lidarr) to manage music. `http://localhost:8686`

## Dashboard
file `docker-compose.crossstack.yaml`

Right now I am using heimdall but in [this post](https://www.netmentor.es/entrada/dashboard-servidor-casero) there are explanations for 3 of them.

## Ebook-Manager
file `docker-compose.books.yaml`.

- [Readarr](https://github.com/Readarr/Readarr) to manage books. `http://localhost:8787`
- [Calibre](https://github.com/kovidgoyal/calibre) is an e-book manager. It can view, convert, edit and catalog e-books in all of the major e-book formats. Along with [calibre-web](https://github.com/janeczku/calibre-web) for a nicer experience (user `admin` password `admin123`)
note: `calibre` and `calibre-web` are in the legacy file as I had no time to migrate it yet. They can be configured along with `Readarr` so synch the books automatically.




## Router level applications 
File `docker-compose.router.yaml`; Note: this apps are configured on my test environment, but i do not use them in the Synology nas yet, some configuration might be wrong or incomplete.

I added them into a `router` file because from an arquitectural point of view they make sense at an entry point level.

- Pihole
- tailscale, to safe connect to my server using a VPN from outside my own home. Note: i have this one installed natively in the Synology.
- Nginx reverse proxy.

### Reverse proxy
To have an easier life and avoid remembering the ports, I configured an [NGINX](https://github.com/nginx/nginx) reverse proxy.

All this information is configured under the file `default.conf` on the repo is located under `config/nginx/default.conf`, but you can locate it anywhere you want. Just remember to point the volume correctly:
```
volumes:
      - D:\Server-config\nginx\default.conf:/etc/nginx/conf.d/default.conf:ro
```

This will allow to use differnt endpoints for different purposes:
- `netdata.server.home` -> netdata
- `files.server.home` -> filebrowser
- `shows.server.home` -> sonarr
- `movies.server.home` -> radar
- `prowlarr.server.home` -> prowlarr
- `books.server.home` -> readarr
- `torrent.server.home` -> transmission
- `plex.server.home` -> plex
- `calibre.server.home` -> calibre
- `calibreweb.server.home` -> calibre

Note: for this to work you have to modfiy your `hosts` file **in EACH MACHINE you want to use them** with `{ip} subdomain.domain` like for example (replace `127.0.0.1` for your server IP):
```
127.0.0.1 server.home
127.0.0.1 files.server.home
127.0.0.1 movies.server.home
127.0.0.1 tv.server.home
127.0.0.1 prowlarr.server.home
127.0.0.1 books.server.home
127.0.0.1 torrent.server.home
127.0.0.1 netdata.server.home
127.0.0.1 plex.server.home
127.0.0.1 calibre.server.home
127.0.0.1 calibreweb.server.home
```

Note: you can configure piHole to deal with the DNS configuration inside your local network, instead of modifying the `hosts` file in each machine.
