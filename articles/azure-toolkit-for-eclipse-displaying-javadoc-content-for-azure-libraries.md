---
title: "Visa Javadoc innehåll i Eclipse för paketet Azure-bibliotek för Java"
description: "Så här visar du Javadoc-innehåll för Azure-biblioteken i Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="d9baa-103">Visa Javadoc innehåll i Eclipse för paketet Azure-bibliotek för Java</span><span class="sxs-lookup"><span data-stu-id="d9baa-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>
<span data-ttu-id="d9baa-104">Javadoc innehållet för Azure-biblioteken för Java kan visas i Eclipse-miljö genom att associera Javadoc innehållet till Azure-biblioteken för Java.</span><span class="sxs-lookup"><span data-stu-id="d9baa-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="d9baa-105">Följande steg visar hur du använder den här funktionen i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="d9baa-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="d9baa-106">Den här proceduren förutsätter att du redan har lagt till Azure-bibliotek för Java build-sökväg.</span><span class="sxs-lookup"><span data-stu-id="d9baa-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="d9baa-107">Att visa Javadoc innehåll i Eclipse för Azure-biblioteken för Java</span><span class="sxs-lookup"><span data-stu-id="d9baa-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>
* <span data-ttu-id="d9baa-108">I Eclipses Projektutforskaren i den **refererar till bibliotek** avsnitt i ditt projekt, öppna snabbmenyn för Azure-bibliotek för Java JAR.</span><span class="sxs-lookup"><span data-stu-id="d9baa-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="d9baa-109">Till exempel **microsoft-windowsazure-api-0.1.0.jar** (versionsnumret kan vara olika, beroende på vilken version som du har installerat).</span><span class="sxs-lookup"><span data-stu-id="d9baa-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

* <span data-ttu-id="d9baa-110">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-110">Click **Properties**.</span></span>

* <span data-ttu-id="d9baa-111">I den **egenskaper** dialogrutan i det vänstra fönstret klickar du på **Javadoc plats**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="d9baa-112">Den **Javadoc plats** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="d9baa-112">The **Javadoc Location** dialog is displayed.</span></span>

* <span data-ttu-id="d9baa-113">Du kan ange en **Javadoc URL**, eller en **Javadoc i arkivet**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="d9baa-114">Om du väljer att ange en **Javadoc URL**, Använd webbadresserna som **http://dl.windowsazure.com/javadoc** eller **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="d9baa-115">Om du väljer att använda **Javadoc i arkivet**, du kan ange en extern fil eller en fil.</span><span class="sxs-lookup"><span data-stu-id="d9baa-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="d9baa-116">Gör du dina val och bläddra/Validera efter behov.</span><span class="sxs-lookup"><span data-stu-id="d9baa-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="d9baa-117">I följande exempel associerar Azure-biblioteken för Java med det motsvarande Javadoc JAR som har hämtats lokalt till en mapp med namnet **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![][ic553487]

* <span data-ttu-id="d9baa-118">*Valfritt steg*: Klicka på **Validera**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-118">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="d9baa-119">Potentiella problem med Javadoc JAR kan visas här.</span><span class="sxs-lookup"><span data-stu-id="d9baa-119">Potential issues with the Javadoc JAR could be displayed here.</span></span>

* <span data-ttu-id="d9baa-120">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9baa-120">Click **OK**.</span></span>

<span data-ttu-id="d9baa-121">När associerat med biblioteket ska Javadoc innehåll visas i ditt Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="d9baa-121">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="d9baa-122">Till exempel om `blob` definieras av typen `CloudBlockBlob` inom din kod följande är ett exempel på Javadoc-innehåll som visas när du skriver `blob.acquireLease` i koden:</span><span class="sxs-lookup"><span data-stu-id="d9baa-122">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![][ic553488]

## <a name="see-also"></a><span data-ttu-id="d9baa-123">Se även</span><span class="sxs-lookup"><span data-stu-id="d9baa-123">See Also</span></span>
<span data-ttu-id="d9baa-124">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d9baa-124">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="d9baa-125">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d9baa-125">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="d9baa-126">[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d9baa-126">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="d9baa-127">Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="d9baa-127">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
