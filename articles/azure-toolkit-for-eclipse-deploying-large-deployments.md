---
title: Distribuera stora distributioner
description: "Lär dig hur du distribuerar stora distributioner som använder Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="e3c39-103">Distribuera stora distributioner</span><span class="sxs-lookup"><span data-stu-id="e3c39-103">Deploying Large Deployments</span></span>
<span data-ttu-id="e3c39-104">Om din distribution är för stort för att finnas i approot standardmappen, du kan använda en resurs för lokal lagring som distributionen rotmappen för din JDK och programservrar.</span><span class="sxs-lookup"><span data-stu-id="e3c39-104">If your deployment is too large to be contained in the default approot folder, you can use a local storage resource as the deployment root folder for your JDK and application server.</span></span>

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="e3c39-105">Att använda en lokal lagring-resurs som distributionen rotmapp för stora distributioner</span><span class="sxs-lookup"><span data-stu-id="e3c39-105">To use a local storage resource as the deployment root folder for large deployments</span></span>
1. <span data-ttu-id="e3c39-106">Skapa en ny resurs för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="e3c39-106">Create a new local storage resource.</span></span> <span data-ttu-id="e3c39-107">Namnet på resursen spelar ingen roll.</span><span class="sxs-lookup"><span data-stu-id="e3c39-107">The name of the resource does not matter.</span></span> <span data-ttu-id="e3c39-108">Lagringsresurser definieras på rollnivå.</span><span class="sxs-lookup"><span data-stu-id="e3c39-108">Storage resources are defined at the role level.</span></span> <span data-ttu-id="e3c39-109">Det snabbaste sättet att komma åt konfigurationsdialogruta lokal lagring, där du kan skapa en ny resurs i lokal lagring är med hjälp av följande steg: Högerklicka på rollen i den **Projektutforskaren** visa (expandera din Azure-projekt Klicka på noden om du inte ser rollen), **Azure**, och klicka sedan på **lokal lagring**.</span><span class="sxs-lookup"><span data-stu-id="e3c39-109">The quickest way to access the local storage configuration dialog, from which you could create a new local storage resource, is by using the following steps: Right-click the role in the **Project Explorer** view (expand your Azure project node if you don't see the role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="e3c39-110">I den **lokal lagring** dialogrutan klickar du på **Lägg till** att skapa en ny resurs för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="e3c39-110">Within the **Local Storage** dialog, click **Add** to create a new local storage resource.</span></span>

2. <span data-ttu-id="e3c39-111">Ange önskad storlek på minst 2 048 MB (något mindre kan ge samma fil storlek problem som du skulle uppstå i approot).</span><span class="sxs-lookup"><span data-stu-id="e3c39-111">Set the desired size to at least 2048 MB (anything less may cause the same file size problems as you would encounter in the approot).</span></span>

3. <span data-ttu-id="e3c39-112">Se till att **rensa innehållet när instansen återanvänds** är markerat; detta förhindrar att distributionens Startlogik körs i konflikt med befintliga filer i resursen när instansen är återanvänds.</span><span class="sxs-lookup"><span data-stu-id="e3c39-112">Ensure that **Clean the contents when the role instance is recycled** is checked; this will help prevent the deployment's startup logic from running into conflicts with pre-existing files in the resource when the role instance is recycled.</span></span>

4. <span data-ttu-id="e3c39-113">Se till att den **miljövariabeln lagra resursens sökväg efter distributionen** värdet strängen **DEPLOYROOT**.</span><span class="sxs-lookup"><span data-stu-id="e3c39-113">Ensure that the **Environment variable storing the resource's directory path after deployment** value is set to the string **DEPLOYROOT**.</span></span> <span data-ttu-id="e3c39-114">Lokal lagring resurs dialogen ser ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="e3c39-114">Your local storage resource dialog will look similar to the following.</span></span>

   ![][ic667943]

<span data-ttu-id="e3c39-115">Alternativt, om du använder **DEPLOYROOT** som den *namn* av din lokala resursen och du inte ändra genereras automatiskt miljövariabeln (som kommer att anges som **DEPLOYROOT_ SÖKVÄGEN** i så fall), som fungerar för ditt program samt.</span><span class="sxs-lookup"><span data-stu-id="e3c39-115">Alternatively, if you use **DEPLOYROOT** as the *name* of your local resource and you do not change the automatically-generated environment variable name (which will be set to **DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="e3c39-116">Mer information om hur du skapar en resurs för lokal lagring finns på [egenskaper för lokal lagring][Local storage properties].</span><span class="sxs-lookup"><span data-stu-id="e3c39-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="see-also"></a><span data-ttu-id="e3c39-117">Se även</span><span class="sxs-lookup"><span data-stu-id="e3c39-117">See Also</span></span>
<span data-ttu-id="e3c39-118">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e3c39-118">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="e3c39-119">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e3c39-119">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="e3c39-120">[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e3c39-120">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="e3c39-121">Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="e3c39-121">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
