---
title: "aaaDisplaying Javadoc innehåll i Eclipse för hello paketet för Azure-bibliotek för Java"
description: "Hur hello toodisplay Javadoc innehåll för hello Azure Libraries i Eclipse."
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
ms.openlocfilehash: 8070023a24dc07eca8df906db5b8b662ceed6ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-hello-azure-libraries-package-for-java"></a>Visa Javadoc innehåll i Eclipse för hello paketet för Azure-bibliotek för Java
Hej Javadoc innehåll för hello Azure-biblioteken för Java kan visas i Eclipse-miljö genom att associera hello Javadoc innehåll toohello Azure-biblioteken för Java. hello följande steg visar hur toouse den här funktionen i Eclipse.

Den här proceduren förutsätter att du redan har lagt till hello Azure-bibliotek för Java byggsökväg för tooyour.

## <a name="toodisplay-javadoc-content-in-eclipse-for-hello-azure-libraries-for-java"></a>toodisplay Javadoc innehållet i Eclipse för hello Azure-biblioteken för Java
* I Eclipses Projektutforskaren i hello **refererar till bibliotek** avsnitt i ditt projekt, öppna hello snabbmenyn för hello Azure-bibliotek för Java JAR. Till exempel **microsoft-windowsazure-api-0.1.0.jar** (hello versionsnumret kan vara olika, beroende på vilken version som du har installerat).

* Klicka på **Egenskaper**.

* Inom hello **egenskaper** dialogrutan i hello vänstra fönstret, klickar du på **Javadoc plats**. Hej **Javadoc plats** dialogrutan visas.

* Du kan ange en **Javadoc URL**, eller en **Javadoc i arkivet**.

   * Om du väljer toospecify en **Javadoc URL**, Använd hello URL: er som **http://dl.windowsazure.com/javadoc** eller **http://dl.windowsazure.com/storage/javadoc**.

   * Om du väljer toouse **Javadoc i arkivet**, du kan ange en extern fil eller en fil.

   Gör du dina val och bläddra/Validera efter behov. hello följande exempel associerar hello Azure-biblioteken för Java med hello motsvarande Javadoc JAR som har hämtats lokalt tooa mapp med namnet **c:\MyAzureJARs**.

   ![][ic553487]

* *Valfritt steg*: Klicka på **Validera**. Potentiella problem med hello Javadoc JAR kan visas här.

* Klicka på **OK**.

När associerat med hello bibliotek ska hello Javadoc innehåll visas i ditt Eclipse IDE. Till exempel om `blob` definieras av typen `CloudBlockBlob` inom din kod hello följande är ett exempel på Javadoc-innehåll som visas när du skriver `blob.acquireLease` i koden:

![][ic553488]

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
