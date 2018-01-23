FROM microsoft/dotnet:2.0-sdk AS test
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY dotnetapp/*.csproj ./dotnetapp/
COPY botutils/*.csproj ./botutils/
COPY bottests/*.csproj ./bottests/
RUN dotnet restore

# copy and build everything else
COPY dotnetapp/. ./dotnetapp/
COPY botutils/. ./botutils/
COPY bottests/. ./bottests/

# run this stage for testing, with:
# docker build --target test -t app:test .
# docker run -ti -v C:\git\dotnet-docker-samples\dotnetapp-dev\TestResults:C:\app\bottests\TestResults app:test
# docker run -ti -v C:\git\dotnet-docker-samples\dotnetapp-dev\TestResults:/app/bottests/TestResults app:test
# After running, you will find test results on your local disk @ left-hand side of volume-mount syntax
# First `docker run` above is for Windows containers. Second is for Linux.
WORKDIR /app/bottests
ENTRYPOINT ["dotnet", "test","--logger:trx"]

# builds and tests application
FROM test AS build
WORKDIR /app/bottests
RUN dotnet test
WORKDIR /app/dotnetapp
RUN dotnet publish -o out

FROM microsoft/dotnet:2.0-runtime AS runtime
WORKDIR /app/
COPY --from=build /app/dotnetapp/out ./
ENTRYPOINT ["dotnet", "dotnetapp.dll"]
