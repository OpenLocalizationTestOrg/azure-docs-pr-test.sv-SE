---
title: aaaMount Azure filresursen via SMB med macOS | Microsoft Docs
description: "Lär dig hur toomount en Azure-fil dela över SMB med macOS."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 7b4924cb42247470521c1ae8b9d03ab1756996e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a><span data-ttu-id="9fa51-103">Montera en Azure-filresurs via SMB med macOS</span><span class="sxs-lookup"><span data-stu-id="9fa51-103">Mount Azure File share over SMB with macOS</span></span>
<span data-ttu-id="9fa51-104">[Azure File storage](storage-dotnet-how-to-use-files.md) Microsofts tjänst som gör att du toocreate och använda nätverksfilresurser i hello Azure använder hello branschstandard.</span><span class="sxs-lookup"><span data-stu-id="9fa51-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's service that enables you toocreate and use network file shares in hello Azure using hello industry standard.</span></span> <span data-ttu-id="9fa51-105">Azure-filresurser kan monteras i macOS Sierra (10.12) och El Capitan (10.11).</span><span class="sxs-lookup"><span data-stu-id="9fa51-105">Azure File shares can be mounted in macOS Sierra (10.12) and El Capitan (10.11).</span></span> <span data-ttu-id="9fa51-106">Den här artikeln innehåller två olika sätt toomount en Azure-filresurs på macOS med hjälp av hello Terminal och hello Finder Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="9fa51-106">This article shows two different ways toomount an Azure File share on macOS with hello Finder UI and using hello Terminal.</span></span>

> [!Note]  
> <span data-ttu-id="9fa51-107">Innan du monterar en Azure-filresurs via SMB rekommenderar vi att du inaktiverar signering av SMB-paket.</span><span class="sxs-lookup"><span data-stu-id="9fa51-107">Before mounting an Azure File share over SMB, we recommend disabling SMB packet signing.</span></span> <span data-ttu-id="9fa51-108">Inte gör det kan ge sämre prestanda vid åtkomst till filresursen för hello Azure från macOS.</span><span class="sxs-lookup"><span data-stu-id="9fa51-108">Not doing so may yield poor performance when accessing hello Azure File share from macOS.</span></span> <span data-ttu-id="9fa51-109">SMB-anslutning krypteras så att detta inte påverkar hello säkerheten för din anslutning.</span><span class="sxs-lookup"><span data-stu-id="9fa51-109">Your SMB connection will be encrypted, so this does not affect hello security of your connection.</span></span> <span data-ttu-id="9fa51-110">Från hello terminal, hello följande kommandon inaktiverar SMB-signering för paket, som beskrivs i den här [Apple support-artikeln om hur du inaktiverar SMB-signering av paket](https://support.apple.com/HT205926):</span><span class="sxs-lookup"><span data-stu-id="9fa51-110">From hello terminal, hello following commands will disable SMB packet signing, as described by this [Apple support article on disabling SMB packet signing](https://support.apple.com/HT205926):</span></span>  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a><span data-ttu-id="9fa51-111">Krav för att montera en Azure-filresurs på macOS</span><span class="sxs-lookup"><span data-stu-id="9fa51-111">Prerequisites for mounting an Azure File share on macOS</span></span>
* <span data-ttu-id="9fa51-112">**Lagringskontonamnet**: toomount ett Azure-filresurs, du behöver hello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9fa51-112">**Storage account name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="9fa51-113">**Lagringskontonyckel**: toomount ett Azure-filresurs, du behöver hello primära (eller sekundära) lagringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="9fa51-113">**Storage account key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="9fa51-114">SAS-nycklar stöds inte för montering.</span><span class="sxs-lookup"><span data-stu-id="9fa51-114">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="9fa51-115">**Kontrollera att port 445 är öppen**: SMB kommunicerar via TCP-port 445.</span><span class="sxs-lookup"><span data-stu-id="9fa51-115">**Ensure port 445 is open**: SMB communicates over TCP port 445.</span></span> <span data-ttu-id="9fa51-116">Kontrollera att inte brandväggen blockerar TCP-port 445 toomake på din klientdator (hello Mac).</span><span class="sxs-lookup"><span data-stu-id="9fa51-116">On your client machine (hello Mac), check toomake sure your firewall is not blocking TCP port 445.</span></span>

## <a name="mount-an-azure-file-share-via-finder"></a><span data-ttu-id="9fa51-117">Montera en Azure-filresurs via Finder</span><span class="sxs-lookup"><span data-stu-id="9fa51-117">Mount an Azure File share via Finder</span></span>
1. <span data-ttu-id="9fa51-118">**Öppna Finder**: Finder är öppen på macOS som standard, men du kan kontrollera att det är hello markerat program genom att klicka på hello ”macOS står inför ikonen” hello dock:</span><span class="sxs-lookup"><span data-stu-id="9fa51-118">**Open Finder**: Finder is open on macOS by default, but you can ensure it is hello currently selected application by clicking hello "macOS face icon" on hello dock:</span></span>  
    <span data-ttu-id="9fa51-119">![hello macOS står inför ikon](media/storage-file-how-to-use-files-mac/mount-via-finder-1.png)</span><span class="sxs-lookup"><span data-stu-id="9fa51-119">![hello macOS face icon](media/storage-file-how-to-use-files-mac/mount-via-finder-1.png)</span></span>

2. <span data-ttu-id="9fa51-120">**Välj ”ansluta tooServer” hello ”gå” menyn**: med hello UNC-sökväg från hello [krav](#preq), konvertera hello början dubbla omvänt snedstreck (`\\`) för`smb://` och alla andra omvända snedstreck (`\`) tooforwards snedstreck (`/`).</span><span class="sxs-lookup"><span data-stu-id="9fa51-120">**Select "Connect tooServer" from hello "Go" Menu**: Using hello UNC path from hello [prerequisites](#preq), convert hello beginning double backslash (`\\`) too`smb://` and all other backslashes (`\`) tooforwards slashes (`/`).</span></span> <span data-ttu-id="9fa51-121">Länken bör se ut som följande hello: ![hello ”ansluta tooServer” dialogrutan](./media/storage-file-how-to-use-files-mac/mount-via-finder-2.png)</span><span class="sxs-lookup"><span data-stu-id="9fa51-121">Your link should look like hello following: ![hello "Connect tooServer" dialog](./media/storage-file-how-to-use-files-mac/mount-via-finder-2.png)</span></span>

3. <span data-ttu-id="9fa51-122">**Använd hello resursen namn och lagring kontonyckel när du tillfrågas om användarnamn och lösenord**: när du klickar på ”Anslut” hello ”ansluta tooServer” dialogrutan tillfrågas du om hello användarnamn och lösenord (detta är autopopulated med din macOS användarnamnet).</span><span class="sxs-lookup"><span data-stu-id="9fa51-122">**Use hello share name and storage account key when prompted for a username and password**: When you click "Connect" on hello "Connect tooServer" dialog, you will be prompted for hello username and password (This will be autopopulated with your macOS username).</span></span> <span data-ttu-id="9fa51-123">Du har hello möjlighet att placera hello resursen namn/lagringskontonyckel i din macOS nyckelringar.</span><span class="sxs-lookup"><span data-stu-id="9fa51-123">You have hello option of placing hello share name/storage account key in your macOS Keychain.</span></span>

4. <span data-ttu-id="9fa51-124">**Använd hello Azure-filresurs som önskade**: hello resursen monteras efter ersätter hello resursen namn och lagring kontonyckel i hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="9fa51-124">**Use hello Azure File share as desired**: After substituting hello share name and storage account key in for hello username and password, hello share will be mounted.</span></span> <span data-ttu-id="9fa51-125">Du kan använda som du vanligtvis använder en lokal mapp/filresurs, inklusive dra och släppa filer i hello filresurs:</span><span class="sxs-lookup"><span data-stu-id="9fa51-125">You may use this as you would normally use a local folder/file share, including dragging and dropping files into hello file share:</span></span>

    ![En ögonblicksbild av en monterad Azure-filresurs](./media/storage-file-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a><span data-ttu-id="9fa51-127">Montera en Azure-filresurs via Terminal</span><span class="sxs-lookup"><span data-stu-id="9fa51-127">Mount an Azure File share via Terminal</span></span>
1. <span data-ttu-id="9fa51-128">Ersätt `<storage-account-name>` med hello namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="9fa51-128">Replace `<storage-account-name>` with hello name of your storage account.</span></span> <span data-ttu-id="9fa51-129">Ange lagringskontonyckeln som lösenord när du uppmanas att ange lösenord.</span><span class="sxs-lookup"><span data-stu-id="9fa51-129">Provide Storage Account Key as password when prompted.</span></span> 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. <span data-ttu-id="9fa51-130">**Använd hello Azure-filresurs som önskade**: hello Azure-filresurs monteras på hello monteringspunkt som anges av hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="9fa51-130">**Use hello Azure File share as desired**: hello Azure File share will be mounted at hello mount point specified by hello previous command.</span></span>  

    ![En ögonblicksbild av hello monterade Azure-filresurs](./media/storage-file-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a><span data-ttu-id="9fa51-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9fa51-132">Next steps</span></span>
<span data-ttu-id="9fa51-133">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="9fa51-133">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="9fa51-134">Support-artikeln Apple - hur tooconnect med fildelning på din Mac.</span><span class="sxs-lookup"><span data-stu-id="9fa51-134">Apple Support Article - How tooconnect with File Sharing on your Mac</span></span>](https://support.apple.com/HT204445)
* [<span data-ttu-id="9fa51-135">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="9fa51-135">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="9fa51-136">Felsökning</span><span class="sxs-lookup"><span data-stu-id="9fa51-136">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)