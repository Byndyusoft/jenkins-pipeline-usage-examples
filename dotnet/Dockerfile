ARG DOTNET_RUNTIME_IMAGE=mcr.microsoft.com/dotnet/aspnet:8.0

FROM $DOTNET_RUNTIME_IMAGE as runtime

ARG APP_DLL
ENV APP_DLL=$APP_DLL
ENV ASPNETCORE_URLS http://*:5000
ENV ASPNETCORE_ENVIRONMENT="Release"

RUN apt-get update && \
    apt-get install -y \
    curl && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY out ./

COPY migrator /migrator

EXPOSE 5000

ENTRYPOINT dotnet $APP_DLL
