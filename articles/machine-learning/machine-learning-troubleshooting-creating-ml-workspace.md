---
title: "Felsöka: Skapa och ansluta tooa Machine Learning-arbetsytan | Microsoft Docs"
description: "Lösningar på vanliga problem med att skapa och ansluta tooan Azure Machine Learning-arbetsytan"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 965a0025e85ba4e22c2b037edfa923e7f7599069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-create-and-connect-tooan-machine-learning-workspace"></a><span data-ttu-id="71b65-103">Felsökningsguiden: skapa och ansluta tooan Machine Learning-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="71b65-103">Troubleshooting guide: Create and connect tooan Machine Learning workspace</span></span>
<span data-ttu-id="71b65-104">Den här guiden ger lösningar för några vanliga påträffade utmaningar när du ställer in Azure Machine Learning arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="71b65-104">This guide provides solutions for some frequently encountered challenges when you are setting up Azure Machine Learning workspaces.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a><span data-ttu-id="71b65-105">Arbetsytan ägare</span><span class="sxs-lookup"><span data-stu-id="71b65-105">Workspace owner</span></span>
<span data-ttu-id="71b65-106">tooopen en arbetsyta i Machine Learning Studio, måste du vara inloggad toohello Account du använde toocreate hello arbetsytan eller måste tooreceive en inbjudan från hello ägare toojoin hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="71b65-106">tooopen a workspace in Machine Learning Studio, you must be signed in toohello Microsoft Account you used toocreate hello workspace, or you need tooreceive an invitation from hello owner toojoin hello workspace.</span></span> <span data-ttu-id="71b65-107">Du kan hantera hello arbetsytan som innehåller hello möjlighet tooconfigure åtkomst från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="71b65-107">From hello Azure portal you can manage hello workspace, which includes hello ability tooconfigure access.</span></span>

<span data-ttu-id="71b65-108">Mer information om hur du hanterar en arbetsyta finns [hantera en Azure Machine Learning-arbetsytan].</span><span class="sxs-lookup"><span data-stu-id="71b65-108">For more information on managing a workspace, see [Manage an Azure Machine Learning workspace].</span></span>

[hantera en Azure Machine Learning-arbetsytan]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a><span data-ttu-id="71b65-110">Tillåtna regioner</span><span class="sxs-lookup"><span data-stu-id="71b65-110">Allowed regions</span></span>
<span data-ttu-id="71b65-111">Machine Learning är tillgänglig i ett begränsat antal regioner.</span><span class="sxs-lookup"><span data-stu-id="71b65-111">Machine Learning is currently available in a limited number of regions.</span></span> <span data-ttu-id="71b65-112">Om din prenumeration inte innehåller något av dessa regioner kan du se hello felmeddelande, ”du har inga prenumerationer i hello tillåtna regioner”.</span><span class="sxs-lookup"><span data-stu-id="71b65-112">If your subscription does not include one of these regions, you may see hello error message, “You have no subscriptions in hello allowed regions.”</span></span>

<span data-ttu-id="71b65-113">toorequest som en region läggs tooyour prenumerationen, skapa en ny Microsoft supportförfrågan från hello Azure portal, väljer **fakturering** som hello problemtyp och följ hello uppmanar toosubmit din begäran.</span><span class="sxs-lookup"><span data-stu-id="71b65-113">toorequest that a region be added tooyour subscription, create a new Microsoft support request from hello Azure portal, choose **Billing** as hello problem type, and follow hello prompts toosubmit your request.</span></span>

## <a name="storage-account"></a><span data-ttu-id="71b65-114">Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="71b65-114">Storage account</span></span>
<span data-ttu-id="71b65-115">hello tjänsten Machine Learning måste en toostore kontodata lagring.</span><span class="sxs-lookup"><span data-stu-id="71b65-115">hello Machine Learning service needs a storage account toostore data.</span></span> <span data-ttu-id="71b65-116">Du kan använda ett befintligt lagringskonto eller skapa ett nytt lagringskonto när du skapar hello nya Machine Learning-arbetsytan (om du har kvoten toocreate ett nytt lagringskonto).</span><span class="sxs-lookup"><span data-stu-id="71b65-116">You can use an existing storage account, or you can create a new storage account when you create hello new Machine Learning workspace (if you have quota toocreate a new storage account).</span></span>

<span data-ttu-id="71b65-117">När hello nya Machine Learning-arbetsytan har skapats kan logga du in tooMachine Learning Studio med hjälp av hello Microsoft-konto du använde toocreate hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="71b65-117">After hello new Machine Learning workspace is created, you can sign in tooMachine Learning Studio by using hello Microsoft account you used toocreate hello workspace.</span></span> <span data-ttu-id="71b65-118">Om det uppstår fel hälsningsmeddelande Använd ”arbetsytan kunde inte hittas” (liknande toohello följande skärmbild), följande steg toodelete hello dina cookies.</span><span class="sxs-lookup"><span data-stu-id="71b65-118">If you encounter hello error message, “Workspace Not Found” (similar toohello following screenshot), please use hello following steps toodelete your browser cookies.</span></span>

![Det gick inte att hitta arbetsytan][screen3]

<span data-ttu-id="71b65-120">**toodelete cookies i webbläsaren**</span><span class="sxs-lookup"><span data-stu-id="71b65-120">**toodelete browser cookies**</span></span>

1. <span data-ttu-id="71b65-121">Om du använder Internet Explorer klickar du på hello **verktyg** i hello övre högra hörnet och välj **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="71b65-121">If you use Internet Explorer, click hello **Tools** button in hello upper-right corner and select **Internet options**.</span></span>  

![Internet-alternativ][screen4]

2. <span data-ttu-id="71b65-123">Under hello **allmänna** klickar du på **ta bort...**</span><span class="sxs-lookup"><span data-stu-id="71b65-123">Under hello **General** tab, click **Delete…**</span></span>

![Fliken Allmänt][screen5]

3. <span data-ttu-id="71b65-125">I hello **ta bort webbhistorik** dialogrutan Kontrollera **Cookies och webbplatsen data** är markerad och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="71b65-125">In hello **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.</span></span>

![Ta bort cookies][screen6]

<span data-ttu-id="71b65-127">Starta om hello webbläsaren efter hello cookies bort, och sedan gå toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) sidan.</span><span class="sxs-lookup"><span data-stu-id="71b65-127">After hello cookies are deleted, restart hello browser and then go toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) page.</span></span> <span data-ttu-id="71b65-128">När du uppmanas ange ett användarnamn och lösenord, hello samma Microsoft-konto som du använde toocreate hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="71b65-128">When you are prompted for a user name and password, enter hello same Microsoft account you used toocreate hello workspace.</span></span>

## <a name="comments"></a><span data-ttu-id="71b65-129">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="71b65-129">Comments</span></span>

<span data-ttu-id="71b65-130">Vårt mål är toomake hello Machine Learning-upplevelse som sömlös som möjligt.</span><span class="sxs-lookup"><span data-stu-id="71b65-130">Our goal is toomake hello Machine Learning experience as seamless as possible.</span></span> <span data-ttu-id="71b65-131">Kontrollera efter eventuella kommentarer och problem vid hello [Azure Machine Learning-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp oss fungerar du bättre.</span><span class="sxs-lookup"><span data-stu-id="71b65-131">Please post any comments and issues at hello [Azure Machine Learning forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp us serve you better.</span></span>

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
