FROM mcr.microsoft.com/dotnet/aspnet:10.0-preview AS base
USER app
WORKDIR /app
EXPOSE 8080
ENV VERSION=A

FROM mcr.microsoft.com/dotnet/sdk:10.0-preview AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DaprExample.Api.csproj", ""]
RUN dotnet restore "./DaprExample.Api.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "./DaprExample.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./DaprExample.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DaprExample.Api.dll"]