#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/CICDApi/CICDApi.csproj", "src/CICDApi/"]
RUN dotnet restore "src/CICDApi/CICDApi.csproj"
COPY . .
WORKDIR "/src/src/CICDApi"
RUN dotnet build "CICDApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CICDApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CICDApi.dll"]