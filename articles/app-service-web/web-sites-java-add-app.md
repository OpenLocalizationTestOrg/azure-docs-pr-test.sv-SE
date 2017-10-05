---
title: "Lägga till en Java-program i Azure App Service Web Apps"
description: "Den här kursen visar hur du lägger till en sida eller ett program till din Azure App Service Web Apps som redan är konfigurerad för att använda Java-instans."
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
ms.openlocfilehash: c28e7c499ed02b759df580f4b14a971b6aec5b67
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a><span data-ttu-id="c6490-103">Lägga till en Java-program i Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="c6490-103">Add a Java application to Azure App Service Web Apps</span></span>
<span data-ttu-id="c6490-104">När du har initierat Java-webbappen i [Azure App Service] [ Azure App Service] som beskrivs i [skapar en Java-webbapp i Azure App Service](web-sites-java-get-started.md), kan du överföra ditt program genom att placera din WAR i den **webbappar** mapp.</span><span class="sxs-lookup"><span data-stu-id="c6490-104">Once you have initialized your Java web app in [Azure App Service][Azure App Service] as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md), you can upload your application by placing your WAR in the **webapps** folder.</span></span>

<span data-ttu-id="c6490-105">Navigeringssökvägen till den **webbappar** mappen skiljer sig åt beroende på hur du konfigurerar din Web Apps-instans.</span><span class="sxs-lookup"><span data-stu-id="c6490-105">The navigation path to the **webapps** folder differs based on how you set up your Web Apps instance.</span></span>

* <span data-ttu-id="c6490-106">Om du har lagt upp ditt webbprogram med hjälp av Azure Marketplace sökvägen till den **webbappar** mappen är i formatet **d:\home\site\wwwroot\bin\application\_server\webapps**, där **programmet\_server** är namnet på programservern gäller för Web Apps-instans.</span><span class="sxs-lookup"><span data-stu-id="c6490-106">If you set up your web app by using the Azure Marketplace, the path to the **webapps** folder is in the form **d:\home\site\wwwroot\bin\application\_server\webapps**, where **application\_server** is the name of the application server in effect for your Web Apps instance.</span></span> 
* <span data-ttu-id="c6490-107">Om du har lagt upp ditt webbprogram med hjälp av Azure Konfigurationsgränssnittet, sökvägen till den **webbappar** mappen är i formatet **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="c6490-107">If you set up your web app by using the Azure configuration UI, the path to the **webapps** folder is in the form **d:\home\site\wwwroot\webapps**.</span></span> 

<span data-ttu-id="c6490-108">Observera att du kan använda källkontrollen för att överföra programmet eller webbsidor, inklusive [kontinuerlig integrationsscenarier](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="c6490-108">Note that you can use source control to upload your application or web pages, including [continuous integration scenarios](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="c6490-109">FTP är också ett alternativ för uppladdning av ditt program eller en webbsida. Mer information om hur du distribuerar ditt program via FTP finns [distribuera din app till Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="c6490-109">FTP is also an option for uploading your application or web pages; for more information about deploying your applications over FTP, see [Deploy your app to Azure App Service].</span></span>

<span data-ttu-id="c6490-110">Obs för Tomcat web apps: när du har överfört WAR-filen till den **webbappar** mappen Tomcat-programservern känner av att du har lagt till den och kommer automatiskt att läsa in den.</span><span class="sxs-lookup"><span data-stu-id="c6490-110">Note for Tomcat web apps: Once you've uploaded your WAR file to the **webapps** folder, the Tomcat application server will detect that you've added it and will automatically load it.</span></span> <span data-ttu-id="c6490-111">Observera att om du kopierar filer (andra än WAR-filer) till rotkatalogen programservern måste startas om innan de filerna som används.</span><span class="sxs-lookup"><span data-stu-id="c6490-111">Note that if you copy files (other than WAR files) to the ROOT directory, the application server will need to be restarted before those files are used.</span></span> <span data-ttu-id="c6490-112">Funktionen autoload för Tomcat Java-webbprogram som körs på Azure baseras på en ny WAR-fil som läggs till, eller lägga till nya filer eller kataloger i **webbappar** mapp.</span><span class="sxs-lookup"><span data-stu-id="c6490-112">The autoload functionality for the Tomcat Java web apps running on Azure is based on a new WAR file being added, or new files or directories added to the **webapps** folder.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="c6490-113">Se även</span><span class="sxs-lookup"><span data-stu-id="c6490-113">See Also</span></span>
<span data-ttu-id="c6490-114">Mer information om hur du använder Java i Azure finns i [Java-utvecklingscenter].</span><span class="sxs-lookup"><span data-stu-id="c6490-114">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

[<span data-ttu-id="c6490-115">Application-Insights-App-Insights-Java-Get-Started</span><span class="sxs-lookup"><span data-stu-id="c6490-115">application-insights-app-insights-java-get-started</span></span>](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

<span data-ttu-id="c6490-116">[Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="c6490-116">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
<span data-ttu-id="c6490-117">[distribuera din app till Azure App Service]: ./web-sites-deploy.md</span><span class="sxs-lookup"><span data-stu-id="c6490-117">[Deploy your app to Azure App Service]: ./web-sites-deploy.md</span></span>
