FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["LunarStorm.Server/LunarStorm.Server.csproj", "LunarStorm.Server/"]
RUN dotnet restore "LunarStorm.Server/LunarStorm.Server.csproj"
COPY . .
WORKDIR "/src/LunarStorm.Server"
RUN dotnet build "./LunarStorm.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./LunarStorm.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "LunarStorm.Server.dll"]
