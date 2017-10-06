---
title: aaaSet upp en ny enhet med Azure AD under installationen | Microsoft Docs
description: "Ett avsnitt som förklarar hur användare kan konfigurera Azure AD Join under deras välkomstprogrammet."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 6afce4be7f084f1956a6f9dbddaa8def0605956d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a><span data-ttu-id="39639-103">Skapa en ny enhet med Azure AD under installationen</span><span class="sxs-lookup"><span data-stu-id="39639-103">Set up a new device with Azure AD during Setup</span></span>
<span data-ttu-id="39639-104">I Windows 10 kan användare ansluta till sina enheter tooAzure Active Directory (AD Azure) i hello förstahandsvalet (FRX).</span><span class="sxs-lookup"><span data-stu-id="39639-104">In Windows 10, users can join their devices tooAzure Active Directory (Azure AD) in hello first-run experience (FRX).</span></span> <span data-ttu-id="39639-105">Detta gör att organisationer toodistribute form av krymp enheter tootheir anställda och studenter eller att de ska välja sina egna enheter (CYOD).</span><span class="sxs-lookup"><span data-stu-id="39639-105">This allows organizations toodistribute shrink-wrapped devices tootheir employees or students, or let them choose their own devices (CYOD).</span></span>
<span data-ttu-id="39639-106">Om Windows 10 Professional eller Windows 10 Enterprise-utgåvan är installerat på en enhet kan uppstå hello standardvärden toohello installationsprocessen för företagsägda enheter.</span><span class="sxs-lookup"><span data-stu-id="39639-106">If either Windows 10 Professional or Windows 10 Enterprise editions is installed on a device, hello experience defaults toohello setup process for company-owned devices.</span></span>

## <a name="toojoin-a-device-tooazure-ad"></a><span data-ttu-id="39639-107">toojoin en enhet tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="39639-107">toojoin a device tooAzure AD</span></span>
1. <span data-ttu-id="39639-108">När du aktiverar den nya enheten och starta installationsprocessen hello, bör du se hello **komma redo** meddelande.</span><span class="sxs-lookup"><span data-stu-id="39639-108">When you turn on your new device and start hello setup process, you should see hello  **Getting Ready** message.</span></span> <span data-ttu-id="39639-109">Följ hello prompter tooset in din enhet.</span><span class="sxs-lookup"><span data-stu-id="39639-109">Follow hello prompts tooset up your device.</span></span>
2. <span data-ttu-id="39639-110">Starta genom att anpassa din region och språk.</span><span class="sxs-lookup"><span data-stu-id="39639-110">Start by customizing your region and language.</span></span> <span data-ttu-id="39639-111">Godkänn sedan hello licensvillkoren.</span><span class="sxs-lookup"><span data-stu-id="39639-111">Then accept hello Microsoft Software License Terms.</span></span>
   <span data-ttu-id="39639-112">![Anpassa för din region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span><span class="sxs-lookup"><span data-stu-id="39639-112">![Customize for your region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span></span>
3. <span data-ttu-id="39639-113">Välj hello-nätverk som du vill toouse för att ansluta toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="39639-113">Select hello network you want toouse for connecting toohello Internet.</span></span>
4. <span data-ttu-id="39639-114">Välj om du använder en personlig enhet eller en företagsägd enhet.</span><span class="sxs-lookup"><span data-stu-id="39639-114">Select whether you're using a personal device or a company-owned device.</span></span> <span data-ttu-id="39639-115">Om det ägs av företaget, klickar du på **den här enheten tillhör toomy organisation**.</span><span class="sxs-lookup"><span data-stu-id="39639-115">If it's company-owned, click **This device belongs toomy organization**.</span></span> <span data-ttu-id="39639-116">Detta startar hello Azure AD Join-upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="39639-116">This starts hello Azure AD Join experience.</span></span> <span data-ttu-id="39639-117">Följande är en skärm som visas om du använder Windows 10 Professional.</span><span class="sxs-lookup"><span data-stu-id="39639-117">Following is a screen that you'll see if you're using Windows 10 Professional.</span></span>
   <span data-ttu-id="39639-118"><center>
   ![Vem äger den här datorn skärmen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span><span class="sxs-lookup"><span data-stu-id="39639-118"><center>
![Who owns this PC screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span></span>
5. <span data-ttu-id="39639-119">Ange hello-autentiseringsuppgifter som angavs tooyou av din organisation.</span><span class="sxs-lookup"><span data-stu-id="39639-119">Enter hello credentials that were provided tooyou by your organization.</span></span>
   <span data-ttu-id="39639-120"><center>
   ![Inloggningssida](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span><span class="sxs-lookup"><span data-stu-id="39639-120"><center>
![Sign-in screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span></span>
6. <span data-ttu-id="39639-121">När du har angett ditt användarnamn, finns en matchande klient i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="39639-121">After you have entered your user name, a matching tenant is located in Azure AD.</span></span> <span data-ttu-id="39639-122">Om du har en federerad domän, kommer du att omdirigerade tooyour lokal säker säkerhetstokentjänst (STS) server – till exempel Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="39639-122">If you are in a federated domain, you will be redirected tooyour on-premises Secure Token Service (STS) server--for example, Active Directory Federation Services (AD FS).</span></span>
7. <span data-ttu-id="39639-123">Ange dina autentiseringsuppgifter direkt på hello Azure AD-värdbaserad sidan om du är en användare i en ofedererad domän.</span><span class="sxs-lookup"><span data-stu-id="39639-123">If you are a user in a non-federated domain, enter your credentials directly on hello Azure AD-hosted page.</span></span> <span data-ttu-id="39639-124">Om företagsanpassning har konfigurerats kommer du också se organisationens logotyp och stöder text.</span><span class="sxs-lookup"><span data-stu-id="39639-124">If company branding was configured, you will also see your organization’s logo and support text.</span></span>
8. <span data-ttu-id="39639-125">Du ombeds ange en utmaning för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="39639-125">You're prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="39639-126">Denna utmaning kan konfigureras av en IT-administratör.</span><span class="sxs-lookup"><span data-stu-id="39639-126">This challenge is configurable by an IT administrator.</span></span>
9. <span data-ttu-id="39639-127">Azure AD kontrollerar om den här användarenhet kräver registrering i hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="39639-127">Azure AD checks whether this user/device requires enrollment in mobile device management.</span></span>
10. <span data-ttu-id="39639-128">Windows registrerar hello enheten i hello organisations katalog i Azure AD och registrerar den i hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="39639-128">Windows registers hello device in hello organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
11. <span data-ttu-id="39639-129">Om du använder en hanterad, tar Windows toohello via hello automatisk inloggning process.</span><span class="sxs-lookup"><span data-stu-id="39639-129">If you are a managed user, Windows takes you toohello desktop through hello automatic sign-in process.</span></span>
12. <span data-ttu-id="39639-130">Om du är en federerad användare dirigeras toohello Windows-inloggning skärmen tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="39639-130">If you are a federated user, you are directed toohello Windows sign-in screen tooenter your credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="39639-131">Ansluter till en lokal Windows Server Active Directory-domän i Windows hello stöds out of box experience inte.</span><span class="sxs-lookup"><span data-stu-id="39639-131">Joining an on-premises Windows Server Active Directory domain in hello Windows out-of-box experience is not supported.</span></span> <span data-ttu-id="39639-132">Om du planerar toojoin en dator tooa domän måste du därför välja hello länk **konfigurera Windows med ett lokalt konto** i stället.</span><span class="sxs-lookup"><span data-stu-id="39639-132">Therefore, if you plan toojoin a computer tooa domain, you should select hello link **Set up Windows with a local account** instead.</span></span> <span data-ttu-id="39639-133">Du kan ansluta hello domänen från hello inställningar på datorn som du har gjort förut.</span><span class="sxs-lookup"><span data-stu-id="39639-133">You can then join hello domain from hello settings on your computer as you’ve done before.</span></span>
> 
> 

## <a name="additional-information"></a><span data-ttu-id="39639-134">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="39639-134">Additional information</span></span>
* [<span data-ttu-id="39639-135">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="39639-135">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="39639-136">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="39639-136">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="39639-137">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="39639-137">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="39639-138">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="39639-138">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="39639-139">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="39639-139">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="39639-140">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="39639-140">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

