# escape=`

FROM theroks/azure-devops-windows-buildagent:base-latest

WORKDIR /azp

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

# Download the Build Tools bootstrapper.
ADD https://aka.ms/vs/16/release/vs_buildtools.exe C:\TEMP\vs_buildtools.exe

# Install Build Tools excluding workloads and components with known issues.
RUN C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache `
    --installPath C:\BuildTools `
    --all `
    --remove Microsoft.VisualStudio.Component.Windows10SDK.10240 `
    --remove Microsoft.VisualStudio.Component.Windows10SDK.10586 `
    --remove Microsoft.VisualStudio.Component.Windows10SDK.14393 `
    --remove Microsoft.VisualStudio.Component.Windows81SDK `
 || IF "%ERRORLEVEL%"=="3010" EXIT 0


SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Install WebDeploy and NuGet with Chocolatey
RUN Install-PackageProvider -Name chocolatey -RequiredVersion 2.8.5.130 -Force; `
    Install-Package -Name nodejs.install -RequiredVersion 11.6.0 -Force; `
    Install-Package -Name webdeploy -RequiredVersion 3.6.0 -Force; `
    Install-Package nuget.commandline -RequiredVersion 4.9.2 -Force; 

# Install .NET Core SDK
ENV DOTNET_SDK_VERSION 2.2.100

RUN Invoke-WebRequest -OutFile dotnet.zip https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$Env:DOTNET_SDK_VERSION/dotnet-sdk-$Env:DOTNET_SDK_VERSION-win-x64.zip; `
    $dotnet_sha512 = '87776c7124cd25b487b14b3d42c784ee31a424c7c8191ed55810294423f3e59ebf799660864862fc1dbd6e6c8d68bd529399426811846e408d8b2fee4ab04fe5'; `
    if ((Get-FileHash dotnet.zip -Algorithm sha512).Hash -ne $dotnet_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive dotnet.zip -force -DestinationPath $Env:ProgramFiles\dotnet; `
    Remove-Item -Force dotnet.zip

RUN setx /M PATH $($Env:PATH + ';' + $Env:ProgramFiles + '\dotnet')