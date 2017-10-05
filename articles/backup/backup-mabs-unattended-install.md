---
title: Tyst installation av Azure Backup Server v2 | Microsoft Docs
description: "Använda ett PowerShell-skript för att installera Azure Backup Server v2. Den här typen av installation kallas även en obevakad installation."
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
ms.openlocfilehash: 91778a67f9ef523aa87b7918197e0d0ded0f5702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a><span data-ttu-id="56add-104">Kör en obevakad installation av Azure Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="56add-104">Run an unattended installation of Azure Backup Server v2</span></span>

<span data-ttu-id="56add-105">Lär dig hur du kör en obevakad installation av Azure Backup Server v2.</span><span class="sxs-lookup"><span data-stu-id="56add-105">Learn how to run an unattended installation of Azure Backup Server v2.</span></span> 

<span data-ttu-id="56add-106">De här stegen gäller inte om du installerar Azure Backup Server v1.</span><span class="sxs-lookup"><span data-stu-id="56add-106">These steps do not apply if you are installing Azure Backup Server v1.</span></span>

## <a name="install-backup-server-v2"></a><span data-ttu-id="56add-107">Installera reservserver v2</span><span class="sxs-lookup"><span data-stu-id="56add-107">Install Backup Server v2</span></span>

1. <span data-ttu-id="56add-108">Skapa en textfil på den server som är värd för Azure Backup Server v2.</span><span class="sxs-lookup"><span data-stu-id="56add-108">On the server that hosts Azure Backup Server v2, create a text file.</span></span> <span data-ttu-id="56add-109">(Du kan skapa filen i anteckningar eller något annat textredigeringsprogram.) Spara filen som MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="56add-109">(You can create the file in Notepad or in another text editor.) Save the file as MABSSetup.ini.</span></span> 

2. <span data-ttu-id="56add-110">Klistra in följande kod i filen MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="56add-110">Paste the following code in the MABSSetup.ini file.</span></span> <span data-ttu-id="56add-111">Ersätt texten inom hakparentes (\< \>) med värden från din miljö.</span><span class="sxs-lookup"><span data-stu-id="56add-111">Replace the text inside the brackets (\< \>) with values from your environment.</span></span> <span data-ttu-id="56add-112">Följande är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="56add-112">The following text is an example:</span></span>

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

3. <span data-ttu-id="56add-113">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="56add-113">Save the file.</span></span> <span data-ttu-id="56add-114">Vid en upphöjd kommandotolk på installationsservern, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="56add-114">Then, at an elevated command prompt on the installation server, enter this command:</span></span>

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

<span data-ttu-id="56add-115">Du kan använda dessa flaggor för installationen:</span><span class="sxs-lookup"><span data-stu-id="56add-115">You can use these flags for the installation:</span></span></br><span data-ttu-id="56add-116">
**/f**: ini-filsökväg</span><span class="sxs-lookup"><span data-stu-id="56add-116">
**/f**: .ini file path</span></span></br><span data-ttu-id="56add-117">
**/l**: loggsökvägen</span><span class="sxs-lookup"><span data-stu-id="56add-117">
**/l**: Log path</span></span></br><span data-ttu-id="56add-118">
**/i**: installationssökväg</span><span class="sxs-lookup"><span data-stu-id="56add-118">
**/i**: Installation path</span></span></br><span data-ttu-id="56add-119">
**/x**: avinstallera sökväg</span><span class="sxs-lookup"><span data-stu-id="56add-119">
**/x**: Uninstall path</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="56add-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56add-120">Next steps</span></span>
<span data-ttu-id="56add-121">När du har installerat Backup Server lär du dig hur du förbereder din server eller börja skydda en arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="56add-121">After you install Backup Server, learn how to prepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="56add-122">Förbereda säkerhetskopiering serverarbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="56add-122">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="56add-123">Använd Backup Server för att säkerhetskopiera en VMware-server</span><span class="sxs-lookup"><span data-stu-id="56add-123">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="56add-124">Använd Backup Server för att säkerhetskopiera SQL Server</span><span class="sxs-lookup"><span data-stu-id="56add-124">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="56add-125">Lägg till moderna Säkerhetskopieringslagring Backup-Server</span><span class="sxs-lookup"><span data-stu-id="56add-125">Add Modern Backup Storage to Backup Server</span></span>](backup-mabs-add-storage.md)
