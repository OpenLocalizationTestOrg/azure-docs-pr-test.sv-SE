---
title: "Felsöka: Skapa och ansluta till en arbetsyta i Machine Learning | Microsoft Docs"
description: "Lösningar på vanliga problem med att skapa och ansluta till en Azure Machine Learning-arbetsytan"
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
ms.openlocfilehash: 398ac3d9c9d32a1ab10413ce0d7ce8d448890409
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a><span data-ttu-id="a442e-103">Felsökningsguide: Skapa och anslut till en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="a442e-103">Troubleshooting guide: Create and connect to an Machine Learning workspace</span></span>
<span data-ttu-id="a442e-104">Den här guiden ger lösningar för några vanliga påträffade utmaningar när du ställer in Azure Machine Learning arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="a442e-104">This guide provides solutions for some frequently encountered challenges when you are setting up Azure Machine Learning workspaces.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a><span data-ttu-id="a442e-105">Arbetsytan ägare</span><span class="sxs-lookup"><span data-stu-id="a442e-105">Workspace owner</span></span>
<span data-ttu-id="a442e-106">Om du vill öppna en arbetsyta i Machine Learning Studio, måste du vara inloggad på Account du använde för att skapa arbetsytan eller måste du få en inbjudan från ägare att ansluta till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a442e-106">To open a workspace in Machine Learning Studio, you must be signed in to the Microsoft Account you used to create the workspace, or you need to receive an invitation from the owner to join the workspace.</span></span> <span data-ttu-id="a442e-107">Du kan hantera arbetsytan, vilket ger möjlighet att konfigurera åtkomst från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a442e-107">From the Azure portal you can manage the workspace, which includes the ability to configure access.</span></span>

<span data-ttu-id="a442e-108">Mer information om hur du hanterar en arbetsyta finns [hantera en Azure Machine Learning-arbetsytan].</span><span class="sxs-lookup"><span data-stu-id="a442e-108">For more information on managing a workspace, see [Manage an Azure Machine Learning workspace].</span></span>

<span data-ttu-id="a442e-109">[hantera en Azure Machine Learning-arbetsytan]: machine-learning-manage-workspace.md</span><span class="sxs-lookup"><span data-stu-id="a442e-109">[Manage an Azure Machine Learning workspace]: machine-learning-manage-workspace.md</span></span>

## <a name="allowed-regions"></a><span data-ttu-id="a442e-110">Tillåtna regioner</span><span class="sxs-lookup"><span data-stu-id="a442e-110">Allowed regions</span></span>
<span data-ttu-id="a442e-111">Machine Learning är tillgänglig i ett begränsat antal regioner.</span><span class="sxs-lookup"><span data-stu-id="a442e-111">Machine Learning is currently available in a limited number of regions.</span></span> <span data-ttu-id="a442e-112">Om din prenumeration inte innehåller något av dessa regioner kan du se ett felmeddelande, ”du har inga prenumerationer i tillåtna regioner”.</span><span class="sxs-lookup"><span data-stu-id="a442e-112">If your subscription does not include one of these regions, you may see the error message, “You have no subscriptions in the allowed regions.”</span></span>

<span data-ttu-id="a442e-113">Om du vill begära att en region som ska läggas till din prenumeration, skapa en ny Microsoft supportförfrågan från Azure portal, väljer **fakturering** som problemtyp, och följ anvisningarna för att skicka din begäran.</span><span class="sxs-lookup"><span data-stu-id="a442e-113">To request that a region be added to your subscription, create a new Microsoft support request from the Azure portal, choose **Billing** as the problem type, and follow the prompts to submit your request.</span></span>

## <a name="storage-account"></a><span data-ttu-id="a442e-114">Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a442e-114">Storage account</span></span>
<span data-ttu-id="a442e-115">Tjänsten Machine Learning måste ett storage-konto för att lagra data.</span><span class="sxs-lookup"><span data-stu-id="a442e-115">The Machine Learning service needs a storage account to store data.</span></span> <span data-ttu-id="a442e-116">Du kan använda ett befintligt lagringskonto eller skapa ett nytt lagringskonto när du skapar den nya Machine Learning-arbetsytan (om du har en kvot för att skapa ett nytt lagringskonto).</span><span class="sxs-lookup"><span data-stu-id="a442e-116">You can use an existing storage account, or you can create a new storage account when you create the new Machine Learning workspace (if you have quota to create a new storage account).</span></span>

<span data-ttu-id="a442e-117">När den nya Machine Learning-arbetsytan har skapats kan logga du in till Machine Learning Studio med hjälp av Microsoft-konto som du använde för att skapa arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a442e-117">After the new Machine Learning workspace is created, you can sign in to Machine Learning Studio by using the Microsoft account you used to create the workspace.</span></span> <span data-ttu-id="a442e-118">Om du får felmeddelandet Använd ”arbetsytan kunde inte hittas” (ungefär som följande skärmbild), följande steg att ta bort cookies i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="a442e-118">If you encounter the error message, “Workspace Not Found” (similar to the following screenshot), please use the following steps to delete your browser cookies.</span></span>

![Det gick inte att hitta arbetsytan][screen3]

<span data-ttu-id="a442e-120">**Ta bort cookies i webbläsaren**</span><span class="sxs-lookup"><span data-stu-id="a442e-120">**To delete browser cookies**</span></span>

1. <span data-ttu-id="a442e-121">Om du använder Internet Explorer klickar du på den **verktyg** i det övre högra hörnet och välj **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="a442e-121">If you use Internet Explorer, click the **Tools** button in the upper-right corner and select **Internet options**.</span></span>  

![Internet-alternativ][screen4]

2. <span data-ttu-id="a442e-123">Under den **allmänna** klickar du på **ta bort...**</span><span class="sxs-lookup"><span data-stu-id="a442e-123">Under the **General** tab, click **Delete…**</span></span>

![Fliken Allmänt][screen5]

3. <span data-ttu-id="a442e-125">I den **ta bort webbhistorik** dialogrutan Kontrollera **Cookies och webbplatsen data** är markerad och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a442e-125">In the **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.</span></span>

![Ta bort cookies][screen6]

<span data-ttu-id="a442e-127">Starta om webbläsaren efter cookies tas bort och går sedan till den [Microsoft Azure Machine Learning](https://studio.azureml.net) sidan.</span><span class="sxs-lookup"><span data-stu-id="a442e-127">After the cookies are deleted, restart the browser and then go to the [Microsoft Azure Machine Learning](https://studio.azureml.net) page.</span></span> <span data-ttu-id="a442e-128">När du ombeds ange ett användarnamn och lösenord anger du samma Microsoft-konto som du använde för att skapa arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a442e-128">When you are prompted for a user name and password, enter the same Microsoft account you used to create the workspace.</span></span>

## <a name="comments"></a><span data-ttu-id="a442e-129">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="a442e-129">Comments</span></span>

<span data-ttu-id="a442e-130">Vårt mål är att göra Machine Learning-upplevelse som sömlös som möjligt.</span><span class="sxs-lookup"><span data-stu-id="a442e-130">Our goal is to make the Machine Learning experience as seamless as possible.</span></span> <span data-ttu-id="a442e-131">Kontrollera efter eventuella kommentarer och problem på den [Azure Machine Learning-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) som hjälper oss att hantera du bättre.</span><span class="sxs-lookup"><span data-stu-id="a442e-131">Please post any comments and issues at the [Azure Machine Learning forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) to help us serve you better.</span></span>

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
