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
# <a name="add-a-java-application-tooazure-app-service-web-apps"></a><span data-ttu-id="2e0e0-103">Lägg till App Service Web Apps för tooAzure en Java-program</span><span class="sxs-lookup"><span data-stu-id="2e0e0-103">Add a Java application tooAzure App Service Web Apps</span></span>
<span data-ttu-id="2e0e0-104">När du har initierat Java-webbappen i [Azure App Service] [ Azure App Service] som beskrivs i [skapar en Java-webbapp i Azure App Service](web-sites-java-get-started.md), kan du överföra ditt program genom att placera din WAR i hello **webbappar** mapp.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-104">Once you have initialized your Java web app in [Azure App Service][Azure App Service] as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md), you can upload your application by placing your WAR in hello **webapps** folder.</span></span>

<span data-ttu-id="2e0e0-105">Hej navigering sökvägen toohello **webbappar** mappen skiljer sig åt beroende på hur du konfigurerar din Web Apps-instans.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-105">hello navigation path toohello **webapps** folder differs based on how you set up your Web Apps instance.</span></span>

* <span data-ttu-id="2e0e0-106">Om du konfigurerar ditt webbprogram med hjälp av hello Azure Marketplace hello sökvägen toohello **webbappar** mappen är i form av hello **d:\home\site\wwwroot\bin\application\_server\webapps**, där **programmet\_server** är hello namnet på hello programserver gäller för Web Apps-instans.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-106">If you set up your web app by using hello Azure Marketplace, hello path toohello **webapps** folder is in hello form **d:\home\site\wwwroot\bin\application\_server\webapps**, where **application\_server** is hello name of hello application server in effect for your Web Apps instance.</span></span> 
* <span data-ttu-id="2e0e0-107">Om du konfigurerar ditt webbprogram med hjälp av hello Azure Konfigurationsgränssnittet hello sökvägen toohello **webbappar** mappen är i form av hello **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-107">If you set up your web app by using hello Azure configuration UI, hello path toohello **webapps** folder is in hello form **d:\home\site\wwwroot\webapps**.</span></span> 

<span data-ttu-id="2e0e0-108">Observera att du kan använda källa kontrollen tooupload programmet eller webbsidor, inklusive [kontinuerlig integrationsscenarier](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2e0e0-108">Note that you can use source control tooupload your application or web pages, including [continuous integration scenarios](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="2e0e0-109">FTP är också ett alternativ för uppladdning av ditt program eller en webbsida. Mer information om hur du distribuerar ditt program via FTP finns [distribuera din app tooAzure Apptjänst].</span><span class="sxs-lookup"><span data-stu-id="2e0e0-109">FTP is also an option for uploading your application or web pages; for more information about deploying your applications over FTP, see [Deploy your app tooAzure App Service].</span></span>

<span data-ttu-id="2e0e0-110">Obs för Tomcat web apps: när du har överfört din WAR-filen toohello **webbappar** mappen hello Tomcat-programservern känner av att du har lagt till den och kommer automatiskt att läsa in den.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-110">Note for Tomcat web apps: Once you've uploaded your WAR file toohello **webapps** folder, hello Tomcat application server will detect that you've added it and will automatically load it.</span></span> <span data-ttu-id="2e0e0-111">Observera att om du kopierar filer (andra än WAR-filer) toohello rotkatalog hello programservern måste toobe startas om innan de filerna som används.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-111">Note that if you copy files (other than WAR files) toohello ROOT directory, hello application server will need toobe restarted before those files are used.</span></span> <span data-ttu-id="2e0e0-112">hello autoload funktioner för hello Tomcat Java-webbappar körs i Azure är baserad på en ny WAR-fil som läggs till eller toohello lägga till nya filer eller kataloger **webbappar** mapp.</span><span class="sxs-lookup"><span data-stu-id="2e0e0-112">hello autoload functionality for hello Tomcat Java web apps running on Azure is based on a new WAR file being added, or new files or directories added toohello **webapps** folder.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="2e0e0-113">Se även</span><span class="sxs-lookup"><span data-stu-id="2e0e0-113">See Also</span></span>
<span data-ttu-id="2e0e0-114">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="2e0e0-114">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

[<span data-ttu-id="2e0e0-115">Application-Insights-App-Insights-Java-Get-Started</span><span class="sxs-lookup"><span data-stu-id="2e0e0-115">application-insights-app-insights-java-get-started</span></span>](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[distribuera din app tooAzure Apptjänst]: ./web-sites-deploy.md
