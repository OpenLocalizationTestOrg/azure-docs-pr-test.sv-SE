---
title: Distribuera program till Azure App Service
description: "Lär dig hur du distribuera program till Apptjänst fungerar"
keywords: App service, azure app service distribuera distribution
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: 
ms.assetid: de12cd6e-e124-4e48-90bc-c3a3801305da
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 347e8b5177eac8e08ab0dea701b736b86d23904a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-deployment-overview"></a><span data-ttu-id="40216-104">Översikt över Azure App Service-distribution</span><span class="sxs-lookup"><span data-stu-id="40216-104">Azure App Service Deployment Overview</span></span>
<span data-ttu-id="40216-105">Azure App Service tillhandahåller en omfattande och integrerade funktioner för att skapa kraftfulla distributionsarbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="40216-105">Azure App Service provides a rich and integrated feature set to support creating powerful and flexible deployment workflows.</span></span> <span data-ttu-id="40216-106">App-distribution kan utnyttja alternativ som omfattar kontinuerlig integrering eller lokala källkontrollen publicering, WebDeploy- och FTP.</span><span class="sxs-lookup"><span data-stu-id="40216-106">App deployment can leverage options that include continuous integration or local source control publishing, WebDeploy, and FTP.</span></span> <span data-ttu-id="40216-107">Den rekommenderade metoden för Produktionsdistribution app är distributionen fack växlingen.</span><span class="sxs-lookup"><span data-stu-id="40216-107">The recommended method for production app deployment is deployment slot swap.</span></span> <span data-ttu-id="40216-108">Distributionsplatser representerar mellanlagrings- och integrering miljöer som är kopplade till produktion appar.</span><span class="sxs-lookup"><span data-stu-id="40216-108">Deployment slots represent staging and integration environments associated with production apps.</span></span> <span data-ttu-id="40216-109">Distributionsplatser kan konfigureras och mål för webbtrafik för verifiering och trafik kan växlas på begäran för distribution till tillverkning med ingen nertid och automatisk värmts.</span><span class="sxs-lookup"><span data-stu-id="40216-109">Deployment slots can be configured and targeted with web traffic for validation, and traffic can be swapped on demand for deployment to production with no down time and automated warm-up.</span></span> <span data-ttu-id="40216-110">Stegen i ett arbetsflöde för distribution kan enkelt automatiskt via versionen management produkter som släpper hantering av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40216-110">The steps of a deployment workflow can be easily automated via release management products such as Visual Studio Release Management.</span></span> <span data-ttu-id="40216-111">Detta är användbart för tillsammans med andra lösning resurser (t.ex. datalager), återkommande och replikering över flera enheter i distributionen.</span><span class="sxs-lookup"><span data-stu-id="40216-111">This is useful for coordination with other solution resources (e.g. data store), recurrence, and replication across multiple units of deployment.</span></span> 

[!INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]

