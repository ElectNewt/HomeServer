# HomeServer
This repo contains the information of the application layer built on top of my personal `Home Server`.


My server is a windows server, as I recycled an old pc for it. For that reason, the `docker volumes` are pointing to the `D:` drive.

The only configuration you should change to make it work on your environment is the local route for example, `D:\Documents:/srv` change it for `C:\users\{user}\Documents:/srv` or the similar in Linux `/home/testuser/Documents:/srv`

On the server you can find:
## File system - Web Client
Personally, I don't want a network shared folder. I rather have access using the browser as my usage is uploading content once I publish it on youtube, and in most cases, I never use it again. 

As a client to access the files on the browser, I have [FileBrowser](https://github.com/filebrowser/filebrowser). The default username and password is `admin:admin`, and you can access it on: `http://localhost:20001`

Note: one of the volumes is a file `filebrowser.db`; remember to create that file manually before starting the compose, or it will be automatically created as a folder and filebrowser won't start.

## Server Monitoring
[netdata](https://github.com/netdata/netdata) is high-fidelity infrastructure monitoring and troubleshooting.
Open-source, free, preconfigured, opinionated, and always real-time.

you can access on: `http://localhost:19999`

## Media server
[Plex](https://github.com/plexinc/plex-media-player) is a global streaming media service and a client–server media player platform.

Which will allow me to stream content from the server into any Plex client (like the one in the Smart TV, Phones, etc).
You can access using the browser on `http://localhost:32400`


Along with `Plex`, a few other apps are included on the compose file:
- [Sonarr](https://github.com/Sonarr/Sonarr) to manage TvShows. `http://localhost:8989`
- [Radarr](https://github.com/Radarr/Radarr) to manage Movies. `http://localhost:7878`
- [Readarr](https://github.com/Readarr/Readarr) to manage books. `http://localhost:8787`
- [prowlarr](https://github.com/Prowlarr/Prowlarr) to manage Torrent trackers. `http://localhost:9696`
- [Transmission](https://github.com/transmission/transmission) Torrent client. `http://localhost:9091`

## Ebook-Manager
[Calibre](https://github.com/kovidgoyal/calibre) is an e-book manager. It can view, convert, edit and catalog e-books in all of the major e-book formats. Along with [calibre-web](https://github.com/janeczku/calibre-web) for a nicer experience (user `admin` password `admin123`)


It can be configured along with `Readarr` so synch the books automatically. 



## Reverse proxy 
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


