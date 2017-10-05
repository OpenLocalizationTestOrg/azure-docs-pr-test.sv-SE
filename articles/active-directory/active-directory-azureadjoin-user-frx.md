---
title: Skapa en ny enhet med Azure AD under installationen | Microsoft Docs
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
ms.openlocfilehash: 4367f453ef7c794653dfa9f305b137a168e0d207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a><span data-ttu-id="0d8b2-103">Skapa en ny enhet med Azure AD under installationen</span><span class="sxs-lookup"><span data-stu-id="0d8b2-103">Set up a new device with Azure AD during Setup</span></span>
<span data-ttu-id="0d8b2-104">I Windows 10 ansluta användare sina enheter till Azure Active Directory (AD Azure) i den första körningen (FRX).</span><span class="sxs-lookup"><span data-stu-id="0d8b2-104">In Windows 10, users can join their devices to Azure Active Directory (Azure AD) in the first-run experience (FRX).</span></span> <span data-ttu-id="0d8b2-105">Detta gör att organisationer kan distribuera form av krymp enheter till sina anställda och studenter eller låta dem välja sina egna enheter (CYOD).</span><span class="sxs-lookup"><span data-stu-id="0d8b2-105">This allows organizations to distribute shrink-wrapped devices to their employees or students, or let them choose their own devices (CYOD).</span></span>
<span data-ttu-id="0d8b2-106">Om Windows 10 Professional eller Windows 10 Enterprise-utgåvor installeras på en enhet, standard upplevelsen installationsprocessen för företagsägda enheter.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-106">If either Windows 10 Professional or Windows 10 Enterprise editions is installed on a device, the experience defaults to the setup process for company-owned devices.</span></span>

## <a name="to-join-a-device-to-azure-ad"></a><span data-ttu-id="0d8b2-107">Att ansluta till en enhet till Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d8b2-107">To join a device to Azure AD</span></span>
1. <span data-ttu-id="0d8b2-108">När du aktiverar den nya enheten och starta installationen, bör du se den **komma redo** meddelande.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-108">When you turn on your new device and start the setup process, you should see the  **Getting Ready** message.</span></span> <span data-ttu-id="0d8b2-109">Följ anvisningarna för att konfigurera din enhet.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-109">Follow the prompts to set up your device.</span></span>
2. <span data-ttu-id="0d8b2-110">Starta genom att anpassa din region och språk.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-110">Start by customizing your region and language.</span></span> <span data-ttu-id="0d8b2-111">Acceptera licensvillkoren för programvara från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-111">Then accept the Microsoft Software License Terms.</span></span>
   <span data-ttu-id="0d8b2-112">![Anpassa för din region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span><span class="sxs-lookup"><span data-stu-id="0d8b2-112">![Customize for your region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span></span>
3. <span data-ttu-id="0d8b2-113">Markera det nätverk som du vill använda för att ansluta till Internet.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-113">Select the network you want to use for connecting to the Internet.</span></span>
4. <span data-ttu-id="0d8b2-114">Välj om du använder en personlig enhet eller en företagsägd enhet.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-114">Select whether you're using a personal device or a company-owned device.</span></span> <span data-ttu-id="0d8b2-115">Om det ägs av företaget, klickar du på **den här enheten tillhör mitt företag**.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-115">If it's company-owned, click **This device belongs to my organization**.</span></span> <span data-ttu-id="0d8b2-116">Detta startar Azure AD Join-upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-116">This starts the Azure AD Join experience.</span></span> <span data-ttu-id="0d8b2-117">Följande är en skärm som visas om du använder Windows 10 Professional.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-117">Following is a screen that you'll see if you're using Windows 10 Professional.</span></span>
   <span data-ttu-id="0d8b2-118"><center>
   ![Vem äger den här datorn skärmen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span><span class="sxs-lookup"><span data-stu-id="0d8b2-118"><center>
![Who owns this PC screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span></span>
5. <span data-ttu-id="0d8b2-119">Ange de autentiseringsuppgifter som har fått av din organisation.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-119">Enter the credentials that were provided to you by your organization.</span></span>
   <span data-ttu-id="0d8b2-120"><center>
   ![Inloggningssida](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span><span class="sxs-lookup"><span data-stu-id="0d8b2-120"><center>
![Sign-in screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span></span>
6. <span data-ttu-id="0d8b2-121">När du har angett ditt användarnamn, finns en matchande klient i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-121">After you have entered your user name, a matching tenant is located in Azure AD.</span></span> <span data-ttu-id="0d8b2-122">Om du är i en federerad domän, omdirigeras du till servern lokalt Secure säkerhetstokentjänst (STS) – till exempel Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="0d8b2-122">If you are in a federated domain, you will be redirected to your on-premises Secure Token Service (STS) server--for example, Active Directory Federation Services (AD FS).</span></span>
7. <span data-ttu-id="0d8b2-123">Ange dina autentiseringsuppgifter direkt på Azure AD-värdbaserad sidan om du är en användare i en ofedererad domän.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-123">If you are a user in a non-federated domain, enter your credentials directly on the Azure AD-hosted page.</span></span> <span data-ttu-id="0d8b2-124">Om företagsanpassning har konfigurerats kommer du också se organisationens logotyp och stöder text.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-124">If company branding was configured, you will also see your organization’s logo and support text.</span></span>
8. <span data-ttu-id="0d8b2-125">Du ombeds ange en utmaning för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-125">You're prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="0d8b2-126">Denna utmaning kan konfigureras av en IT-administratör.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-126">This challenge is configurable by an IT administrator.</span></span>
9. <span data-ttu-id="0d8b2-127">Azure AD kontrollerar om den här användarenhet kräver registrering i hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-127">Azure AD checks whether this user/device requires enrollment in mobile device management.</span></span>
10. <span data-ttu-id="0d8b2-128">Windows registrerar enheten i organisationens katalog i Azure AD och registrerar den i hantering av mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-128">Windows registers the device in the organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
11. <span data-ttu-id="0d8b2-129">Om du är en användare går du till skrivbordet genom processen för automatisk inloggning i Windows.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-129">If you are a managed user, Windows takes you to the desktop through the automatic sign-in process.</span></span>
12. <span data-ttu-id="0d8b2-130">Om du är en federerad användare dirigeras du till skärmen för Windows att ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-130">If you are a federated user, you are directed to the Windows sign-in screen to enter your credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="0d8b2-131">Ansluter till en lokal Windows Server Active Directory-domän i Windows out of box experience stöds inte.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-131">Joining an on-premises Windows Server Active Directory domain in the Windows out-of-box experience is not supported.</span></span> <span data-ttu-id="0d8b2-132">Därför, om du planerar att ansluta en dator till en domän, ska du markera länken **konfigurera Windows med ett lokalt konto** i stället.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-132">Therefore, if you plan to join a computer to a domain, you should select the link **Set up Windows with a local account** instead.</span></span> <span data-ttu-id="0d8b2-133">Du kan ansluta till domänen från inställningarna på datorn som du har gjort förut.</span><span class="sxs-lookup"><span data-stu-id="0d8b2-133">You can then join the domain from the settings on your computer as you’ve done before.</span></span>
> 
> 

## <a name="additional-information"></a><span data-ttu-id="0d8b2-134">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="0d8b2-134">Additional information</span></span>
* [<span data-ttu-id="0d8b2-135">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="0d8b2-135">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="0d8b2-136">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="0d8b2-136">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="0d8b2-137">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="0d8b2-137">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="0d8b2-138">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="0d8b2-138">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="0d8b2-139">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="0d8b2-139">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="0d8b2-140">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="0d8b2-140">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

