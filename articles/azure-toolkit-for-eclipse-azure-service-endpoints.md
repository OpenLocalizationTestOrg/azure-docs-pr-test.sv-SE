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
# <a name="azure-service-endpoints"></a>Azure-Tjänsteslutpunkter
Azure-Tjänsteslutpunkter avgör om ditt program är distribuerade tooand som hanteras av hello globala Azure-plattformen, Azure som drivs av 21Vianet i Kina eller en privat Azure-plattformen. Hej **Tjänsteslutpunkter** dialogrutan kan du toospecify som tjänstens slutpunkter som du vill toouse. tooopen hello **Tjänsteslutpunkter** dialogrutan i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure**, och klicka sedan på **Tjänstens slutpunkter**. Inställningen hello **Active ange** bestämmer vilka Azure-tjänsten slutpunkter som ska användas för hello Azure projekt i din aktuella arbetsytan.

hello följande visar hello **Tjänsteslutpunkter** dialogrutan.

![][ic719493]

## <a name="tooset-hello-service-endpoints"></a>tooset hello slutpunkter
I hello **Tjänsteslutpunkter** dialogrutan, utför något av hello följande åtgärder:

* Om du vill toouse hello globala Azure-plattformen, från hello **Active ange** listrutan, Välj **windowsazure.com** och på **OK**.

* Om du vill toouse Azure drivs av 21Vianet i Kina, från hello **Active ange** listrutan, Välj **windowsazure.cn (Kina)** och på **OK**.

* Om du vill toouse privat Azure-plattformen:

  1. Klicka på **Redigera**.

  2. Öppnas en dialogruta som talar om att hello **Tjänsteslutpunkter** dialogrutan stängs och hello konfigurationsfil anger öppnas. Klicka på **OK**.

  3. Skapa en ny i hello preferencesets.xml filen `preferenceset` element. Den här nya element, skapa `name`, `blob`, `management`, `portalURL` och `publishsettings` attribut och lägga till värden för dem som motsvarar tooyour privat Azure-plattformen. Du kan använda hello värden för hello befintliga `preferenceset` element som en mall. **Obs**: hello värde som används för hello `blob` attributet måste innehålla hello text ”blob” hello-URL.

  4. Spara och Stäng preferencesets.xml.

  5. Öppna hello **Tjänsteslutpunkter** dialogrutan.

  6. Från hello **Active ange** listrutan, Välj hello active ange som du skapat och klicka på **OK**.

  7. När du har skapat din privata Azure-plattformen `preferenceset` element, kan du ändra hello värden som har tilldelats tooit genom att klicka på hello **redigera** knapp i hello **Services-ändpunkten** dialogrutan. Du kan också skapa flera privat Azure-plattformen `preferenceset` element, om du vill ha.

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->
