---
title: "aaaSync innehåll från ett moln mappen tooAzure Apptjänst"
description: "Lär dig hur toodeploy din app tooAzure Apptjänst via innehåll Synkronisera från en mapp i molnet."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: e1c6d53a427c36126d9cdb33cc21b4126b9d9c2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sync-content-from-a-cloud-folder-tooazure-app-service"></a>Synkronisera innehåll från ett moln mappen tooAzure Apptjänst
De här självstudierna visar hur toodeploy för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) genom att synkronisera innehållet från populära lagring molntjänster som Dropbox och OneDrive. 

## <a name="overview"></a>Översikt över distribution av innehåll synkronisering
hello innehåll på begäran-synkronisering distribution drivs av hello [Kudu-distributionsmotorn](https://github.com/projectkudu/kudu/wiki) integrerad med App Service. I hello [Azure Portal](https://portal.azure.com), kan du utse en mapp i molnlagringen, arbeta med din app koden och innehållet i mappen och sync tooApp Service med hello på en knapp. Synkronisering av innehåll använder hello Kudu-processen för att bygga och distribution. 

## <a name="contentsync"></a>Hur tooenable innehåll synkronisera distribution
tooenable innehåll sync från hello [Azure Portal](https://portal.azure.com), gör du följande:

1. I bladet för din app i hello Azure-portalen klickar du på **inställningar** > **distributionskälla**. Klicka på **Välj källa**och välj **OneDrive** eller **Dropbox** som hello källa för distribution. 
   
    ![Synkronisering av innehåll](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > På grund av underliggande skillnader i hello API: er, **OneDrive för företag** stöds inte just nu. 
   > 
   > 
2. Fullständig hello auktorisering arbetsflöde tooenable Apptjänst tooaccess en specifik fördefinierade angiven sökväg för OneDrive eller Dropbox där allt Apptjänst innehåll sparas.  
    Efter tillstånd hello Apptjänst-plattformen ger utsedda hello alternativet toocreate innehållsmappen under hello sökväg för innehåll eller toochoose en befintlig mapp som innehåll under den här avsedda sökväg för innehåll. hello avses sökvägar till innehåll under din molnlagringskonton som används för synkronisering av Apptjänst är hello följande:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Efter hello kan inledande innehåll synkronisering hello innehåll synkronisering startas på begäran från hello Azure-portalen. Distributionshistoriken är tillgänglig med hello **distributioner** bladet.
   
    ![Distributionshistoriken](./media/app-service-deploy-content-sync/onedrive_sync.png)

Mer information om Dropbox distributionen är tillgänglig under [distribuera från Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 

