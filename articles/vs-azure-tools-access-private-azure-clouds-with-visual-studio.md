---
title: aaaAccessing privata Azure moln med Visual Studio | Microsoft Docs
description: "Lär dig hur tooaccess privata molnet resurser med hjälp av Visual Studio."
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
ms.openlocfilehash: 5cfd6439afdcf98c6f7d7f29ab6c4256ed02533a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a><span data-ttu-id="6a910-103">Åtkomst till privata moln som Azure med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a910-103">Accessing private Azure clouds with Visual Studio</span></span>
<span data-ttu-id="6a910-104">Som standard stöder Visual Studio REST-slutpunkter för offentliga Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="6a910-104">By default, Visual Studio supports public Azure cloud REST endpoints.</span></span> <span data-ttu-id="6a910-105">I det här avsnittet lär du dig hur toouse ditt privata moln vars certifikat tooaccess - och interagera med - hello privat moln från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a910-105">In this topic, you learn how toouse your private cloud's certificate tooaccess - and interact with - hello private cloud from Visual Studio.</span></span>

## <a name="tooaccess-a-private-azure-cloud-in-visual-studio"></a><span data-ttu-id="6a910-106">tooaccess en privat Azure cloud i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a910-106">tooaccess a private Azure cloud in Visual Studio</span></span>
1. <span data-ttu-id="6a910-107">I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) ladda ner filen publiceringsinställningarna för hello privata moln, eller kontakta administratören för en publiceringsinställningarna fil.</span><span class="sxs-lookup"><span data-stu-id="6a910-107">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) for hello private cloud, download your publish-settings file, or contact your administrator for a publish-settings file.</span></span> <span data-ttu-id="6a910-108">Hej länken toodownload detta är på hello offentliga versionen av Azure, [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span><span class="sxs-lookup"><span data-stu-id="6a910-108">On hello public version of Azure, hello link toodownload this is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span></span> <span data-ttu-id="6a910-109">(hello nedladdade filen måste ha filnamnstillägget `.publishsettings`)</span><span class="sxs-lookup"><span data-stu-id="6a910-109">(hello downloaded file should have an extension of `.publishsettings`)</span></span>

1. <span data-ttu-id="6a910-110">Öppna Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a910-110">Open Visual Studio</span></span>

1. <span data-ttu-id="6a910-111">I **Server Explorer**, högerklicka på hello **Azure** nod hello snabbmenyn, Välj **hantera och Filter prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="6a910-111">In **Server Explorer**, right-click hello **Azure** node and, from hello context menu, select **Manage and Filter Subscriptions**.</span></span>
   
    ![Hantera prenumerationer kommando](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. <span data-ttu-id="6a910-113">I hello **hantera Azure-prenumerationer** dialogrutan, Välj hello **certifikat** och välj sedan **importera**.</span><span class="sxs-lookup"><span data-stu-id="6a910-113">In hello **Manage Microsoft Azure Subscriptions** dialog, select hello **Certificates** tab, and then select **Import**.</span></span>
   
    ![Import av Azure certifikat](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. <span data-ttu-id="6a910-115">I hello **Importera Microsoft Azure-prenumerationer** markerar **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="6a910-115">In hello **Import Microsoft Azure Subscriptions** dialog, select **Browse**.</span></span>

    ![Bläddra i dialogrutan för hello Importera Microsoft Azure-prenumerationer](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. <span data-ttu-id="6a910-117">I hello **öppna** dialogrutan Bläddra toohello katalog där du sparade hello-publiceringsinställningsfilen, Välj hello filen och sedan markera **öppna**.</span><span class="sxs-lookup"><span data-stu-id="6a910-117">In hello **Open** dialog, browse toohello directory where you saved hello publish-settings file, select hello file, and then select **Open**.</span></span>

    ![Välj hello-publiceringsinställningsfil](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. <span data-ttu-id="6a910-119">När returnerade toohello **Importera Microsoft Azure-prenumerationer** markerar **importera**.</span><span class="sxs-lookup"><span data-stu-id="6a910-119">When returned toohello **Import Microsoft Azure Subscriptions** dialog, select **Import**.</span></span>

    ![Importera hello-publiceringsinställningsfil](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    <span data-ttu-id="6a910-121">hello certifikat har importerats från hello-publiceringsinställningsfilen till Visual Studio och du kan nu interagera med resurser för privata moln.</span><span class="sxs-lookup"><span data-stu-id="6a910-121">hello certificates are imported from hello publish-settings file into Visual Studio, and you can now interact with your private cloud resources.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="6a910-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a910-122">Next steps</span></span>
- [<span data-ttu-id="6a910-123">Publishing tooan Azure Cloud Service från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a910-123">Publishing tooan Azure Cloud Service from Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- <span data-ttu-id="6a910-124">[Så här: hämta och importera publicera inställningar och prenumerationsinformation om](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span><span class="sxs-lookup"><span data-stu-id="6a910-124">[How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span></span>
