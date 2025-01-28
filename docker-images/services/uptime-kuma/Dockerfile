FROM digidanieltech/alpine:3.21.2

RUN apk add --no-cache git npm

RUN git clone --depth 1 -b 1.23.16 https://github.com/louislam/uptime-kuma.git /build

RUN npm install pm2 -g && pm2 install pm2-logrotate

RUN cd /build \
  && npm run setup \
  && cd / \
  && mv /build /opt/uptime-kuma
 
EXPOSE 3001

# CMD ["pm2", "start", "/opt/uptime-kuma/server/server.js", "--name", "uptime-kuma"]
