FROM public.ecr.aws/lambda/dotnet:6 AS base

FROM mcr.microsoft.com/dotnet/sdk:6.0.202 as build
WORKDIR /src
COPY ["Lambda.csproj", "Lambda/"]
RUN dotnet restore "Lambda/Lambda.csproj"

WORKDIR "/src/Lambda"
COPY . .
RUN dotnet build "Lambda.csproj" --configuration Release --output /app/build

FROM build AS publish
RUN dotnet publish "Lambda.csproj" \
            --configuration Release \ 
            --runtime linux-x64 \
            --self-contained false \ 
            --output /app/publish \
            -p:PublishReadyToRun=true  

FROM base AS final
WORKDIR /var/task
COPY --from=publish /app/publish .
