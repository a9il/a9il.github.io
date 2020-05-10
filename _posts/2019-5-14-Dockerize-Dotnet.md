---
layout: post
title: Dockerize .NET
comments: true
---

Convert dotnet docker container to volume mount docker container dotnet runtime.  
Default dockerfile for dotnet app:

```Dockerfile
FROM microsoft/dotnet:sdk AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM microsoft/dotnet:aspnetcore-runtime
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "app.dll"]
```
Build a docker image with:

```
docker build -t my_dotnet_app .
```
But you need to build an image and rerun the container with the new image every time the app is updated. Let's convert it to build image for dotnet runtime only.  
Dockerfile:
```Dockerfile
# Build runtime image
FROM microsoft/dotnet:aspnetcore-runtime
WORKDIR /app/
```
Build a docker image with:
```
docker build -t dotnet_runtime .
```
Run dotnet app A and dotnet app B with the same image:  
Run dotnet app A:
```
docker run -v c:/path/to/published/dotnet/app/a/:/app/ -p 192.168.100.4:90:80 -d --entrypoint="dotnet" dotnet_runtime app_a.dll
```
Run dotnet app B:
```
docker run -v c:/path/to/published/dotnet/app/b/:/app/ -p 192.168.100.4:91:80 -d --entrypoint="dotnet" dotnet_runtime app_b.dll
```