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
- [Jackett](https://github.com/Jackett/Jackett) to manage Torrent trackers. `http://localhost:9117`
- [Transmission](https://github.com/transmission/transmission) Torrent client. `http://localhost:9091`




