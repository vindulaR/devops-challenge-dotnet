#Build application with .net version 5.0
FROM mcr.microsoft.com/dotnet/sdk:5.0 as build-env
WORKDIR /src

COPY . .
RUN dotnet restore

RUN dotnet publish -c Release -o /publish

#Runtime Image with .net version 5.0
FROM mcr.microsoft.com/dotnet/aspnet:5.0 as runtime
WORKDIR /server
COPY --from=build-env /publish .
EXPOSE 80
ENTRYPOINT ["dotnet", "DevOpsChallenge.SalesApi.dll"]
