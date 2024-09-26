# Stage 1: Base image with the latest .NET runtime
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Stage 2: Build image with the latest .NET SDK
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["CoffeeService.csproj", "CoffeeService/"]
RUN dotnet restore "CoffeeService/CoffeeService.csproj"

WORKDIR "/src/CoffeeService"
COPY . .

# Build the application
RUN dotnet build "CoffeeService.csproj" -c Release -o /app/build

# Stage 3: Publish the application
FROM build AS publish
RUN dotnet publish "CoffeeService.csproj" -c Release -o /app/publish

# Stage 4: Final image to run the app
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Set the entrypoint to run the application
ENTRYPOINT ["dotnet", "CoffeeService.dll"]

