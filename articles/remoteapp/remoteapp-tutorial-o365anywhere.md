---
title: "aaaGet hello samma Office 365-upplevelse på alla enheter med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooshare någon Office 365-app med dina användare via Azure RemoteApp."
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
ms.openlocfilehash: 140056c22c8c69b9ec605318e35a72b144da07eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-same-office-365-experience-on-any-device-with-azure-remoteapp"></a><span data-ttu-id="6593c-103">Get hello samma Office 365-upplevelse på alla enheter med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="6593c-103">Get hello same Office 365 experience on any device with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6593c-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="6593c-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="6593c-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="6593c-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="6593c-106">Den här artikeln beskriver hur toodeploy Office 365 på alla enheter i företaget.</span><span class="sxs-lookup"><span data-stu-id="6593c-106">This article will cover how toodeploy Office 365 on any device in your company.</span></span> <span data-ttu-id="6593c-107">Användarna kan få hello samma funktioner och användargränssnitt inloggningen på Android, Apple och Windows.</span><span class="sxs-lookup"><span data-stu-id="6593c-107">Your users can get hello same capabilities and UI experience on Android, Apple and Windows.</span></span>

<span data-ttu-id="6593c-108">Vi gör det genom att använda Azure RemoteApp och låta Office 365 vara värd för skalningsbara virtuella datorer i Azure som användarna kan ansluta till.</span><span class="sxs-lookup"><span data-stu-id="6593c-108">We will accomplish this using Azure RemoteApp by hosting Office 365 on scale-able virtual machines in Azure that users can connect to.</span></span> <span data-ttu-id="6593c-109">Den här uppsättningen av virtuella datorer kallar vi ”molnsamlingen”.</span><span class="sxs-lookup"><span data-stu-id="6593c-109">This set of virtual machines we call a "cloud collection".</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="6593c-110">Skapa en molnsamling</span><span class="sxs-lookup"><span data-stu-id="6593c-110">Create a cloud collection</span></span>
<span data-ttu-id="6593c-111">Först när du har skapat ett Azure-konto navigerar för**RemoteApp** genom att klicka på hello länk på hello vänster sida.</span><span class="sxs-lookup"><span data-stu-id="6593c-111">First after you have created an Azure account, navigate too**RemoteApp** by clicking on hello link on hello left side.</span></span>
<span data-ttu-id="6593c-112">![Visa Azure RemoteApp på hello Azure-portalen](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span><span class="sxs-lookup"><span data-stu-id="6593c-112">![Showing Azure RemoteApp on hello Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span></span>

<span data-ttu-id="6593c-113">Fortsätt sedan genom att klicka på **nya** på hello nedre och ”snabbt skapa” en samling.</span><span class="sxs-lookup"><span data-stu-id="6593c-113">Then continue by clicking **new** on hello bottom and "quick creating" a collection.</span></span> <span data-ttu-id="6593c-114">Ange ett namn, hello region, hello prenumeration, hello plan och hello bilden ”Office Proffesional 2013” som vi tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="6593c-114">Provide a name, hello region, hello subscription, hello plan and hello image "Office Proffesional 2013" that we provide.</span></span>
<span data-ttu-id="6593c-115">![Dialogrutan Skapa](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span><span class="sxs-lookup"><span data-stu-id="6593c-115">![Create Dialog](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span></span>

<span data-ttu-id="6593c-116">När du är klar ska hello formuläret hello samling processen starta.</span><span class="sxs-lookup"><span data-stu-id="6593c-116">Once you finish hello form hello collection creation process should start.</span></span> <span data-ttu-id="6593c-117">Det kan ta upp tooan timme.</span><span class="sxs-lookup"><span data-stu-id="6593c-117">This may take up tooan hour or so.</span></span>

![Väntar](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

<span data-ttu-id="6593c-119">När hello processen är klar, ser den ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="6593c-119">Once hello process is done, it will look something like this.</span></span> <span data-ttu-id="6593c-120">Om vi klickar på **Publicera** kan vi se att de flesta Office-program redan har publicerats för oss.</span><span class="sxs-lookup"><span data-stu-id="6593c-120">If we click **Publishing** we can see that most Office applications have been published for us already.</span></span>
<span data-ttu-id="6593c-121">![Samlingen har skapats](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span><span class="sxs-lookup"><span data-stu-id="6593c-121">![Collection created](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span></span>

![Publicerade appar](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

<span data-ttu-id="6593c-123">Då du kan också lägga till fler användare som har åtkomst toothis samling genom att klicka på **användaråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="6593c-123">At this point you can also add more users that have access toothis collection by clicking **User Access**.</span></span>
<span data-ttu-id="6593c-124">![Konfigurera användaråtkomst](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span><span class="sxs-lookup"><span data-stu-id="6593c-124">![Configure user access](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span></span>

<span data-ttu-id="6593c-125">Nu ska vi prova att ansluta tooOffice 365!</span><span class="sxs-lookup"><span data-stu-id="6593c-125">Now let's try out connecting tooOffice 365!</span></span>

## <a name="connect-toooffice-365"></a><span data-ttu-id="6593c-126">Ansluta tooOffice 365</span><span class="sxs-lookup"><span data-stu-id="6593c-126">Connect tooOffice 365</span></span>
<span data-ttu-id="6593c-127">Vi ska gå för[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), bläddra nedåt och klicka på **hämta klienter** tooinstall hello Azure RemoteApp-klienten på hello-enhet som du är på.</span><span class="sxs-lookup"><span data-stu-id="6593c-127">We'll head over too[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scroll down  and click **Download clients** tooinstall hello Azure RemoteApp client on hello device you're on.</span></span> <span data-ttu-id="6593c-128">hello skärmbilderna nedan gäller för Windows.</span><span class="sxs-lookup"><span data-stu-id="6593c-128">hello screenshots below are for Windows.</span></span>

<span data-ttu-id="6593c-129">När hello programmet startas blir du ombedd toosign in med ditt Microsoft-konto (kallades förut ”Live ID”), Använd hello samma som ditt Azure-konto för tillfället.</span><span class="sxs-lookup"><span data-stu-id="6593c-129">Once hello application starts you'll be asked toosign in with your Microsoft account (formerly called a "Live ID"), use hello same one as your Azure account for now.</span></span> <span data-ttu-id="6593c-130">När du är inloggad bör du se ett meddelande om nya inbjudningar. Om du klickar på det ska du se en lista som liknar den nedan.</span><span class="sxs-lookup"><span data-stu-id="6593c-130">When you're signed in you should see a notification about new invitations, click there and you should see a list like one below.</span></span> <span data-ttu-id="6593c-131">Acceptera hello inbjudan som matchar Azure-konto ägarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="6593c-131">Accept hello invitation that matches your Azure account owner email.</span></span>

![Ny inbjudan](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

<span data-ttu-id="6593c-133">Hur det ser ut när det finns nya inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="6593c-133">What it looks like when there are new invitations.</span></span>

![Godkänna ett program](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

<span data-ttu-id="6593c-135">När du har accepterat inbjudan hello bör du se alla hello Office-appar i hello Azure RemoteApp-klienten.</span><span class="sxs-lookup"><span data-stu-id="6593c-135">Once you accept hello invitation you should see all hello Office apps in hello Azure RemoteApp client.</span></span>

![Lista över appar](./media/remoteapp-tutorial-o365anywhere/9-work.png)

<span data-ttu-id="6593c-137">Om du klickar på någon av dessa hello-program ska starta på hello Azure-dator och du är klar!</span><span class="sxs-lookup"><span data-stu-id="6593c-137">When you click on any of these hello application should start on hello Azure virtual machine and you should be all set!</span></span> <span data-ttu-id="6593c-138">Ha det så kul!</span><span class="sxs-lookup"><span data-stu-id="6593c-138">Enjoy!</span></span>

![startar](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

