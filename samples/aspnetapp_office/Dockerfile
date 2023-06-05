FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
COPY aspnetapp/*.config ./aspnetapp/
RUN nuget restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /app/aspnetapp
RUN msbuild /p:Configuration=Release -r:False


FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8 AS runtime

# Download access db engine
ADD https://download.microsoft.com/download/3/5/C/35C84C36-661A-44E6-9324-8786B8DBE231/accessdatabaseengine_X64.exe c:/temp/accessdatabaseengine_X64.exe
# Install access db engine
RUN c:\temp\accessdatabaseengine_X64.exe /quiet /norestart
# Clean up installer
#RUN powershell.exe -Command Remove-Item c:\accessdatabaseengine_X64.exe -Force

# Install prereqs
#RUN powershell.exe -Command \
#    $ErrorActionPreference = 'Stop'; \
#    wget https://download.microsoft.com/download/3/5/C/35C84C36-661A-44E6-9324-8786B8DBE231/accessdatabaseengine_X64.exe  -OutFile c:/temp/accessdatabaseengine_X64.exe ; \
#    Start-Process c:\temp\accessdatabaseengine_X64.exe -ArgumentList '/quiet /norestart' -Wait ; \
#    Remove-Item c:\temp\accessdatabaseengine_X64.exe -Force

# Copy app folder to iis root
WORKDIR /inetpub/wwwroot
COPY --from=build /app/aspnetapp/. ./
