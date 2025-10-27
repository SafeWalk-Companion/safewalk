FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["SafeWalk.Platform/SafeWalk.Platform.csproj", "SafeWalk.Platform/"]
RUN dotnet restore "SafeWalk.Platform/SafeWalk.Platform.csproj"
COPY . .
WORKDIR "/src/SafeWalk.Platform"
RUN dotnet build "./SafeWalk.Platform.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./SafeWalk.Platform.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SafeWalk.Platform.dll"]
