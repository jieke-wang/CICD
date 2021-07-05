#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
ADD sources.list /etc/apt/
RUN apt-get update -y && apt-get install -y libgdiplus && apt-get clean && ln -s /usr/lib/libgdiplus.so /usr/lib/gdiplus.dll
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["ImageWebApi/ImageWebApi.csproj", "ImageWebApi/"]
RUN dotnet restore "ImageWebApi/ImageWebApi.csproj"
COPY . .
WORKDIR "/src/ImageWebApi"
RUN dotnet build "ImageWebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ImageWebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ImageWebApi.dll"]