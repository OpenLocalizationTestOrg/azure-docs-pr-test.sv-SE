---
title: "aaaAdd App Service Web Apps för tooAzure en Java-program"
description: "Den här kursen visar hur tooadd en page eller application tooyour instans av Azure App Service Web Apps som redan konfigurerat toouse Java."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 2feb464b2933921ad2887779a6b7589634e2e2f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-java-application-tooazure-app-service-web-apps"></a>Lägg till App Service Web Apps för tooAzure en Java-program
När du har initierat Java-webbappen i [Azure App Service] [ Azure App Service] som beskrivs i [skapar en Java-webbapp i Azure App Service](web-sites-java-get-started.md), kan du överföra ditt program genom att placera din WAR i hello **webbappar** mapp.

Hej navigering sökvägen toohello **webbappar** mappen skiljer sig åt beroende på hur du konfigurerar din Web Apps-instans.

* Om du konfigurerar ditt webbprogram med hjälp av hello Azure Marketplace hello sökvägen toohello **webbappar** mappen är i form av hello **d:\home\site\wwwroot\bin\application\_server\webapps**, där **programmet\_server** är hello namnet på hello programserver gäller för Web Apps-instans. 
* Om du konfigurerar ditt webbprogram med hjälp av hello Azure Konfigurationsgränssnittet hello sökvägen toohello **webbappar** mappen är i form av hello **d:\home\site\wwwroot\webapps**. 

Observera att du kan använda källa kontrollen tooupload programmet eller webbsidor, inklusive [kontinuerlig integrationsscenarier](app-service-continuous-deployment.md). FTP är också ett alternativ för uppladdning av ditt program eller en webbsida. Mer information om hur du distribuerar ditt program via FTP finns [distribuera din app tooAzure Apptjänst].

Obs för Tomcat web apps: när du har överfört din WAR-filen toohello **webbappar** mappen hello Tomcat-programservern känner av att du har lagt till den och kommer automatiskt att läsa in den. Observera att om du kopierar filer (andra än WAR-filer) toohello rotkatalog hello programservern måste toobe startas om innan de filerna som används. hello autoload funktioner för hello Tomcat Java-webbappar körs i Azure är baserad på en ny WAR-fil som läggs till eller toohello lägga till nya filer eller kataloger **webbappar** mapp. 

<a name="see-also"></a>

## <a name="see-also"></a>Se även
Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

[Application-Insights-App-Insights-Java-Get-Started](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[distribuera din app tooAzure Apptjänst]: ./web-sites-deploy.md
