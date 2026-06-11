# ---------- BUILD STAGE ----------
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# copy solution and project files (cache friendly)
# build context is repository root, so these relative paths exist
COPY ./*.sln ./
COPY ./Logic/*.csproj Logic/
COPY ./Models/*.csproj Models/
COPY ./Repository/*.csproj Repository/
COPY ./ovipraktarkezeloseged/*.csproj ovipraktarkezeloseged/

RUN dotnet restore

# copy remaining sources
COPY . .

# publish the specific project
RUN dotnet publish ovipraktarkezeloseged/ovipraktarkezeloseged.csproj -c Release -o /app/publish

# ---------- RUNTIME STAGE ----------
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app

COPY --from=build /app/publish .

# fontos Dockerhez
ENV ASPNETCORE_URLS=http://+:8080

EXPOSE 8080

# ⚠️ EZT NE FELEJTSD EL ÁTÍRNI A SAJÁT DLL NEVRE
ENTRYPOINT ["dotnet", "ovipraktarkezeloseged.dll"]