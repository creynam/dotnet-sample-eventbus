FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS publish

COPY SampleEventBus.sln src/
COPY BuildingBlocks/EventBus/EventBus/EventBus.csproj src/BuildingBlocks/EventBus/EventBus/
COPY BuildingBlocks/EventBus/EventBus.Tests/EventBus.Tests.csproj src/BuildingBlocks/EventBus/EventBus.Tests/
COPY BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.csproj src/BuildingBlocks/EventBus/EventBusRabbitMQ/
COPY BuildingBlocks/EventBus/EventBusServiceBus/EventBusServiceBus.csproj src/BuildingBlocks/EventBus/EventBusServiceBus/
COPY BuildingBlocks/EventBus/IntegrationEventLogEF/IntegrationEventLogEF.csproj src/BuildingBlocks/EventBus/IntegrationEventLogEF/
COPY BuildingBlocks/WebHostCustomization/WebHost.Customization/WebHost.Customization.csproj src/BuildingBlocks/WebHostCustomization/WebHost.Customization/
COPY SampleEB/SampleEB.API/SampleEB.API.csproj src/SampleEB/SampleEB.API/
COPY SampleEB/SampleEB.Domain/SampleEB.Domain.csproj src/SampleEB/SampleEB.Domain/
COPY SampleEB/SampleEB.Persistence/SampleEB.Persistence.csproj src/SampleEB/SampleEB.Persistence/
COPY SampleEB/SampleEB.Tasks/SampleEB.Tasks.csproj src/SampleEB/SampleEB.Tasks/
COPY SampleEB/SampleEB.Tests/SampleEB.Tests.csproj src/SampleEB/SampleEB.Tests/

WORKDIR /src
RUN dotnet restore SampleEventBus.sln

COPY . .
WORKDIR /src/SampleEB/SampleEB.API/
RUN dotnet publish --no-restore -c Release -o /publish

FROM base AS final
WORKDIR /app
COPY --from=publish /publish /app

ENTRYPOINT ["dotnet", "SampleEB.API.dll"]