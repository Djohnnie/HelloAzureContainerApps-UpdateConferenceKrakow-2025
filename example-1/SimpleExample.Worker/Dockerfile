﻿FROM mcr.microsoft.com/dotnet/runtime:10.0-preview AS base
USER app
WORKDIR /app
ENV STATUS_ENDPOINT_URI=http://localhost:5113/status

FROM mcr.microsoft.com/dotnet/sdk:10.0-preview AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["SimpleExample.Worker.csproj", ""]
RUN dotnet restore "./SimpleExample.Worker.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "./SimpleExample.Worker.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./SimpleExample.Worker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SimpleExample.Worker.dll"]