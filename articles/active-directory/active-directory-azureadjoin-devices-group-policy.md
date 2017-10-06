---
title: "aaaConnect domänanslutna enheter tooAzure AD för Windows 10-upplevelser | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera en grupprincip tooenable enheter toobe domänanslutna toohello företagets nätverk."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 9766aa702352dea2ecad3a9a0bdf8d3286ee6d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-domain-joined-devices-tooazure-ad-for-windows-10-experiences"></a><span data-ttu-id="a8d87-103">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="a8d87-103">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>
<span data-ttu-id="a8d87-104">Anslut till domän är hello traditionella sätt organisationer har anslutna enheter för arbete hello senaste 15 år och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="a8d87-104">Domain join is hello traditional way organizations have connected devices for work for hello last 15 years and more.</span></span> <span data-ttu-id="a8d87-105">Den har aktiverats användare toosign i tootheir enheter med hjälp av Windows Server Active Directory (Active Directory) arbetet eller skolkonton och tillåtna IT toofully hantera dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="a8d87-105">It has enabled users toosign in tootheir devices by using their Windows Server Active Directory (Active Directory) work or school accounts and allowed IT toofully manage these devices.</span></span> <span data-ttu-id="a8d87-106">Organisationer bygger vanligtvis på imaging metoder tooprovision enheter toousers och vanligtvis använder System Center Configuration Manager (SCCM) eller en grupprincip toomanage dem.</span><span class="sxs-lookup"><span data-stu-id="a8d87-106">Organizations typically rely on imaging methods tooprovision devices toousers and generally use System Center Configuration Manager (SCCM) or Group Policy toomanage them.</span></span>


<span data-ttu-id="a8d87-107">Anslut till domän i Windows 10 ger dig hello följande fördelar när du ansluter enheter tooAzure Active Directory (AD Azure):</span><span class="sxs-lookup"><span data-stu-id="a8d87-107">Domain join in Windows 10 provides you with hello following benefits after you connect devices tooAzure Active Directory (Azure AD):</span></span>

* <span data-ttu-id="a8d87-108">Enkel inloggning (SSO) tooAzure AD resurser från valfri plats</span><span class="sxs-lookup"><span data-stu-id="a8d87-108">Single sign-on (SSO) tooAzure AD resources from anywhere</span></span>
* <span data-ttu-id="a8d87-109">Komma åt toohello enterprise Windows Store med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)</span><span class="sxs-lookup"><span data-stu-id="a8d87-109">Access toohello enterprise Windows Store by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="a8d87-110">Enterprise-kompatibel roaming av användarinställningar över enheter med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)</span><span class="sxs-lookup"><span data-stu-id="a8d87-110">Enterprise-compliant roaming of user settings across devices by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="a8d87-111">Stark autentisering och smidig inloggning för arbetet eller skolan konton med Windows Hello för företag och Windows Hello</span><span class="sxs-lookup"><span data-stu-id="a8d87-111">Strong authentication and convenient sign-in for work or school accounts with Windows Hello for Business and Windows Hello</span></span>
* <span data-ttu-id="a8d87-112">Möjlighet toorestrict komma åt toodevices som uppfyller organisationens Enhetsinställningar för Grupprincip</span><span class="sxs-lookup"><span data-stu-id="a8d87-112">Ability toorestrict access only toodevices that comply with organizational device Group Policy settings</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8d87-113">Krav</span><span class="sxs-lookup"><span data-stu-id="a8d87-113">Prerequisites</span></span>
<span data-ttu-id="a8d87-114">Domänanslutning fortsätter toobe användbart.</span><span class="sxs-lookup"><span data-stu-id="a8d87-114">Domain join continues toobe useful.</span></span> <span data-ttu-id="a8d87-115">Dock tooget hello Azure AD fördelarna med enkel inloggning, centrala inställningar med arbetsplats eller skolkonton, och komma åt tooWindows Store med arbete eller skola konton, måste hello följande:</span><span class="sxs-lookup"><span data-stu-id="a8d87-115">However, tooget hello Azure AD benefits of SSO, roaming of settings with work or school accounts, and access tooWindows Store with work or school accounts, you will need hello following:</span></span>

* <span data-ttu-id="a8d87-116">Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8d87-116">Azure AD subscription</span></span>
* <span data-ttu-id="a8d87-117">Azure AD Connect tooextend hello lokalt directory tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="a8d87-117">Azure AD Connect tooextend hello on-premises directory tooAzure AD</span></span>
* <span data-ttu-id="a8d87-118">Princip som har angetts tooconnect domänanslutna enheter tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="a8d87-118">Policy that's set tooconnect domain-joined devices tooAzure AD</span></span>
* <span data-ttu-id="a8d87-119">Windows 10-version (build 10551 eller senare) för enheter</span><span class="sxs-lookup"><span data-stu-id="a8d87-119">Windows 10 build (build 10551 or newer) for devices</span></span>

<span data-ttu-id="a8d87-120">tooenable Windows Hello för företag och Windows Hello, måste du också hello följande:</span><span class="sxs-lookup"><span data-stu-id="a8d87-120">tooenable Windows Hello for Business and Windows Hello, you will also need hello following:</span></span>

- <span data-ttu-id="a8d87-121">**Infrastruktur för offentliga nycklar (PKI)** för utfärdande av certifikat för användaren.</span><span class="sxs-lookup"><span data-stu-id="a8d87-121">**Public key infrastructure (PKI)** for user certificates issuance.</span></span>

- <span data-ttu-id="a8d87-122">**System Center Configuration Manager Current Branch** -du behöver tooinstall version 1606 eller bättre.</span><span class="sxs-lookup"><span data-stu-id="a8d87-122">**System Center Configuration Manager Current Branch** - You need tooinstall version 1606 or better.</span></span>  
<span data-ttu-id="a8d87-123">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="a8d87-123">For more information, see:</span></span> 
    - [<span data-ttu-id="a8d87-124">Dokumentation för System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="a8d87-124">Documentation for System Center Configuration Manager</span></span>](https://technet.microsoft.com/library/mt346023.aspx)
    - [<span data-ttu-id="a8d87-125">System Center Configuration Manager Teamblogg</span><span class="sxs-lookup"><span data-stu-id="a8d87-125">System Center Configuration Manager Team Blog</span></span>](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [<span data-ttu-id="a8d87-126">Windows Hello för företag-inställningar i System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="a8d87-126">Windows Hello for Business settings in System Center Configuration Manager</span></span>](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

<span data-ttu-id="a8d87-127">Som ett alternativ toohello PKI-distributionskrav, kan du göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="a8d87-127">As an alternative toohello PKI deployment requirement, you can do hello following:</span></span>

* <span data-ttu-id="a8d87-128">Har några domänkontrollanter med Windows Server 2016 Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a8d87-128">Have a few domain controllers with Windows Server 2016 Active Directory Domain Services.</span></span>

<span data-ttu-id="a8d87-129">tooenable villkorlig åtkomst, kan du skapa grupprincipinställningar som tillåter åtkomst toodomain-anslutna enheter med några ytterligare distributioner.</span><span class="sxs-lookup"><span data-stu-id="a8d87-129">tooenable conditional access, you can create Group Policy settings that allow access toodomain-joined devices with no additional deployments.</span></span> <span data-ttu-id="a8d87-130">toomanage åtkomstkontroll baserat på hello enhets efterlevnad, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a8d87-130">toomanage access control based on compliance of hello device, you will need hello following:</span></span>

* <span data-ttu-id="a8d87-131">System Center Configuration Manager Current Branch (1606 eller senare) för Windows Hello för företag-scenarier</span><span class="sxs-lookup"><span data-stu-id="a8d87-131">System Center Configuration Manager Current Branch (1606 or later) for Windows Hello for Business scenarios</span></span>

## <a name="deployment-instructions"></a><span data-ttu-id="a8d87-132">Anvisningar för distribution</span><span class="sxs-lookup"><span data-stu-id="a8d87-132">Deployment instructions</span></span>

<span data-ttu-id="a8d87-133">toodeploy, följ hello stegen i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="a8d87-133">toodeploy, follow hello steps listed in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

## <a name="next-step"></a><span data-ttu-id="a8d87-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8d87-134">Next step</span></span>
* [<span data-ttu-id="a8d87-135">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="a8d87-135">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="a8d87-136">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="a8d87-136">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="a8d87-137">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="a8d87-137">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="a8d87-138">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="a8d87-138">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="a8d87-139">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="a8d87-139">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

