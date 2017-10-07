---
title: "aaaConfigure Azure Marketplace inställningarna i Azure DevTest Labs | Microsoft Docs"
description: "Konfigurera vilka Azure Marketplace-bilder som kan användas när du skapar en virtuell dator i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: bb4b7f1c0cbe967bee724f7ee20f64f8c4ea58ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a><span data-ttu-id="5a09a-103">Konfigurera inställningar för Azure Marketplace-avbildning i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5a09a-103">Configure Azure Marketplace image settings in Azure DevTest Labs</span></span>
<span data-ttu-id="5a09a-104">DevTest Labs har stöd för att skapa virtuella datorer baserat på Azure Marketplace-bilder beroende på hur du har konfigurerat Azure Marketplace-avbildningar toobe används i labbet.</span><span class="sxs-lookup"><span data-stu-id="5a09a-104">DevTest Labs supports creating VMs based on Azure Marketplace images depending on how you have configured Azure Marketplace images toobe used in your lab.</span></span> <span data-ttu-id="5a09a-105">Den här artikeln beskrivs hur du toospecify som eventuellt Azure Marketplace-bilder kan vara används när du skapar virtuella datorer i ett labb.</span><span class="sxs-lookup"><span data-stu-id="5a09a-105">This article shows you how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span> <span data-ttu-id="5a09a-106">Detta säkerställer att din grupp endast har åtkomst toohello Marketplace-bilder som de behöver.</span><span class="sxs-lookup"><span data-stu-id="5a09a-106">This ensures that your team only has access toohello Marketplace images they need.</span></span> 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a><span data-ttu-id="5a09a-107">Välj vilka Azure Marketplace-bilder tillåts när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5a09a-107">Select which Azure Marketplace images are allowed when creating a VM</span></span>
1. <span data-ttu-id="5a09a-108">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="5a09a-108">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="5a09a-109">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="5a09a-109">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="5a09a-110">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="5a09a-110">From hello list of labs, select hello desired lab.</span></span> 
4. <span data-ttu-id="5a09a-111">På bladet hello lab väljer **konfiguration och principer**.</span><span class="sxs-lookup"><span data-stu-id="5a09a-111">On hello lab's blade, select **Configuration and policies**.</span></span>
5. <span data-ttu-id="5a09a-112">På lab's **konfiguration och principer** bladet under **virtuella Datorbaser**väljer **Marketplace-bilder**.</span><span class="sxs-lookup"><span data-stu-id="5a09a-112">On lab's **Configuration and policies** blade under **Virtual Machine Bases**, select **Marketplace images**.</span></span>
6. <span data-ttu-id="5a09a-113">Ange om du vill att alla hello kvalificerade Azure Marketplace-avbildningar toobe användas som bas för en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5a09a-113">Specify whether you want all hello qualified Azure Marketplace images toobe available for use as a base of a new VM.</span></span> <span data-ttu-id="5a09a-114">Om du väljer **Ja**, sedan alla hello Azure Marketplace-bilder som uppfyller alla hello följande villkor är tillåtna i hello testlabb:</span><span class="sxs-lookup"><span data-stu-id="5a09a-114">If you select **Yes**, then all hello Azure Marketplace images that meet all hello following criteria are allowed in hello lab:</span></span>
   
   * <span data-ttu-id="5a09a-115">hello bild skapas en enda virtuell dator **och**</span><span class="sxs-lookup"><span data-stu-id="5a09a-115">hello image creates a single VM, **and**</span></span>
   * <span data-ttu-id="5a09a-116">hello avbildningen använder Azure Resource Manager tooprovision VMs **och**</span><span class="sxs-lookup"><span data-stu-id="5a09a-116">hello image uses Azure Resource Manager tooprovision VMs, **and**</span></span>
   * <span data-ttu-id="5a09a-117">hello bilden behöver inte köpa ett extra licensieringsplan</span><span class="sxs-lookup"><span data-stu-id="5a09a-117">hello image doesn't require purchasing an extra licensing plan</span></span>
     
    <span data-ttu-id="5a09a-118">Om du vill att inga bilder toobe tillåts eller toospecify vilka bilder kan användas, Välj önskade **nr**.</span><span class="sxs-lookup"><span data-stu-id="5a09a-118">If you want no images toobe allowed, or you want toospecify which images can be used, select **No**.</span></span>
     
     ![Alternativet tooallow alla Marketplace-avbildningar toobe används som bas avbildningar för virtuella datorer](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. <span data-ttu-id="5a09a-120">Om du väljer **nr** toohello föregående steg, hello **tillåtna bilder/Välj alla** kryssrutan aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5a09a-120">If you select **No** toohello previous step, hello **Allowed images/Select all** checkbox is enabled.</span></span> 
   <span data-ttu-id="5a09a-121">Du kan använda det här alternativet tillsammans med hello Sök tooquickly Markera eller avmarkera alla hello objekt visas i hello lista.</span><span class="sxs-lookup"><span data-stu-id="5a09a-121">You can use this option together with hello search box tooquickly select or deselect all hello items displayed in hello list.</span></span>
   * <span data-ttu-id="5a09a-122">Välj önskade tooallow för att skapa Virtuella individuellt genom att markera kryssrutan för varje avbildning motsvarande hello Azure Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="5a09a-122">Select hello Azure Marketplace images you want tooallow for VM creation individually by checking each image's corresponding checkbox.</span></span>
   * <span data-ttu-id="5a09a-123">Välj ingenting hello-listan om du inte vill tooallow alla Azure Marketplace-avbildningar toobe används i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="5a09a-123">Select nothing from hello list if you don't want tooallow any Azure Marketplace images toobe used in hello lab.</span></span>
   
    ![Du kan ange vilken Azure Marketplace-avbildningar kan användas som bas avbildningar för virtuella datorer](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="5a09a-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a09a-125">Next steps</span></span>
<span data-ttu-id="5a09a-126">När du har konfigurerat hur Azure Marketplace-bilder tillåts när du skapar en virtuell dator, hello nästa steg är för[lägga till ett labb för VM-tooyour](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="5a09a-126">Once you have configured how Azure Marketplace images are allowed when creating a VM, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

