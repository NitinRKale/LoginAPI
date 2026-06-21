# Runtime Image
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 5000
EXPOSE 8081

# Build Image
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build

ARG BUILD_CONFIGURATION=Release

WORKDIR /src

COPY ["Login.API.csproj", "./"]

RUN dotnet restore "Login.API.csproj"

COPY . .

RUN dotnet build "Login.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish
FROM build AS publish

ARG BUILD_CONFIGURATION=Release

RUN dotnet publish "Login.API.csproj" \
    -c $BUILD_CONFIGURATION \
    -o /app/publish \
    /p:UseAppHost=false

# Final Image
FROM base AS final

WORKDIR /app

COPY --from=publish /app/publish .

ENTRYPOINT ["dotnet","Login.API.dll"]