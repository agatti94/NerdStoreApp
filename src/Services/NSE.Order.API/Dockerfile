#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Services/NSE.Order.API/NSE.Order.API.csproj", "src/Services/NSE.Order.API/"]
RUN dotnet restore "src/Services/NSE.Order.API/NSE.Order.API.csproj"
COPY . .
WORKDIR "/src/src/Services/NSE.Order.API"
RUN dotnet build "NSE.Order.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NSE.Order.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NSE.Order.API.dll"]