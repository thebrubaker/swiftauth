FROM node:stretch AS build
WORKDIR /dist
COPY . .
RUN ['npm', 'install']
RUN ['npm', 'build']

FROM node:stretch
WORKDIR /www/public
COPY --from=build /dist .