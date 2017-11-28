---
title: aaaDeploying stora distributioner
description: "Lär dig hur toodeploy stora distributioner med hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 6b1d2a7a5e49c78154fc856a221e64ca8dcfbe9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="7f961-103">Distribuera stora distributioner</span><span class="sxs-lookup"><span data-stu-id="7f961-103">Deploying Large Deployments</span></span>
<span data-ttu-id="7f961-104">Om din distribution är för stor toobe i hello approot standardmappen, du kan använda en resurs för lokal lagring som hello distribution rotmappen för din JDK och programservrar.</span><span class="sxs-lookup"><span data-stu-id="7f961-104">If your deployment is too large toobe contained in hello default approot folder, you can use a local storage resource as hello deployment root folder for your JDK and application server.</span></span>

## <a name="toouse-a-local-storage-resource-as-hello-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="7f961-105">toouse en resurs för lokal lagring som hello distribution rotmapp för stora distributioner</span><span class="sxs-lookup"><span data-stu-id="7f961-105">toouse a local storage resource as hello deployment root folder for large deployments</span></span>
1. <span data-ttu-id="7f961-106">Skapa en ny resurs för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="7f961-106">Create a new local storage resource.</span></span> <span data-ttu-id="7f961-107">hello namnet på hello resursen spelar ingen roll.</span><span class="sxs-lookup"><span data-stu-id="7f961-107">hello name of hello resource does not matter.</span></span> <span data-ttu-id="7f961-108">Lagringsresurser definieras på hello rollnivå.</span><span class="sxs-lookup"><span data-stu-id="7f961-108">Storage resources are defined at hello role level.</span></span> <span data-ttu-id="7f961-109">hello snabbaste sättet tooaccess hello lokal lagring konfigurationsdialogruta, där du kan skapa en ny resurs i lokal lagring är med hjälp av följande hello: Högerklicka på hello roll i hello **Projektutforskaren** visa (expandera din Azure-projekt nod om du inte ser hello rollen), klickar du på **Azure**, och klicka sedan på **lokal lagring**.</span><span class="sxs-lookup"><span data-stu-id="7f961-109">hello quickest way tooaccess hello local storage configuration dialog, from which you could create a new local storage resource, is by using hello following steps: Right-click hello role in hello **Project Explorer** view (expand your Azure project node if you don't see hello role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="7f961-110">Inom hello **lokal lagring** dialogrutan klickar du på **Lägg till** toocreate en ny resurs för lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="7f961-110">Within hello **Local Storage** dialog, click **Add** toocreate a new local storage resource.</span></span>

2. <span data-ttu-id="7f961-111">Ange hello önskad storlek tooat minst 2 048 MB (något mindre kan orsaka hello samma storlek problem som du kan stöta på i hello approot).</span><span class="sxs-lookup"><span data-stu-id="7f961-111">Set hello desired size tooat least 2048 MB (anything less may cause hello same file size problems as you would encounter in hello approot).</span></span>

3. <span data-ttu-id="7f961-112">Se till att **Rensa hello innehållet när hälsningspaket rollinstansen återanvänds** är markerat; detta förhindrar att hello distribution Startlogik körs i konflikt med befintliga filer i hello resurs när hello roll instansen återanvänds.</span><span class="sxs-lookup"><span data-stu-id="7f961-112">Ensure that **Clean hello contents when hello role instance is recycled** is checked; this will help prevent hello deployment's startup logic from running into conflicts with pre-existing files in hello resource when hello role instance is recycled.</span></span>

4. <span data-ttu-id="7f961-113">Se till att hello **miljö variabeln lagra hello resursens sökväg efter distributionen** toohello sträng har värdet **DEPLOYROOT**.</span><span class="sxs-lookup"><span data-stu-id="7f961-113">Ensure that hello **Environment variable storing hello resource's directory path after deployment** value is set toohello string **DEPLOYROOT**.</span></span> <span data-ttu-id="7f961-114">Lokal lagring resurs dialogen ser liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="7f961-114">Your local storage resource dialog will look similar toohello following.</span></span>

   ![][ic667943]

<span data-ttu-id="7f961-115">Alternativt, om du använder **DEPLOYROOT** som hello *namn* av din lokala resursen och du inte ändra hello genereras automatiskt Miljövariabelnamnet (som anges för **DEPLOYROOT_PATH** i så fall), som fungerar för ditt program samt.</span><span class="sxs-lookup"><span data-stu-id="7f961-115">Alternatively, if you use **DEPLOYROOT** as hello *name* of your local resource and you do not change hello automatically-generated environment variable name (which will be set too**DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="7f961-116">Mer information om hur du skapar en resurs för lokal lagring finns på [egenskaper för lokal lagring][Local storage properties].</span><span class="sxs-lookup"><span data-stu-id="7f961-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="see-also"></a><span data-ttu-id="7f961-117">Se även</span><span class="sxs-lookup"><span data-stu-id="7f961-117">See Also</span></span>
<span data-ttu-id="7f961-118">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7f961-118">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="7f961-119">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7f961-119">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="7f961-120">[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7f961-120">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="7f961-121">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="7f961-121">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
