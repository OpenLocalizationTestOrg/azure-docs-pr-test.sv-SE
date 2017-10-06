---
title: "aaaSetting in Azure AD Join för dina användare | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera Azure AD Join för din lokala katalog och enhetsregistrering."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 60a5aeb11292cb6057ab1065c3ab77e5981d0cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a><span data-ttu-id="f0916-103">Konfigurera Azure AD Join i din organisation</span><span class="sxs-lookup"><span data-stu-id="f0916-103">Setting up Azure AD Join in your organization</span></span>
<span data-ttu-id="f0916-104">Innan du konfigurerar Azure Active Directory Join (Azure AD Join) måste tooeither synkronisering upp din lokala katalog för användare toohello molnet eller manuellt skapa hanterade konton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0916-104">Before you set up Azure Active Directory Join (Azure AD Join), you need tooeither sync up your on-premises directory of users toohello cloud or manually create managed accounts in Azure AD.</span></span>

<span data-ttu-id="f0916-105">Detaljerade instruktioner för att synkronisera dina lokala användare tooAzure AD beskrivs i [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="f0916-105">Detailed instructions for syncing your on-premises users tooAzure AD is covered in [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="f0916-106">toomanually skapa och hantera användare i Azure AD, referera för[användarhantering i Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0916-106">toomanually create and manage users in Azure AD, refer too[User management in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span></span>

## <a name="set-up-device-registration"></a><span data-ttu-id="f0916-107">Konfigurera enhetsregistrering</span><span class="sxs-lookup"><span data-stu-id="f0916-107">Set up device registration</span></span>
1. <span data-ttu-id="f0916-108">Logga in toohello Azure portal som administratör.</span><span class="sxs-lookup"><span data-stu-id="f0916-108">Log on toohello Azure portal as an administrator.</span></span>
2. <span data-ttu-id="f0916-109">Hello vänster, Välj **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f0916-109">On hello left pane, select **Active Directory**.</span></span>
3. <span data-ttu-id="f0916-110">På hello **Directory** , Välj din katalog.</span><span class="sxs-lookup"><span data-stu-id="f0916-110">On hello **Directory** tab, select your directory.</span></span>
4. <span data-ttu-id="f0916-111">Välj hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="f0916-111">Select hello **Configure** tab.</span></span>
5. <span data-ttu-id="f0916-112">Gå toohello **enheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f0916-112">Go toohello **Devices** section.</span></span>
6. <span data-ttu-id="f0916-113">På hello **enheter** ställer du in hello följande:</span><span class="sxs-lookup"><span data-stu-id="f0916-113">On hello **devices** tab, set hello following:</span></span>  
   * <span data-ttu-id="f0916-114">**HÖGSTA antal enheter PER användare**: Välj hello högsta antalet enheter som en användare kan ha i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0916-114">**MAXIMUM NUMBER OF DEVICES PER USER**: Select hello maximum number of devices that a user can have in Azure AD.</span></span>  <span data-ttu-id="f0916-115">Om användarna når den här kvoten, kommer de inte att kunna tooadd ytterligare enheter förrän en eller flera av deras befintliga enheter tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f0916-115">If a user reaches this quota, they will not be able tooadd additional devices until one or more of their existing devices are removed.</span></span>
   * <span data-ttu-id="f0916-116">**KRÄV Multi-FACTOR Authentication tooJOIN enheter**: Ange om användare som är nödvändiga tooprovide en andra autentiseringsmetod factor toojoin deras enhet tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="f0916-116">**REQUIRE MULTI-FACTOR AUTH tooJOIN DEVICES**: Set whether users are required tooprovide a second authentication factor toojoin their device tooAzure AD.</span></span> <span data-ttu-id="f0916-117">Mer information om Azure Multi-Factor Authentication finns [komma igång med Azure Multi-Factor Authentication i molnet hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="f0916-117">For more information on Azure Multi-Factor Authentication, see [Getting started with Azure Multi-Factor Authentication in hello cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>
   * <span data-ttu-id="f0916-118">**ANVÄNDARE kan ansluta enheter för AZURE AD**: Välj hello användare och grupper som tillåts toojoin enheter tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="f0916-118">**USERS MAY AZURE AD JOIN DEVICES**: Select hello users and groups that are allowed toojoin devices tooAzure AD.</span></span>
   * <span data-ttu-id="f0916-119">**YTTERLIGARE ADMINISTRATÖRER för AZURE AD-anslutna enheter**: med Azure AD Premium eller hello Enterprise Mobility Suite (EMS) kan du välja vilka användare som beviljas lokal administratörsbehörighet toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="f0916-119">**ADDITIONAL ADMINISTRATORS ON AZURE AD JOINED DEVICES**: With Azure AD Premium or hello Enterprise Mobility Suite (EMS), you can choose which users are granted local administrator rights toohello device.</span></span> <span data-ttu-id="f0916-120">Globala administratörer och enhetsägare beviljas lokal administratörsbehörighet som standard.</span><span class="sxs-lookup"><span data-stu-id="f0916-120">Global administrators and device owners are granted local administrator rights by default.</span></span>

<span data-ttu-id="f0916-121"><center>![Konfigurera enhetsregistrering](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center></span><span class="sxs-lookup"><span data-stu-id="f0916-121"><center>![Set up device regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center></span></span>

<span data-ttu-id="f0916-122">När du konfigurerar Azure AD Join för dina användare, kan de ansluta tooAzure AD via deras företagets eller personliga enheter.</span><span class="sxs-lookup"><span data-stu-id="f0916-122">After you set up Azure AD Join for your users, they can connect tooAzure AD through their corporate or personal devices.</span></span>

<span data-ttu-id="f0916-123">Följande är hello tre scenarier som du kan använda tooenable användare-tooset upp Azure AD Join:</span><span class="sxs-lookup"><span data-stu-id="f0916-123">Following are hello three scenarios you can use tooenable your users tooset up Azure AD Join:</span></span>

* <span data-ttu-id="f0916-124">En användare att ansluta en företagsägd enhet direkt tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="f0916-124">Users join a company-owned device directly tooAzure AD.</span></span>
* <span data-ttu-id="f0916-125">Användarna domänansluter en företagsägd enhet toohello lokala Active Directory och utökar hello enheten tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="f0916-125">Users domain-join a company-owned device toohello on-premises Active Directory and then extend hello device tooAzure AD.</span></span>
* <span data-ttu-id="f0916-126">Användarna lägger till arbetet eller skolan konton tooWindows på en personlig enhet</span><span class="sxs-lookup"><span data-stu-id="f0916-126">Users add work or school accounts tooWindows on a personal device</span></span>

## <a name="additional-information"></a><span data-ttu-id="f0916-127">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="f0916-127">Additional information</span></span>
* [<span data-ttu-id="f0916-128">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="f0916-128">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="f0916-129">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="f0916-129">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="f0916-130">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="f0916-130">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="f0916-131">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="f0916-131">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="f0916-132">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="f0916-132">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

