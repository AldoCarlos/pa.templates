#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG NugetAzureArtifactsUrl
ARG NugetAzureArtifactsPatToken
WORKDIR /src
COPY ["Empresa.Apm.Mvc/Empresa.Apm.Mvc.csproj", "Empresa.Apm.Mvc/"]

RUN dotnet new nugetconfig
RUN dotnet nuget add source $NugetAzureArtifactsUrl --name Empresa --username foobar --password $NugetAzureArtifactsPatToken --store-password-in-clear-text --configfile nuget.config --valid-authentication-types basic
RUN dotnet restore "Empresa.Apm.Mvc/Empresa.Apm.Mvc.csproj"

COPY . .
WORKDIR "/src/Empresa.Apm.Mvc"
RUN dotnet build "Empresa.Apm.Mvc.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Empresa.Apm.Mvc.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Empresa.Apm.Mvc.dll"]