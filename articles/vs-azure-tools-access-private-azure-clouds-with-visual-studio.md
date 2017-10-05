---
title: "Åtkomst till privata moln som Azure med Visual Studio | Microsoft Docs"
description: "Lär dig hur du kommer åt resurser för privata moln med hjälp av Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a><span data-ttu-id="3820d-103">Åtkomst till privata moln som Azure med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3820d-103">Accessing private Azure clouds with Visual Studio</span></span>
<span data-ttu-id="3820d-104">Som standard stöder Visual Studio REST-slutpunkter för offentliga Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="3820d-104">By default, Visual Studio supports public Azure cloud REST endpoints.</span></span> <span data-ttu-id="3820d-105">I det här avsnittet du lär dig hur du använder certifikat för ditt privata moln för att komma åt - och interagera med - privat moln från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3820d-105">In this topic, you learn how to use your private cloud's certificate to access - and interact with - the private cloud from Visual Studio.</span></span>

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a><span data-ttu-id="3820d-106">Åtkomst till en privat Azure cloud i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3820d-106">To access a private Azure cloud in Visual Studio</span></span>
1. <span data-ttu-id="3820d-107">I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) hämta publiceringsinställningarna filen för det privata molnet, eller kontakta administratören för en publiceringsinställningarna fil.</span><span class="sxs-lookup"><span data-stu-id="3820d-107">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) for the private cloud, download your publish-settings file, or contact your administrator for a publish-settings file.</span></span> <span data-ttu-id="3820d-108">På den offentliga versionen av Azure länken för att hämta den här är [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span><span class="sxs-lookup"><span data-stu-id="3820d-108">On the public version of Azure, the link to download this is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span></span> <span data-ttu-id="3820d-109">(Den hämta filen ska ha filnamnstillägget `.publishsettings`)</span><span class="sxs-lookup"><span data-stu-id="3820d-109">(The downloaded file should have an extension of `.publishsettings`)</span></span>

1. <span data-ttu-id="3820d-110">Öppna Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3820d-110">Open Visual Studio</span></span>

1. <span data-ttu-id="3820d-111">I **Server Explorer**, högerklicka på den **Azure** nod och på snabbmenyn Välj **hantera och Filter prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="3820d-111">In **Server Explorer**, right-click the **Azure** node and, from the context menu, select **Manage and Filter Subscriptions**.</span></span>
   
    ![Hantera prenumerationer kommando](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. <span data-ttu-id="3820d-113">I den **hantera Azure-prenumerationer** markerar den **certifikat** och välj sedan **importera**.</span><span class="sxs-lookup"><span data-stu-id="3820d-113">In the **Manage Microsoft Azure Subscriptions** dialog, select the **Certificates** tab, and then select **Import**.</span></span>
   
    ![Import av Azure certifikat](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. <span data-ttu-id="3820d-115">I den **Importera Microsoft Azure-prenumerationer** markerar **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="3820d-115">In the **Import Microsoft Azure Subscriptions** dialog, select **Browse**.</span></span>

    ![Bläddra i dialogrutan Importera Microsoft Azure-prenumerationer](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. <span data-ttu-id="3820d-117">I den **öppna** dialogrutan, bläddra till den katalog där du sparade filen publiceringsinställningarna, markera filen och välj sedan **öppna**.</span><span class="sxs-lookup"><span data-stu-id="3820d-117">In the **Open** dialog, browse to the directory where you saved the publish-settings file, select the file, and then select **Open**.</span></span>

    ![Välj publiceringsinställningarna fil](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. <span data-ttu-id="3820d-119">När tillbaka till den **Importera Microsoft Azure-prenumerationer** markerar **importera**.</span><span class="sxs-lookup"><span data-stu-id="3820d-119">When returned to the **Import Microsoft Azure Subscriptions** dialog, select **Import**.</span></span>

    ![Importera filen publiceringsinställningarna](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    <span data-ttu-id="3820d-121">Certifikaten har importerats från publiceringsinställningarna filen till Visual Studio och du kan nu interagera med resurser för privata moln.</span><span class="sxs-lookup"><span data-stu-id="3820d-121">The certificates are imported from the publish-settings file into Visual Studio, and you can now interact with your private cloud resources.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="3820d-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3820d-122">Next steps</span></span>
- [<span data-ttu-id="3820d-123">Publicering till en Azure-molntjänst från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3820d-123">Publishing to an Azure Cloud Service from Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- <span data-ttu-id="3820d-124">[Så här: hämta och importera publicera inställningar och prenumerationsinformation om](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span><span class="sxs-lookup"><span data-stu-id="3820d-124">[How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span></span>
