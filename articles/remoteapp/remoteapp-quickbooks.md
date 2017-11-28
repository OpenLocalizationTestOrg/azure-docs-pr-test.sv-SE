---
title: aaaDeploy QuickBooks i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur tooshare QuickBooks med Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c21b1ac311449be2281f9ce157828260e856f55d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a><span data-ttu-id="8c89f-103">Hur distribuerar du QuickBooks i Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="8c89f-103">How do you deploy QuickBooks in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8c89f-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="8c89f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8c89f-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="8c89f-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8c89f-106">Använd följande information tooshare QuickBooks som en app i Azure RemoteApp hello.</span><span class="sxs-lookup"><span data-stu-id="8c89f-106">Use hello following information tooshare QuickBooks as an app in Azure RemoteApp.</span></span>

<span data-ttu-id="8c89f-107">Du kan dela QuickBooks 2015 Enterprise med Azure RemoteApp i en hybrid eller molnet samling.</span><span class="sxs-lookup"><span data-stu-id="8c89f-107">You can share QuickBooks 2015 Enterprise with Azure RemoteApp in either a hybrid or cloud collection.</span></span> <span data-ttu-id="8c89f-108">hello företagsfil måste finnas på en virtuell dator som kör QuickBooks databasserver som är separat från hello Azure RemoteApp-servrar.</span><span class="sxs-lookup"><span data-stu-id="8c89f-108">hello company file must reside on a VM that is running QuickBooks database server that is separate from hello Azure RemoteApp servers.</span></span> <span data-ttu-id="8c89f-109">Lagrar aldrig hello företagsfil på Azure RemoteApp-avbildning - dataförlust förväntas om du gör detta.</span><span class="sxs-lookup"><span data-stu-id="8c89f-109">Never store hello company file on your Azure RemoteApp image - data loss is expected if you do this.</span></span> <span data-ttu-id="8c89f-110">Endast QuickBooks Enterprise stöder värd hello QuickBooks-filen på en extern resurs med QuickBooks databasserver som är tillgänglig via vanliga Windows-nätverk.</span><span class="sxs-lookup"><span data-stu-id="8c89f-110">Only QuickBooks Enterprise supports hosting hello QuickBooks file on an external share with QuickBooks database server accessible via standard Windows networking.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="8c89f-111">hello QuickBooks databasserver som är värd för hello företagsfil måste finnas på en separat virtuell dator i hello samma virtuella nätverk som hello Azure RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="8c89f-111">hello QuickBooks database server that is hosting hello company file must reside on a separate VM within hello same VNET as hello Azure RemoteApp collection.</span></span>  
> 
> 

## <a name="steps-toodeploy-quickbooks"></a><span data-ttu-id="8c89f-112">Steg toodeploy QuickBooks</span><span class="sxs-lookup"><span data-stu-id="8c89f-112">Steps toodeploy QuickBooks</span></span>
1. <span data-ttu-id="8c89f-113">Skapa en virtuell dator i Azure och installera QuickBooks QuickBooks-databasservern och placera hello företagsfil i en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="8c89f-113">Create an Azure VM and install QuickBooks, QuickBooks database server, and place hello company file on a Azure VM.</span></span>  <span data-ttu-id="8c89f-114">Se till att tooproperly konfigurera brandväggens regler.</span><span class="sxs-lookup"><span data-stu-id="8c89f-114">Make sure tooproperly configure firewall rules.</span></span>
2. <span data-ttu-id="8c89f-115">Installera QuickBooks på en [anpassad avbildning](remoteapp-imageoptions.md) och skapa en [Azure RemoteApp-samlingen](remoteapp-collections.md), moln eller hybrid inom hello exakt samma virtuella nätverk där hello VM värd hello QuickBooks databasserver med företagets filer finns.</span><span class="sxs-lookup"><span data-stu-id="8c89f-115">Install QuickBooks on a [custom image](remoteapp-imageoptions.md) and create an [Azure RemoteApp collection](remoteapp-collections.md), either cloud or hybrid, within hello exact same VNET where hello VM hosting hello QuickBooks database server with company files resides.</span></span> 
3. <span data-ttu-id="8c89f-116">[Publicera](remoteapp-publish.md) QuickBooks app toousers</span><span class="sxs-lookup"><span data-stu-id="8c89f-116">[Publish](remoteapp-publish.md) QuickBooks app toousers</span></span>
4. <span data-ttu-id="8c89f-117">Starta hello Azure RemoteApp-värdbaserad QuickBooks klienten, navigera använder standard Windows nätverks-toohello VM hello QuickBooks databasen värdservern och öppna hello företagsfil.</span><span class="sxs-lookup"><span data-stu-id="8c89f-117">Launch hello Azure RemoteApp-hosted QuickBooks client, navigate using standard Windows networking toohello VM hosting hello QuickBooks database server and open hello company file.</span></span> 

## <a name="documentation-references"></a><span data-ttu-id="8c89f-118">Dokumentationen referenser</span><span class="sxs-lookup"><span data-stu-id="8c89f-118">Documentation references</span></span>
* <span data-ttu-id="8c89f-119">QuickBooks [konfigurationer som stöds](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span><span class="sxs-lookup"><span data-stu-id="8c89f-119">QuickBooks [supported configurations](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span></span>
* <span data-ttu-id="8c89f-120">QuickBooks [distributionsalternativ](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span><span class="sxs-lookup"><span data-stu-id="8c89f-120">QuickBooks [deployment options](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span></span>

<span data-ttu-id="8c89f-121">Du kan också kontrollera ut presentationen Ignite [grunderna för Microsoft Azure RemoteApp-hantering och Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -framåt too1:02:45 tooget toohello QuickBooks del.</span><span class="sxs-lookup"><span data-stu-id="8c89f-121">You can also check out my Ignite presentation, [Fundamentals of Microsoft Azure RemoteApp Management and Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - fast-forward too1:02:45 tooget toohello QuickBooks part.</span></span>

## <a name="deployment-architecture"></a><span data-ttu-id="8c89f-122">Arkitektur för distribution</span><span class="sxs-lookup"><span data-stu-id="8c89f-122">Deployment architecture</span></span>
![QuickBooks + Azure RemoteApp-distribution](./media/remoteapp-quickbooks/ra-quickbooks.png)

