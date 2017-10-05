---
title: "Visa Javadoc innehåll i Eclipse för paketet Azure-bibliotek för Java"
description: "Så här visar du Javadoc-innehåll för Azure-biblioteken i Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Visa Javadoc innehåll i Eclipse för paketet Azure-bibliotek för Java
Javadoc innehållet för Azure-biblioteken för Java kan visas i Eclipse-miljö genom att associera Javadoc innehållet till Azure-biblioteken för Java. Följande steg visar hur du använder den här funktionen i Eclipse.

Den här proceduren förutsätter att du redan har lagt till Azure-bibliotek för Java build-sökväg.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Att visa Javadoc innehåll i Eclipse för Azure-biblioteken för Java
* I Eclipses Projektutforskaren i den **refererar till bibliotek** avsnitt i ditt projekt, öppna snabbmenyn för Azure-bibliotek för Java JAR. Till exempel **microsoft-windowsazure-api-0.1.0.jar** (versionsnumret kan vara olika, beroende på vilken version som du har installerat).

* Klicka på **Egenskaper**.

* I den **egenskaper** dialogrutan i det vänstra fönstret klickar du på **Javadoc plats**. Den **Javadoc plats** dialogrutan visas.

* Du kan ange en **Javadoc URL**, eller en **Javadoc i arkivet**.

   * Om du väljer att ange en **Javadoc URL**, Använd webbadresserna som **http://dl.windowsazure.com/javadoc** eller **http://dl.windowsazure.com/storage/javadoc**.

   * Om du väljer att använda **Javadoc i arkivet**, du kan ange en extern fil eller en fil.

   Gör du dina val och bläddra/Validera efter behov. I följande exempel associerar Azure-biblioteken för Java med det motsvarande Javadoc JAR som har hämtats lokalt till en mapp med namnet **c:\MyAzureJARs**.

   ![][ic553487]

* *Valfritt steg*: Klicka på **Validera**. Potentiella problem med Javadoc JAR kan visas här.

* Klicka på **OK**.

När associerat med biblioteket ska Javadoc innehåll visas i ditt Eclipse IDE. Till exempel om `blob` definieras av typen `CloudBlockBlob` inom din kod följande är ett exempel på Javadoc-innehåll som visas när du skriver `blob.acquireLease` i koden:

![][ic553488]

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
