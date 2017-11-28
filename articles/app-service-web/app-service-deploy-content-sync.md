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
# <a name="sync-content-from-a-cloud-folder-tooazure-app-service"></a><span data-ttu-id="8dae3-103">Synkronisera innehåll från ett moln mappen tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="8dae3-103">Sync content from a cloud folder tooAzure App Service</span></span>
<span data-ttu-id="8dae3-104">De här självstudierna visar hur toodeploy för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) genom att synkronisera innehållet från populära lagring molntjänster som Dropbox och OneDrive.</span><span class="sxs-lookup"><span data-stu-id="8dae3-104">This tutorial shows you how toodeploy too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by syncing your content from popular cloud storage services like Dropbox and OneDrive.</span></span> 

## <span data-ttu-id="8dae3-105"><a name="overview"></a>Översikt över distribution av innehåll synkronisering</span><span class="sxs-lookup"><span data-stu-id="8dae3-105"><a name="overview"></a>Overview of content sync deployment</span></span>
<span data-ttu-id="8dae3-106">hello innehåll på begäran-synkronisering distribution drivs av hello [Kudu-distributionsmotorn](https://github.com/projectkudu/kudu/wiki) integrerad med App Service.</span><span class="sxs-lookup"><span data-stu-id="8dae3-106">hello on-demand content sync deployment is powered by hello [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki) integrated with App Service.</span></span> <span data-ttu-id="8dae3-107">I hello [Azure Portal](https://portal.azure.com), kan du utse en mapp i molnlagringen, arbeta med din app koden och innehållet i mappen och sync tooApp Service med hello på en knapp.</span><span class="sxs-lookup"><span data-stu-id="8dae3-107">In hello [Azure Portal](https://portal.azure.com), you can designate a folder in your cloud storage, work with your app code and content in that folder, and sync tooApp Service with hello click of a button.</span></span> <span data-ttu-id="8dae3-108">Synkronisering av innehåll använder hello Kudu-processen för att bygga och distribution.</span><span class="sxs-lookup"><span data-stu-id="8dae3-108">Content sync utilizes hello Kudu process for build and deployment.</span></span> 

## <span data-ttu-id="8dae3-109"><a name="contentsync"></a>Hur tooenable innehåll synkronisera distribution</span><span class="sxs-lookup"><span data-stu-id="8dae3-109"><a name="contentsync"></a>How tooenable content sync deployment</span></span>
<span data-ttu-id="8dae3-110">tooenable innehåll sync från hello [Azure Portal](https://portal.azure.com), gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8dae3-110">tooenable content sync from hello [Azure Portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="8dae3-111">I bladet för din app i hello Azure-portalen klickar du på **inställningar** > **distributionskälla**.</span><span class="sxs-lookup"><span data-stu-id="8dae3-111">In your app's blade in hello Azure Portal, click **Settings** > **Deployment Source**.</span></span> <span data-ttu-id="8dae3-112">Klicka på **Välj källa**och välj **OneDrive** eller **Dropbox** som hello källa för distribution.</span><span class="sxs-lookup"><span data-stu-id="8dae3-112">Click **Choose Source**, then select **OneDrive** or **Dropbox** as hello source for deployment.</span></span> 
   
    ![Synkronisering av innehåll](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > <span data-ttu-id="8dae3-114">På grund av underliggande skillnader i hello API: er, **OneDrive för företag** stöds inte just nu.</span><span class="sxs-lookup"><span data-stu-id="8dae3-114">Because of underlying differences in hello APIs, **OneDrive for Business** is not supported at this time.</span></span> 
   > 
   > 
2. <span data-ttu-id="8dae3-115">Fullständig hello auktorisering arbetsflöde tooenable Apptjänst tooaccess en specifik fördefinierade angiven sökväg för OneDrive eller Dropbox där allt Apptjänst innehåll sparas.</span><span class="sxs-lookup"><span data-stu-id="8dae3-115">Complete hello authorization workflow tooenable App Service tooaccess a specific pre-defined designated path for OneDrive or Dropbox where all of your App Service content will be stored.</span></span>  
    <span data-ttu-id="8dae3-116">Efter tillstånd hello Apptjänst-plattformen ger utsedda hello alternativet toocreate innehållsmappen under hello sökväg för innehåll eller toochoose en befintlig mapp som innehåll under den här avsedda sökväg för innehåll.</span><span class="sxs-lookup"><span data-stu-id="8dae3-116">After authorization hello App Service platform will give you hello option toocreate a content folder under hello designated content path, or toochoose an existing content folder under this designated content path.</span></span> <span data-ttu-id="8dae3-117">hello avses sökvägar till innehåll under din molnlagringskonton som används för synkronisering av Apptjänst är hello följande:</span><span class="sxs-lookup"><span data-stu-id="8dae3-117">hello designated content paths under your cloud storage accounts used for App Service sync are hello following:</span></span>  
   
   * <span data-ttu-id="8dae3-118">**OneDrive**:`Apps\Azure Web Apps`</span><span class="sxs-lookup"><span data-stu-id="8dae3-118">**OneDrive**: `Apps\Azure Web Apps`</span></span> 
   * <span data-ttu-id="8dae3-119">**Dropbox**:`Dropbox\Apps\Azure`</span><span class="sxs-lookup"><span data-stu-id="8dae3-119">**Dropbox**: `Dropbox\Apps\Azure`</span></span>
3. <span data-ttu-id="8dae3-120">Efter hello kan inledande innehåll synkronisering hello innehåll synkronisering startas på begäran från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8dae3-120">After hello initial content sync hello content sync can be initiated on demand from hello Azure portal.</span></span> <span data-ttu-id="8dae3-121">Distributionshistoriken är tillgänglig med hello **distributioner** bladet.</span><span class="sxs-lookup"><span data-stu-id="8dae3-121">Deployment history is available with hello **Deployments** blade.</span></span>
   
    ![Distributionshistoriken](./media/app-service-deploy-content-sync/onedrive_sync.png)

<span data-ttu-id="8dae3-123">Mer information om Dropbox distributionen är tillgänglig under [distribuera från Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dae3-123">More information for Dropbox deployment is available under [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span></span> 

