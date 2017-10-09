---
title: "aaaMigrate användardata från Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur toomigrate användardata till och från Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d7e4fbf1-cb42-4430-94a0-ed6d4676fc86
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aefc6ccc2c6173754acf6cad06102f27c8cb1d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-data-into-and-out-of-azure-remoteapp"></a><span data-ttu-id="ac8c6-103">Hur toomigrate data till och från Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="ac8c6-103">How toomigrate data into and out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ac8c6-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="ac8c6-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="ac8c6-106">Du kan använda många olika verktyg och metoder tootransfer [användardata](remoteapp-upd.md) in och ut från Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-106">You can use many different tools and methods tootransfer [user data](remoteapp-upd.md) into and out of Azure RemoteApp.</span></span> <span data-ttu-id="ac8c6-107">Här följer några metoder:</span><span class="sxs-lookup"><span data-stu-id="ac8c6-107">Here are a few methods:</span></span>

* <span data-ttu-id="ac8c6-108">Kopiera och klistra in med hjälp av delning av Urklipp</span><span class="sxs-lookup"><span data-stu-id="ac8c6-108">Copy and paste using clipboard sharing</span></span>
* <span data-ttu-id="ac8c6-109">Kopiera filer och data tooa-filserver</span><span class="sxs-lookup"><span data-stu-id="ac8c6-109">Copy files and data tooa file server</span></span>
* <span data-ttu-id="ac8c6-110">Kopiera filerna tooOneDrive för företag via en webbläsare</span><span class="sxs-lookup"><span data-stu-id="ac8c6-110">Copy files tooOneDrive for Business through a browser</span></span>
* <span data-ttu-id="ac8c6-111">Kopiera filerna med hjälp av omdirigering</span><span class="sxs-lookup"><span data-stu-id="ac8c6-111">Copy files using redirection</span></span>

> [!NOTE]
> <span data-ttu-id="ac8c6-112">Du kan inte aktivera hello OneDrive för företag eller konsumenten sync-agenter – de [stöds inte](remoteapp-onedrive.md) i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-112">You cannot enable hello OneDrive for Business or Consumer sync agents - they [are not supported](remoteapp-onedrive.md) in Azure RemoteApp.</span></span>
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a><span data-ttu-id="ac8c6-113">Kopiera och klistra in i Utforskaren</span><span class="sxs-lookup"><span data-stu-id="ac8c6-113">Use copy and paste in File Explorer</span></span>
<span data-ttu-id="ac8c6-114">Kopiera och klistra in Urklipp hello är aktiverat i RemoteApp-distributioner [som standard](remoteapp-redirection.md).</span><span class="sxs-lookup"><span data-stu-id="ac8c6-114">Copy and paste using hello clipboard is enabled in RemoteApp deployments [by default](remoteapp-redirection.md).</span></span> <span data-ttu-id="ac8c6-115">Detta gör att användarna kopiera filer mellan sina lokala datorer och RemoteApp-appar.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-115">This lets users copy files between their local PC and RemoteApp apps.</span></span> <span data-ttu-id="ac8c6-116">Ofta via hello normala av appar i RemoteApp-har användare sparat filer tootheir UPD: ar - flytta att data ut från RemoteApp är enkel:</span><span class="sxs-lookup"><span data-stu-id="ac8c6-116">Often, through hello normal course of using apps in RemoteApp, users have saved files tootheir UPDs - moving that data out of RemoteApp is easy:</span></span>

1. <span data-ttu-id="ac8c6-117">[Publicera Utforskaren som en app](remoteapp-publish.md) i RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-117">[Publish File Explorer as an app](remoteapp-publish.md) in a RemoteApp collection.</span></span> <span data-ttu-id="ac8c6-118">(Observera att detta är en administrativ aktivitet.)</span><span class="sxs-lookup"><span data-stu-id="ac8c6-118">(Note that this is an administrative task.)</span></span>
2. <span data-ttu-id="ac8c6-119">Dirigera användare toolaunch hello Utforskaren appen som du har publicerat och toouse som toocopy och klistra in filer både till deras UPD och från den.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-119">Direct your users toolaunch hello File Explorer app you published and toouse that toocopy and paste files both into their UPD and out of it.</span></span>

## <a name="upload-files-and-data-tooa-file-server-by-using-standard-network-file-copy"></a><span data-ttu-id="ac8c6-120">Överföra filer och data tooa filserver med hjälp av standardnätverk filkopiering</span><span class="sxs-lookup"><span data-stu-id="ac8c6-120">Upload files and data tooa file server by using standard network file copy</span></span>
<span data-ttu-id="ac8c6-121">Ofta organisationer använda servrar toostore allmänna fildata.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-121">Often organizations use file servers toostore general data.</span></span> <span data-ttu-id="ac8c6-122">Om du vet hello-servernamn eller plats, användarna Bläddra hello lokalt nätverk för hello-servern och kopiera sina filer, mycket ut som ovan.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-122">If you know hello server name or location, your users can browse hello local network for hello server and then copy their files there, much like they did above.</span></span> <span data-ttu-id="ac8c6-123">Igen du vill toopublish Utforskaren tooRemoteApp och dela den med dina användare.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-123">Again you'll want toopublish File Explorer tooRemoteApp and then share it with your users.</span></span>

> [!NOTE]
> <span data-ttu-id="ac8c6-124">hello filservern måste finnas på hello dirigerbara nätverk RemoteApp har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-124">hello file server must be on hello routable network that RemoteApp was deployed into.</span></span>
> 
> 

## <a name="copy-files-tooonedrive-for-business"></a><span data-ttu-id="ac8c6-125">Kopiera filerna tooOneDrive för företag</span><span class="sxs-lookup"><span data-stu-id="ac8c6-125">Copy files tooOneDrive for Business</span></span>
<span data-ttu-id="ac8c6-126">Även om du inte aktivera hello OneDrive för företag sync agent i RemoteApp, kan du fortfarande kopiera filer från UPD-tooOneDrive för företag via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-126">Although you cannot enable hello OneDrive for Business sync agent in RemoteApp, you can still copy files from your UPD tooOneDrive for Business through a browser.</span></span> 

1. <span data-ttu-id="ac8c6-127">Publicera Utforskaren tooRemoteApp och sedan ber du användarna tooaccess hello filer via appen.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-127">Publish File Explorer tooRemoteApp and then tell users tooaccess hello files through that app.</span></span> 
2. <span data-ttu-id="ac8c6-128">Det är den enklaste tootransfer filer om de komprimeras, så att användarna ska skapa en ZIP-fil som innehåller alla hello filer toomove tooOneDrive för företag.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-128">It's easiest tootransfer files if they are compressed, so users should create a .zip file that contains all of hello files toomove tooOneDrive for Business.</span></span>
3. <span data-ttu-id="ac8c6-129">Be användare toogo toohello Office 365-portalen och gå sedan tooOneDrive och överför hello ZIP-filen.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-129">Ask users toogo toohello Office 365 portal, and then go tooOneDrive and upload hello .zip file.</span></span>

## <a name="copy-files-by-using-drive-redirection"></a><span data-ttu-id="ac8c6-130">Kopiera filerna med hjälp av omdirigering</span><span class="sxs-lookup"><span data-stu-id="ac8c6-130">Copy files by using drive redirection</span></span>
<span data-ttu-id="ac8c6-131">Om du har aktiverat [omdirigering av](remoteapp-redirection.md), du redan har skapat en mappad enhet för dina användare.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-131">If you have enabled [drive redirection](remoteapp-redirection.md), you have already created a mapped drive for your users.</span></span> <span data-ttu-id="ac8c6-132">I det här fallet de zip sina filer på hello omdirigerad enhet och spara dem tootheir lokala dator.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-132">In this case, they can zip their files on hello redirected drive and then save them tootheir local PC.</span></span>

## <a name="how-administrators-can-export-data"></a><span data-ttu-id="ac8c6-133">Hur administratörer kan använda för att exportera data</span><span class="sxs-lookup"><span data-stu-id="ac8c6-133">How administrators can export data</span></span>

<span data-ttu-id="ac8c6-134">Tillämpar för Azure RemoteApp kan exportera alla användarprofil-diskar (UPD) för alla samlingar i prenumerationen-tooAzure lagring med hjälp av Azure PowerShell-cmdleten Export-AzureRemoteAppUserDisk.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-134">Administers for Azure RemoteApp can export all user profile disks (UPD) for all collections within a subscription tooAzure Storage using Azure PowerShell cmdlet, Export-AzureRemoteAppUserDisk.</span></span>  <span data-ttu-id="ac8c6-135">Det finns ingen möjlighet tooselect enskilda UPD'S.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-135">There is no ability tooselect individual UPD's.</span></span>  <span data-ttu-id="ac8c6-136">När hello PowerShell-kommandot utförs kommer varje användare disk vara 50gb fasta diskstorleken och exporterade tooAzure för lagring.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-136">When hello PowerShell command is executed, each user disk will be a 50gb in fixed disk size and be exported tooAzure storage.</span></span>  <span data-ttu-id="ac8c6-137">Kostnader för Azure storage påförs direkt för den här lagring.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-137">Costs of Azure storage will incur immediately for this storage.</span></span>  <span data-ttu-id="ac8c6-138">När du kör detta kommando Se till att finns det inga sessioner som på annat sätt hello exportera misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-138">When running this command ensure there are no sessions otherwise hello export will fail.</span></span>

<span data-ttu-id="ac8c6-139">UPDS för domänanslutna Azure RemoteApp-distributioner kan bara användas igen i en RDS-distribution, kopplade icke-domän-distributioner kan inte användas.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-139">UPD's for domain joined Azure RemoteApp deployments can only be used again in an RDS deployment, non-domain joined deployments cannot be used.</span></span>  <span data-ttu-id="ac8c6-140">Om dessa diskar kommer att användas i en RDS-distribution rekommenderar vi toouse vår [automatiserade skript](https://github.com/arcadiahlyy/aramigration) som ska exportera, konvertera och importera hello UPDS till en RDS-distribution.</span><span class="sxs-lookup"><span data-stu-id="ac8c6-140">If these disks will be used in an RDS deployment we recommend toouse our [automated scripts](https://github.com/arcadiahlyy/aramigration) that will export, convert, and import hello UPD's into an RDS deployment.</span></span>

