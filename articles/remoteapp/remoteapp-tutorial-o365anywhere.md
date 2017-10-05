---
title: "Få samma Office 365-upplevelse på alla enheter med Azure RemoteApp | Microsoft Docs"
description: "Lär dig att dela en Office 365-app med dina användare genom att använda Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
ms.openlocfilehash: 584c781c97097cda3c1455ade05cba8659f11073
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a><span data-ttu-id="05d1f-103">Få samma Office 365-upplevelse på alla enheter med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="05d1f-103">Get the same Office 365 experience on any device with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="05d1f-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="05d1f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="05d1f-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="05d1f-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="05d1f-106">Den här artikeln beskriver hur du distribuerar Office 365 på alla enheter i företaget.</span><span class="sxs-lookup"><span data-stu-id="05d1f-106">This article will cover how to deploy Office 365 on any device in your company.</span></span> <span data-ttu-id="05d1f-107">Användarna kan få samma funktioner och användargränssnitt på Android, Apple och Windows.</span><span class="sxs-lookup"><span data-stu-id="05d1f-107">Your users can get the same capabilities and UI experience on Android, Apple and Windows.</span></span>

<span data-ttu-id="05d1f-108">Vi gör det genom att använda Azure RemoteApp och låta Office 365 vara värd för skalningsbara virtuella datorer i Azure som användarna kan ansluta till.</span><span class="sxs-lookup"><span data-stu-id="05d1f-108">We will accomplish this using Azure RemoteApp by hosting Office 365 on scale-able virtual machines in Azure that users can connect to.</span></span> <span data-ttu-id="05d1f-109">Den här uppsättningen av virtuella datorer kallar vi ”molnsamlingen”.</span><span class="sxs-lookup"><span data-stu-id="05d1f-109">This set of virtual machines we call a "cloud collection".</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="05d1f-110">Skapa en molnsamling</span><span class="sxs-lookup"><span data-stu-id="05d1f-110">Create a cloud collection</span></span>
<span data-ttu-id="05d1f-111">Först när du har skapat ett Azure-konto navigerar du till **RemoteApp** genom att klicka på länken till vänster.</span><span class="sxs-lookup"><span data-stu-id="05d1f-111">First after you have created an Azure account, navigate to **RemoteApp** by clicking on the link on the left side.</span></span>
<span data-ttu-id="05d1f-112">![Visa Azure RemoteApp på Azure-portalen](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span><span class="sxs-lookup"><span data-stu-id="05d1f-112">![Showing Azure RemoteApp on the Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span></span>

<span data-ttu-id="05d1f-113">Fortsätt sedan genom att klicka på **ny** längst ned och ”snabbt skapa” en samling.</span><span class="sxs-lookup"><span data-stu-id="05d1f-113">Then continue by clicking **new** on the bottom and "quick creating" a collection.</span></span> <span data-ttu-id="05d1f-114">Ange de uppgifter som vi tillhandahåller om namn, region, prenumeration och plan samt bilden ”Office Proffesional 2013”.</span><span class="sxs-lookup"><span data-stu-id="05d1f-114">Provide a name, the region, the subscription, the plan and the image "Office Proffesional 2013" that we provide.</span></span>
<span data-ttu-id="05d1f-115">![Dialogrutan Skapa](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span><span class="sxs-lookup"><span data-stu-id="05d1f-115">![Create Dialog](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span></span>

<span data-ttu-id="05d1f-116">När du har gjort klart formuläret ska samlingen börja skapas.</span><span class="sxs-lookup"><span data-stu-id="05d1f-116">Once you finish the form the collection creation process should start.</span></span> <span data-ttu-id="05d1f-117">Det kan ta upp till ungefär en timme.</span><span class="sxs-lookup"><span data-stu-id="05d1f-117">This may take up to an hour or so.</span></span>

![Väntar](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

<span data-ttu-id="05d1f-119">När processen är klar ser det ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="05d1f-119">Once the process is done, it will look something like this.</span></span> <span data-ttu-id="05d1f-120">Om vi klickar på **Publicera** kan vi se att de flesta Office-program redan har publicerats för oss.</span><span class="sxs-lookup"><span data-stu-id="05d1f-120">If we click **Publishing** we can see that most Office applications have been published for us already.</span></span>
<span data-ttu-id="05d1f-121">![Samlingen har skapats](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span><span class="sxs-lookup"><span data-stu-id="05d1f-121">![Collection created](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span></span>

![Publicerade appar](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

<span data-ttu-id="05d1f-123">Vid det här laget kan du också lägga till fler användare som har åtkomst till den här samlingen genom att klicka på **Användaråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="05d1f-123">At this point you can also add more users that have access to this collection by clicking **User Access**.</span></span>
<span data-ttu-id="05d1f-124">![Konfigurera användaråtkomst](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span><span class="sxs-lookup"><span data-stu-id="05d1f-124">![Configure user access](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span></span>

<span data-ttu-id="05d1f-125">Nu ska vi prova att ansluta till Office 365!</span><span class="sxs-lookup"><span data-stu-id="05d1f-125">Now let's try out connecting to Office 365!</span></span>

## <a name="connect-to-office-365"></a><span data-ttu-id="05d1f-126">Ansluta till Office 365</span><span class="sxs-lookup"><span data-stu-id="05d1f-126">Connect to Office 365</span></span>
<span data-ttu-id="05d1f-127">Vi ska gå till [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), rulla ned och klicka på **Ladda ned klienter** och installera Azure RemoteApp-klienten på den enheten du använder.</span><span class="sxs-lookup"><span data-stu-id="05d1f-127">We'll head over to [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scroll down  and click **Download clients** to install the Azure RemoteApp client on the device you're on.</span></span> <span data-ttu-id="05d1f-128">Skärmbilderna nedan gäller för Windows.</span><span class="sxs-lookup"><span data-stu-id="05d1f-128">The screenshots below are for Windows.</span></span>

<span data-ttu-id="05d1f-129">När programmet startas blir du ombedd att logga in med ditt Microsoft-konto (kallades förut ”Live ID”). Använd samma som ditt Azure-konto för tillfället.</span><span class="sxs-lookup"><span data-stu-id="05d1f-129">Once the application starts you'll be asked to sign in with your Microsoft account (formerly called a "Live ID"), use the same one as your Azure account for now.</span></span> <span data-ttu-id="05d1f-130">När du är inloggad bör du se ett meddelande om nya inbjudningar. Om du klickar på det ska du se en lista som liknar den nedan.</span><span class="sxs-lookup"><span data-stu-id="05d1f-130">When you're signed in you should see a notification about new invitations, click there and you should see a list like one below.</span></span> <span data-ttu-id="05d1f-131">Acceptera den inbjudan som matchar din mejl som Azure-kontoägare.</span><span class="sxs-lookup"><span data-stu-id="05d1f-131">Accept the invitation that matches your Azure account owner email.</span></span>

![Ny inbjudan](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

<span data-ttu-id="05d1f-133">Hur det ser ut när det finns nya inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="05d1f-133">What it looks like when there are new invitations.</span></span>

![Godkänna ett program](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

<span data-ttu-id="05d1f-135">Du bör se alla Office-program i Azure RemoteApp-klienten när du har accepterat inbjudan.</span><span class="sxs-lookup"><span data-stu-id="05d1f-135">Once you accept the invitation you should see all the Office apps in the Azure RemoteApp client.</span></span>

![Lista över appar](./media/remoteapp-tutorial-o365anywhere/9-work.png)

<span data-ttu-id="05d1f-137">När du klickar på någon av dessa bör programmet startas på den virtuella Azure-datorn och du är klar!</span><span class="sxs-lookup"><span data-stu-id="05d1f-137">When you click on any of these the application should start on the Azure virtual machine and you should be all set!</span></span> <span data-ttu-id="05d1f-138">Ha det så kul!</span><span class="sxs-lookup"><span data-stu-id="05d1f-138">Enjoy!</span></span>

![startar](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

