---
title: "Azure-Tjänsteslutpunkter"
description: "Beskriver Azure tjänstslutpunkten inställningarna i Azure-verktygen för Eclipse."
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
ms.openlocfilehash: 6059c292c2687f1bf3d9be04c03aaaaf6adde945
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-endpoints"></a><span data-ttu-id="7650c-103">Azure-Tjänsteslutpunkter</span><span class="sxs-lookup"><span data-stu-id="7650c-103">Azure Service Endpoints</span></span>
<span data-ttu-id="7650c-104">Azure-Tjänsteslutpunkter bestämma om programmet distribueras till och hanteras av globala Azure-plattformen, Azure som drivs av 21Vianet i Kina eller en privat Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7650c-104">Azure service endpoints determine whether your application is deployed to and managed by the global Azure platform, Azure operated by 21Vianet in China, or a private Azure platform.</span></span> <span data-ttu-id="7650c-105">Den **Tjänsteslutpunkter** dialogrutan kan du ange vilka Tjänsteslutpunkter som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="7650c-105">The **Service Endpoints** dialog allows you to specify which service endpoints you want to use.</span></span> <span data-ttu-id="7650c-106">Öppna den **Tjänsteslutpunkter** dialogrutan i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure**, och klicka sedan på **Tjänsteslutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="7650c-106">To open the **Service Endpoints** dialog, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Service Endpoints**.</span></span> <span data-ttu-id="7650c-107">Ange den **Active ange** bestämmer vilka Azure-Tjänsteslutpunkter som ska användas för Azure projekten i din aktuella arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7650c-107">Setting the **Active Set** field determines which Azure service endpoints will be used for the Azure projects in your current workspace.</span></span>

<span data-ttu-id="7650c-108">I följande visas den **Tjänsteslutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7650c-108">The following shows the **Service Endpoints** dialog.</span></span>

![][ic719493]

## <a name="to-set-the-service-endpoints"></a><span data-ttu-id="7650c-109">Ange Tjänsteslutpunkter</span><span class="sxs-lookup"><span data-stu-id="7650c-109">To set the service endpoints</span></span>
<span data-ttu-id="7650c-110">I den **Tjänsteslutpunkter** dialogrutan, utför något av följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="7650c-110">In the **Service Endpoints** dialog, take one of the following actions:</span></span>

* <span data-ttu-id="7650c-111">Om du vill använda globala Azure-plattformen, från den **Active ange** listrutan, Välj **windowsazure.com** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7650c-111">If you want to use the global Azure platform, from the **Active Set** dropdown list, select **windowsazure.com** and click **OK**.</span></span>

* <span data-ttu-id="7650c-112">Om du vill använda Azure drivs av 21Vianet i Kina, från den **Active ange** listrutan, Välj **windowsazure.cn (Kina)** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7650c-112">If you want to use Azure operated by 21Vianet in China, from the **Active Set** dropdown list, select **windowsazure.cn (China)** and click **OK**.</span></span>

* <span data-ttu-id="7650c-113">Om du vill använda en privat Azure-plattformen:</span><span class="sxs-lookup"><span data-stu-id="7650c-113">If you want to use a private Azure platform:</span></span>

  1. <span data-ttu-id="7650c-114">Klicka på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="7650c-114">Click **Edit**.</span></span>

  2. <span data-ttu-id="7650c-115">Öppnas en dialogruta som talar om som den **Tjänsteslutpunkter** dialogrutan stängs och inställningar anger filen öppnas.</span><span class="sxs-lookup"><span data-stu-id="7650c-115">A dialog box opens, informing you that the **Service Endpoints** dialog will be closed, and the preference sets file will be opened.</span></span> <span data-ttu-id="7650c-116">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7650c-116">Click **OK**.</span></span>

  3. <span data-ttu-id="7650c-117">Skapa en ny i filen preferencesets.xml `preferenceset` element.</span><span class="sxs-lookup"><span data-stu-id="7650c-117">In the preferencesets.xml file, create a new `preferenceset` element.</span></span> <span data-ttu-id="7650c-118">Den här nya element, skapa `name`, `blob`, `management`, `portalURL` och `publishsettings` attribut och lägga till värden för dem som är kopplade till ditt privata Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7650c-118">For this new element, create `name`, `blob`, `management`, `portalURL` and `publishsettings` attributes, and add values for them that correspond to your private Azure platform.</span></span> <span data-ttu-id="7650c-119">Du kan använda de angivna värdena för den befintliga `preferenceset` element som en mall.</span><span class="sxs-lookup"><span data-stu-id="7650c-119">You can use the values provided for the existing `preferenceset` elements as templates.</span></span> <span data-ttu-id="7650c-120">**Obs**: det värde som används för den `blob` attributet måste innehålla text ”blob” i Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="7650c-120">**Note**: The value used for the `blob` attribute must contain the text "blob" in the URL.</span></span>

  4. <span data-ttu-id="7650c-121">Spara och Stäng preferencesets.xml.</span><span class="sxs-lookup"><span data-stu-id="7650c-121">Save and close preferencesets.xml.</span></span>

  5. <span data-ttu-id="7650c-122">Öppna den **Tjänsteslutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7650c-122">Reopen the **Service Endpoints** dialog.</span></span>

  6. <span data-ttu-id="7650c-123">Från den **Active ange** listrutan, Välj aktivt ange som du skapat och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7650c-123">From the **Active Set** dropdown list, select the active set that you created and click **OK**.</span></span>

  7. <span data-ttu-id="7650c-124">När du har skapat din privata Azure-plattformen `preferenceset` element som du kan ändra värdena för den genom att klicka på den **redigera** knappen i den **Services-ändpunkten** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7650c-124">Once you've created your private Azure platform `preferenceset` element, you can change the values assigned to it by clicking the **Edit** button in the **Services Endpoint** dialog.</span></span> <span data-ttu-id="7650c-125">Du kan också skapa flera privat Azure-plattformen `preferenceset` element, om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="7650c-125">You can also create multiple private Azure platform `preferenceset` elements, if you desire.</span></span>

## <a name="see-also"></a><span data-ttu-id="7650c-126">Se även</span><span class="sxs-lookup"><span data-stu-id="7650c-126">See Also</span></span>
<span data-ttu-id="7650c-127">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7650c-127">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="7650c-128">[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7650c-128">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="7650c-129">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7650c-129">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="7650c-130">Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="7650c-130">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
