---
title: "aaaNon tekniska krav för att skapa ett erbjudande om hello Azure Marketplace | Microsoft Docs"
description: "Förstå hello kraven för att skapa och distribuera ett erbjudande toohello Azure Marketplace för andra toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ms.openlocfilehash: 472096863084cc58dc921313419ab60b1a08a3bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="general-prerequisites-for-creating-an-offer-for-hello-azure-marketplace"></a><span data-ttu-id="d3ed8-103">Allmänna krav för att skapa ett erbjudande om hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="d3ed8-103">General prerequisites for creating an offer for hello Azure Marketplace</span></span>
<span data-ttu-id="d3ed8-104">Förstå hello Allmänt, business-process-elementbaserade krav som är nödvändiga toomove med hello erbjuder skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-104">Understand hello general, business-process-centric prerequisites that are needed toomove forward with hello offer creation process.</span></span>

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a><span data-ttu-id="d3ed8-105">Se till att du är registrerad som en säljare med Microsoft</span><span class="sxs-lookup"><span data-stu-id="d3ed8-105">Ensure that you are registered as a seller with Microsoft</span></span>
<span data-ttu-id="d3ed8-106">Detaljerade instruktioner om hur du registrerar ett säljare-konto med Microsoft gå för[skapande av konton och registrering](marketplace-publishing-accounts-creation-registration.md).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-106">For detailed instructions on registering a seller account with Microsoft, go too[Account creation and registration](marketplace-publishing-accounts-creation-registration.md).</span></span>

* <span data-ttu-id="d3ed8-107">**Om ditt företag har redan registrerats som en säljare i hello Dev Center och du vill toocreate ett nytt erbjudande** sedan inloggningen toohello publicering portal med hello samma e-id med vilka Dev Center har registrerats.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-107">**If your company is already registered as a seller in hello Dev Center and you want toocreate a new offer,** then login toohello Publishing portal with hello same email id with which Dev Center registration is done.</span></span> <span data-ttu-id="d3ed8-108">Det här steget krävs så att hello Dev Center och publicering portal är kopplade till varandra.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-108">This step is required so that hello Dev Center and Publishing portal are linked with each other.</span></span>
* <span data-ttu-id="d3ed8-109">**Om ditt företag har redan registrerats som en säljare i hello Dev Center och du vill tooedit ett befintligt erbjudande** sedan antingen inloggningen toohello publicering portal hello administratörskonto eller med ett konto som har lagts till som medadministratör i hello publicering av portalen .</span><span class="sxs-lookup"><span data-stu-id="d3ed8-109">**If your company is already registered as a seller in hello Dev Center and you want tooedit an existing offer,** then either login toohello Publishing portal with hello admin account or with an account which is added as a co-admin in hello Publishing portal.</span></span> <span data-ttu-id="d3ed8-110">Steg tooadd konto för en medadministratör anges nedan.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-110">Steps tooadd a co-admin account is given below.</span></span>

## <a name="steps-tooadd-a-co-admin-in-hello-publishing-portal"></a><span data-ttu-id="d3ed8-111">Tooadd steg för en medadministratör i hello Publishing Portal</span><span class="sxs-lookup"><span data-stu-id="d3ed8-111">Steps tooadd a co-admin in hello Publishing Portal</span></span>
<span data-ttu-id="d3ed8-112">Administratörer av hello publicering portal kan lägga till andra medlemmar i hello företag som arbetar på hello program som medadministratör i hello hello publicering av portalen.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-112">Admins of hello Publishing portal can add hello other members of hello company, who are working on hello application, as a co-admin in hello Publishing portal.</span></span> <span data-ttu-id="d3ed8-113">**Förutsatt att du är Hej administratör** anges nedan är hello steg tooadd co-administratör.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-113">**Assuming that you are hello admin,** given below are hello steps tooadd a co-admin.</span></span>

> [!NOTE]
> <span data-ttu-id="d3ed8-114">För nya användare innan du lägger till en medadministratör hello publicering portal, se till att du har skapat minst ett program i hello publicering av portalen.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-114">For new users, before you add a co-admin in hello Publishing portal, ensure that you have created at least one application in hello Publishing portal.</span></span> <span data-ttu-id="d3ed8-115">Detta krävs som hello **UTGIVARE** fliken visas endast när du har skapat minst ett program i hello publicering av portalen.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-115">This is required as hello **PUBLISHERS** tab appear only after creating at least one application in hello Publishing portal.</span></span>
> 
> 

1. <span data-ttu-id="d3ed8-116">Se till att hello medadministratör e-post-id är ett Microsoft-account(MSA).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-116">Ensure that hello co-admin email id is a Microsoft account(MSA).</span></span> <span data-ttu-id="d3ed8-117">Om inte, registrera den som en MSA med detta [länk](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-117">If not, register it as a MSA using this [link](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).</span></span>
2. <span data-ttu-id="d3ed8-118">Kontrollera att det finns minst ett program under hello administratörskonto innan du försöker tooadd co-administratör.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-118">Ensure that there is at least one application under hello admin account before trying tooadd a co-admin.</span></span>
3. <span data-ttu-id="d3ed8-119">Hello senare steg när du är klar, logga in toohello publicera portal med hello medadministratör e-id och sedan logga ut.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-119">After hello above steps are done, login toohello Publishing portal with hello co-admin email id and then log out.</span></span>
4. <span data-ttu-id="d3ed8-120">Nu inloggningen toohello publicering portal med ID: t för hello admin-e-post.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-120">Now login toohello Publishing portal with hello admin email id.</span></span>
5. <span data-ttu-id="d3ed8-121">Navigera tooPublishers -> ditt konto -> Välj administratörer -> Lägg till hello medadministratör (skärmbilden nedan)</span><span class="sxs-lookup"><span data-stu-id="d3ed8-121">Navigate tooPublishers->select your account->Administrators->Add hello co-admin (screenshot given below)</span></span>
   
    ![Rita](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. <span data-ttu-id="d3ed8-123">Kontrollera som e-ID: n anges vid hello olika stegen i hello publiceringsprocessen (t.ex. Dev Center, publicera portal) som ska övervakas för all kommunikation från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-123">Ensure that email ids provided at hello various stages of hello publishing process (e.g. Dev Center, Publishing portal) are monitored for any communication from Microsoft.</span></span>
7. <span data-ttu-id="d3ed8-124">För registrering av Dev Center Undvik att använda ett konto som är associerade med en enskild person.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-124">For Dev Center registration, avoid using an account associated with a single person.</span></span> <span data-ttu-id="d3ed8-125">Detta rekommenderas för beroendet från en person.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-125">This is suggested for removing dependency from one individual.</span></span>
8. <span data-ttu-id="d3ed8-126">Om du står inför eventuella problem med registreringen Dev Center sedan Utlös en biljett med den här [länk](https://developer.microsoft.com/en-us/windows/support).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-126">If you face any issues with Dev Center registration, then please raise a ticket using this [link](https://developer.microsoft.com/en-us/windows/support).</span></span>

## <a name="steps-toodelete-a-co-admin-in-hello-publishing-portal"></a><span data-ttu-id="d3ed8-127">Toodelete steg för en medadministratör i hello publicering av portalen</span><span class="sxs-lookup"><span data-stu-id="d3ed8-127">Steps toodelete a co-admin in hello Publishing portal</span></span>
<span data-ttu-id="d3ed8-128">**Förutsatt att du är Hej administratör** anges nedan är hello steg toodelete co-administratör.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-128">**Assuming that you are hello admin,** given below are hello steps toodelete a co-admin.</span></span>

1. <span data-ttu-id="d3ed8-129">Inloggningen toohello publicering portal med ID: t för hello admin-e-post.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-129">Login toohello Publishing portal with hello admin email id.</span></span>
2. <span data-ttu-id="d3ed8-130">Navigera för**utgivare** -> Välj ditt konto -> **administratörer** -> **Medadministratörer**.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-130">Navigate too**Publishers** -> select your account -> **Administrators** -> **Co-Admins**.</span></span>
3. <span data-ttu-id="d3ed8-131">Klicka på hello **X** knappen Nästa toohello medadministratör du vill ta bort totalvärde (skärmbilden nedan).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-131">Click on hello **X** button next toohello co-admin you want tot delete (screenshot given below).</span></span>
   
    ![Rita](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> <span data-ttu-id="d3ed8-133">Du har inte toocomplete skatt och bank företagsinformation om du planerar toopublish endast kostnadsfria erbjudanden (eller använda din egen licens).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-133">You do not have toocomplete company tax and bank information if you are planning toopublish only free offers (or bring your own license).</span></span>
> 
> <span data-ttu-id="d3ed8-134">hello företagets registreringen måste vara slutförd tooget igång.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-134">hello company registration must be completed tooget started.</span></span> <span data-ttu-id="d3ed8-135">Men när ditt företag fungerar på hello skatt och bank informationen i hello Microsoft Developer konto, hello utvecklare kan börja jobba skapar hello avbildning av virtuell dator i hello [Publiceringsportal](https://publish.windowsazure.com), tas den certifierade, och testning i hello Azure mellanlagringsmiljön.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-135">However, while your company works on hello tax and bank information in hello Microsoft Developer account, hello developers can start working on creating hello virtual machine image in hello [Publishing Portal](https://publish.windowsazure.com), getting it certified, and testing it in hello Azure staging environment.</span></span> <span data-ttu-id="d3ed8-136">Hello fullständig säljare konto godkännande behöver du bara för hello sista steget i att publicera ditt erbjudande toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-136">You will need hello complete seller account approval only for hello final step of publishing your offer toohello Azure Marketplace.</span></span>
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a><span data-ttu-id="d3ed8-137">Skaffa en Azure-prenumeration ”betalning per användning”</span><span class="sxs-lookup"><span data-stu-id="d3ed8-137">Acquire an Azure "pay-as-you-go" subscription</span></span>
<span data-ttu-id="d3ed8-138">Detta är hello prenumeration som du vill använda toocreate VM-avbildningar och överlämna hello bilder toohello [Azure Marketplace](https://azure.microsoft.com/marketplace/).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-138">This is hello subscription that you will use toocreate your VM images and hand over hello images toohello [Azure Marketplace](https://azure.microsoft.com/marketplace/).</span></span> <span data-ttu-id="d3ed8-139">Om du inte har en befintlig prenumeration sedan logga på https://account.windowsazure.com/signup?offer=ms-azr-0003p.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-139">If you do not have an existing subscription, then please sign up at https://account.windowsazure.com/signup?offer=ms-azr-0003p.</span></span>

## <a name="sell-from-countries"></a><span data-ttu-id="d3ed8-140">”Säljer-från-länder</span><span class="sxs-lookup"><span data-stu-id="d3ed8-140">"Sell-from" countries</span></span>
> [!WARNING]
> <span data-ttu-id="d3ed8-141">I ordning toosell dina tjänster på hello Azure Marketplace, du måste se till att registrerade entiteten är från en hello godkända ”säljer-från-länder.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-141">In order toosell your services on hello Azure Marketplace, you must make sure that your registered entity is from one of hello approved “sell-from” countries.</span></span> <span data-ttu-id="d3ed8-142">Den här begränsningen är beloppet och skatt skäl.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-142">This restriction is for payout and taxation reasons.</span></span> <span data-ttu-id="d3ed8-143">Vi söker aktivt tooexpand denna lista över länder i hello nära framtiden, så Håll ögonen öppna.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-143">We are actively looking tooexpand this list of countries in hello near future, so stay tuned.</span></span> <span data-ttu-id="d3ed8-144">Hello fullständig lista finns i avsnittet 1b av hello [Azure Marketplace deltagande principer](http://go.microsoft.com/fwlink/?LinkID=526833).</span><span class="sxs-lookup"><span data-stu-id="d3ed8-144">For hello complete list, see section 1b of hello [Azure Marketplace participation policies](http://go.microsoft.com/fwlink/?LinkID=526833).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d3ed8-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3ed8-145">Next steps</span></span>
<span data-ttu-id="d3ed8-146">När hello icke-tekniska krav är uppfyllda, nästa är hello erbjudande specifika tekniska krav.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-146">Once hello non-technical pre-requisites are fulfilled, next are hello offer specific technical prerequisites.</span></span> <span data-ttu-id="d3ed8-147">Klicka på hello länken toohello artikel för hello respektive erbjudande som du vill att toocreate för hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d3ed8-147">Click hello link toohello article for hello respective offer type that you would like toocreate for hello Azure Marketplace.</span></span>

* [<span data-ttu-id="d3ed8-148">Tekniska krav för VM</span><span class="sxs-lookup"><span data-stu-id="d3ed8-148">VM technical pre-requisites</span></span>](marketplace-publishing-vm-image-creation-prerequisites.md)
* [<span data-ttu-id="d3ed8-149">Lösning mallen tekniska krav</span><span class="sxs-lookup"><span data-stu-id="d3ed8-149">Solution Template technical pre-requisites</span></span>](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a><span data-ttu-id="d3ed8-150">Se även</span><span class="sxs-lookup"><span data-stu-id="d3ed8-150">See also</span></span>
* [<span data-ttu-id="d3ed8-151">Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="d3ed8-151">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)

