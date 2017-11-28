---
title: "Ladda upp en anpassad avbildning för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du laddar upp en anpassad avbildning för Azure RemoteApp"
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
ms.openlocfilehash: 5a235fac88d6e95ea294bda197641108acb4a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a><span data-ttu-id="dfde6-103">Ladda upp en anpassad avbildning för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="dfde6-103">Upload a custom image for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="dfde6-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="dfde6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="dfde6-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="dfde6-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="dfde6-106">Nu när du har skapat en egeninställd mallavbildning eller har uppdaterats med ändringar som du behöver överför avbildningen till din Azure RemoteApp-bildbibliotek.</span><span class="sxs-lookup"><span data-stu-id="dfde6-106">Now that you have created your custom template image or have updated it with changes, you need to upload that image to your Azure RemoteApp image library.</span></span> <span data-ttu-id="dfde6-107">Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="dfde6-107">Use these steps.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="dfde6-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="dfde6-108">Before you start</span></span>
1. <span data-ttu-id="dfde6-109">Verifiera den anpassade avbildningen uppfyller de [bildkrav](remoteapp-imagereqs.md) och [programkrav](remoteapp-appreqs.md).</span><span class="sxs-lookup"><span data-stu-id="dfde6-109">Verify your custom image meets the [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).</span></span>
2. <span data-ttu-id="dfde6-110">Installera den [Azure PowerShell-modulen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dfde6-110">Install the [Azure PowerShell module](/powershell/azure/overview).</span></span>

## <a name="step-by-step-on-how-to-upload-custom-image"></a><span data-ttu-id="dfde6-111">Steg för steg om hur du överför anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="dfde6-111">Step by step on how to upload custom image</span></span>
1. <span data-ttu-id="dfde6-112">Öppna Azure-hanteringsportalen och gå till sidan RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="dfde6-112">Open Azure Management Portal and navigate to the RemoteApp page.</span></span>
2. <span data-ttu-id="dfde6-113">På den **mallavbildningarna** klickar du på **överför** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="dfde6-113">On the **Template images** tab, click **Upload** at the bottom of the page.</span></span>
3. <span data-ttu-id="dfde6-114">Ange ett eget namn för bilden och ange kontot lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="dfde6-114">Enter a friendly name for your image and specify the storage account location.</span></span> <span data-ttu-id="dfde6-115">Se till att platsen är samma plats som RemoteApp-samling eller en plats där du vill skapa en.</span><span class="sxs-lookup"><span data-stu-id="dfde6-115">Ensure the location is the same location as your RemoteApp collection or a location where you want to create one.</span></span>
4. <span data-ttu-id="dfde6-116">När du uppmanas, hämta skriptet till din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="dfde6-116">When prompted, download the script to your local PC.</span></span>
5. <span data-ttu-id="dfde6-117">Kopiera kommandoparametrarna i textrutan till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="dfde6-117">Copy the command parameters in the text box to your clipboard.</span></span>
6. <span data-ttu-id="dfde6-118">Öppna en kommandotolk med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dfde6-118">Open an elevated Windows PowerShell window.</span></span>
7. <span data-ttu-id="dfde6-119">Gå till samma katalog där du har hämtat skriptet från en upphöjd Windows PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="dfde6-119">From the elevated Windows PowerShell window, navigate to the same directory where you downloaded the script.</span></span>
8. <span data-ttu-id="dfde6-120">Klistra in den kopierade kommando och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="dfde6-120">Paste the copied command and press **Enter**.</span></span>
   
   <span data-ttu-id="dfde6-121">Överföringen startar och varaktighet beror på många faktorer, inklusive nätverkshastigheten och storlek</span><span class="sxs-lookup"><span data-stu-id="dfde6-121">The upload process will begin and duration may depend on many factors including your network speed and size of the image</span></span>
9. <span data-ttu-id="dfde6-122">Om överföra misslyckas på grund av avbrott i nätverket eller saker som de som kan du alltid återuppta överföringen du började.</span><span class="sxs-lookup"><span data-stu-id="dfde6-122">If your upload does not succeed because of network interruption or things like that, you can always resume the upload process you began.</span></span> <span data-ttu-id="dfde6-123">Kör skriptet igen med samma kommandorad för att återuppta en överföring.</span><span class="sxs-lookup"><span data-stu-id="dfde6-123">To resume an upload, run the script again using the same command line.</span></span>

> [!WARNING]
> <span data-ttu-id="dfde6-124">Ändra aldrig överföringsskriptet.</span><span class="sxs-lookup"><span data-stu-id="dfde6-124">Never modify the upload script.</span></span> <span data-ttu-id="dfde6-125">Specifika kontroller har genomförts för att säkerställa att avbildningen uppfyller avbildningen kraven och programkrav.</span><span class="sxs-lookup"><span data-stu-id="dfde6-125">Specific checks have been implemented to ensure that the image meets the image requirements and application requirements.</span></span>
> 
> 

## <a name="common-problems"></a><span data-ttu-id="dfde6-126">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="dfde6-126">Common problems</span></span>
* <span data-ttu-id="dfde6-127">Kontrollera att du använder Windows PowerShell, inte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dfde6-127">Make sure you use Windows PowerShell, not Azure PowerShell.</span></span> <span data-ttu-id="dfde6-128">Du måste installera Azure PowerShell-modulen eftersom vissa moduler behövs under överföringen.</span><span class="sxs-lookup"><span data-stu-id="dfde6-128">You need to install the Azure PowerShell module because certain modules are needed during the upload process.</span></span>
* <span data-ttu-id="dfde6-129">Ändra aldrig skriptet, verifieringar finns det för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="dfde6-129">Never alter the script, validations are there for your convenience.</span></span>
* <span data-ttu-id="dfde6-130">Om vhd-filen hämtar utelåst under överföring, kopiera filen eller flytta den till en ny överför plats och försök igen.</span><span class="sxs-lookup"><span data-stu-id="dfde6-130">If the vhd file gets locked out during upload, copy the file or move it to a new location and attempt upload again.</span></span> <span data-ttu-id="dfde6-131">Det kan finnas vissa Windows-process som hindrar överföringen.</span><span class="sxs-lookup"><span data-stu-id="dfde6-131">There might be some Windows process that is preventing upload.</span></span>  

