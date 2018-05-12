FROM node:stretch AS build
WORKDIR /build
COPY ./package.json ./package.json
COPY ./package-lock.json ./package-lock.json
RUN npm install

FROM node:stretch
WORKDIR /www
COPY . .
COPY --from=build /build/node_modules ./node_modules
CMD [ "npm", "start" ]