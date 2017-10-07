---
title: "aaaAzure moln Shell (förhandsgranskning) översikt | Microsoft Docs"
description: "Översikt över hello Azure Cloud-gränssnittet."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 45c6c85b167a90947a333f44a9186e2c01b4fa7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a><span data-ttu-id="5b45a-103">Översikt över Azure-molnet Shell (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="5b45a-103">Overview of Azure Cloud Shell (Preview)</span></span>
<span data-ttu-id="5b45a-104">Azure Cloud-gränssnittet är en interaktiv, webbläsare-tillgängliga shell för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5b45a-104">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span>

![](media/overview-pic.png)

## <a name="features"></a><span data-ttu-id="5b45a-105">Funktioner</span><span class="sxs-lookup"><span data-stu-id="5b45a-105">Features</span></span>
### <a name="browser-based-shell-experience"></a><span data-ttu-id="5b45a-106">Shell-webbläsarbaserad upplevelse</span><span class="sxs-lookup"><span data-stu-id="5b45a-106">Browser-based shell experience</span></span>
<span data-ttu-id="5b45a-107">Moln Shell aktiverar åtkomst tooa webbläsarbaserad kommandoradsverktyget erfarenheterna med Azure-hanteringsuppgifter i åtanke.</span><span class="sxs-lookup"><span data-stu-id="5b45a-107">Cloud Shell enables access tooa browser-based command-line experience built with Azure management tasks in mind.</span></span> <span data-ttu-id="5b45a-108">Utnyttja molnet Shell toowork obunden från en lokal dator på ett sätt som endast hello molnet kan ge.</span><span class="sxs-lookup"><span data-stu-id="5b45a-108">Leverage Cloud Shell toowork untethered from a local machine in a way only hello cloud can provide.</span></span>

### <a name="pre-configured-azure-workstation"></a><span data-ttu-id="5b45a-109">Förkonfigurerade Azure arbetsstation</span><span class="sxs-lookup"><span data-stu-id="5b45a-109">Pre-configured Azure workstation</span></span>
<span data-ttu-id="5b45a-110">Molnet Shell finns förinstallerat med populära kommandoradsverktyg och språkstöd så att du kan arbeta snabbare.</span><span class="sxs-lookup"><span data-stu-id="5b45a-110">Cloud Shell comes pre-installed with popular command-line tools and language support so you can work faster.</span></span>

[<span data-ttu-id="5b45a-111">Visa hello fullständig verktygsuppsättning lista för Azure Cloud Shell här.</span><span class="sxs-lookup"><span data-stu-id="5b45a-111">View hello full tooling list for Azure Cloud Shell here.</span></span>](features.md#tools)

### <a name="automatic-authentication"></a><span data-ttu-id="5b45a-112">Automatisk autentisering</span><span class="sxs-lookup"><span data-stu-id="5b45a-112">Automatic authentication</span></span>
<span data-ttu-id="5b45a-113">Molnet Shell autentiserar på ett säkert sätt automatiskt på varje session för direktåtkomst tooyour resurser via hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b45a-113">Cloud Shell securely authenticates automatically on each session for instant access tooyour resources through hello Azure CLI 2.0.</span></span>

### <a name="connect-your-azure-file-storage"></a><span data-ttu-id="5b45a-114">Anslut Azure File storage</span><span class="sxs-lookup"><span data-stu-id="5b45a-114">Connect your Azure File storage</span></span>
<span data-ttu-id="5b45a-115">Molnet Shell datorer är tillfälliga och därför kräver ett Azure file share toobe monterade som `clouddrive` toopersist $Home-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5b45a-115">Cloud Shell machines are temporary and as a result require an Azure file share toobe mounted as `clouddrive` toopersist your $Home directory.</span></span>
<span data-ttu-id="5b45a-116">Startas för första uppmanar molnet Shell toocreate en resursgrupp, storage-konto, och filresursen för din räkning.</span><span class="sxs-lookup"><span data-stu-id="5b45a-116">On first launch Cloud Shell prompts toocreate a resource group, storage account, and file share on your behalf.</span></span> <span data-ttu-id="5b45a-117">Detta är ett enstaka steg och bifogas automatiskt för alla sessioner.</span><span class="sxs-lookup"><span data-stu-id="5b45a-117">This is a one-time step and will be automatically attached for all sessions.</span></span> 

#### <a name="create-new-storage"></a><span data-ttu-id="5b45a-118">Skapa nya lagringsenheter</span><span class="sxs-lookup"><span data-stu-id="5b45a-118">Create new storage</span></span>
![](media/basic-storage.png)

<span data-ttu-id="5b45a-119">Ett lokalt redundant lagringskonto (LRS) kan skapas för din räkning med en Azure-filresurs som innehåller en disk på 5 GB standardavbildningen.</span><span class="sxs-lookup"><span data-stu-id="5b45a-119">A locally-redundant storage (LRS) account can be created on your behalf with an Azure file share containing a default 5-GB disk image.</span></span> <span data-ttu-id="5b45a-120">hello filresurs monterar som `clouddrive` dela interaktion med hello diskavbildning för filen som används toosync och bevara $Home-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5b45a-120">hello file share mounts as `clouddrive` for file share interaction with hello disk image being used toosync and persist your $Home directory.</span></span> <span data-ttu-id="5b45a-121">Vanliga lagringskostnader gäller.</span><span class="sxs-lookup"><span data-stu-id="5b45a-121">Regular storage costs apply.</span></span>

<span data-ttu-id="5b45a-122">Tre resurser kommer att skapas för din räkning:</span><span class="sxs-lookup"><span data-stu-id="5b45a-122">Three resources will be created on your behalf:</span></span>
1. <span data-ttu-id="5b45a-123">En resursgrupp med namnet:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="5b45a-123">Resource Group named: `cloud-shell-storage-<region>`</span></span>
2. <span data-ttu-id="5b45a-124">Lagringskontonamnet:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="5b45a-124">Storage Account named: `cs<uniqueGuid>`</span></span>
3. <span data-ttu-id="5b45a-125">Filresurs med namnet:`cs-<user>-<domain>-com-uniqueGuid`</span><span class="sxs-lookup"><span data-stu-id="5b45a-125">File Share named: `cs-<user>-<domain>-com-uniqueGuid`</span></span>

> [!Note]
> <span data-ttu-id="5b45a-126">Alla filer i katalogen $Home, till exempel SSH-nycklar finns kvar i din användare diskavbildning som lagras i monterade filresursen.</span><span class="sxs-lookup"><span data-stu-id="5b45a-126">All files in your $Home directory such as SSH keys are persisted in your user disk image stored in your mounted file share.</span></span> <span data-ttu-id="5b45a-127">Tillämpa metodtips när du sparar filer i din $Home katalog och monterade filresurs.</span><span class="sxs-lookup"><span data-stu-id="5b45a-127">Apply best practices when saving files in your $Home directory and mounted file share.</span></span>

#### <a name="use-existing-resources"></a><span data-ttu-id="5b45a-128">Använda befintliga resurser</span><span class="sxs-lookup"><span data-stu-id="5b45a-128">Use existing resources</span></span>
![](media/advanced-storage.png)

<span data-ttu-id="5b45a-129">Ett avancerat alternativ finns även vilket gör att du tooassociate befintliga resurser tooCloud Shell.</span><span class="sxs-lookup"><span data-stu-id="5b45a-129">An advanced option is also provided allowing you tooassociate existing resources tooCloud Shell.</span></span> <span data-ttu-id="5b45a-130">Klicka på ”Visa avancerade inställningar” tooselect ytterligare alternativ när hello lagring installationsprogrammet Kommandotolken kan.</span><span class="sxs-lookup"><span data-stu-id="5b45a-130">When presented with hello storage setup prompt, click "Show advanced settings" tooselect additional options.</span></span> <span data-ttu-id="5b45a-131">Nedrullningsbara listorna filtreras för molntjänster Shell regionen och lokalt/globalt-redundant storage-konton.</span><span class="sxs-lookup"><span data-stu-id="5b45a-131">Dropdowns are filtered for your assigned Cloud Shell region and locally/globally-redundant storage accounts.</span></span>

<span data-ttu-id="5b45a-132">[Läs mer om molntjänster Shell lagring, uppdatera filresurser och ladda upp/hämta filer.] (beständighet-shell-storage.md)</span><span class="sxs-lookup"><span data-stu-id="5b45a-132">[Learn about Cloud Shell storage, updating file shares, and uploading/downloading files.] (persisting-shell-storage.md)</span></span>

## <a name="concepts"></a><span data-ttu-id="5b45a-133">Koncept</span><span class="sxs-lookup"><span data-stu-id="5b45a-133">Concepts</span></span>
* <span data-ttu-id="5b45a-134">Molnet Shell körs på en tillfällig virtuell dator på en per session, per användare</span><span class="sxs-lookup"><span data-stu-id="5b45a-134">Cloud Shell runs on a temporary machine provided on a per-session, per-user basis</span></span>
* <span data-ttu-id="5b45a-135">Molnet Shell timeout efter 20 minuter utan interaktiva aktivitet</span><span class="sxs-lookup"><span data-stu-id="5b45a-135">Cloud Shell times out after 20 minutes without interactive activity</span></span>
* <span data-ttu-id="5b45a-136">Molnet Shell kan endast användas med en filresurs som ansluten</span><span class="sxs-lookup"><span data-stu-id="5b45a-136">Cloud Shell can only be accessed with a file share attached</span></span>
* <span data-ttu-id="5b45a-137">Molnet Shell tilldelas en dator per konto</span><span class="sxs-lookup"><span data-stu-id="5b45a-137">Cloud Shell is assigned one machine per user account</span></span>
* <span data-ttu-id="5b45a-138">Behörigheter har angetts som en vanlig Linux-användare</span><span class="sxs-lookup"><span data-stu-id="5b45a-138">Permissions are set as a regular Linux user</span></span>

[<span data-ttu-id="5b45a-139">Läs mer om alla moln Shell-funktioner.</span><span class="sxs-lookup"><span data-stu-id="5b45a-139">Learn more about all Cloud Shell features.</span></span>](features.md)

## <a name="examples"></a><span data-ttu-id="5b45a-140">Exempel</span><span class="sxs-lookup"><span data-stu-id="5b45a-140">Examples</span></span>
* <span data-ttu-id="5b45a-141">Skapa eller redigera skript tooautomate Azure management</span><span class="sxs-lookup"><span data-stu-id="5b45a-141">Create or edit scripts tooautomate Azure management</span></span>
* <span data-ttu-id="5b45a-142">Samtidigt hantera resurser via Azure-portalen och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5b45a-142">Simultaneously manage resources via Azure portal and Azure CLI 2.0</span></span>
* <span data-ttu-id="5b45a-143">Testkör Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5b45a-143">Test-drive Azure CLI 2.0</span></span>

[<span data-ttu-id="5b45a-144">Prova att använda de här exemplen på hello molnet Shell Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="5b45a-144">Try out all these examples at hello Cloud Shell quickstart.</span></span>](quickstart.md)

## <a name="pricing"></a><span data-ttu-id="5b45a-145">Prissättning</span><span class="sxs-lookup"><span data-stu-id="5b45a-145">Pricing</span></span>
<span data-ttu-id="5b45a-146">hello-dator som är värd molnet Shell är ledig, med ett krav för en monterad Azure fil dela toopersist $Home-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5b45a-146">hello machine hosting Cloud Shell is free, with a pre-requisite of a mounted Azure file share toopersist your $Home directory.</span></span> <span data-ttu-id="5b45a-147">Vanliga lagringskostnader gäller.</span><span class="sxs-lookup"><span data-stu-id="5b45a-147">Regular storage costs apply.</span></span>

## <a name="supported-browsers"></a><span data-ttu-id="5b45a-148">Webbläsare som stöds</span><span class="sxs-lookup"><span data-stu-id="5b45a-148">Supported browsers</span></span>
<span data-ttu-id="5b45a-149">Molnet Shell rekommenderas för Chrome, kant och Safari.</span><span class="sxs-lookup"><span data-stu-id="5b45a-149">Cloud Shell is recommended for Chrome, Edge, and Safari.</span></span> <span data-ttu-id="5b45a-150">När molnet Shell stöds för Chrome, Firefox, Safari, Internet Explorer eller Edge, är moln-gränssnittet ämne toospecific webbläsarinställningar.</span><span class="sxs-lookup"><span data-stu-id="5b45a-150">While Cloud Shell is supported for Chrome, Firefox, Safari, IE, and Edge, Cloud Shell is subject toospecific browser settings.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="5b45a-151">Felsökning</span><span class="sxs-lookup"><span data-stu-id="5b45a-151">Troubleshooting</span></span>
1. <span data-ttu-id="5b45a-152">När du använder en prenumeration på Azure Active Directory, det går inte att skapa lagring på grund av tooError: 400 DisallowedOperation.</span><span class="sxs-lookup"><span data-stu-id="5b45a-152">When using an Azure Active Directory subscription, I cannot create storage due tooError: 400 DisallowedOperation.</span></span> <span data-ttu-id="5b45a-153">tooresolve, Använd en Azure-prenumeration kan skapa lagringsresurser.</span><span class="sxs-lookup"><span data-stu-id="5b45a-153">tooresolve this, please use an Azure subscription capable of creating storage resources.</span></span> <span data-ttu-id="5b45a-154">AD-prenumerationer är inte toocreate Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5b45a-154">AD subscriptions are not able toocreate Azure resources.</span></span>

<span data-ttu-id="5b45a-155">Vissa kända begränsningar finns [begränsningar i molnet Shell](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="5b45a-155">For specific known limitations, visit [limitations of Cloud Shell](limitations.md).</span></span>
