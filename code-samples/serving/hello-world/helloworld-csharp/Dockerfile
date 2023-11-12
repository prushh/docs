# Use Microsoft's official build .NET image.
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0

ARG USER=knative
ARG USER_UID=1001
ARG USER_GID=$USER_UID

# Create and change to the app directory.
WORKDIR "/home/${USER}/app"

# Creates a non-root user to be used exclusively to run the application.
RUN groupadd -g $USER_GID $USER && \
    useradd -u $USER_UID -g $USER_GID -m $USER

COPY --from=build-env /app/out .

# Set the non-root user as current.
USER $USER

# Run the web service on container startup.
ENTRYPOINT ["dotnet", "helloworld-csharp.dll"]
