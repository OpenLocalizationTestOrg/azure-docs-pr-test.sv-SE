---
title: "aaaUpload en anpassad avbildning för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooupload en anpassad avbildning för Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
ms.openlocfilehash: 6ad40fe58795ece37f4c7900be01bc713938da87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a><span data-ttu-id="a7905-103">Ladda upp en anpassad avbildning för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="a7905-103">Upload a custom image for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a7905-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="a7905-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a7905-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="a7905-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="a7905-106">Nu när du har skapat en egeninställd mallavbildning eller har uppdaterats med ändringar, måste du tooupload bildbibliotek det bild tooyour Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a7905-106">Now that you have created your custom template image or have updated it with changes, you need tooupload that image tooyour Azure RemoteApp image library.</span></span> <span data-ttu-id="a7905-107">Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="a7905-107">Use these steps.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="a7905-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a7905-108">Before you start</span></span>
1. <span data-ttu-id="a7905-109">Verifiera den anpassade avbildningen uppfyller hello [bildkrav](remoteapp-imagereqs.md) och [programkrav](remoteapp-appreqs.md).</span><span class="sxs-lookup"><span data-stu-id="a7905-109">Verify your custom image meets hello [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).</span></span>
2. <span data-ttu-id="a7905-110">Installera hello [Azure PowerShell-modulen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a7905-110">Install hello [Azure PowerShell module](/powershell/azure/overview).</span></span>

## <a name="step-by-step-on-how-tooupload-custom-image"></a><span data-ttu-id="a7905-111">Steg för steg om hur tooupload anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="a7905-111">Step by step on how tooupload custom image</span></span>
1. <span data-ttu-id="a7905-112">Öppna Azure-hanteringsportalen och navigera toohello RemoteApp-sidan.</span><span class="sxs-lookup"><span data-stu-id="a7905-112">Open Azure Management Portal and navigate toohello RemoteApp page.</span></span>
2. <span data-ttu-id="a7905-113">På hello **mallavbildningarna** klickar du på **överför** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="a7905-113">On hello **Template images** tab, click **Upload** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="a7905-114">Ange ett eget namn för bilden och ange hello lagringskontoplatsen.</span><span class="sxs-lookup"><span data-stu-id="a7905-114">Enter a friendly name for your image and specify hello storage account location.</span></span> <span data-ttu-id="a7905-115">Kontrollera hello plats är hello samma plats som RemoteApp-samling eller en plats där du vill toocreate en.</span><span class="sxs-lookup"><span data-stu-id="a7905-115">Ensure hello location is hello same location as your RemoteApp collection or a location where you want toocreate one.</span></span>
4. <span data-ttu-id="a7905-116">När du uppmanas att hämta hello skriptet tooyour lokala dator.</span><span class="sxs-lookup"><span data-stu-id="a7905-116">When prompted, download hello script tooyour local PC.</span></span>
5. <span data-ttu-id="a7905-117">Kopiera hello parametrar i hello text rutan tooyour Urklipp.</span><span class="sxs-lookup"><span data-stu-id="a7905-117">Copy hello command parameters in hello text box tooyour clipboard.</span></span>
6. <span data-ttu-id="a7905-118">Öppna en kommandotolk med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a7905-118">Open an elevated Windows PowerShell window.</span></span>
7. <span data-ttu-id="a7905-119">Utökade Windows PowerShell-fönstret från hello navigera toohello samma katalog där du hämtade hello skript.</span><span class="sxs-lookup"><span data-stu-id="a7905-119">From hello elevated Windows PowerShell window, navigate toohello same directory where you downloaded hello script.</span></span>
8. <span data-ttu-id="a7905-120">Klistra in hello kopieras kommando och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a7905-120">Paste hello copied command and press **Enter**.</span></span>
   
   <span data-ttu-id="a7905-121">hello överföringen startar och varaktighet beror på många faktorer, inklusive nätverkshastigheten och hello-bild</span><span class="sxs-lookup"><span data-stu-id="a7905-121">hello upload process will begin and duration may depend on many factors including your network speed and size of hello image</span></span>
9. <span data-ttu-id="a7905-122">Om överföra misslyckas på grund av avbrott i nätverket eller saker som de som kan du alltid återuppta hello överföringsprocessen du började.</span><span class="sxs-lookup"><span data-stu-id="a7905-122">If your upload does not succeed because of network interruption or things like that, you can always resume hello upload process you began.</span></span> <span data-ttu-id="a7905-123">tooresume en överföring kör hello skriptet igen med hello samma kommandorad.</span><span class="sxs-lookup"><span data-stu-id="a7905-123">tooresume an upload, run hello script again using hello same command line.</span></span>

> [!WARNING]
> <span data-ttu-id="a7905-124">Ändra aldrig hello överför skript.</span><span class="sxs-lookup"><span data-stu-id="a7905-124">Never modify hello upload script.</span></span> <span data-ttu-id="a7905-125">Specifika kontroller har implementerat tooensure som hello avbildningen uppfyller hello avbildningen krav och programkrav.</span><span class="sxs-lookup"><span data-stu-id="a7905-125">Specific checks have been implemented tooensure that hello image meets hello image requirements and application requirements.</span></span>
> 
> 

## <a name="common-problems"></a><span data-ttu-id="a7905-126">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="a7905-126">Common problems</span></span>
* <span data-ttu-id="a7905-127">Kontrollera att du använder Windows PowerShell, inte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a7905-127">Make sure you use Windows PowerShell, not Azure PowerShell.</span></span> <span data-ttu-id="a7905-128">Du behöver tooinstall hello Azure PowerShell-modulen eftersom vissa moduler behövs under hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="a7905-128">You need tooinstall hello Azure PowerShell module because certain modules are needed during hello upload process.</span></span>
* <span data-ttu-id="a7905-129">Ändra aldrig hello skript, verifieringar finns det för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="a7905-129">Never alter hello script, validations are there for your convenience.</span></span>
* <span data-ttu-id="a7905-130">Om hello vhd-filen hämtar utelåst under överföring, kopiera hello filen eller flytta den överför för tooa ny plats och försök igen.</span><span class="sxs-lookup"><span data-stu-id="a7905-130">If hello vhd file gets locked out during upload, copy hello file or move it tooa new location and attempt upload again.</span></span> <span data-ttu-id="a7905-131">Det kan finnas vissa Windows-process som hindrar överföringen.</span><span class="sxs-lookup"><span data-stu-id="a7905-131">There might be some Windows process that is preventing upload.</span></span>  

