# escape=`

FROM theroks/azure-devops-windows-buildagent:base-latest

WORKDIR /azp

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

# Download the Visual Studio Build Tools bootstrapper.
ADD https://aka.ms/vs/16/release/vs_enterprise.exe C:\Temp\vs_enterprise.exe

# For help on command-line syntax:
# https://docs.microsoft.com/en-us/visualstudio/install/use-command-line-parameters-to-install-visual-studio
RUN C:\Temp\vs_enterprise.exe --in customInstall.json `
  || IF "%ERRORLEVEL%"=="3010" EXIT 0

RUN "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\Enterprise\\Common7\\IDE\\StorePID.exe" AAAAA-BBBBB-CCCCC-DDDDDD-EEEEEE 09260


# Install Python and Git.
RUN powershell.exe -ExecutionPolicy RemoteSigned `
  iex (new-object net.webclient).downloadstring('https://get.scoop.sh'); `
  scoop install python git

# Start developer command prompt with any other commands specified.
ENTRYPOINT C:\BuildTools\Common7\Tools\VsDevCmd.bat &&

# Default to PowerShell if no other command specified.
CMD ["powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]