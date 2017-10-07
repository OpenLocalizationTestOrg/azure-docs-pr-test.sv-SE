---
title: "aaaAdd användaren-tooyour Azure RemoteApp-samlingen | Microsoft Docs"
description: "Lär dig hur tooadd användare tooyour Azure RemoteApp-samling"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 6b751fd2-2b11-499f-a2eb-12cb47f3129b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 0ae88e04c8bfc2ed55dc963945ed7e9ff687b603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-user-tooyour-azure-remoteapp-collection"></a><span data-ttu-id="8939b-103">Hur tooadd användaren-tooyour Azure RemoteApp-samling</span><span class="sxs-lookup"><span data-stu-id="8939b-103">How tooadd a user tooyour Azure RemoteApp collection</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8939b-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="8939b-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8939b-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="8939b-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8939b-106">Innan användarna kan se och använda dina appar i Azure RemoteApp, ha toogrant dem åtkomst till tooyour samling.</span><span class="sxs-lookup"><span data-stu-id="8939b-106">Before your users can see and use your apps in Azure RemoteApp, you have toogrant them access tooyour collection.</span></span> <span data-ttu-id="8939b-107">Detta är en del enkelt hello: på hello **användaråtkomst** fliken ange hello kontoinformation för hello användaren och klicka sedan på kryssmarkeringen hello.</span><span class="sxs-lookup"><span data-stu-id="8939b-107">This is hello easy part: On hello **User Access** tab, enter hello account information for hello user, and then click hello check mark.</span></span>

<span data-ttu-id="8939b-108">Information om vad behöver du?</span><span class="sxs-lookup"><span data-stu-id="8939b-108">What account information do you need?</span></span> <span data-ttu-id="8939b-109">Det beror på hello typen av samling du skapade (moln eller hybrid) och om du använder Office 365 ProPlus i samlingen.</span><span class="sxs-lookup"><span data-stu-id="8939b-109">That depends on hello type of collection you created (cloud or hybrid) and whether you are using Office 365 ProPlus in that collection.</span></span>

## <a name="supported-user-identities"></a><span data-ttu-id="8939b-110">Användaridentiteter som stöds</span><span class="sxs-lookup"><span data-stu-id="8939b-110">Supported user identities</span></span>
<span data-ttu-id="8939b-111">hello olika samlingstyper (moln eller hybrid) stöd för användning av olika användaridentiteter för åtkomst tooapplications.</span><span class="sxs-lookup"><span data-stu-id="8939b-111">hello different collection types (cloud vs. hybrid) support using different user identities for access tooapplications.</span></span>  

<span data-ttu-id="8939b-112">För en hybridsamling av RemoteApp-du behöver tooset upp en Active Directory-domän infrastruktur på lokal och en Azure Active Directory-klient med katalogintegrering (och enkel alternativt inloggning).</span><span class="sxs-lookup"><span data-stu-id="8939b-112">For a hybrid collection of RemoteApp, you need tooset up an Active Directory domain infrastructure on premises and an Azure Active Directory tenant with Directory Integration (and optionally single sign-on).</span></span> <span data-ttu-id="8939b-113">Dessutom måste toocreate vissa Active Directory-objekt i hello lokala katalog.</span><span class="sxs-lookup"><span data-stu-id="8939b-113">Additionally, you need toocreate some Active Directory objects in hello on-premises directory.</span></span>  

<span data-ttu-id="8939b-114">För en molnsamling i RemoteApp-kan användare som har stöd för identiteter för Azure Active Directory beviljas användaren åtkomst tooRemoteApp tooinclude Microsoft Accounts.</span><span class="sxs-lookup"><span data-stu-id="8939b-114">For a cloud collection of RemoteApp, any user that has Azure Active Directory support identities can be granted user access tooRemoteApp tooinclude Microsoft Accounts.</span></span>  <span data-ttu-id="8939b-115">Se hello tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="8939b-115">See hello table below.</span></span>

<span data-ttu-id="8939b-116">Office 365-användare är Azure Active Directory-användare.</span><span class="sxs-lookup"><span data-stu-id="8939b-116">Office 365 users are Azure Active Directory users.</span></span> <span data-ttu-id="8939b-117">Om de har Azure Active Directory hybrid kan Directory synkroniseras konton, de beviljas användaråtkomst i en distribution med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8939b-117">If they have Azure Active Directory hybrid, Directory synchronized accounts, they can be granted user access in a RemoteApp hybrid deployment.</span></span>   

<span data-ttu-id="8939b-118">Du kan använda den här tabellen som en snabbreferens som identitet stöds i din samling och hello Active Directory-krav är.</span><span class="sxs-lookup"><span data-stu-id="8939b-118">You can use this table as a quick reference for which identity is supported in your collection and what hello Active Directory requirements are.</span></span>

| <span data-ttu-id="8939b-119">Användarkonton</span><span class="sxs-lookup"><span data-stu-id="8939b-119">User accounts</span></span> | <span data-ttu-id="8939b-120">Molnet</span><span class="sxs-lookup"><span data-stu-id="8939b-120">Cloud</span></span> | <span data-ttu-id="8939b-121">Hybrid</span><span class="sxs-lookup"><span data-stu-id="8939b-121">Hybrid</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8939b-122">Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="8939b-122">Microsoft Account</span></span> |<span data-ttu-id="8939b-123">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-123">Yes</span></span> |<span data-ttu-id="8939b-124">Nej</span><span class="sxs-lookup"><span data-stu-id="8939b-124">No</span></span> |
| <span data-ttu-id="8939b-125">Azure Active Directory (AD Azure)</span><span class="sxs-lookup"><span data-stu-id="8939b-125">Azure Active Directory (Azure AD)</span></span> | | |
| <span data-ttu-id="8939b-126">Azure AD cloud endast</span><span class="sxs-lookup"><span data-stu-id="8939b-126">Azure AD cloud only</span></span> |<span data-ttu-id="8939b-127">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-127">Yes</span></span> |<span data-ttu-id="8939b-128">Nej</span><span class="sxs-lookup"><span data-stu-id="8939b-128">No</span></span> |
| <span data-ttu-id="8939b-129">ADsync med Lösenordssynkronisering</span><span class="sxs-lookup"><span data-stu-id="8939b-129">ADsync with password sync</span></span> |<span data-ttu-id="8939b-130">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-130">Yes</span></span> |<span data-ttu-id="8939b-131">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-131">Yes</span></span> |
| <span data-ttu-id="8939b-132">ADsync utan Lösenordssynkronisering</span><span class="sxs-lookup"><span data-stu-id="8939b-132">ADsync without password sync</span></span> |<span data-ttu-id="8939b-133">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-133">Yes</span></span> |<span data-ttu-id="8939b-134">Nej</span><span class="sxs-lookup"><span data-stu-id="8939b-134">No</span></span> |
| <span data-ttu-id="8939b-135">ADsync med AD FS</span><span class="sxs-lookup"><span data-stu-id="8939b-135">ADsync with AD FS</span></span> |<span data-ttu-id="8939b-136">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-136">Yes</span></span> |<span data-ttu-id="8939b-137">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-137">Yes</span></span> |
| <span data-ttu-id="8939b-138">[3-part Azure stöds identitetsleverantörer](https://msdn.microsoft.com/library/azure/jj679342.aspx) (exempel Ping)</span><span class="sxs-lookup"><span data-stu-id="8939b-138">[3rd-party Azure supported identity providers](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (example Ping)</span></span> |<span data-ttu-id="8939b-139">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-139">Yes</span></span> |<span data-ttu-id="8939b-140">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-140">Yes</span></span> |
| <span data-ttu-id="8939b-141">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="8939b-141">Multi-Factor Authentication</span></span> |<span data-ttu-id="8939b-142">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-142">Yes</span></span> |<span data-ttu-id="8939b-143">Ja</span><span class="sxs-lookup"><span data-stu-id="8939b-143">Yes</span></span> |

<span data-ttu-id="8939b-144">Checka ut [mer](remoteapp-ad.md) om att konfigurera Active Directory för RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8939b-144">Check out [more information](remoteapp-ad.md) about configuring Active Directory for RemoteApp.</span></span>

> [!NOTE]
> <span data-ttu-id="8939b-145">hello Azure Active Directory-användare måste komma från hello-klient som är associerad med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8939b-145">hello Azure Active Directory users must be from hello tenant that's associated with your subscription.</span></span> <span data-ttu-id="8939b-146">(Du kan visa och ändra din prenumeration på hello **inställningar** fliken hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="8939b-146">(You can view and modify your subscription on hello **Settings** tab in hello portal.</span></span> <span data-ttu-id="8939b-147">Se [ändra hello Azure Active Directory-klient som används av RemoteApp](remoteapp-changetenant.md) för mer information.)</span><span class="sxs-lookup"><span data-stu-id="8939b-147">See [Change hello Azure Active Directory tenant used by RemoteApp](remoteapp-changetenant.md) for more information.)</span></span>
> 
> 

## <a name="office-365-proplus-user-account-information"></a><span data-ttu-id="8939b-148">Office 365 ProPlus-kontoinformation</span><span class="sxs-lookup"><span data-stu-id="8939b-148">Office 365 ProPlus user account information</span></span>
<span data-ttu-id="8939b-149">Om du använder Office 365 ProPlus hello-mallavbildningen i samlingen *eller* om du har skapat en anpassad avbildning som använder Office 365 är bara tillåtna tooadd Azure Active Directory-användare som har Office 365-prenumerationer för hello standarddomän för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8939b-149">If you are using hello Office 365 ProPlus template image in your collection *or* if you created a custom image that uses Office 365, you are only allowed tooadd Azure Active Directory users that have Office 365 subscriptions for hello default domain of your subscription.</span></span> <span data-ttu-id="8939b-150">Se [med hjälp av Office 365 med Azure RemoteApp](remoteapp-o365.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8939b-150">See [Using Office 365 with Azure RemoteApp](remoteapp-o365.md) for more information.</span></span>

