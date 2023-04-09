# Download Steam client
FROM cm2network/steamcmd:latest AS steam

# ------
# Build app
FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build
ARG TARGETARCH

RUN apk add --update --no-cache bash

SHELL ["/bin/bash", "-c"]

RUN echo linux-${TARGETARCH/amd/x} > ~/.RuntimeIdentifier

WORKDIR /app

COPY . ./

RUN cd AssettoServer && dotnet add package NLua

WORKDIR /app
RUN dotnet publish -r linux-x64 -c Release --no-self-contained

# -----
# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0

WORKDIR /app

COPY --from=steam /home/steam/steamcmd/linux64/* /root/.steam/sdk64/
COPY --from=build /app/out-linux-* .

WORKDIR /data

ENTRYPOINT ["/app/AssettoServer", "--plugins-from-workdir"]
