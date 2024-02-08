#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Ghyro.GoalService.Host/Ghyro.GoalService.Host.csproj", "Ghyro.GoalService.Host/"]
RUN dotnet restore "./Ghyro.GoalService.Host/./Ghyro.GoalService.Host.csproj"
COPY . .
WORKDIR "/src/Ghyro.GoalService.Host"
RUN dotnet build "./Ghyro.GoalService.Host.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Ghyro.GoalService.Host.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Ghyro.GoalService.Host.dll"]