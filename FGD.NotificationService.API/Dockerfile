#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see http://aka.ms/containercompat 

FROM microsoft/dotnet:2.2-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.2-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY FGD.NotificationService.API/FGD.NotificationService.API.csproj FGD.NotificationService.API/
COPY FGD.Api.Model/FGD.Api.Model.csproj FGD.Api.Model/
COPY FGD.SharedTypes/FGD.SharedTypes.csproj FGD.SharedTypes/
RUN dotnet restore FGD.NotificationService.API/FGD.NotificationService.API.csproj
COPY . .
WORKDIR /src/FGD.NotificationService.API
RUN dotnet build FGD.NotificationService.API.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish FGD.NotificationService.API.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "FGD.NotificationService.API.dll"]
