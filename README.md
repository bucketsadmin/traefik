# traefik

Repo for default Traefik docker compose.

## Default
Default compose file is located in ```/default``` and has the insecure backend flag set

Wildcard certificate generated for containers not using the tls flag

### Active Middlewares

Unique middlewares only need to be defined by a singular label to be used by any connected container.
The default MWs are currently applied to the Traefik dashboard.

- compression
- XFF
- headers



      ### enable compression
      - traefik.http.middlewares.mw_compress.compress=true
      ### send xff
      - traefik.http.middlewares.mw_xff.headers.customrequestheaders.X-Forwarded-Proto=https
      ### standard headers
      - traefik.http.middlewares.mw_headers.headers.browserXssFilter=true
      - traefik.http.middlewares.mw_headers.headers.forceSTSHeader=true
      - traefik.http.middlewares.mw_headers.headers.frameDeny=true
      - traefik.http.middlewares.mw_headers.headers.referrerPolicy=no-referrer-when-downgrade
      - traefik.http.middlewares.mw_headers.headers.stsSeconds=31536000
      ### headers with frameDeny disabled
      - traefik.http.middlewares.mw_headers_frame.headers.browserXssFilter=true
      - traefik.http.middlewares.mw_headers_frame.headers.forceSTSHeader=true
      - traefik.http.middlewares.mw_headers_frame.headers.frameDeny=false
      - traefik.http.middlewares.mw_headers_frame.headers.referrerPolicy=no-referrer-when-downgrade
      - traefik.http.middlewares.mw_headers_frame.headers.stsSeconds=31536000


## basic app label template

    labels
      - traefik.enable=true
      - traefik.docker.network=nginx_net
      - traefik.http.routers.rtr-app.rule=Host(`${SUB}.${DOMAIN}`)
      - traefik.http.routers.rtr-app.service=srv-app
      - traefik.http.routers.rtr-app.tls.certResolver=letsencrypt
      - traefik.http.routers.rtr-app.entrypoints=websecure
      - traefik.http.services.srv-app.loadbalancer.server.port=3099
      - traefik.http.services.srv-app.loadbalancer.passhostheader=true
      - traefik.http.routers.rtr-app.middlewares=mw_compress,mw_headers,mw_xff


### Per-domain certificate

To activate a unique certificate for each individual domain add the following line:


     - traefik.http.routers.rtr-app.tls=true