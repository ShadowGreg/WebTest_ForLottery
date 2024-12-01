FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebTest_ForLottery/WebTest_ForLottery.csproj", "WebTest_ForLottery/"]
RUN dotnet restore "WebTest_ForLottery/WebTest_ForLottery.csproj"
COPY . .
WORKDIR "/src/WebTest_ForLottery"
RUN dotnet build "WebTest_ForLottery.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "WebTest_ForLottery.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebTest_ForLottery.dll"]
