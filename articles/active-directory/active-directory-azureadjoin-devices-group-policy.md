---
title: "Ansluta domänanslutna enheter till Azure AD för Windows 10-upplevelser | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera en grupprincip för att aktivera enheter måste vara domänansluten till företagets nätverk."
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
ms.openlocfilehash: 9c91579d20bb84701f6d0b97d944728c84044adf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a><span data-ttu-id="64d9b-103">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="64d9b-103">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>
<span data-ttu-id="64d9b-104">Anslut till domän är det traditionella sätt organisationer har anslutit enheter för arbete för de senaste 15 åren och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="64d9b-104">Domain join is the traditional way organizations have connected devices for work for the last 15 years and more.</span></span> <span data-ttu-id="64d9b-105">Det har aktiverat användare att logga in på sina enheter med hjälp av Windows Server Active Directory (Active Directory) arbetet eller skolan konton och tillåtna IT för att hantera dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="64d9b-105">It has enabled users to sign in to their devices by using their Windows Server Active Directory (Active Directory) work or school accounts and allowed IT to fully manage these devices.</span></span> <span data-ttu-id="64d9b-106">Organisationer bygger vanligtvis på avbildning metoder för att etablera enheter till användare och vanligtvis använda System Center Configuration Manager (SCCM) eller en grupprincip för att hantera dem.</span><span class="sxs-lookup"><span data-stu-id="64d9b-106">Organizations typically rely on imaging methods to provision devices to users and generally use System Center Configuration Manager (SCCM) or Group Policy to manage them.</span></span>


<span data-ttu-id="64d9b-107">Anslut till domän i Windows 10 ger följande fördelar när du ansluter enheter till Azure Active Directory (AD Azure):</span><span class="sxs-lookup"><span data-stu-id="64d9b-107">Domain join in Windows 10 provides you with the following benefits after you connect devices to Azure Active Directory (Azure AD):</span></span>

* <span data-ttu-id="64d9b-108">Enkel inloggning (SSO) till Azure AD-resurser från valfri plats</span><span class="sxs-lookup"><span data-stu-id="64d9b-108">Single sign-on (SSO) to Azure AD resources from anywhere</span></span>
* <span data-ttu-id="64d9b-109">Åtkomst till Windows Store för företag med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)</span><span class="sxs-lookup"><span data-stu-id="64d9b-109">Access to the enterprise Windows Store by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="64d9b-110">Enterprise-kompatibel roaming av användarinställningar över enheter med hjälp av arbetet eller skolan konton (ingen Microsoft-konto krävs)</span><span class="sxs-lookup"><span data-stu-id="64d9b-110">Enterprise-compliant roaming of user settings across devices by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="64d9b-111">Stark autentisering och smidig inloggning för arbetet eller skolan konton med Windows Hello för företag och Windows Hello</span><span class="sxs-lookup"><span data-stu-id="64d9b-111">Strong authentication and convenient sign-in for work or school accounts with Windows Hello for Business and Windows Hello</span></span>
* <span data-ttu-id="64d9b-112">Möjligheten att begränsa åtkomst till enheter som uppfyller organisationens Enhetsinställningar för Grupprincip</span><span class="sxs-lookup"><span data-stu-id="64d9b-112">Ability to restrict access only to devices that comply with organizational device Group Policy settings</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64d9b-113">Krav</span><span class="sxs-lookup"><span data-stu-id="64d9b-113">Prerequisites</span></span>
<span data-ttu-id="64d9b-114">Domänanslutning fortsätter att vara användbara.</span><span class="sxs-lookup"><span data-stu-id="64d9b-114">Domain join continues to be useful.</span></span> <span data-ttu-id="64d9b-115">Emellertid att hämta Azure AD-fördelarna med enkel inloggning, roaming av inställningar med arbetets eller skolans konton och åtkomst till Windows Store med arbets-eller skolkonton, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="64d9b-115">However, to get the Azure AD benefits of SSO, roaming of settings with work or school accounts, and access to Windows Store with work or school accounts, you will need the following:</span></span>

* <span data-ttu-id="64d9b-116">Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="64d9b-116">Azure AD subscription</span></span>
* <span data-ttu-id="64d9b-117">Azure AD Connect att utöka lokala katalog till Azure AD</span><span class="sxs-lookup"><span data-stu-id="64d9b-117">Azure AD Connect to extend the on-premises directory to Azure AD</span></span>
* <span data-ttu-id="64d9b-118">Princip som har angetts till ansluta domänanslutna enheter till Azure AD</span><span class="sxs-lookup"><span data-stu-id="64d9b-118">Policy that's set to connect domain-joined devices to Azure AD</span></span>
* <span data-ttu-id="64d9b-119">Windows 10-version (build 10551 eller senare) för enheter</span><span class="sxs-lookup"><span data-stu-id="64d9b-119">Windows 10 build (build 10551 or newer) for devices</span></span>

<span data-ttu-id="64d9b-120">Om du vill aktivera Windows Hello för företag och Windows Hello, måste du också följande:</span><span class="sxs-lookup"><span data-stu-id="64d9b-120">To enable Windows Hello for Business and Windows Hello, you will also need the following:</span></span>

- <span data-ttu-id="64d9b-121">**Infrastruktur för offentliga nycklar (PKI)** för utfärdande av certifikat för användaren.</span><span class="sxs-lookup"><span data-stu-id="64d9b-121">**Public key infrastructure (PKI)** for user certificates issuance.</span></span>

- <span data-ttu-id="64d9b-122">**System Center Configuration Manager Current Branch** -måste du installera versionen 1606 eller bättre.</span><span class="sxs-lookup"><span data-stu-id="64d9b-122">**System Center Configuration Manager Current Branch** - You need to install version 1606 or better.</span></span>  
<span data-ttu-id="64d9b-123">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="64d9b-123">For more information, see:</span></span> 
    - [<span data-ttu-id="64d9b-124">Dokumentation för System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="64d9b-124">Documentation for System Center Configuration Manager</span></span>](https://technet.microsoft.com/library/mt346023.aspx)
    - [<span data-ttu-id="64d9b-125">System Center Configuration Manager Teamblogg</span><span class="sxs-lookup"><span data-stu-id="64d9b-125">System Center Configuration Manager Team Blog</span></span>](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [<span data-ttu-id="64d9b-126">Windows Hello för företag-inställningar i System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="64d9b-126">Windows Hello for Business settings in System Center Configuration Manager</span></span>](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

<span data-ttu-id="64d9b-127">Som ett alternativ till kravet på PKI-distribution kan du göra följande:</span><span class="sxs-lookup"><span data-stu-id="64d9b-127">As an alternative to the PKI deployment requirement, you can do the following:</span></span>

* <span data-ttu-id="64d9b-128">Har några domänkontrollanter med Windows Server 2016 Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="64d9b-128">Have a few domain controllers with Windows Server 2016 Active Directory Domain Services.</span></span>

<span data-ttu-id="64d9b-129">Om du vill aktivera villkorlig åtkomst, kan du skapa grupprincipinställningar som tillåter åtkomst till domänanslutna enheter med några ytterligare distributioner.</span><span class="sxs-lookup"><span data-stu-id="64d9b-129">To enable conditional access, you can create Group Policy settings that allow access to domain-joined devices with no additional deployments.</span></span> <span data-ttu-id="64d9b-130">Om du vill hantera åtkomstkontroll utifrån kompatibilitet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="64d9b-130">To manage access control based on compliance of the device, you will need the following:</span></span>

* <span data-ttu-id="64d9b-131">System Center Configuration Manager Current Branch (1606 eller senare) för Windows Hello för företag-scenarier</span><span class="sxs-lookup"><span data-stu-id="64d9b-131">System Center Configuration Manager Current Branch (1606 or later) for Windows Hello for Business scenarios</span></span>

## <a name="deployment-instructions"></a><span data-ttu-id="64d9b-132">Anvisningar för distribution</span><span class="sxs-lookup"><span data-stu-id="64d9b-132">Deployment instructions</span></span>

<span data-ttu-id="64d9b-133">Om du vill distribuera, följer du stegen i [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="64d9b-133">To deploy, follow the steps listed in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

## <a name="next-step"></a><span data-ttu-id="64d9b-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64d9b-134">Next step</span></span>
* [<span data-ttu-id="64d9b-135">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="64d9b-135">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="64d9b-136">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="64d9b-136">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="64d9b-137">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="64d9b-137">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="64d9b-138">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="64d9b-138">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="64d9b-139">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="64d9b-139">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

