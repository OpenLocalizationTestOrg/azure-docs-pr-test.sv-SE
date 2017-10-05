---
title: "Synkronisera innehåll från en mapp i molnet till Azure App Service"
description: "Lär dig mer om att distribuera din app till Azure App Service via synkronisering av innehåll från en mapp i molnet."
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
ms.openlocfilehash: 010e7dc492abefaa3afe814c0322af9f6fe5acd2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a><span data-ttu-id="5de85-103">Synkronisera innehåll från en mapp i molnet till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5de85-103">Sync content from a cloud folder to Azure App Service</span></span>
<span data-ttu-id="5de85-104">Den här kursen visar hur du distribuerar till [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) genom att synkronisera innehållet från populära lagring molntjänster som Dropbox och OneDrive.</span><span class="sxs-lookup"><span data-stu-id="5de85-104">This tutorial shows you how to deploy to [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by syncing your content from popular cloud storage services like Dropbox and OneDrive.</span></span> 

## <span data-ttu-id="5de85-105"><a name="overview"></a>Översikt över distribution av innehåll synkronisering</span><span class="sxs-lookup"><span data-stu-id="5de85-105"><a name="overview"></a>Overview of content sync deployment</span></span>
<span data-ttu-id="5de85-106">Distributionen av innehåll på begäran-synkronisering drivs av den [Kudu-distributionsmotorn](https://github.com/projectkudu/kudu/wiki) integrerad med App Service.</span><span class="sxs-lookup"><span data-stu-id="5de85-106">The on-demand content sync deployment is powered by the [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki) integrated with App Service.</span></span> <span data-ttu-id="5de85-107">I den [Azure Portal](https://portal.azure.com), kan du utse en mapp i molnlagringen, fungerar med din app koden och innehållet i mappen och synkronisera till App Service klickar på en knapp.</span><span class="sxs-lookup"><span data-stu-id="5de85-107">In the [Azure Portal](https://portal.azure.com), you can designate a folder in your cloud storage, work with your app code and content in that folder, and sync to App Service with the click of a button.</span></span> <span data-ttu-id="5de85-108">Synkronisering av innehåll använder Kudu-processen för att bygga och distribution.</span><span class="sxs-lookup"><span data-stu-id="5de85-108">Content sync utilizes the Kudu process for build and deployment.</span></span> 

## <span data-ttu-id="5de85-109"><a name="contentsync"></a>Så här aktiverar du innehåll sync-distribution</span><span class="sxs-lookup"><span data-stu-id="5de85-109"><a name="contentsync"></a>How to enable content sync deployment</span></span>
<span data-ttu-id="5de85-110">Att aktivera synkronisering av innehåll från den [Azure Portal](https://portal.azure.com), gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5de85-110">To enable content sync from the [Azure Portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="5de85-111">I bladet för din app i Azure-portalen klickar du på **inställningar** > **distributionskälla**.</span><span class="sxs-lookup"><span data-stu-id="5de85-111">In your app's blade in the Azure Portal, click **Settings** > **Deployment Source**.</span></span> <span data-ttu-id="5de85-112">Klicka på **Välj källa**och välj **OneDrive** eller **Dropbox** som källa för distribution.</span><span class="sxs-lookup"><span data-stu-id="5de85-112">Click **Choose Source**, then select **OneDrive** or **Dropbox** as the source for deployment.</span></span> 
   
    ![Synkronisering av innehåll](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > <span data-ttu-id="5de85-114">På grund av underliggande skillnader i API: er, **OneDrive för företag** stöds inte just nu.</span><span class="sxs-lookup"><span data-stu-id="5de85-114">Because of underlying differences in the APIs, **OneDrive for Business** is not supported at this time.</span></span> 
   > 
   > 
2. <span data-ttu-id="5de85-115">Slutför auktorisering arbetsflödet om du vill aktivera App Service att få åtkomst till en specifik fördefinierade avsedda sökväg för OneDrive eller Dropbox där allt Apptjänst innehåll sparas.</span><span class="sxs-lookup"><span data-stu-id="5de85-115">Complete the authorization workflow to enable App Service to access a specific pre-defined designated path for OneDrive or Dropbox where all of your App Service content will be stored.</span></span>  
    <span data-ttu-id="5de85-116">Efter auktoriseringen App Service får plattform du alternativet att skapa innehållsmappen under angiven sökväg för innehåll eller välj en befintlig mapp som innehåll under den här avsedda sökväg för innehåll.</span><span class="sxs-lookup"><span data-stu-id="5de85-116">After authorization the App Service platform will give you the option to create a content folder under the designated content path, or to choose an existing content folder under this designated content path.</span></span> <span data-ttu-id="5de85-117">De avsedda sökvägarna till innehåll under din molnlagringskonton som används för synkronisering av Apptjänst är följande:</span><span class="sxs-lookup"><span data-stu-id="5de85-117">The designated content paths under your cloud storage accounts used for App Service sync are the following:</span></span>  
   
   * <span data-ttu-id="5de85-118">**OneDrive**:`Apps\Azure Web Apps`</span><span class="sxs-lookup"><span data-stu-id="5de85-118">**OneDrive**: `Apps\Azure Web Apps`</span></span> 
   * <span data-ttu-id="5de85-119">**Dropbox**:`Dropbox\Apps\Azure`</span><span class="sxs-lookup"><span data-stu-id="5de85-119">**Dropbox**: `Dropbox\Apps\Azure`</span></span>
3. <span data-ttu-id="5de85-120">Efter den första synkroniseringen av innehåll kan innehåll synkroniseringen initieras på begäran från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5de85-120">After the initial content sync the content sync can be initiated on demand from the Azure portal.</span></span> <span data-ttu-id="5de85-121">Distributionshistoriken är tillgängliga med den **distributioner** bladet.</span><span class="sxs-lookup"><span data-stu-id="5de85-121">Deployment history is available with the **Deployments** blade.</span></span>
   
    ![Distributionshistoriken](./media/app-service-deploy-content-sync/onedrive_sync.png)

<span data-ttu-id="5de85-123">Mer information om Dropbox distributionen är tillgänglig under [distribuera från Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span><span class="sxs-lookup"><span data-stu-id="5de85-123">More information for Dropbox deployment is available under [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span></span> 

