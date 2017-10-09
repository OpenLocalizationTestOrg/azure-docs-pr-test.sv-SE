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
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a><span data-ttu-id="f5b19-104">Kör en obevakad installation av Azure Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="f5b19-104">Run an unattended installation of Azure Backup Server v2</span></span>

<span data-ttu-id="f5b19-105">Lär dig hur toorun en obevakad installation av Azure Backup Server v2.</span><span class="sxs-lookup"><span data-stu-id="f5b19-105">Learn how toorun an unattended installation of Azure Backup Server v2.</span></span> 

<span data-ttu-id="f5b19-106">De här stegen gäller inte om du installerar Azure Backup Server v1.</span><span class="sxs-lookup"><span data-stu-id="f5b19-106">These steps do not apply if you are installing Azure Backup Server v1.</span></span>

## <a name="install-backup-server-v2"></a><span data-ttu-id="f5b19-107">Installera reservserver v2</span><span class="sxs-lookup"><span data-stu-id="f5b19-107">Install Backup Server v2</span></span>

1. <span data-ttu-id="f5b19-108">Skapa en textfil på hello-server som är värd för Azure Backup Server v2.</span><span class="sxs-lookup"><span data-stu-id="f5b19-108">On hello server that hosts Azure Backup Server v2, create a text file.</span></span> <span data-ttu-id="f5b19-109">(Du kan skapa hello-fil i anteckningar eller i en annan text editor.) Spara filen hello som MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="f5b19-109">(You can create hello file in Notepad or in another text editor.) Save hello file as MABSSetup.ini.</span></span> 

2. <span data-ttu-id="f5b19-110">Klistra in följande kod i hello MABSSetup.ini fil hello.</span><span class="sxs-lookup"><span data-stu-id="f5b19-110">Paste hello following code in hello MABSSetup.ini file.</span></span> <span data-ttu-id="f5b19-111">Ersätt hello texten i hello hakparenteser (\< \>) med värden från din miljö.</span><span class="sxs-lookup"><span data-stu-id="f5b19-111">Replace hello text inside hello brackets (\< \>) with values from your environment.</span></span> <span data-ttu-id="f5b19-112">hello efter texten är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="f5b19-112">hello following text is an example:</span></span>

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

3. <span data-ttu-id="f5b19-113">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="f5b19-113">Save hello file.</span></span> <span data-ttu-id="f5b19-114">Vid en upphöjd kommandotolk på installationsservern hello skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f5b19-114">Then, at an elevated command prompt on hello installation server, enter this command:</span></span>

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

<span data-ttu-id="f5b19-115">Du kan använda dessa flaggor för hello installation:</span><span class="sxs-lookup"><span data-stu-id="f5b19-115">You can use these flags for hello installation:</span></span></br><span data-ttu-id="f5b19-116">
**/f**: ini-filsökväg</span><span class="sxs-lookup"><span data-stu-id="f5b19-116">
**/f**: .ini file path</span></span></br><span data-ttu-id="f5b19-117">
**/l**: loggsökvägen</span><span class="sxs-lookup"><span data-stu-id="f5b19-117">
**/l**: Log path</span></span></br><span data-ttu-id="f5b19-118">
**/i**: installationssökväg</span><span class="sxs-lookup"><span data-stu-id="f5b19-118">
**/i**: Installation path</span></span></br><span data-ttu-id="f5b19-119">
**/x**: avinstallera sökväg</span><span class="sxs-lookup"><span data-stu-id="f5b19-119">
**/x**: Uninstall path</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="f5b19-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5b19-120">Next steps</span></span>
<span data-ttu-id="f5b19-121">När du har installerat Backup Server lär du dig hur tooprepare servern, eller börja skydda en arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="f5b19-121">After you install Backup Server, learn how tooprepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="f5b19-122">Förbereda säkerhetskopiering serverarbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="f5b19-122">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="f5b19-123">Använda Backup Server tooback in en VMware-server</span><span class="sxs-lookup"><span data-stu-id="f5b19-123">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="f5b19-124">Använda Backup Server tooback in SQL Server</span><span class="sxs-lookup"><span data-stu-id="f5b19-124">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="f5b19-125">Lägg till tooBackup moderna Backup Storage Server</span><span class="sxs-lookup"><span data-stu-id="f5b19-125">Add Modern Backup Storage tooBackup Server</span></span>](backup-mabs-add-storage.md)
