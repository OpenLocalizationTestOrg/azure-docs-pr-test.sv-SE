---
title: aaaSilent installation av Azure Backup Server v2 | Microsoft Docs
description: "Använd en PowerShell-skriptet toosilently installera Azure Backup Server v2. Den här typen av installation kallas även en obevakad installation."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/30/2017
ms.author: markgal;masaran
ms.openlocfilehash: 6b94b4a278bfcd5f8c5c363cb811bd8eec984243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Kör en obevakad installation av Azure Backup Server v2

Lär dig hur toorun en obevakad installation av Azure Backup Server v2. 

De här stegen gäller inte om du installerar Azure Backup Server v1.

## <a name="install-backup-server-v2"></a>Installera reservserver v2

1. Skapa en textfil på hello-server som är värd för Azure Backup Server v2. (Du kan skapa hello-fil i anteckningar eller i en annan text editor.) Spara filen hello som MABSSetup.ini. 

2. Klistra in följande kod i hello MABSSetup.ini fil hello. Ersätt hello texten i hello hakparenteser (\< \>) med värden från din miljö. hello efter texten är ett exempel:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Spara hello-filen. Vid en upphöjd kommandotolk på installationsservern hello skriver du följande kommando:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Du kan använda dessa flaggor för hello installation:</br>
**/f**: ini-filsökväg</br>
**/l**: loggsökvägen</br>
**/i**: installationssökväg</br>
**/x**: avinstallera sökväg</br>

## <a name="next-steps"></a>Nästa steg
När du har installerat Backup Server lär du dig hur tooprepare servern, eller börja skydda en arbetsbelastning.

- [Förbereda säkerhetskopiering serverarbetsbelastningar](backup-azure-microsoft-azure-backup.md)
- [Använda Backup Server tooback in en VMware-server](backup-azure-backup-server-vmware.md)
- [Använda Backup Server tooback in SQL Server](backup-azure-sql-mabs.md)
- [Lägg till tooBackup moderna Backup Storage Server](backup-mabs-add-storage.md)
