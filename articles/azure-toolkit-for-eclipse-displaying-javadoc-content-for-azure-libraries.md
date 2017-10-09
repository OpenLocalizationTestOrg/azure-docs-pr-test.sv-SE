---
title: "aaaDisplaying Javadoc innehåll i Eclipse för hello paketet för Azure-bibliotek för Java"
description: "Hur hello toodisplay Javadoc innehåll för hello Azure Libraries i Eclipse."
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
ms.openlocfilehash: 8070023a24dc07eca8df906db5b8b662ceed6ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-hello-azure-libraries-package-for-java"></a><span data-ttu-id="cdf18-103">Visa Javadoc innehåll i Eclipse för hello paketet för Azure-bibliotek för Java</span><span class="sxs-lookup"><span data-stu-id="cdf18-103">Displaying Javadoc Content in Eclipse for hello Azure Libraries Package for Java</span></span>
<span data-ttu-id="cdf18-104">Hej Javadoc innehåll för hello Azure-biblioteken för Java kan visas i Eclipse-miljö genom att associera hello Javadoc innehåll toohello Azure-biblioteken för Java.</span><span class="sxs-lookup"><span data-stu-id="cdf18-104">hello Javadoc content for hello Azure Libraries for Java can be viewed within your Eclipse environment by associating hello Javadoc content toohello Azure Libraries for Java.</span></span> <span data-ttu-id="cdf18-105">hello följande steg visar hur toouse den här funktionen i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="cdf18-105">hello following steps show you how toouse this functionality within Eclipse.</span></span>

<span data-ttu-id="cdf18-106">Den här proceduren förutsätter att du redan har lagt till hello Azure-bibliotek för Java byggsökväg för tooyour.</span><span class="sxs-lookup"><span data-stu-id="cdf18-106">This procedure assumes you have already added hello Azure Library for Java tooyour build path.</span></span>

## <a name="toodisplay-javadoc-content-in-eclipse-for-hello-azure-libraries-for-java"></a><span data-ttu-id="cdf18-107">toodisplay Javadoc innehållet i Eclipse för hello Azure-biblioteken för Java</span><span class="sxs-lookup"><span data-stu-id="cdf18-107">toodisplay Javadoc content in Eclipse for hello Azure Libraries for Java</span></span>
* <span data-ttu-id="cdf18-108">I Eclipses Projektutforskaren i hello **refererar till bibliotek** avsnitt i ditt projekt, öppna hello snabbmenyn för hello Azure-bibliotek för Java JAR.</span><span class="sxs-lookup"><span data-stu-id="cdf18-108">Within Eclipse's Project Explorer, in hello **Referenced Libraries** section of your project, open hello context menu for hello Azure Library for Java JAR.</span></span> <span data-ttu-id="cdf18-109">Till exempel **microsoft-windowsazure-api-0.1.0.jar** (hello versionsnumret kan vara olika, beroende på vilken version som du har installerat).</span><span class="sxs-lookup"><span data-stu-id="cdf18-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (hello version number may be different, dependent upon which version you have installed).</span></span>

* <span data-ttu-id="cdf18-110">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-110">Click **Properties**.</span></span>

* <span data-ttu-id="cdf18-111">Inom hello **egenskaper** dialogrutan i hello vänstra fönstret, klickar du på **Javadoc plats**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-111">Within hello **Properties** dialog, in hello left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="cdf18-112">Hej **Javadoc plats** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="cdf18-112">hello **Javadoc Location** dialog is displayed.</span></span>

* <span data-ttu-id="cdf18-113">Du kan ange en **Javadoc URL**, eller en **Javadoc i arkivet**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="cdf18-114">Om du väljer toospecify en **Javadoc URL**, Använd hello URL: er som **http://dl.windowsazure.com/javadoc** eller **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-114">If you choose toospecify a **Javadoc URL**, use hello URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="cdf18-115">Om du väljer toouse **Javadoc i arkivet**, du kan ange en extern fil eller en fil.</span><span class="sxs-lookup"><span data-stu-id="cdf18-115">If you choose toouse **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="cdf18-116">Gör du dina val och bläddra/Validera efter behov.</span><span class="sxs-lookup"><span data-stu-id="cdf18-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="cdf18-117">hello följande exempel associerar hello Azure-biblioteken för Java med hello motsvarande Javadoc JAR som har hämtats lokalt tooa mapp med namnet **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-117">hello following example associates hello Azure Libraries for Java with hello corresponding Javadoc JAR that has been downloaded locally tooa folder named **c:\MyAzureJARs**.</span></span>

   ![][ic553487]

* <span data-ttu-id="cdf18-118">*Valfritt steg*: Klicka på **Validera**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-118">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="cdf18-119">Potentiella problem med hello Javadoc JAR kan visas här.</span><span class="sxs-lookup"><span data-stu-id="cdf18-119">Potential issues with hello Javadoc JAR could be displayed here.</span></span>

* <span data-ttu-id="cdf18-120">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cdf18-120">Click **OK**.</span></span>

<span data-ttu-id="cdf18-121">När associerat med hello bibliotek ska hello Javadoc innehåll visas i ditt Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="cdf18-121">Once associated with hello library, hello Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="cdf18-122">Till exempel om `blob` definieras av typen `CloudBlockBlob` inom din kod hello följande är ett exempel på Javadoc-innehåll som visas när du skriver `blob.acquireLease` i koden:</span><span class="sxs-lookup"><span data-stu-id="cdf18-122">For example, if `blob` is defined of type `CloudBlockBlob` within your code, hello following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![][ic553488]

## <a name="see-also"></a><span data-ttu-id="cdf18-123">Se även</span><span class="sxs-lookup"><span data-stu-id="cdf18-123">See Also</span></span>
<span data-ttu-id="cdf18-124">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="cdf18-124">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="cdf18-125">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="cdf18-125">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="cdf18-126">[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="cdf18-126">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="cdf18-127">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="cdf18-127">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
