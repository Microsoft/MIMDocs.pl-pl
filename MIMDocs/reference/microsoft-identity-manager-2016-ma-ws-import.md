---
title: Importuj łącznik usług sieci Web | Microsoft Docs
description: Zaimportuj łącznik usług sieci Web z wieloma konfiguracjami usług sieci Web w Microsoft Identity Manager.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/19/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ac4ee4f7b6d9f1f70198b79e628737b64b4cb3a0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760546"
---
# <a name="import-web-services-connector"></a>Importuj łącznik usług sieci Web

Po zakończeniu operacji importu serwera importowania zawierającej agenta zarządzania usługami sieci Web (MA) z powodu błędu "nie można pobrać parametrów konfiguracji z tego rozszerzenia" wyeksportowane pliki konfiguracji muszą zostać sprawdzone.

Ten błąd jest spowodowany przez ograniczenie usługi synchronizacji. Usługa nie może zaimportować konfiguracji serwera za pomocą usług sieci Web MA i zwraca błąd "nie można pobrać parametrów konfiguracji z rozszerzenia". W niektórych przypadkach usługi sieci Web MA mogą zawierać nieprawidłowe ustawienia, które należy naprawić. 

## <a name="how-to-fix-the-issue"></a>Jak rozwiązać problem

1. Skopiuj pliki konfiguracji serwera na serwer docelowy. 

2. Na serwerze docelowym skopiuj skrypt <a href="#web-service-powershell-script">WebServiceMA_apply_before_import_server_configuration.ps1</a> PowerShell do folderu, który zawiera wyeksportowane pliki konfiguracji serwera.

3. Uruchom skrypt WebServiceMA_apply_before_import_server_configuration.ps1 PowerShell.

<h3 id="web-service-powershell-script">Skrypt programu PowerShell</h3>

```
# --------------------------------------------------------------------- 
# <copyright file="WebServiceMA_apply_before_import_server_configuration.ps1" company="Microsoft"> 
# Copyright (c). All rights reserved. 
# </copyright> 

# --------------------------------------------------------------------- 
# If the script doesn't work, check the ExecutionPolicy
# by using the following commands: 
# 
#    - Get-ExecutionPolicy 
#    - Set-ExecutionPolicy RemoteSigned 
#

function Get-MIMInstallationPath { 

<# 
  .SYNOPSIS 

  Get the installation path of MIM from the Windows registry.

  .DESCRIPTION 

  Get the installation path of MIM from the Windows registry: 

       HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\
       Forefront Identity Manager\2010\Synchronization Service\ 

  and the paramter Location. 

  .EXAMPLE 

  $path = Get-MIMInstallationPath 
#>   

  try 
  { 
    $MIMpath = Get-ItemProperty -Path "Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Synchronization Service\" 
  } 

  catch 
  { 
    $ErrorMessage = $_.Exception.Message 
    throw "Get-MIMInstallationPath exception: $ErrorMessage" 
  } 

  Write-Output "$($MIMpath.Location)Synchronization Service\Extensions\" 

} 

function Get-WsconfigFilesNamesFromExtensionsFolder([string] $path) { 

<# 
  .SYNOPSIS 

  Get the comma-separated list of *.wsconfig files from Extensions folder. 

  .DESCRIPTION 

  Get the comma-separated list of *.wsconfig files from Extensions folder. 

  .PARAMETER path 

  The path to the Extensions folder that contains the *.wsconfig files. 

  .EXAMPLE 

  $wsconfigFiles = Get-WsconfigFilesNamesFromExtensionsFolder("./") 
#>  

 if( [string]::IsNullOrEmpty($path) ) 

  { 
    throw "Get-WsconfigFilesNamesFromExtensionsFolder error: the input variable Path cannot be empty." 
  } 

  try 
  { 
    $files = Get-ChildItem $path -filter "*.wsconfig" -name | ForEach-Object {$_ -replace ".wsconfig", ""} 

    $wsconfigFiles=$files -join "," 
  } 

  catch 
  { 
    $ErrorMessage = $_.Exception.Message 

    Write-Host "Get-WsconfigFilesNamesFromExtensionsFolder exception: $ErrorMessage" 
    Write-Output "" 
  } 

  Write-Output $wsconfigFiles 

} 

function Set-AvailableWebServiceProjects([string] $path) { 

<# 

  .SYNOPSIS 

  Patch exported wsconfig files for Web Service project management agent. 


  .DESCRIPTION 

  The script tries to find the config files of Web Service management agents and tries to fix Web Service project configuration parameter. 

  .PARAMETER path 

  The path to the folder that contains the exported config files of management agents.

  .EXAMPLE 

  $count = Set-AvailableWebServiceProjects("./") 

#>   

  if( [string]::IsNullOrEmpty($path) ) 

  { 
    throw "Set-AvailableWebServiceProjects: path variable is empty" 
  } 

  try 
  { 
    $countPatchedFiles=0 

    $files = Get-ChildItem $path -filter "MA-{*}.xml" -name 

    foreach ($file in $files)  
    { 
      [xml]$xmlConfig = Get-Content $file 

      $connectorName = $xmlConfig."saved-ma-configuration"."ma-data"."ma-listname" 

      if($connectorName -ne "Web Service (Microsoft)" ) 
      { 
        continue 
      } 

      $parameters = $xmlConfig."saved-ma-configuration"."ma-data"."private-configuration"."MAConfig"."parameter-definitions" 
     $target = ($parameters."parameter"|where {$_.name -eq "Web Service project" -and $_.use -eq "connectivity" -and $_.type -eq "drop-down"})          

      if($target -eq $null) 
      { 
        continue 

      } 

     $extensionsFolderPath = Get-MIMInstallationPath 
     $wsconfigFiles = Get-WsconfigFilesNamesFromExtensionsFolder($extensionsFolderPath)

     if($wsconfigFiles -eq '') 
     { 
       continue 
     } 

     $target.validation = [string]$wsconfigFiles   
     $defaultConfig = $wsconfigFiles.Split("{,}") 
     $target."default-value" = $defaultConfig[0] 
     $fileFullPath = "$path$file" 
     Set-ItemProperty $fileFullPath -name IsReadOnly -value $false 
     $xmlConfig.Save($fileFullPath) 
     Set-ItemProperty $fileFullPath -name IsReadOnly -value $true 
     $countPatchedFiles++ 
   } 
  } 

  catch 
  { 
   $ErrorMessage = $_.Exception.Message 

   throw "Set-AvailableWebServiceProjects exception: $ErrorMessage" 
  } 
  Write-Output $countPatchedFiles 
} 

########################################## 
####              Main                #### 
########################################## 

cls 

try 
{ 
  $currentFolder = (Get-Location).path + "\" 

  $countPatchedFiles = Set-AvailableWebServiceProjects($currentFolder) 

  Write-Host "Count of patched Web Service config files:" $countPatchedFiles 
} 
catch 
{ 
  $ErrorMessage = $_.Exception.Message 
  Write-Host "The script was run with excetion: $ErrorMessage" 
} 

[void](Read-Host 'Press Enter to exit…') 
```

## <a name="next-steps"></a>Następne kroki

- [Instalowanie narzędzia konfiguracji usługi sieci Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Przewodnik wdrażania protokołu SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Przewodnik wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
