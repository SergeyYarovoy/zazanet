FROM node:16.13.0 as UI
WORKDIR /ui/
COPY ./priv/ui/package.json ./priv/ui/package-lock.json ./
RUN npm install
COPY ./priv/ui/ ./
RUN npm run build

FROM erlang:24.1.3.0 AS PROD

WORKDIR /usr/src/app/

COPY rebar.config rebar.lock ./
RUN rebar3 compile

COPY ./config ./config/
COPY ./src ./src/
COPY --from=UI /ui/dist/ ./priv/ui/dist/
RUN rebar3 as prod release

ENTRYPOINT ["./_build/prod/rel/zazanet/bin/zazanet"]
