FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MarketPlaceApi/MarketPlaceApi.csproj", "MarketPlaceApi/"]
RUN dotnet restore "./MarketPlaceApi/MarketPlaceApi.csproj"
COPY . .
WORKDIR "/src/MarketPlaceApi"
RUN dotnet publish "./MarketPlaceApi.csproj" -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
EXPOSE 8080
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "MarketPlaceApi.dll"]