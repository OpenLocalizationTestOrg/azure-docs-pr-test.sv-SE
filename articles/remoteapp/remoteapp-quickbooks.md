---
title: Distribuera QuickBooks i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur du delar QuickBooks med Azure RemoteApp."
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
ms.openlocfilehash: bbfac45220f6ef36c9951daae0ced1974e8ddabb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a><span data-ttu-id="5d6ef-103">Hur distribuerar du QuickBooks i Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="5d6ef-103">How do you deploy QuickBooks in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5d6ef-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="5d6ef-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="5d6ef-106">Använd följande information för att dela QuickBooks som en app i Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-106">Use the following information to share QuickBooks as an app in Azure RemoteApp.</span></span>

<span data-ttu-id="5d6ef-107">Du kan dela QuickBooks 2015 Enterprise med Azure RemoteApp i en hybrid eller molnet samling.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-107">You can share QuickBooks 2015 Enterprise with Azure RemoteApp in either a hybrid or cloud collection.</span></span> <span data-ttu-id="5d6ef-108">Filen för företagets måste finnas på en virtuell dator som kör QuickBooks databasserver som är separat från Azure RemoteApp-servrar.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-108">The company file must reside on a VM that is running QuickBooks database server that is separate from the Azure RemoteApp servers.</span></span> <span data-ttu-id="5d6ef-109">Lagrar aldrig företagsfil på Azure RemoteApp-avbildning - dataförlust förväntas om du gör detta.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-109">Never store the company file on your Azure RemoteApp image - data loss is expected if you do this.</span></span> <span data-ttu-id="5d6ef-110">Endast QuickBooks Enterprise har stöd för värd för filen QuickBooks på en extern resurs med QuickBooks databasserver som är tillgänglig via vanliga Windows-nätverk.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-110">Only QuickBooks Enterprise supports hosting the QuickBooks file on an external share with QuickBooks database server accessible via standard Windows networking.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="5d6ef-111">QuickBooks databasservern som är värd för filen för företagets måste finnas på en separat virtuell dator i samma virtuella nätverk som Azure RemoteApp-samling.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-111">The QuickBooks database server that is hosting the company file must reside on a separate VM within the same VNET as the Azure RemoteApp collection.</span></span>  
> 
> 

## <a name="steps-to-deploy-quickbooks"></a><span data-ttu-id="5d6ef-112">Steg för att distribuera QuickBooks</span><span class="sxs-lookup"><span data-stu-id="5d6ef-112">Steps to deploy QuickBooks</span></span>
1. <span data-ttu-id="5d6ef-113">Skapa en virtuell dator i Azure och installera QuickBooks QuickBooks-databasservern och placera filen för företagets på en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-113">Create an Azure VM and install QuickBooks, QuickBooks database server, and place the company file on a Azure VM.</span></span>  <span data-ttu-id="5d6ef-114">Se till att konfigurera brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-114">Make sure to properly configure firewall rules.</span></span>
2. <span data-ttu-id="5d6ef-115">Installera QuickBooks på en [anpassad avbildning](remoteapp-imageoptions.md) och skapa en [Azure RemoteApp-samlingen](remoteapp-collections.md), moln eller hybrid i exakt samma virtuella nätverk där den virtuella datorn som värd för databasservern QuickBooks med företagets filer finns.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-115">Install QuickBooks on a [custom image](remoteapp-imageoptions.md) and create an [Azure RemoteApp collection](remoteapp-collections.md), either cloud or hybrid, within the exact same VNET where the VM hosting the QuickBooks database server with company files resides.</span></span> 
3. <span data-ttu-id="5d6ef-116">[Publicera](remoteapp-publish.md) QuickBooks app till användare</span><span class="sxs-lookup"><span data-stu-id="5d6ef-116">[Publish](remoteapp-publish.md) QuickBooks app to users</span></span>
4. <span data-ttu-id="5d6ef-117">Starta Azure RemoteApp-värdbaserad QuickBooks klienten, navigera med hjälp av Windows-nätverk som standard till den virtuella datorn som värd för databasservern QuickBooks och öppna filen för företagets.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-117">Launch the Azure RemoteApp-hosted QuickBooks client, navigate using standard Windows networking to the VM hosting the QuickBooks database server and open the company file.</span></span> 

## <a name="documentation-references"></a><span data-ttu-id="5d6ef-118">Dokumentationen referenser</span><span class="sxs-lookup"><span data-stu-id="5d6ef-118">Documentation references</span></span>
* <span data-ttu-id="5d6ef-119">QuickBooks [konfigurationer som stöds](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span><span class="sxs-lookup"><span data-stu-id="5d6ef-119">QuickBooks [supported configurations](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)</span></span>
* <span data-ttu-id="5d6ef-120">QuickBooks [distributionsalternativ](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span><span class="sxs-lookup"><span data-stu-id="5d6ef-120">QuickBooks [deployment options](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)</span></span>

<span data-ttu-id="5d6ef-121">Du kan också kontrollera ut presentationen Ignite [grunderna för Microsoft Azure RemoteApp-hantering och Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -spola framåt till 1:02:45 till QuickBooks del.</span><span class="sxs-lookup"><span data-stu-id="5d6ef-121">You can also check out my Ignite presentation, [Fundamentals of Microsoft Azure RemoteApp Management and Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - fast-forward to 1:02:45 to get to the QuickBooks part.</span></span>

## <a name="deployment-architecture"></a><span data-ttu-id="5d6ef-122">Arkitektur för distribution</span><span class="sxs-lookup"><span data-stu-id="5d6ef-122">Deployment architecture</span></span>
![QuickBooks + Azure RemoteApp-distribution](./media/remoteapp-quickbooks/ra-quickbooks.png)

