---
title: aaaAzure slutpunkter
description: "Beskriver hello Azure tjänstslutpunkten inställningar i hello Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9c6125ec-7278-461e-b69c-ed56e844f742
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 357aa56409a894719077f2c8f302575c8ebb6883
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-endpoints"></a><span data-ttu-id="69dbe-103">Azure-Tjänsteslutpunkter</span><span class="sxs-lookup"><span data-stu-id="69dbe-103">Azure Service Endpoints</span></span>
<span data-ttu-id="69dbe-104">Azure-Tjänsteslutpunkter avgör om ditt program är distribuerade tooand som hanteras av hello globala Azure-plattformen, Azure som drivs av 21Vianet i Kina eller en privat Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="69dbe-104">Azure service endpoints determine whether your application is deployed tooand managed by hello global Azure platform, Azure operated by 21Vianet in China, or a private Azure platform.</span></span> <span data-ttu-id="69dbe-105">Hej **Tjänsteslutpunkter** dialogrutan kan du toospecify som tjänstens slutpunkter som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="69dbe-105">hello **Service Endpoints** dialog allows you toospecify which service endpoints you want toouse.</span></span> <span data-ttu-id="69dbe-106">tooopen hello **Tjänsteslutpunkter** dialogrutan i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure**, och klicka sedan på **Tjänstens slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="69dbe-106">tooopen hello **Service Endpoints** dialog, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Service Endpoints**.</span></span> <span data-ttu-id="69dbe-107">Inställningen hello **Active ange** bestämmer vilka Azure-tjänsten slutpunkter som ska användas för hello Azure projekt i din aktuella arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69dbe-107">Setting hello **Active Set** field determines which Azure service endpoints will be used for hello Azure projects in your current workspace.</span></span>

<span data-ttu-id="69dbe-108">hello följande visar hello **Tjänsteslutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="69dbe-108">hello following shows hello **Service Endpoints** dialog.</span></span>

![][ic719493]

## <a name="tooset-hello-service-endpoints"></a><span data-ttu-id="69dbe-109">tooset hello slutpunkter</span><span class="sxs-lookup"><span data-stu-id="69dbe-109">tooset hello service endpoints</span></span>
<span data-ttu-id="69dbe-110">I hello **Tjänsteslutpunkter** dialogrutan, utför något av hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="69dbe-110">In hello **Service Endpoints** dialog, take one of hello following actions:</span></span>

* <span data-ttu-id="69dbe-111">Om du vill toouse hello globala Azure-plattformen, från hello **Active ange** listrutan, Välj **windowsazure.com** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="69dbe-111">If you want toouse hello global Azure platform, from hello **Active Set** dropdown list, select **windowsazure.com** and click **OK**.</span></span>

* <span data-ttu-id="69dbe-112">Om du vill toouse Azure drivs av 21Vianet i Kina, från hello **Active ange** listrutan, Välj **windowsazure.cn (Kina)** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="69dbe-112">If you want toouse Azure operated by 21Vianet in China, from hello **Active Set** dropdown list, select **windowsazure.cn (China)** and click **OK**.</span></span>

* <span data-ttu-id="69dbe-113">Om du vill toouse privat Azure-plattformen:</span><span class="sxs-lookup"><span data-stu-id="69dbe-113">If you want toouse a private Azure platform:</span></span>

  1. <span data-ttu-id="69dbe-114">Klicka på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="69dbe-114">Click **Edit**.</span></span>

  2. <span data-ttu-id="69dbe-115">Öppnas en dialogruta som talar om att hello **Tjänsteslutpunkter** dialogrutan stängs och hello konfigurationsfil anger öppnas.</span><span class="sxs-lookup"><span data-stu-id="69dbe-115">A dialog box opens, informing you that hello **Service Endpoints** dialog will be closed, and hello preference sets file will be opened.</span></span> <span data-ttu-id="69dbe-116">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="69dbe-116">Click **OK**.</span></span>

  3. <span data-ttu-id="69dbe-117">Skapa en ny i hello preferencesets.xml filen `preferenceset` element.</span><span class="sxs-lookup"><span data-stu-id="69dbe-117">In hello preferencesets.xml file, create a new `preferenceset` element.</span></span> <span data-ttu-id="69dbe-118">Den här nya element, skapa `name`, `blob`, `management`, `portalURL` och `publishsettings` attribut och lägga till värden för dem som motsvarar tooyour privat Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="69dbe-118">For this new element, create `name`, `blob`, `management`, `portalURL` and `publishsettings` attributes, and add values for them that correspond tooyour private Azure platform.</span></span> <span data-ttu-id="69dbe-119">Du kan använda hello värden för hello befintliga `preferenceset` element som en mall.</span><span class="sxs-lookup"><span data-stu-id="69dbe-119">You can use hello values provided for hello existing `preferenceset` elements as templates.</span></span> <span data-ttu-id="69dbe-120">**Obs**: hello värde som används för hello `blob` attributet måste innehålla hello text ”blob” hello-URL.</span><span class="sxs-lookup"><span data-stu-id="69dbe-120">**Note**: hello value used for hello `blob` attribute must contain hello text "blob" in hello URL.</span></span>

  4. <span data-ttu-id="69dbe-121">Spara och Stäng preferencesets.xml.</span><span class="sxs-lookup"><span data-stu-id="69dbe-121">Save and close preferencesets.xml.</span></span>

  5. <span data-ttu-id="69dbe-122">Öppna hello **Tjänsteslutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="69dbe-122">Reopen hello **Service Endpoints** dialog.</span></span>

  6. <span data-ttu-id="69dbe-123">Från hello **Active ange** listrutan, Välj hello active ange som du skapat och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="69dbe-123">From hello **Active Set** dropdown list, select hello active set that you created and click **OK**.</span></span>

  7. <span data-ttu-id="69dbe-124">När du har skapat din privata Azure-plattformen `preferenceset` element, kan du ändra hello värden som har tilldelats tooit genom att klicka på hello **redigera** knapp i hello **Services-ändpunkten** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="69dbe-124">Once you've created your private Azure platform `preferenceset` element, you can change hello values assigned tooit by clicking hello **Edit** button in hello **Services Endpoint** dialog.</span></span> <span data-ttu-id="69dbe-125">Du kan också skapa flera privat Azure-plattformen `preferenceset` element, om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="69dbe-125">You can also create multiple private Azure platform `preferenceset` elements, if you desire.</span></span>

## <a name="see-also"></a><span data-ttu-id="69dbe-126">Se även</span><span class="sxs-lookup"><span data-stu-id="69dbe-126">See Also</span></span>
<span data-ttu-id="69dbe-127">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="69dbe-127">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="69dbe-128">[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="69dbe-128">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="69dbe-129">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="69dbe-129">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="69dbe-130">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="69dbe-130">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
