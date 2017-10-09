---
title: "molnsamlingar för aaaTroubleshoot RemoteApp - skapa | Microsoft Docs"
description: "Lär dig hur tootroubleshoot RemoteApp cloud fel vid skapande av samling"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9484ecbdb048ede8df725215b313e049cc7648f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a><span data-ttu-id="3a05f-103">Felsöka molnsamlingar för att skapa RemoteApp</span><span class="sxs-lookup"><span data-stu-id="3a05f-103">Troubleshoot creating RemoteApp cloud collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3a05f-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="3a05f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="3a05f-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="3a05f-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="3a05f-106">Om du har problem med att skapa en molnsamling kan du kolla hello följande information.</span><span class="sxs-lookup"><span data-stu-id="3a05f-106">If you are having problems creating a cloud collection, check out hello following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="3a05f-107">Bilden är ogiltig</span><span class="sxs-lookup"><span data-stu-id="3a05f-107">Your image is invalid</span></span>
<span data-ttu-id="3a05f-108">Om du ser ett meddelande som ”GoldImageInvalid” när du väntar Azure tooprovision din samling, innebär det att mallavbildningen inte uppfyller hello [definierat avbildningen krav](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="3a05f-108">If you see a message like, "GoldImageInvalid" when you are waiting for Azure tooprovision your collection, it means that your template image doesn't meet hello [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="3a05f-109">Därför gå läsa de [krav](remoteapp-imagereqs.md), åtgärda avbildningen och försök toocreate samlingen igen.</span><span class="sxs-lookup"><span data-stu-id="3a05f-109">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try toocreate your collection again.</span></span>

## <a name="common-errors-seen-in-hello-azure-management-portal"></a><span data-ttu-id="3a05f-110">Vanliga fel som visas i hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="3a05f-110">Common errors seen in hello Azure Management portal</span></span>
    DNS server could not be reached
    ProvisioningTimeout

<span data-ttu-id="3a05f-111">Molnet samlingar ofta misslyckas under skapande av på grund av du använder anpassade avbildningar.</span><span class="sxs-lookup"><span data-stu-id="3a05f-111">Cloud collections often fail during creation because of you are using custom images.</span></span>  <span data-ttu-id="3a05f-112">Om du ser något av hello ovan fel och du använder en anpassad avbildning toocreate hello samling, kontrollera hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="3a05f-112">If you see one of hello above errors and you are using a custom image toocreate hello collection, please check hello following things:</span></span>

* <span data-ttu-id="3a05f-113">Kontrollera att hello anpassad avbildning som du har överfört uppfyller kraven för avbildningen.</span><span class="sxs-lookup"><span data-stu-id="3a05f-113">Ensure that hello custom image you uploaded meets image requirements.</span></span>
* <span data-ttu-id="3a05f-114">Oftast hello vanligt problem är hello avbildningen inte var korrekt Sysprep.</span><span class="sxs-lookup"><span data-stu-id="3a05f-114">Most often hello common problem is that hello image was not properly syspreped.</span></span>  
* <span data-ttu-id="3a05f-115">Kontrollera hello avbildningen kan starta i Hyper-V eller försök att skapa ett IAAS VM direkt i din Azure-prenumeration med hjälp av hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="3a05f-115">Verify hello image can boot within Hyper-V or try creating an IAAS VM directly in your Azure subscription using hello image.</span></span> <span data-ttu-id="3a05f-116">Om hello VM misslyckas tooboot och inte starta sedan indikerar detta vanligtvis den egna avbildningen hello inte förbereddes på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="3a05f-116">If hello VM fails tooboot and not start, then this usually indicates that hello custom image was not prepared correctly.</span></span>  <span data-ttu-id="3a05f-117">Kontrollera hello anpassad avbildning har skapats följande hello hur toocreate en anpassad mall bild för RemoteApp</span><span class="sxs-lookup"><span data-stu-id="3a05f-117">Verify hello custom image was built following hello How toocreate a custom template image for RemoteApp</span></span>

<span data-ttu-id="3a05f-118">Om du använder en av hello Microsoft avbildningarna som ingår i din prenumeration, försök igen toocreate hello-samling.</span><span class="sxs-lookup"><span data-stu-id="3a05f-118">If you are using one of hello Microsoft images included with your subscription, try toocreate hello collection again.</span></span> <span data-ttu-id="3a05f-119">Hello problemet kvarstår sedan kontakta Microsoft support.</span><span class="sxs-lookup"><span data-stu-id="3a05f-119">If hello issue persists then please contact Microsoft support.</span></span>

    PlatformImageTrialModeOnly

<span data-ttu-id="3a05f-120">Om du ser felet detta innebär vanligen som du har uppgraderat tooa betalt konto, men du försöker toouse en avbildning av Microsoft tillhandahållna som är endast giltig vid hello utvärderingsversion läge hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3a05f-120">If you see this error this usually means that you been upgraded tooa paid account but you are trying toouse a Microsoft provided image that is valid only during hello trial mode of hello service.</span></span> <span data-ttu-id="3a05f-121">I det här fallet försök toocreate molnsamling igen, men vara säker på att toospecify hello rätt avbildning.</span><span class="sxs-lookup"><span data-stu-id="3a05f-121">In this case, try toocreate your cloud collection again, but be sure toospecify hello correct image.</span></span>

