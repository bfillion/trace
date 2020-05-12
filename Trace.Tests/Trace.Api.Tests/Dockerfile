#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY Trace.Api.Tests/Trace.Api.Tests.csproj Trace.Api.Tests/
RUN dotnet restore "Trace.Api.Tests/Trace.Api.Tests.csproj"
COPY . .
WORKDIR "/src/Trace.Api.Tests"
RUN dotnet build "Trace.Api.Tests.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Trace.Api.Tests.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Trace.Api.Tests.dll"]
