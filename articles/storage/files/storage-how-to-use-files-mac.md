---
title: Montera en Azure-filresurs via SMB med macOS | Microsoft Docs
description: "Lär dig hur du monterar en Azure-filresurs via SMB med macOS."
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
ms.openlocfilehash: bc3d67745afb8bbffe7ec3462e995104daff9632
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a><span data-ttu-id="f25fc-103">Montera en Azure-filresurs via SMB med macOS</span><span class="sxs-lookup"><span data-stu-id="f25fc-103">Mount Azure File share over SMB with macOS</span></span>
<span data-ttu-id="f25fc-104">[Azure File Storage](../storage-dotnet-how-to-use-files.md) är en Microsoft-tjänst som gör att du kan skapa och använda nätverksfilresurser i Azure med protokollet som är branschstandard.</span><span class="sxs-lookup"><span data-stu-id="f25fc-104">[Azure File storage](../storage-dotnet-how-to-use-files.md) is Microsoft's service that enables you to create and use network file shares in the Azure using the industry standard.</span></span> <span data-ttu-id="f25fc-105">Azure-filresurser kan monteras i macOS Sierra (10.12) och El Capitan (10.11).</span><span class="sxs-lookup"><span data-stu-id="f25fc-105">Azure File shares can be mounted in macOS Sierra (10.12) and El Capitan (10.11).</span></span> <span data-ttu-id="f25fc-106">Den här artikeln visar två olika sätt att montera en Azure-filresurs på macOS med hjälp av användargränssnittet Finder och med terminalen.</span><span class="sxs-lookup"><span data-stu-id="f25fc-106">This article shows two different ways to mount an Azure File share on macOS with the Finder UI and using the Terminal.</span></span>

> [!Note]  
> <span data-ttu-id="f25fc-107">Innan du monterar en Azure-filresurs via SMB rekommenderar vi att du inaktiverar signering av SMB-paket.</span><span class="sxs-lookup"><span data-stu-id="f25fc-107">Before mounting an Azure File share over SMB, we recommend disabling SMB packet signing.</span></span> <span data-ttu-id="f25fc-108">Om du inte gör det kan det orsaka sämre prestanda när du får åtkomst till Azure-filresursen från macOS.</span><span class="sxs-lookup"><span data-stu-id="f25fc-108">Not doing so may yield poor performance when accessing the Azure File share from macOS.</span></span> <span data-ttu-id="f25fc-109">SMB-anslutningen krypteras så att det inte påverkar din anslutnings säkerhet.</span><span class="sxs-lookup"><span data-stu-id="f25fc-109">Your SMB connection will be encrypted, so this does not affect the security of your connection.</span></span> <span data-ttu-id="f25fc-110">Från terminalen används följande kommandon för att inaktivera signering av SMB-paket, vilket beskrivs i den här [Apple Support-artikeln om hur du inaktiverar signering av SMB-paket](https://support.apple.com/HT205926):</span><span class="sxs-lookup"><span data-stu-id="f25fc-110">From the terminal, the following commands will disable SMB packet signing, as described by this [Apple support article on disabling SMB packet signing](https://support.apple.com/HT205926):</span></span>  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a><span data-ttu-id="f25fc-111">Krav för att montera en Azure-filresurs på macOS</span><span class="sxs-lookup"><span data-stu-id="f25fc-111">Prerequisites for mounting an Azure File share on macOS</span></span>
* <span data-ttu-id="f25fc-112">**Lagringskontonamn**: Om du vill montera en Azure-filresurs behöver du namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f25fc-112">**Storage account name**: To mount an Azure File share, you will need the name of the storage account.</span></span>

* <span data-ttu-id="f25fc-113">**Lagringskontonyckel**: Om du vill montera en Azure-filresurs behöver du den primära (eller sekundära) lagringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="f25fc-113">**Storage account key**: To mount an Azure File share, you will need the primary (or secondary) storage key.</span></span> <span data-ttu-id="f25fc-114">SAS-nycklar stöds inte för montering.</span><span class="sxs-lookup"><span data-stu-id="f25fc-114">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="f25fc-115">**Kontrollera att port 445 är öppen**: SMB kommunicerar via TCP-port 445.</span><span class="sxs-lookup"><span data-stu-id="f25fc-115">**Ensure port 445 is open**: SMB communicates over TCP port 445.</span></span> <span data-ttu-id="f25fc-116">Kontrollera att brandväggen på klientdatorn (Mac) inte blockerar TCP-port 445.</span><span class="sxs-lookup"><span data-stu-id="f25fc-116">On your client machine (the Mac), check to make sure your firewall is not blocking TCP port 445.</span></span>

## <a name="mount-an-azure-file-share-via-finder"></a><span data-ttu-id="f25fc-117">Montera en Azure-filresurs via Finder</span><span class="sxs-lookup"><span data-stu-id="f25fc-117">Mount an Azure File share via Finder</span></span>
1. <span data-ttu-id="f25fc-118">**Öppna Finder**: Som standard är Finder redan öppet på macOS, men du kan kontrollera att det är det för tillfället markerade programmet genom att klicka på "ikonen med macOS-ansiktet" i Dock:</span><span class="sxs-lookup"><span data-stu-id="f25fc-118">**Open Finder**: Finder is open on macOS by default, but you can ensure it is the currently selected application by clicking the "macOS face icon" on the dock:</span></span>  
    <span data-ttu-id="f25fc-119">![ikonen med macOS-ansiktet](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span><span class="sxs-lookup"><span data-stu-id="f25fc-119">![The macOS face icon](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span></span>

2. <span data-ttu-id="f25fc-120">**Välj "Anslut till server" från menyn "Gå"**: Med hjälp av UNC-sökvägen från [krav](#preq) konverterar du de första dubbla omvända snedstrecken (`\\`) till `smb://` och alla andra omvända snedstreck (`\`) till snedstreck (`/`).</span><span class="sxs-lookup"><span data-stu-id="f25fc-120">**Select "Connect to Server" from the "Go" Menu**: Using the UNC path from the [prerequisites](#preq), convert the beginning double backslash (`\\`) to `smb://` and all other backslashes (`\`) to forwards slashes (`/`).</span></span> <span data-ttu-id="f25fc-121">Länken bör se ut ungefär så här: ![Dialogrutan "Anslut till server"](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span><span class="sxs-lookup"><span data-stu-id="f25fc-121">Your link should look like the following: ![The "Connect to Server" dialog](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span></span>

3. <span data-ttu-id="f25fc-122">**Använd resursnamnet och lagringskontonyckeln när du tillfrågas om användarnamn och lösenord**: När du klickar på "Anslut" i dialogrutan "Anslut till server" uppmanas du att ange ett användarnamn och lösenord (det kommer att vara ifyllt automatiskt med ditt macOS-användarnamn).</span><span class="sxs-lookup"><span data-stu-id="f25fc-122">**Use the share name and storage account key when prompted for a username and password**: When you click "Connect" on the "Connect to Server" dialog, you will be prompted for the username and password (This will be autopopulated with your macOS username).</span></span> <span data-ttu-id="f25fc-123">Du har möjlighet att lägga till ditt resursnamn/lagringskontonyckel i din nyckelring för macOS.</span><span class="sxs-lookup"><span data-stu-id="f25fc-123">You have the option of placing the share name/storage account key in your macOS Keychain.</span></span>

4. <span data-ttu-id="f25fc-124">**Använd Azure-filresursen som du vill**: När du har ersatt resursnamnet och lagringskontonyckeln med användarnamnet och lösenordet så monteras resursen.</span><span class="sxs-lookup"><span data-stu-id="f25fc-124">**Use the Azure File share as desired**: After substituting the share name and storage account key in for the username and password, the share will be mounted.</span></span> <span data-ttu-id="f25fc-125">Du kan använda det här på samma sätt som du vanligtvis använder en lokal mapp/filresurs, inklusive att dra och släppa filer till filresursen:</span><span class="sxs-lookup"><span data-stu-id="f25fc-125">You may use this as you would normally use a local folder/file share, including dragging and dropping files into the file share:</span></span>

    ![En ögonblicksbild av en monterad Azure-filresurs](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a><span data-ttu-id="f25fc-127">Montera en Azure-filresurs via Terminal</span><span class="sxs-lookup"><span data-stu-id="f25fc-127">Mount an Azure File share via Terminal</span></span>
1. <span data-ttu-id="f25fc-128">Ersätt `<storage-account-name>` med namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f25fc-128">Replace `<storage-account-name>` with the name of your storage account.</span></span> <span data-ttu-id="f25fc-129">Ange lagringskontonyckeln som lösenord när du uppmanas att ange lösenord.</span><span class="sxs-lookup"><span data-stu-id="f25fc-129">Provide Storage Account Key as password when prompted.</span></span> 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. <span data-ttu-id="f25fc-130">**Använd Azure-filresursen som du vill**: Azure-filresursen monteras på monteringspunkten som angavs av det föregående kommandot.</span><span class="sxs-lookup"><span data-stu-id="f25fc-130">**Use the Azure File share as desired**: The Azure File share will be mounted at the mount point specified by the previous command.</span></span>  

    ![En ögonblicksbild av den monterade Azure-filresursen](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a><span data-ttu-id="f25fc-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f25fc-132">Next steps</span></span>
<span data-ttu-id="f25fc-133">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="f25fc-133">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="f25fc-134">Apple Support-artikel – Så här ansluter du med fildelning på din Mac.</span><span class="sxs-lookup"><span data-stu-id="f25fc-134">Apple Support Article - How to connect with File Sharing on your Mac</span></span>](https://support.apple.com/HT204445)
* [<span data-ttu-id="f25fc-135">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="f25fc-135">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="f25fc-136">Felsökning i Windows</span><span class="sxs-lookup"><span data-stu-id="f25fc-136">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="f25fc-137">Felsökning i Linux</span><span class="sxs-lookup"><span data-stu-id="f25fc-137">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    