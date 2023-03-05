#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

ENV HrmInterviewDb="Data Source=DESKTOP-CV88RRF,1433;Initial Catalog=HrmInterviewDb;User ID=sa;Password=SGF86pop;TrustServerCertificate=True"

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Hrm.Interview.APILayer/Hrm.Interview.APILayer.csproj", "Hrm.Interview.APILayer/"]
COPY ["Hrm.Interview.ApplicationCore/Hrm.Interview.ApplicationCore.csproj", "Hrm.Interview.ApplicationCore/"]
COPY ["Hrm.Interview.Infrastructure/Hrm.Interview.Infrastructure.csproj", "Hrm.Interview.Infrastructure/"]
RUN dotnet restore "Hrm.Interview.APILayer/Hrm.Interview.APILayer.csproj"
COPY . .
WORKDIR "/src/Hrm.Interview.APILayer"
RUN dotnet build "Hrm.Interview.APILayer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Hrm.Interview.APILayer.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Hrm.Interview.APILayer.dll"]