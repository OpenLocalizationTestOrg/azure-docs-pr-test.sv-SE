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
# <a name="azure-service-endpoints"></a>Azure-Tjänsteslutpunkter
Azure-Tjänsteslutpunkter bestämma om programmet distribueras till och hanteras av globala Azure-plattformen, Azure som drivs av 21Vianet i Kina eller en privat Azure-plattformen. Den **Tjänsteslutpunkter** dialogrutan kan du ange vilka Tjänsteslutpunkter som du vill använda. Öppna den **Tjänsteslutpunkter** dialogrutan i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure**, och klicka sedan på **Tjänsteslutpunkter**. Ange den **Active ange** bestämmer vilka Azure-Tjänsteslutpunkter som ska användas för Azure projekten i din aktuella arbetsytan.

I följande visas den **Tjänsteslutpunkter** dialogrutan.

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Ange Tjänsteslutpunkter
I den **Tjänsteslutpunkter** dialogrutan, utför något av följande åtgärder:

* Om du vill använda globala Azure-plattformen, från den **Active ange** listrutan, Välj **windowsazure.com** och på **OK**.

* Om du vill använda Azure drivs av 21Vianet i Kina, från den **Active ange** listrutan, Välj **windowsazure.cn (Kina)** och på **OK**.

* Om du vill använda en privat Azure-plattformen:

  1. Klicka på **Redigera**.

  2. Öppnas en dialogruta som talar om som den **Tjänsteslutpunkter** dialogrutan stängs och inställningar anger filen öppnas. Klicka på **OK**.

  3. Skapa en ny i filen preferencesets.xml `preferenceset` element. Den här nya element, skapa `name`, `blob`, `management`, `portalURL` och `publishsettings` attribut och lägga till värden för dem som är kopplade till ditt privata Azure-plattformen. Du kan använda de angivna värdena för den befintliga `preferenceset` element som en mall. **Obs**: det värde som används för den `blob` attributet måste innehålla text ”blob” i Webbadressen.

  4. Spara och Stäng preferencesets.xml.

  5. Öppna den **Tjänsteslutpunkter** dialogrutan.

  6. Från den **Active ange** listrutan, Välj aktivt ange som du skapat och klicka på **OK**.

  7. När du har skapat din privata Azure-plattformen `preferenceset` element som du kan ändra värdena för den genom att klicka på den **redigera** knappen i den **Services-ändpunkten** dialogrutan. Du kan också skapa flera privat Azure-plattformen `preferenceset` element, om du vill ha.

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
