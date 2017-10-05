---
title: "Konfigurera Azure AD Join för dina användare| Microsoft Docs"
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
ms.openlocfilehash: c37adc2654f7e931fdda22627e4a6ece2789fd86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a><span data-ttu-id="ab5e5-103">Konfigurera Azure AD Join i din organisation</span><span class="sxs-lookup"><span data-stu-id="ab5e5-103">Setting up Azure AD Join in your organization</span></span>
<span data-ttu-id="ab5e5-104">Innan du konfigurerar Azure Active Directory Join (Azure AD Join) måste du antingen synkronisera din lokala katalog med användare till molnet eller manuellt skapa hanterade konton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-104">Before you set up Azure Active Directory Join (Azure AD Join), you need to either sync up your on-premises directory of users to the cloud or manually create managed accounts in Azure AD.</span></span>

<span data-ttu-id="ab5e5-105">Detaljerade instruktioner för hur du synkroniserar dina lokala användare till Azure AD finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="ab5e5-105">Detailed instructions for syncing your on-premises users to Azure AD is covered in [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="ab5e5-106">Om du vill skapa och hantera användare manuellt i Azure AD läser du [Användarhantering i Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span><span class="sxs-lookup"><span data-stu-id="ab5e5-106">To manually create and manage users in Azure AD, refer to [User management in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).</span></span>

## <a name="set-up-device-registration"></a><span data-ttu-id="ab5e5-107">Konfigurera enhetsregistrering</span><span class="sxs-lookup"><span data-stu-id="ab5e5-107">Set up device registration</span></span>
1. <span data-ttu-id="ab5e5-108">Logga in på Azure-portalen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-108">Log on to the Azure portal as an administrator.</span></span>
2. <span data-ttu-id="ab5e5-109">Välj **Active Directory** i det vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-109">On the left pane, select **Active Directory**.</span></span>
3. <span data-ttu-id="ab5e5-110">Välj din katalog på fliken **Katalog**.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-110">On the **Directory** tab, select your directory.</span></span>
4. <span data-ttu-id="ab5e5-111">Välj fliken **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-111">Select the **Configure** tab.</span></span>
5. <span data-ttu-id="ab5e5-112">Gå till avsnittet **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-112">Go to the **Devices** section.</span></span>
6. <span data-ttu-id="ab5e5-113">Ange följande på fliken **Enheter**:</span><span class="sxs-lookup"><span data-stu-id="ab5e5-113">On the **devices** tab, set the following:</span></span>  
   * <span data-ttu-id="ab5e5-114">**Högsta antal tillåtna enheter per användare**: Välj det högsta antalet enheter som en användare kan ha i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-114">**MAXIMUM NUMBER OF DEVICES PER USER**: Select the maximum number of devices that a user can have in Azure AD.</span></span>  <span data-ttu-id="ab5e5-115">Om användarna når den här kvoten kan de inte lägga till fler enheter förrän en eller flera av deras befintliga enheter tagits bort.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-115">If a user reaches this quota, they will not be able to add additional devices until one or more of their existing devices are removed.</span></span>
   * <span data-ttu-id="ab5e5-116">**Kräv Multi-Factor Authentication för att ansluta enheter**: Ange om användarna måste ange en andra autentiseringsfaktor för att ansluta deras enhet till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-116">**REQUIRE MULTI-FACTOR AUTH TO JOIN DEVICES**: Set whether users are required to provide a second authentication factor to join their device to Azure AD.</span></span> <span data-ttu-id="ab5e5-117">Mer information om Azure Multi-Factor Authentication finns i [Komma igång med Azure Multi-Factor Authentication i molnet](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="ab5e5-117">For more information on Azure Multi-Factor Authentication, see [Getting started with Azure Multi-Factor Authentication in the cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>
   * <span data-ttu-id="ab5e5-118">**Användare kan ansluta enheter till Azure AD**: Välj användare och grupper som ska kunna ansluta enheter till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-118">**USERS MAY AZURE AD JOIN DEVICES**: Select the users and groups that are allowed to join devices to Azure AD.</span></span>
   * <span data-ttu-id="ab5e5-119">**Ytterligare administratörer för Azure AD-anslutna enheter**: Med Azure AD Premium eller Enterprise Mobility Suite (EMS) kan du välja vilka användare som beviljas lokal administratörsbehörighet på enheten.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-119">**ADDITIONAL ADMINISTRATORS ON AZURE AD JOINED DEVICES**: With Azure AD Premium or the Enterprise Mobility Suite (EMS), you can choose which users are granted local administrator rights to the device.</span></span> <span data-ttu-id="ab5e5-120">Globala administratörer och enhetsägare beviljas lokal administratörsbehörighet som standard.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-120">Global administrators and device owners are granted local administrator rights by default.</span></span>

<span data-ttu-id="ab5e5-121"><center>![Konfigurera enhetsregistrering](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center></span><span class="sxs-lookup"><span data-stu-id="ab5e5-121"><center>![Set up device regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center></span></span>

<span data-ttu-id="ab5e5-122">När du har konfigurerat Azure AD Join för dina användare kan de ansluta till Azure AD via deras företagsägda eller personliga enheter.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-122">After you set up Azure AD Join for your users, they can connect to Azure AD through their corporate or personal devices.</span></span>

<span data-ttu-id="ab5e5-123">Följande är de tre scenarier som du kan använda om du vill att dina användare ska kunna konfigurera Azure AD Join:</span><span class="sxs-lookup"><span data-stu-id="ab5e5-123">Following are the three scenarios you can use to enable your users to set up Azure AD Join:</span></span>

* <span data-ttu-id="ab5e5-124">Användarna ansluter en företagsägd enhet direkt till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-124">Users join a company-owned device directly to Azure AD.</span></span>
* <span data-ttu-id="ab5e5-125">Användarna domänansluter en företagsägd enhet till lokala Active Directory och utökar sedan enheten till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab5e5-125">Users domain-join a company-owned device to the on-premises Active Directory and then extend the device to Azure AD.</span></span>
* <span data-ttu-id="ab5e5-126">Användarna lägger till arbets- eller skolkonton till Windows på en personlig enhet</span><span class="sxs-lookup"><span data-stu-id="ab5e5-126">Users add work or school accounts to Windows on a personal device</span></span>

## <a name="additional-information"></a><span data-ttu-id="ab5e5-127">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="ab5e5-127">Additional information</span></span>
* [<span data-ttu-id="ab5e5-128">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="ab5e5-128">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="ab5e5-129">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="ab5e5-129">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="ab5e5-130">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="ab5e5-130">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="ab5e5-131">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="ab5e5-131">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="ab5e5-132">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="ab5e5-132">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

