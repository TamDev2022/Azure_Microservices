#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Services/Catalog/WebAPI.Catalog/WebAPI.Catalog.csproj", "src/Services/Catalog/WebAPI.Catalog/"]
RUN dotnet restore "src/Services/Catalog/WebAPI.Catalog/WebAPI.Catalog.csproj"
COPY . .
WORKDIR "/src/src/Services/Catalog/WebAPI.Catalog"
RUN dotnet build "WebAPI.Catalog.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebAPI.Catalog.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAPI.Catalog.dll"]