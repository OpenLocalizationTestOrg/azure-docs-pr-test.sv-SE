---
title: "Självstudier: Konfigurera Workday för automatisk användaretablering med lokala Active Directory och Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toouse Workday som datakälla identitet för Active Directory och Azure Active Directory."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/26/2017
ms.author: asmalser
ms.openlocfilehash: d4eb3237b8fe7614606c58b39fbefcb44f4060fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a><span data-ttu-id="0e3d2-103">Självstudier: Konfigurera Workday för automatisk användaretablering med lokala Active Directory och Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e3d2-103">Tutorial: Configure Workday for automatic user provisioning with on-premises Active Directory and Azure Active Directory</span></span>
<span data-ttu-id="0e3d2-104">hello syftet med den här kursen är tooshow du hello stegen tooperform tooimport personer från Workday till både Active Directory och Azure Active Directory med valfria tillbakaskrivning av vissa attribut tooWorkday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-104">hello objective of this tutorial is tooshow you hello steps you need tooperform tooimport people from Workday into both Active Directory and Azure Active Directory, with optional writeback of some attributes tooWorkday.</span></span> 



## <a name="overview"></a><span data-ttu-id="0e3d2-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0e3d2-105">Overview</span></span>

<span data-ttu-id="0e3d2-106">Hej [Azure Active Directory-användare som etableras](active-directory-saas-app-provisioning.md) kan integreras med hello [Workday personal API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) ordning tooprovision användarkonton.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-106">hello [Azure Active Directory user provisioning service](active-directory-saas-app-provisioning.md) integrates with hello [Workday Human Resources API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) in order tooprovision user accounts.</span></span> <span data-ttu-id="0e3d2-107">Azure AD använder den här anslutningen tooenable hello efter användaren etablering arbetsflöden:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-107">Azure AD uses this connection tooenable hello following user provisioning workflows:</span></span>

* <span data-ttu-id="0e3d2-108">**Etablera användare tooActive Directory** -Synkronisera markerade uppsättningar med användare från Workday till en eller flera Active Directory-skogar.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-108">**Provisioning users tooActive Directory** - Synchronize selected sets of users from Workday into one or more Active Directory forests.</span></span> 

* <span data-ttu-id="0e3d2-109">**Endast molnbaserad användare tooAzure Active Directory-etablering** -Hybrid-användare som finns i både Active Directory och Azure Active Directory kan etableras till hello senare med hjälp av [AAD Connect](connect/active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-109">**Provisioning cloud-only users tooAzure Active Directory** - Hybrid users who exist in both Active Directory and Azure Active Directory can be provisioned into hello latter using [AAD Connect](connect/active-directory-aadconnect.md).</span></span> <span data-ttu-id="0e3d2-110">Användare som är molnbaserad kan dock etableras direkt från Workday tooAzure Active Directory med hjälp av hello Azure AD-användare som etableras.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-110">However, users that are cloud-only can be provisioned directly from Workday tooAzure Active Directory using hello Azure AD user provisioning service.</span></span>

* <span data-ttu-id="0e3d2-111">**Tillbakaskrivning av e-post adresser tooWorkday** -hello Azure AD-användaren som etablerar tjänsten kan skriva valt Azure AD användaren attribut tillbaka tooWorkday, till exempel hello e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-111">**Writeback of email addresses tooWorkday** - hello Azure AD user provisioning service can write selected Azure AD user attributes back tooWorkday, such as hello email address.</span></span>

### <a name="scenarios-covered"></a><span data-ttu-id="0e3d2-112">Scenarier som tas upp</span><span class="sxs-lookup"><span data-stu-id="0e3d2-112">Scenarios covered</span></span>

<span data-ttu-id="0e3d2-113">Hej Workday användaretablering arbetsflöden som stöds av etableras hello Azure AD-användare kan automatisering av hello efter personal identity livscykel scenarier och:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-113">hello Workday user provisioning workflows supported by hello Azure AD user provisioning service enables automation of hello following human resources and identity lifecycle management scenarios:</span></span>

* <span data-ttu-id="0e3d2-114">**Uthyrning nyanställda** – när en ny medarbetare läggs tooWorkday, ett konto skapas automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD ](active-directory-saas-app-provisioning.md), vid skrivning baksidan hello e-postadress tooWorkday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-114">**Hiring new employees** - When a new employee is added tooWorkday, a user account will be automatically created in Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md), with write back of hello email address tooWorkday.</span></span>

* <span data-ttu-id="0e3d2-115">**Medarbetare-attribut och profilen uppdateringar** – när en anställd uppdateras i Workday (till exempel deras namn, avdelning eller manager), sitt användarkonto uppdateras automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-115">**Employee attribute and profile updates** - When an employee record is updated in Workday (such as their name, title, or manager), their user account will be automatically updated in Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md).</span></span>

* <span data-ttu-id="0e3d2-116">**Medarbetare uppsägningar** – när en anställd avslutas i arbetsdagen, sitt användarkonto inaktiveras automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-116">**Employee terminations** - When an employee is terminated in Workday, their user account is automatically disabled in Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md).</span></span>

* <span data-ttu-id="0e3d2-117">**Medarbetare nytt anlitar** – när en anställd rehired i Workday, sina gamla kontot kan återaktiveras automatiskt eller etablerade igen (beroende på dina inställningar) tooActive Directory, Azure Active Directory, och du kan också Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-117">**Employee re-hires** - When an employee is rehired in Workday, their old account can be automatically reactivated or re-provisioned (depending on your preference) tooActive Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md).</span></span>


## <a name="planning-your-solution"></a><span data-ttu-id="0e3d2-118">Planerar din lösning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-118">Planning your solution</span></span>

<span data-ttu-id="0e3d2-119">Innan du börjar din Workday-integrering, kontrollera hello krav nedan och Läs hello följande anvisningar om hur toomatch nuvarande Active Directory-arkitektur och användaretablering krav med hello solution(s) som tillhandahålls av Azure Active Katalog.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-119">Before beginning your Workday integration, check hello prerequisites below and read hello following guidance on how toomatch your current Active Directory architecture and user provisioning requirements with hello solution(s) provided by Azure Active Directory.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="0e3d2-120">Krav</span><span class="sxs-lookup"><span data-stu-id="0e3d2-120">Prerequisites</span></span>

<span data-ttu-id="0e3d2-121">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-121">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="0e3d2-122">En giltig Azure AD Premium P1-prenumeration med global administratörsbehörighet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-122">A valid Azure AD Premium P1 subscription with global administrator access</span></span>
* <span data-ttu-id="0e3d2-123">En klient för implementering av Workday för testning och integrering</span><span class="sxs-lookup"><span data-stu-id="0e3d2-123">A Workday implementation tenant for testing and integration purposes</span></span>
* <span data-ttu-id="0e3d2-124">Administratörsbehörighet i Workday toocreate en systemanvändare integrering och se ändringar tootest medarbetardata för testning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-124">Administrator permissions in Workday toocreate a system integration user, and make changes tootest employee data for testing purposes</span></span>
* <span data-ttu-id="0e3d2-125">En domänansluten server som kör tjänsten Windows 2012 eller högre är obligatoriska toohost hello för användaren etablering tooActive Directory [lokalt lösenordssynkroniseringsagenten](https://go.microsoft.com/fwlink/?linkid=847801)</span><span class="sxs-lookup"><span data-stu-id="0e3d2-125">For user provisioning tooActive Directory, a domain-joined server running Windows Service 2012 or greater is required toohost hello [on-premises synchronization agent](https://go.microsoft.com/fwlink/?linkid=847801)</span></span>
* <span data-ttu-id="0e3d2-126">[Azure AD Connect](connect/active-directory-aadconnect.md) för synkronisering mellan Active Directory och Azure AD</span><span class="sxs-lookup"><span data-stu-id="0e3d2-126">[Azure AD Connect](connect/active-directory-aadconnect.md) for synchronizing between Active Directory and Azure AD</span></span>

> [!NOTE]
> <span data-ttu-id="0e3d2-127">Om Azure AD-klienten finns i Europa finns hello [kända problem](#known-issues) nedan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-127">If your Azure AD tenant is located in Europe, please see hello [Known issues](#known-issues) section below.</span></span>


### <a name="solution-architecture"></a><span data-ttu-id="0e3d2-128">Lösningsarkitektur</span><span class="sxs-lookup"><span data-stu-id="0e3d2-128">Solution architecture</span></span>

<span data-ttu-id="0e3d2-129">Azure AD innehåller en omfattande uppsättning etablering kopplingar toohelp du lösa etablering och identitet livscykelhantering från Workday tooActive Directory, Azure AD, SaaS-appar och framåt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-129">Azure AD provides a rich set of provisioning connectors toohelp you solve provisioning and identity lifecycle management from Workday tooActive Directory, Azure AD, SaaS apps, and beyond.</span></span> <span data-ttu-id="0e3d2-130">Vilka funktioner du använder och hur du ställer in hello lösning varierar beroende på din organisations miljö och behov.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-130">Which features you will use and how you set up hello solution will vary depending on your organization's environment and requirements.</span></span> <span data-ttu-id="0e3d2-131">Går igenom hur många av hello följande är närvarande och distribuerade i din organisation som ett första steg:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-131">As a first step, take stock of how many of hello following are present and deployed in your organization:</span></span>

* <span data-ttu-id="0e3d2-132">Hur många Active Directory-skogar finns i använda?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-132">How many Active Directory Forests are in use?</span></span>
* <span data-ttu-id="0e3d2-133">Hur många Active Directory-domäner finns i använda?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-133">How many Active Directory Domains are in use?</span></span>
* <span data-ttu-id="0e3d2-134">Hur många Active Directory organisationsenheter (OU) används?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-134">How many Active Directory Organizational Units (OUs) are in use?</span></span>
* <span data-ttu-id="0e3d2-135">Hur många Azure Active Directory-klienter finns i använda?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-135">How many Azure Active Directory tenants are in use?</span></span>
* <span data-ttu-id="0e3d2-136">Finns det användare som behöver toobe etableras tooboth Active Directory och Azure Active Directory (t.ex. ”hybrid” användare)?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-136">Are there users who need toobe provisioned tooboth Active Directory and Azure Active Directory (e.g. "hybrid" users)?</span></span>
* <span data-ttu-id="0e3d2-137">Finns det användare som behöver toobe etableras tooAzure Active Directory, men inte Active Directory (t.ex. ”endast molnbaserad” användare)?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-137">Are there users who need toobe provisioned tooAzure Active Directory, but not Active Directory (e.g. "cloud-only" users)?</span></span>
* <span data-ttu-id="0e3d2-138">Behöver användare e-postadresser toobe skrivs tillbaka tooWorkday?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-138">Do user email addresses need toobe written back tooWorkday?</span></span>

<span data-ttu-id="0e3d2-139">När du har svaren toothese frågor kan du planera din arbetsdag allokering distribution av hello vägledningen nedan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-139">Once you have answers toothese questions, you can plan your Workday provisioning deployment by following hello guidance below.</span></span>

#### <a name="using-provisioning-connector-apps"></a><span data-ttu-id="0e3d2-140">Använda etablering connector appar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-140">Using provisioning connector apps</span></span>

<span data-ttu-id="0e3d2-141">Azure Active Directory stöder förintegrerade etablering kopplingar för Workday och många andra SaaS-program.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-141">Azure Active Directory supports pre-integrated provisioning connectors for Workday and a large number of other SaaS applications.</span></span> 

<span data-ttu-id="0e3d2-142">En enkel etablering koppling gränssnitt med hello API i en enda källsystemet och hjälper etablera data tooa enda målsystemet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-142">A single provisioning connector interfaces with hello API of a single source system, and helps provision data tooa single target system.</span></span> <span data-ttu-id="0e3d2-143">De flesta etablering kopplingar som har stöd för Azure AD är för en enda källa och målsystem (t.ex. Azure AD-tooServiceNow) och kan konfigureras genom att lägga till hello appen i fråga från hello Azure AD app-galleriet (t.ex. ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-143">Most provisioning connectors that Azure AD supports are for a single source and target system (e.g. Azure AD tooServiceNow), and can be setup by simply adding hello app in question from hello Azure AD app gallery (e.g. ServiceNow).</span></span> 

<span data-ttu-id="0e3d2-144">Det finns en 1: 1-relation mellan etablering koppling instanser och app-instanser i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-144">There is a one-to-one relationship between provisioning connector instances and app instances in Azure AD:</span></span>

| <span data-ttu-id="0e3d2-145">Källsystemet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-145">Source System</span></span> | <span data-ttu-id="0e3d2-146">Målsystem</span><span class="sxs-lookup"><span data-stu-id="0e3d2-146">Target System</span></span> |
| ---------- | ---------- | 
| <span data-ttu-id="0e3d2-147">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="0e3d2-147">Azure AD tenant</span></span> | <span data-ttu-id="0e3d2-148">SaaS-program</span><span class="sxs-lookup"><span data-stu-id="0e3d2-148">SaaS application</span></span> |


<span data-ttu-id="0e3d2-149">När du arbetar med Workday och Active Directory, finns det dock flera käll- och system toobe-anses vara:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-149">However, when working with Workday and Active Directory, there are multiple source and target systems toobe considered:</span></span>

| <span data-ttu-id="0e3d2-150">Källsystemet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-150">Source System</span></span> | <span data-ttu-id="0e3d2-151">Målsystem</span><span class="sxs-lookup"><span data-stu-id="0e3d2-151">Target System</span></span> | <span data-ttu-id="0e3d2-152">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-152">Notes</span></span> |
| ---------- | ---------- | ---------- |
| <span data-ttu-id="0e3d2-153">Arbetsdagar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-153">Workday</span></span> | <span data-ttu-id="0e3d2-154">Active Directory-skog</span><span class="sxs-lookup"><span data-stu-id="0e3d2-154">Active Directory Forest</span></span> | <span data-ttu-id="0e3d2-155">Varje skog behandlas som ett distinkta målsystem</span><span class="sxs-lookup"><span data-stu-id="0e3d2-155">Each forest is treated as a distinct target system</span></span> |
| <span data-ttu-id="0e3d2-156">Arbetsdagar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-156">Workday</span></span> | <span data-ttu-id="0e3d2-157">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="0e3d2-157">Azure AD tenant</span></span> | <span data-ttu-id="0e3d2-158">Som krävs för endast molnbaserad användare</span><span class="sxs-lookup"><span data-stu-id="0e3d2-158">As required for cloud-only users</span></span> |
| <span data-ttu-id="0e3d2-159">Active Directory-skog</span><span class="sxs-lookup"><span data-stu-id="0e3d2-159">Active Directory Forest</span></span> | <span data-ttu-id="0e3d2-160">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="0e3d2-160">Azure AD tenant</span></span> | <span data-ttu-id="0e3d2-161">Detta flöde hanteras av AAD Connect idag</span><span class="sxs-lookup"><span data-stu-id="0e3d2-161">This flow is handled by AAD Connect today</span></span> |
| <span data-ttu-id="0e3d2-162">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="0e3d2-162">Azure AD tenant</span></span> | <span data-ttu-id="0e3d2-163">Arbetsdagar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-163">Workday</span></span> | <span data-ttu-id="0e3d2-164">För tillbakaskrivning av e-postadresser</span><span class="sxs-lookup"><span data-stu-id="0e3d2-164">For writeback of email addresses</span></span> |

<span data-ttu-id="0e3d2-165">toofacilitate flera arbetsflöden toomultiple käll- och systemen, Azure AD innehåller flera etablering connector-appar som du kan lägga till från hello Azure AD app-galleriet:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-165">toofacilitate these multiple workflows toomultiple source and target systems, Azure AD provides multiple provisioning connector apps that you can add from hello Azure AD app gallery:</span></span>

![AAD App-galleriet](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* <span data-ttu-id="0e3d2-167">**Arbetsdagar tooActive Directory etablering** -den här appen förenklar användaretablering konto från Workday tooa Active Directory-skog.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-167">**Workday tooActive Directory Provisioning** - This app facilitates user account provisioning from Workday tooa single Active Directory forest.</span></span> <span data-ttu-id="0e3d2-168">Om du har flera skogar, kan du lägga till en instans av den här appen från hello Azure AD app-galleriet för varje Active Directory-skog som du behöver tooprovision till.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-168">If you have multiple forests, you can add one instance of this app from hello Azure AD app gallery for each Active Directory forest you need tooprovision to.</span></span>

* <span data-ttu-id="0e3d2-169">**Arbetsdagar tooAzure AD etablering** -medan AAD Connect är hello-verktyget som ska använda toosynchronize Active Directory-användare tooAzure Active Directory, så den här appen kan använda toofacilitate etablering av endast molnbaserad användare från Workday tooa enda Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-169">**Workday tooAzure AD Provisioning** - While AAD Connect is hello tool that should be used toosynchronize Active Directory users tooAzure Active Directory, this app can be used toofacilitate provisioning of cloud-only users from Workday tooa single Azure Active Directory tenant.</span></span>

* <span data-ttu-id="0e3d2-170">**Arbetsdagar tillbakaskrivning** -den här appen förenklar tillbakaskrivning av användarens e-postadresser från Azure Active Directory tooWorkday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-170">**Workday Writeback** - This app facilitates writeback of user's email addresses from Azure Active Directory tooWorkday.</span></span>

> [!TIP]
> <span data-ttu-id="0e3d2-171">hello reguljära ”Workday” app används för att konfigurera enkel inloggning mellan Workday och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-171">hello regular "Workday" app is used for setting up single sign-on between Workday and Azure Active Directory.</span></span> 

<span data-ttu-id="0e3d2-172">Hur tooset upp och konfigurera dessa särskilda etablering connector appar är hello ämnet för hello återstående avsnitten i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-172">How tooset up and configure these special provisioning connector apps is hello subject of hello remaining sections of this tutorial.</span></span> <span data-ttu-id="0e3d2-173">Vilka appar som du väljer tooconfigure beror på vilka system måste Active Directory-skogar och Azure AD för tooprovision till, och hur många finns klienter i din miljö.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-173">Which apps you choose tooconfigure will depend on which systems you need tooprovision to, and how many Active Directory Forests and Azure AD tenants are in your environment.</span></span>

![Översikt](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a><span data-ttu-id="0e3d2-175">Konfigurera en systemanvändare integrering i Workday</span><span class="sxs-lookup"><span data-stu-id="0e3d2-175">Configure a system integration user in Workday</span></span>
<span data-ttu-id="0e3d2-176">Ett vanligt krav för alla hello Workday etablering kopplingar är de kräver autentiseringsuppgifter för en arbetsdag system integration konto tooconnect toohello Workday personal API.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-176">A common requirement of all hello Workday provisioning connectors is they require credentials for a Workday system integration account tooconnect toohello Workday Human Resources API.</span></span> <span data-ttu-id="0e3d2-177">Det här avsnittet beskrivs hur toocreate en systemintegreraren konto i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-177">This section describes how toocreate a system integrator account in Workday.</span></span>

> [!NOTE]
> <span data-ttu-id="0e3d2-178">Det är möjligt toobypass den här proceduren och i stället använda ett globalt administratörskonto för Workday som hello systemkonto för integrering.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-178">It is possible toobypass this procedure and instead use a Workday global administrator account as hello system integration account.</span></span> <span data-ttu-id="0e3d2-179">Detta fungerar bra för demonstrationer, men det rekommenderas inte för Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-179">This may work fine for demos, but is not recommended for production deployments.</span></span>

### <a name="create-an-integration-system-user"></a><span data-ttu-id="0e3d2-180">Skapa en integration system-användare</span><span class="sxs-lookup"><span data-stu-id="0e3d2-180">Create an integration system user</span></span>

<span data-ttu-id="0e3d2-181">**toocreate en integration systemanvändare:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-181">**toocreate an integration system user:**</span></span>

1. <span data-ttu-id="0e3d2-182">Logga in på din Workday-klient med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-182">Sign into your Workday tenant using an administrator account.</span></span> <span data-ttu-id="0e3d2-183">I hello **Workday arbetsstationen**, ange skapa användare i hello sökrutan och klicka sedan på **skapa Integration systemanvändare**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-183">In hello **Workday Workbench**, enter create user in hello search box, and then click **Create Integration System User**.</span></span> 
   
    <span data-ttu-id="0e3d2-184">![Skapa användare](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-184">![Create user](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Create user")</span></span>
2. <span data-ttu-id="0e3d2-185">Fullständig hello **skapa Integration systemanvändare** aktiviteten genom att ange ett användarnamn och lösenord för en ny Integration systemanvändare.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-185">Complete hello **Create Integration System User** task by supplying a user name and password for a new Integration System User.</span></span>  
 * <span data-ttu-id="0e3d2-186">Lämna hello **kräver nya lösenord vid nästa inloggning** alternativet är avmarkerat, eftersom den här användaren loggar in via programmering.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-186">Leave hello **Require New Password at Next Sign In** option unchecked, because this user will be logging on programmatically.</span></span> 
 * <span data-ttu-id="0e3d2-187">Lämna hello **antal minuter för sessionens Timeout** med standardvärdet 0, vilket förhindrar att hello användarsessioner timeout för tidigt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-187">Leave hello **Session Timeout Minutes** with its default value of 0, which will prevent hello user’s sessions from timing out prematurely.</span></span> 
   
    <span data-ttu-id="0e3d2-188">![Skapa Integration systemanvändare](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "skapa Integration systemanvändare")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-188">![Create Integration System User](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Create Integration System User")</span></span>

### <a name="create-a-security-group"></a><span data-ttu-id="0e3d2-189">Skapa en säkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="0e3d2-189">Create a security group</span></span>
<span data-ttu-id="0e3d2-190">Du behöver toocreate en säkerhetsgrupp för obegränsad integration system och tilldela hello användaren tooit.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-190">You need toocreate an unconstrained integration system security group and assign hello user tooit.</span></span>

<span data-ttu-id="0e3d2-191">**toocreate en säkerhetsgrupp:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-191">**toocreate a security group:**</span></span>

1. <span data-ttu-id="0e3d2-192">Ange skapa säkerhetsgrupp i hello sökrutan och klicka sedan på **skapa säkerhetsgruppen**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-192">Enter create security group in hello search box, and then click **Create Security Group**.</span></span> 
   
    <span data-ttu-id="0e3d2-193">![CreateSecurity grupp](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity grupp")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-193">![CreateSecurity Group](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity Group")</span></span>
2. <span data-ttu-id="0e3d2-194">Fullständig hello **skapa säkerhetsgrupp** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-194">Complete hello **Create Security Group** task.</span></span>  
3. <span data-ttu-id="0e3d2-195">Välj Integration Systemsäkerhetsgrupp – obegränsad från hello **typ av innehavare säkerhetsgrupp** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-195">Select Integration System Security Group—Unconstrained from hello **Type of Tenanted Security Group** dropdown.</span></span>
4. <span data-ttu-id="0e3d2-196">Skapa en säkerhet grupp toowhich medlemmar läggs explicit.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-196">Create a security group toowhich members will be explicitly added.</span></span> 
   
    <span data-ttu-id="0e3d2-197">![CreateSecurity grupp](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity grupp")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-197">![CreateSecurity Group](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity Group")</span></span>

### <a name="assign-hello-integration-system-user-toohello-security-group"></a><span data-ttu-id="0e3d2-198">Tilldela hello integration system toohello säkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="0e3d2-198">Assign hello integration system user toohello security group</span></span>

<span data-ttu-id="0e3d2-199">**tooassign hello integration systemanvändare:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-199">**tooassign hello integration system user:**</span></span>

1. <span data-ttu-id="0e3d2-200">Ange redigera säkerhetsgrupp i hello sökrutan och klicka sedan på **redigera säkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-200">Enter edit security group in hello search box, and then click **Edit Security Group**.</span></span> 
   
    <span data-ttu-id="0e3d2-201">![Redigera säkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "redigera säkerhetsgrupp")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-201">![Edit Security Group](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Edit Security Group")</span></span>
2. <span data-ttu-id="0e3d2-202">Sök efter och välj hello ny säkerhetsgrupp för integrering av namn.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-202">Search for, and select hello new integration security group by name.</span></span> 
   
    <span data-ttu-id="0e3d2-203">![Redigera säkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "redigera säkerhetsgrupp")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-203">![Edit Security Group](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Edit Security Group")</span></span>
3. <span data-ttu-id="0e3d2-204">Lägg till hello nya integration system toohello ny säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-204">Add hello new integration system user toohello new security group.</span></span> 
   
    <span data-ttu-id="0e3d2-205">![Systemsäkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Systemsäkerhetsgrupp")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-205">![System Security Group](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "System Security Group")</span></span>  

### <a name="configure-security-group-options"></a><span data-ttu-id="0e3d2-206">Konfigurera alternativ för säkerhet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-206">Configure security group options</span></span>
<span data-ttu-id="0e3d2-207">I det här steget kan du bevilja toohello nya security gruppbehörigheter för **hämta** och **placera** hello-objekt som skyddas av hello följande säkerhetsprinciper för domänen:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-207">In this step, you grant toohello new security group permissions for **Get** and **Put** operations on hello objects secured by hello following domain security policies:</span></span>

* <span data-ttu-id="0e3d2-208">Externa konto-etablering</span><span class="sxs-lookup"><span data-stu-id="0e3d2-208">External Account Provisioning</span></span>
* <span data-ttu-id="0e3d2-209">Worker Data: Public Worker rapporter</span><span class="sxs-lookup"><span data-stu-id="0e3d2-209">Worker Data: Public Worker Reports</span></span>
* <span data-ttu-id="0e3d2-210">Worker Data: Alla positioner</span><span class="sxs-lookup"><span data-stu-id="0e3d2-210">Worker Data: All Positions</span></span>
* <span data-ttu-id="0e3d2-211">Worker Data: Current bemanning Information</span><span class="sxs-lookup"><span data-stu-id="0e3d2-211">Worker Data: Current Staffing Information</span></span>
* <span data-ttu-id="0e3d2-212">Worker Data: Business titel på Worker profil</span><span class="sxs-lookup"><span data-stu-id="0e3d2-212">Worker Data: Business Title on Worker Profile</span></span>

<span data-ttu-id="0e3d2-213">**tooconfigure säkerhetsalternativ till gruppen:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-213">**tooconfigure security group options:**</span></span>

1. <span data-ttu-id="0e3d2-214">Ange säkerhetsprinciper för domänen i hello sökrutan och klicka sedan på hello länken **domän säkerhetsprinciper för funktionsområde**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-214">Enter domain security policies in hello search box, and then click on hello link **Domain Security Policies for Functional Area**.</span></span>  
   
    <span data-ttu-id="0e3d2-215">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-215">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domain Security Policies")</span></span>  
2. <span data-ttu-id="0e3d2-216">Sök efter system och välj hello **System** funktionsområde.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-216">Search for system and select hello **System** functional area.</span></span>  <span data-ttu-id="0e3d2-217">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-217">Click **OK**.</span></span>  
   
    <span data-ttu-id="0e3d2-218">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-218">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domain Security Policies")</span></span>  
3. <span data-ttu-id="0e3d2-219">Hello lista över säkerhetsprinciper för hello System funktionsområde, expandera **säkerhetsadministration** och välj hello säkerhetsprincip **externa Kontoetablering**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-219">In hello list of security policies for hello System functional area, expand **Security Administration** and select hello domain security policy **External Account Provisioning**.</span></span>  
   
    <span data-ttu-id="0e3d2-220">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-220">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domain Security Policies")</span></span>  
4. <span data-ttu-id="0e3d2-221">Klicka på **Redigera behörigheter**, och klicka sedan på hello **Redigera behörigheter**dialogrutan Lägg till hello nya säkerhet toohello grupplistan säkerhetsgrupper med **hämta** och **Placera** integrering behörigheter.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-221">Click **Edit Permissions**, and then, on hello **Edit Permissions**dialog page, add hello new security group toohello list of security groups with **Get** and **Put** integration permissions.</span></span> 
   
    <span data-ttu-id="0e3d2-222">![Redigera behörighet](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "redigera behörighet")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-222">![Edit Permission](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Edit Permission")</span></span>  
5. <span data-ttu-id="0e3d2-223">Upprepa steg 1 ovan tooreturn toohello skärmen för att välja huvudområden och nu, Sök efter personal, Välj hello **bemanning funktionsområde** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-223">Repeat step 1 above tooreturn toohello screen for selecting functional areas, and this time, search for staffing, select hello **Staffing functional area** and click **OK**.</span></span>
   
    <span data-ttu-id="0e3d2-224">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-224">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domain Security Policies")</span></span>  
6. <span data-ttu-id="0e3d2-225">Expandera i hello lista över säkerhetsprinciper för hello Staffing funktionsområde, **Worker Data: Staffing** och upprepa ovanstående steg 4 för varje återstående säkerhetsprinciper:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-225">In hello list of security policies for hello Staffing functional area, expand **Worker Data: Staffing** and repeat step 4 above for each of these remaining security policies:</span></span>

   * <span data-ttu-id="0e3d2-226">Worker Data: Public Worker rapporter</span><span class="sxs-lookup"><span data-stu-id="0e3d2-226">Worker Data: Public Worker Reports</span></span>
   * <span data-ttu-id="0e3d2-227">Worker Data: Alla positioner</span><span class="sxs-lookup"><span data-stu-id="0e3d2-227">Worker Data: All Positions</span></span>
   * <span data-ttu-id="0e3d2-228">Worker Data: Current bemanning Information</span><span class="sxs-lookup"><span data-stu-id="0e3d2-228">Worker Data: Current Staffing Information</span></span>
   * <span data-ttu-id="0e3d2-229">Worker Data: Business titel på Worker profil</span><span class="sxs-lookup"><span data-stu-id="0e3d2-229">Worker Data: Business Title on Worker Profile</span></span>
   
7. <span data-ttu-id="0e3d2-230">Upprepa steg 1 ovan tooreturn toohello skärmen för att välja huvudområden och den här tiden kan söka efter **kontaktinformation**, Välj hello Staffing funktionsområde och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-230">Repeat step 1, above, tooreturn toohello screen for selecting  functional areas, and this time, search for **Contact Information**,  select hello Staffing functional area, and click **OK**.</span></span>

8.  <span data-ttu-id="0e3d2-231">Hello lista över säkerhetsprinciper för hello Staffing funktionsområde, expandera **Worker Data: Kontakta arbetsinformation**, och upprepa steg 4 ovan för hello säkerhetsprinciper nedan:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-231">In hello list of security policies for hello Staffing functional area, expand **Worker Data: Work Contact Information**, and repeat step 4 above for hello security policies below:</span></span>

    * <span data-ttu-id="0e3d2-232">Worker Data: E-post</span><span class="sxs-lookup"><span data-stu-id="0e3d2-232">Worker Data: Work Email</span></span>

    <span data-ttu-id="0e3d2-233">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-233">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domain Security Policies")</span></span>  
    
### <a name="activate-security-policy-changes"></a><span data-ttu-id="0e3d2-234">Aktivera ändringar i säkerhet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-234">Activate security policy changes</span></span>

<span data-ttu-id="0e3d2-235">**ändringar i tooactivate säkerhet:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-235">**tooactivate security policy changes:**</span></span>

1. <span data-ttu-id="0e3d2-236">Ange aktiverar i hello sökrutan och klicka sedan på hello länken **aktivera väntande ändringar i säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-236">Enter activate in hello search box, and then click on hello link **Activate Pending Security Policy Changes**.</span></span> 
   
    <span data-ttu-id="0e3d2-237">![Aktivera](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "aktivera")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-237">![Activate](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Activate")</span></span> 
2. <span data-ttu-id="0e3d2-238">Begin hello aktivera väntande ändringar i säkerhet genom att ange en kommentar för granskningsändamål, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-238">Begin hello Activate Pending Security Policy Changes task by entering a comment for auditing purposes, and then click **OK**.</span></span> 
   
    <span data-ttu-id="0e3d2-239">![Aktivera väntande säkerhet](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "aktivera väntande säkerhet")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-239">![Activate Pending Security](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Activate Pending Security")</span></span>   
3. <span data-ttu-id="0e3d2-240">Fullständig hello aktivitet på nästa hello-skärmen genom att markera kryssrutan hello **Bekräfta**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-240">Complete hello task on hello next screen by checking hello checkbox **Confirm**, and then click **OK**.</span></span> 
   
    <span data-ttu-id="0e3d2-241">![Aktivera väntande säkerhet](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "aktivera väntande säkerhet")</span><span class="sxs-lookup"><span data-stu-id="0e3d2-241">![Activate Pending Security](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Activate Pending Security")</span></span>  

## <a name="configuring-user-provisioning-from-workday-tooactive-directory"></a><span data-ttu-id="0e3d2-242">Konfigurering av användarförsörjning från Workday tooActive Directory</span><span class="sxs-lookup"><span data-stu-id="0e3d2-242">Configuring user provisioning from Workday tooActive Directory</span></span>
<span data-ttu-id="0e3d2-243">Följ dessa instruktioner tooconfigure användarkonto etablering från Workday tooeach Active Directory-skog som du behöver etablering till.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-243">Follow these instructions tooconfigure user account provisioning from Workday tooeach Active Directory forest that you require provisioning to.</span></span>

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a><span data-ttu-id="0e3d2-244">Del 1: Lägga till hello etablering connector appen och skapa hello anslutning tooWorkday</span><span class="sxs-lookup"><span data-stu-id="0e3d2-244">Part 1: Adding hello provisioning connector app and creating hello connection tooWorkday</span></span>

<span data-ttu-id="0e3d2-245">**tooconfigure Workday tooActive Directory etablering:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-245">**tooconfigure Workday tooActive Directory provisioning:**</span></span>

1.  <span data-ttu-id="0e3d2-246">Gå för<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="0e3d2-246">Go too<https://portal.azure.com></span></span>

2.  <span data-ttu-id="0e3d2-247">Välj i hello vänstra navigeringsfältet **Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-247">In hello left navigation bar, select **Azure Active Directory**</span></span>

3.  <span data-ttu-id="0e3d2-248">Välj **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-248">Select **Enterprise Applications**, then **All Applications**.</span></span>

4.  <span data-ttu-id="0e3d2-249">Välj **lägga till ett program**, och välj hello **alla** kategori.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-249">Select **Add an application**, and select hello **All** category.</span></span>

5.  <span data-ttu-id="0e3d2-250">Sök efter **Workday etablering tooActive Directory**, och Lägg till appen från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-250">Search for **Workday Provisioning tooActive Directory**, and add that app from hello gallery.</span></span>

6.  <span data-ttu-id="0e3d2-251">När hello app har lagts till och hello app information visas, Välj **etablering**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-251">After hello app is added and hello app details screen is shown, select **Provisioning**</span></span>

7.  <span data-ttu-id="0e3d2-252">Ändra hello **etablering** **läge** för**automatisk**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-252">Change hello **Provisioning** **Mode** too**Automatic**</span></span>

8.  <span data-ttu-id="0e3d2-253">Fullständig hello **administratörsautentiseringsuppgifter** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-253">Complete hello **Admin Credentials** section as follows:</span></span>

   * <span data-ttu-id="0e3d2-254">**Admin Username** – ange hello användarnamnet för hello systemkontot för Workday-integrering med hello klient domännamnet tillagt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-254">**Admin Username** – Enter hello username of hello Workday integration system account, with hello tenant domain name appended.</span></span> <span data-ttu-id="0e3d2-255">**Ska se ut ungefär:username@contoso4**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-255">**Should look something like: username@contoso4**</span></span>

   * <span data-ttu-id="0e3d2-256">**Administratörslösenordet –** ange hello lösenord för hello systemkontot för Workday-integrering</span><span class="sxs-lookup"><span data-stu-id="0e3d2-256">**Admin password –** Enter hello password of hello Workday    integration system account</span></span>

   * <span data-ttu-id="0e3d2-257">**Klient URL –** ange hello URL toohello Workday web services-slutpunkten för din klient.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-257">**Tenant URL –** Enter hello URL toohello Workday web services endpoint for your tenant.</span></span> <span data-ttu-id="0e3d2-258">Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med hello rätt miljö sträng.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-258">This should look like: https://wd3-impl-services1.workday.com/ccx/service/contoso4, where contoso4 is replaced with your correct tenant name and wd3-impl is replaced with hello correct environment string.</span></span>

   * <span data-ttu-id="0e3d2-259">**Active Directory-skogar -** hello ”Name” för Active Directory-skog som returneras av hello Get-ADForest powershell cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-259">**Active Directory Forest -** hello “Name” of your Active Directory forest, as returned by hello Get-ADForest powershell commandlet.</span></span> <span data-ttu-id="0e3d2-260">Detta är vanligtvis en sträng som: *contoso.com*</span><span class="sxs-lookup"><span data-stu-id="0e3d2-260">This is typically a string like: *contoso.com*</span></span>

   * <span data-ttu-id="0e3d2-261">**Active Directory-behållare -** ange hello behållaren sträng som innehåller alla användare i din AD-skog.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-261">**Active Directory Container -** Enter hello container string that contains all users in your AD forest.</span></span> <span data-ttu-id="0e3d2-262">Exempel: *OU = standardanvändare, OU = Users, DC = contoso, DC = test*</span><span class="sxs-lookup"><span data-stu-id="0e3d2-262">Example: *OU=Standard Users,OU=Users,DC=contoso,DC=test*</span></span>

   * <span data-ttu-id="0e3d2-263">**E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-263">**Notification Email –** Enter your email address, and check the    “send email if failure occurs” checkbox.</span></span>

   * <span data-ttu-id="0e3d2-264">Klicka på hello **Testanslutningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-264">Click hello **Test Connection** button.</span></span> <span data-ttu-id="0e3d2-265">Om anslutningstestet hello lyckas klickar du på hello **spara** knappen hello överst.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-265">If hello connection test succeeds, click hello **Save** button at hello top.</span></span> <span data-ttu-id="0e3d2-266">Om det misslyckas, kan du kontrollera att hello Workday-autentiseringsuppgifter är giltiga i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-266">If it fails, double-check that hello Workday credentials are valid in Workday.</span></span> 

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a><span data-ttu-id="0e3d2-268">Del 2: Konfigurera attributmappning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-268">Part 2: Configure attribute mappings</span></span> 

<span data-ttu-id="0e3d2-269">I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-269">In this section, you will configure how user data flows from Workday to Active Directory.</span></span>

1.  <span data-ttu-id="0e3d2-270">Hello etablering på fliken **mappningar**, klickar du på **synkronisera Workday arbetare tooOnPremises**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-270">On hello Provisioning tab under **Mappings**, click **Synchronize Workday Workers tooOnPremises**.</span></span>

2.  <span data-ttu-id="0e3d2-271">I hello **källa Objektområde** fält, kan du välja vilka uppsättningar med användare i Workday ska vara i omfånget för att etablera tooAD, genom att definiera en uppsättning attributbaserad filter.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-271">In hello **Source Object Scope** field, you can select which sets of users in Workday should be in scope for provisioning tooAD, by defining a set of attribute-based filters.</span></span> <span data-ttu-id="0e3d2-272">hello standardscope är ”alla användare i Workday”.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-272">hello default scope is “all users in Workday”.</span></span> <span data-ttu-id="0e3d2-273">Exempel filter:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-273">Example filters:</span></span>

   * <span data-ttu-id="0e3d2-274">Exempel: Scope toousers med Worker-ID: N mellan 1000000 och 2000000</span><span class="sxs-lookup"><span data-stu-id="0e3d2-274">Example: Scope toousers with Worker IDs between 1000000 and    2000000</span></span>

      * <span data-ttu-id="0e3d2-275">Attributet: WorkerID</span><span class="sxs-lookup"><span data-stu-id="0e3d2-275">Attribute: WorkerID</span></span>

      * <span data-ttu-id="0e3d2-276">Operatorn: REGEX-matchning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-276">Operator: REGEX Match</span></span>

      * <span data-ttu-id="0e3d2-277">Värde: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span><span class="sxs-lookup"><span data-stu-id="0e3d2-277">Value: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span></span>

   * <span data-ttu-id="0e3d2-278">Exempel: Endast anställda och inte contingent personer</span><span class="sxs-lookup"><span data-stu-id="0e3d2-278">Example: Only employees and not contingent workers</span></span> 

      * <span data-ttu-id="0e3d2-279">Attributet: EmployeeID</span><span class="sxs-lookup"><span data-stu-id="0e3d2-279">Attribute: EmployeeID</span></span>

      * <span data-ttu-id="0e3d2-280">Operatorn: Inte är NULL</span><span class="sxs-lookup"><span data-stu-id="0e3d2-280">Operator: IS NOT NULL</span></span>

3.  <span data-ttu-id="0e3d2-281">I hello **mål objektåtgärder** fält du kan globalt filtrera vilka åtgärder tillåts toobe utförs på Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-281">In hello **Target Object Actions** field, you can globally filter what actions are allowed toobe performed on Active Directory.</span></span> <span data-ttu-id="0e3d2-282">**Skapa** och **uppdatering** är de vanligaste.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-282">**Create** and **Update** are most common.</span></span>

4.  <span data-ttu-id="0e3d2-283">I hello **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappa tooActive katalogattribut.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-283">In hello **Attribute mappings** section, you can define how individual Workday attributes map tooActive Directory attributes.</span></span>

5. <span data-ttu-id="0e3d2-284">Klicka på ett befintligt attribut mappning tooupdate den, eller klicka **Lägg till ny mappning** längst hello hello skärmen tooadd nya mappningar.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-284">Click on an existing attribute mapping tooupdate it, or click **Add new mapping** at hello bottom of hello screen tooadd new mappings.</span></span> <span data-ttu-id="0e3d2-285">En enskild attributmappning stöder dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-285">An individual attribute mapping supports these properties:</span></span>

      * <span data-ttu-id="0e3d2-286">**Avbildningstyp**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-286">**Mapping Type**</span></span>

         * <span data-ttu-id="0e3d2-287">**Direkt** – skriver hello värdet för hello Workday attributet toohello AD-attributet, utan ändringar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-287">**Direct** – Writes hello value of hello Workday attribute      toohello AD attribute, with no changes</span></span>

         * <span data-ttu-id="0e3d2-288">**Konstant** -skriva ett statiska, konstant strängvärde till hello AD-attribut</span><span class="sxs-lookup"><span data-stu-id="0e3d2-288">**Constant** - Write a static, constant string value to      hello AD attribute</span></span>

         * <span data-ttu-id="0e3d2-289">**Uttrycket** – kan du toowrite ett anpassat värde till hello AD-attribut, baserat på en eller flera Workday-attribut.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-289">**Expression** – Allows you toowrite a custom value to hello AD attribute, based on one or more Workday attributes.</span></span> <span data-ttu-id="0e3d2-290">[Mer information finns i den här artikeln på uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-290">[For more info, see this article on expressions](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>

      * <span data-ttu-id="0e3d2-291">**Källattributet** -hello användarattribut från Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-291">**Source attribute** - hello user attribute from Workday.</span></span>

      * <span data-ttu-id="0e3d2-292">**Standardvärde** – det är valfritt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-292">**Default value** – Optional.</span></span> <span data-ttu-id="0e3d2-293">Om hello källattribut har ett tomt värde, skrivs hello mappningen det här värdet i stället.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-293">If hello source attribute has an empty value, hello mapping will write this value instead.</span></span>
            <span data-ttu-id="0e3d2-294">De vanligaste konfigurationen är tooleave detta tomt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-294">Most common configuration is tooleave this blank.</span></span>

      * <span data-ttu-id="0e3d2-295">**Målattribut** – hello användarattribut i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-295">**Target attribute** – hello user attribute in Active     Directory.</span></span>

      * <span data-ttu-id="0e3d2-296">**Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen används toouniquely identifiera användare mellan Workday och Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-296">**Match objects using this attribute** – Whether or not this mapping should be used toouniquely identify users between Workday and Active Directory.</span></span> <span data-ttu-id="0e3d2-297">Detta anges normalt Worker ID-fältet för Workday som vanligtvis är mappad till hello anställnings-ID-attribut i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-297">This is typically set on the Worker ID field for Workday, which is typically mapped to one of hello Employee ID attributes in Active Directory.</span></span>

      * <span data-ttu-id="0e3d2-298">**Matchar prioritet** – flera matchande attribut kan anges.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-298">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="0e3d2-299">När det finns flera, utvärderas de i den ordning som anges av det här fältet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-299">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="0e3d2-300">När en matchning hittas matchar ingen ytterligare attribut utvärderas.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-300">As soon as a match is found, no further matching attributes are evaluated.</span></span>

      * <span data-ttu-id="0e3d2-301">**Använd den här mappningen**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-301">**Apply this mapping**</span></span>
       
         * <span data-ttu-id="0e3d2-302">**Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e3d2-302">**Always** – Apply this mapping on both user creation      and update actions</span></span>

         * <span data-ttu-id="0e3d2-303">**Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e3d2-303">**Only during creation** - Apply this mapping only on      user creation actions</span></span>

6. <span data-ttu-id="0e3d2-304">toosave kopplingar, klicka på **spara** hello överst i avsnittet attributmappning.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-304">toosave your mappings, click **Save** at hello top of the      Attribute Mapping section.</span></span>

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

<span data-ttu-id="0e3d2-306">**Här följer några exempel attributmappning mellan Workday och Active Directory med vissa vanliga uttryck**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-306">**Below are some example attribute mappings between Workday and Active Directory, with some common expressions**</span></span>

-   <span data-ttu-id="0e3d2-307">hello-uttrycket som mappar toohello parentDistinguishedName AD-attributet kan vara används tooprovision en användare tooa Organisationsenheten baserat på en eller flera Workday källa attribut.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-307">hello expression that maps toohello parentDistinguishedName AD attribute can be used tooprovision a user tooa specific OU based on one or more Workday source attributes.</span></span> <span data-ttu-id="0e3d2-308">Det här exemplet placerar användare i olika organisationsenheter beroende på deras stad data i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-308">This example places users in different OUs depending on their city data in Workday.</span></span>

-   <span data-ttu-id="0e3d2-309">hello-uttryck som mappar toohello userPrincipalName AD-attributet skapar en UPN för firstName.LastName@contoso.com. Den ersätter också ogiltiga specialtecken.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-309">hello expression that maps toohello userPrincipalName AD attribute create a UPN of firstName.LastName@contoso.com. It also replaces illegal special characters.</span></span>

-   [<span data-ttu-id="0e3d2-310">Det finns dokumentationen om hur du skriver här uttryck</span><span class="sxs-lookup"><span data-stu-id="0e3d2-310">There is documentation on writing expressions here</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| <span data-ttu-id="0e3d2-311">ARBETSDAGAR ATTRIBUT</span><span class="sxs-lookup"><span data-stu-id="0e3d2-311">WORKDAY ATTRIBUTE</span></span> | <span data-ttu-id="0e3d2-312">ACTIVE DIRECTORY-ATTRIBUT</span><span class="sxs-lookup"><span data-stu-id="0e3d2-312">ACTIVE DIRECTORY ATTRIBUTE</span></span> |  <span data-ttu-id="0e3d2-313">MATCHANDE ID?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-313">MATCHING ID?</span></span> | <span data-ttu-id="0e3d2-314">SKAPA / UPPDATERA</span><span class="sxs-lookup"><span data-stu-id="0e3d2-314">CREATE / UPDATE</span></span> |
| ---------- | ---------- | ---------- | ---------- |
|  <span data-ttu-id="0e3d2-315">**WorkerID**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-315">**WorkerID**</span></span>  |  <span data-ttu-id="0e3d2-316">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="0e3d2-316">EmployeeID</span></span> | <span data-ttu-id="0e3d2-317">**Ja**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-317">**Yes**</span></span> | <span data-ttu-id="0e3d2-318">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="0e3d2-318">Written on create only</span></span> | 
|  <span data-ttu-id="0e3d2-319">**Namnet**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-319">**Municipality**</span></span>   |   <span data-ttu-id="0e3d2-320">L</span><span class="sxs-lookup"><span data-stu-id="0e3d2-320">l</span></span>   |     | <span data-ttu-id="0e3d2-321">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-321">Create + update</span></span> |
|  <span data-ttu-id="0e3d2-322">**Företag**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-322">**Company**</span></span>         | <span data-ttu-id="0e3d2-323">Företag</span><span class="sxs-lookup"><span data-stu-id="0e3d2-323">company</span></span>   |     |  <span data-ttu-id="0e3d2-324">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-324">Create + update</span></span> |
|  <span data-ttu-id="0e3d2-325">**CountryReferenceTwoLetter**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-325">**CountryReferenceTwoLetter**</span></span>      |   <span data-ttu-id="0e3d2-326">CO</span><span class="sxs-lookup"><span data-stu-id="0e3d2-326">co</span></span> |     |   <span data-ttu-id="0e3d2-327">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-327">Create + update</span></span> |
| <span data-ttu-id="0e3d2-328">**CountryReferenceTwoLetter**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-328">**CountryReferenceTwoLetter**</span></span>    |  <span data-ttu-id="0e3d2-329">C</span><span class="sxs-lookup"><span data-stu-id="0e3d2-329">c</span></span>  |     |         <span data-ttu-id="0e3d2-330">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-330">Create + update</span></span> |
| <span data-ttu-id="0e3d2-331">**SupervisoryOrganization**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-331">**SupervisoryOrganization**</span></span>  | <span data-ttu-id="0e3d2-332">Avdelning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-332">department</span></span>  |     |  <span data-ttu-id="0e3d2-333">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-333">Create + update</span></span> |
|  <span data-ttu-id="0e3d2-334">**PreferredNameData**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-334">**PreferredNameData**</span></span>  |  <span data-ttu-id="0e3d2-335">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="0e3d2-335">displayName</span></span> |     |   <span data-ttu-id="0e3d2-336">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-336">Create + update</span></span> |
| <span data-ttu-id="0e3d2-337">**EmployeeID**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-337">**EmployeeID**</span></span>    |  <span data-ttu-id="0e3d2-338">CN</span><span class="sxs-lookup"><span data-stu-id="0e3d2-338">cn</span></span>    |   |   <span data-ttu-id="0e3d2-339">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="0e3d2-339">Written on create only</span></span> |
| <span data-ttu-id="0e3d2-340">**Fax**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-340">**Fax**</span></span>      | <span data-ttu-id="0e3d2-341">facsimileTelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="0e3d2-341">facsimileTelephoneNumber</span></span>     |     |    <span data-ttu-id="0e3d2-342">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-342">Create + update</span></span> |
| <span data-ttu-id="0e3d2-343">**Förnamn**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-343">**FirstName**</span></span>   | <span data-ttu-id="0e3d2-344">givenName</span><span class="sxs-lookup"><span data-stu-id="0e3d2-344">givenName</span></span>       |     |    <span data-ttu-id="0e3d2-345">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-345">Create + update</span></span> |
| <span data-ttu-id="0e3d2-346">**Växel (\[Active\],, ”0”, ”True”, ”1”)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-346">**Switch(\[Active\], , "0", "True", "1",)**</span></span> |  <span data-ttu-id="0e3d2-347">AccountDisabled</span><span class="sxs-lookup"><span data-stu-id="0e3d2-347">accountDisabled</span></span>      |     | <span data-ttu-id="0e3d2-348">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-348">Create + update</span></span> |
| <span data-ttu-id="0e3d2-349">**Mobile**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-349">**Mobile**</span></span>  |    <span data-ttu-id="0e3d2-350">mobila</span><span class="sxs-lookup"><span data-stu-id="0e3d2-350">mobile</span></span>       |     |       <span data-ttu-id="0e3d2-351">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="0e3d2-351">Written on create only</span></span> |
| <span data-ttu-id="0e3d2-352">**E-postadress**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-352">**EmailAddress**</span></span>    | <span data-ttu-id="0e3d2-353">E-post</span><span class="sxs-lookup"><span data-stu-id="0e3d2-353">mail</span></span>    |     |     <span data-ttu-id="0e3d2-354">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-354">Create + update</span></span> |
| <span data-ttu-id="0e3d2-355">**ManagerReference**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-355">**ManagerReference**</span></span>   | <span data-ttu-id="0e3d2-356">Manager</span><span class="sxs-lookup"><span data-stu-id="0e3d2-356">manager</span></span>  |     |  <span data-ttu-id="0e3d2-357">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-357">Create + update</span></span> |
| <span data-ttu-id="0e3d2-358">**WorkSpaceReference**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-358">**WorkSpaceReference**</span></span> | <span data-ttu-id="0e3d2-359">physicalDeliveryOfficeName</span><span class="sxs-lookup"><span data-stu-id="0e3d2-359">physicalDeliveryOfficeName</span></span>    |     |  <span data-ttu-id="0e3d2-360">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-360">Create + update</span></span> |
| <span data-ttu-id="0e3d2-361">**Postnummer**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-361">**PostalCode**</span></span>  |   <span data-ttu-id="0e3d2-362">Postnummer</span><span class="sxs-lookup"><span data-stu-id="0e3d2-362">postalCode</span></span>  |     | <span data-ttu-id="0e3d2-363">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-363">Create + update</span></span> |
| <span data-ttu-id="0e3d2-364">**LocalReference**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-364">**LocalReference**</span></span> |  <span data-ttu-id="0e3d2-365">preferredLanguage</span><span class="sxs-lookup"><span data-stu-id="0e3d2-365">preferredLanguage</span></span>  |     |  <span data-ttu-id="0e3d2-366">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-366">Create + update</span></span> |
| <span data-ttu-id="0e3d2-367">** Ersätta (Mid (Ersätt (\[EmployeeID\]”, (\[ \\ \\ / \\ \\ \\ \\ \\ \\ \[\\\\\]\\\\:\\\\;\\\\</span><span class="sxs-lookup"><span data-stu-id="0e3d2-367">**Replace(Mid(Replace(\[EmployeeID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\</span></span>|<span data-ttu-id="0e3d2-368">\\\\=\\\\,\\\\+\\\\\*\\\\?\\ \\ &lt; \\ \\ &gt; \]) ””, ”,), 1, 20)”, ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-368">\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**</span></span>      |    <span data-ttu-id="0e3d2-369">SAMAccountName</span><span class="sxs-lookup"><span data-stu-id="0e3d2-369">sAMAccountName</span></span>            |     |         <span data-ttu-id="0e3d2-370">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="0e3d2-370">Written on create only</span></span> |
| <span data-ttu-id="0e3d2-371">**Efternamn**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-371">**LastName**</span></span>   |   <span data-ttu-id="0e3d2-372">SN</span><span class="sxs-lookup"><span data-stu-id="0e3d2-372">sn</span></span>   |     |  <span data-ttu-id="0e3d2-373">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-373">Create + update</span></span> |
| <span data-ttu-id="0e3d2-374">**CountryRegionReference**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-374">**CountryRegionReference**</span></span> |  <span data-ttu-id="0e3d2-375">St</span><span class="sxs-lookup"><span data-stu-id="0e3d2-375">st</span></span>     |     | <span data-ttu-id="0e3d2-376">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-376">Create + update</span></span> |
| <span data-ttu-id="0e3d2-377">**AddressLineData**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-377">**AddressLineData**</span></span>    |  <span data-ttu-id="0e3d2-378">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="0e3d2-378">streetAddress</span></span>  |     |   <span data-ttu-id="0e3d2-379">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-379">Create + update</span></span> |
| <span data-ttu-id="0e3d2-380">**PrimaryWorkTelephone**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-380">**PrimaryWorkTelephone**</span></span>  |  <span data-ttu-id="0e3d2-381">telephoneNumber</span><span class="sxs-lookup"><span data-stu-id="0e3d2-381">telephoneNumber</span></span>   |     | <span data-ttu-id="0e3d2-382">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="0e3d2-382">Written on create only</span></span> |
| <span data-ttu-id="0e3d2-383">**BusinessTitle**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-383">**BusinessTitle**</span></span>   |  <span data-ttu-id="0e3d2-384">Rubrik</span><span class="sxs-lookup"><span data-stu-id="0e3d2-384">title</span></span>     |     |  <span data-ttu-id="0e3d2-385">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-385">Create + update</span></span> |
| <span data-ttu-id="0e3d2-386">**Ansluta (”@”, Ersätt (Ersätt (ersätta (Ersätt (ersätta (Ersätt (ersätta (ersätta (Ersätt ((Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt ( Ersätt (ansluta till (””., [Förnamn], [Efternamn]), ”([Øø])”, ”Outlook Express”,), ”[Ææ]”, ”ae”,), ”([äãàâãåáąÄÃÀÂÃÅÁĄA])”, ”a”,), ”[B]”, ”b”,), ”([CçčćÇČĆ])”, ”c”,), ”([ďĎD])”, ”d”,), ”([ëèéêęěËÈÉÊĘĚE])”, ”e”,), ”[F]”, ”f”,), ”([G])” ”g”,), ”[H]”, ”h”,), ”([ïîìíÏÎÌÍI])”, ”i”,), ”[J]”, ”j”,), ”([K])”, ”k”,), ”([ľłŁĽL])”, ”l”,), ”([M])”, ”m”,), ”([ñńňÑŃŇN])”, ”n”,), ”([öòőõôóÖÒŐÕÔÓO])”, ”o”,), ”([P])”, ”p”,), ”([Q])”, ”q”,),  ”([ŘŘR])”, ”r”,), ”([ßšśŠŚS])”, ”s”,), ”([TŤť])”, ”t”,), ”([üùûúůűÜÙÛÚŮŰU])”, ”u”,), ”([V])”, ”v”,), ”([B])”, ”b”,), ”([ýÿýŸÝY])”, ”y”,), ”([źžżŹŽŻZ])”, ”z”,) ”,”, ””,), ”contoso.com”)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-386">**Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])", , "m", , ), , "([ñńňÑŃŇN])", , "n", , ), , "([öòőõôóÖÒŐÕÔÓO])", , "o", , ), , "([P])", , "p", , ), , "([Q])", , "q", , ), , "([řŘR])", , "r", , ), , "([ßšśŠŚS])", , "s", , ), , "([TŤť])", , "t", , ), , "([üùûúůűÜÙÛÚŮŰU])", , "u", , ), , "([V])", , "v", , ), , "([W])", , "w", , ), , "([ýÿýŸÝY])", , "y", , ), , "([źžżŹŽŻZ])", , "z", , ), " ", , , "", , ), "contoso.com")**</span></span>   | <span data-ttu-id="0e3d2-387">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="0e3d2-387">userPrincipalName</span></span>     |     | <span data-ttu-id="0e3d2-388">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-388">Create + update</span></span>                                                   
| <span data-ttu-id="0e3d2-389">**Växel (\[namnet\]”, OU standardanvändare, OU = = användare, OU = standard, OU = platser, DC = contoso, DC = com”, ”Dallas” ”, OU standardanvändare, OU = = användare, OU Dallas, OU = platser, DC = = contoso, DC = com”, ”Austin” ”OU standardanvändare, OU = = Användare, OU Austin, OU = platser, DC = = contoso, DC = com ”,” Seattle ””, OU standardanvändare, OU = = användare, OU Seattle, OU = = platser, DC = contoso, DC = com ”,” London ””, OU standardanvändare, OU = = användare, OU London, OU = platser, DC = = contoso, DC = com ”)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-389">**Switch(\[Municipality\], "OU=Standard Users,OU=Users,OU=Default,OU=Locations,DC=contoso,DC=com", "Dallas", "OU=Standard Users,OU=Users,OU=Dallas,OU=Locations,DC=contoso,DC=com", "Austin", "OU=Standard Users,OU=Users,OU=Austin,OU=Locations,DC=contoso,DC=com", "Seattle", "OU=Standard Users,OU=Users,OU=Seattle,OU=Locations,DC=contoso,DC=com", “London", "OU=Standard Users,OU=Users,OU=London,OU=Locations,DC=contoso,DC=com")**</span></span>  | <span data-ttu-id="0e3d2-390">parentDistinguishedName</span><span class="sxs-lookup"><span data-stu-id="0e3d2-390">parentDistinguishedName</span></span>     |     |  <span data-ttu-id="0e3d2-391">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="0e3d2-391">Create + update</span></span> |
  
### <a name="part-3-configure-hello-on-premises-synchronization-agent"></a><span data-ttu-id="0e3d2-392">Del 3: Konfigurera hello lokalt synkronisering agent</span><span class="sxs-lookup"><span data-stu-id="0e3d2-392">Part 3: Configure hello on-premises synchronization agent</span></span>

<span data-ttu-id="0e3d2-393">En agent måste installeras på en domänansluten server i hello önskan Active Directory-skog i ordning tooprovision tooActive Directory lokalt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-393">In order tooprovision tooActive Directory on-premises, an agent must be installed on a domain-joined server in hello desire Active Directory forest.</span></span> <span data-ttu-id="0e3d2-394">Domänadministratörer (eller företagsadministratör) autentiseringsuppgifter krävs för att slutföra hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-394">Domain admin (or Enterprise admin) credentials are required to complete hello procedure.</span></span>

<span data-ttu-id="0e3d2-395">**[Du kan hämta hello lokalt lösenordssynkroniseringsagenten här](https://go.microsoft.com/fwlink/?linkid=847801)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-395">**[You can download hello on-premises synchronization agent here](https://go.microsoft.com/fwlink/?linkid=847801)**</span></span>

<span data-ttu-id="0e3d2-396">När du har installerat agenten, kör du hello Powershell-kommandon nedan tooconfigure hello agent för din miljö.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-396">After installing agent, run hello Powershell commands below tooconfigure hello agent for your environment.</span></span>

<span data-ttu-id="0e3d2-397">**Kommandot #1**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-397">**Command #1**</span></span>

> <span data-ttu-id="0e3d2-398">CD C:\\programfiler\\Microsoft Azure Active Directory-synkronisering Agent\\moduler\\AADSyncAgent</span><span class="sxs-lookup"><span data-stu-id="0e3d2-398">cd C:\\Program Files\\Microsoft Azure Active Directory Synchronization Agent\\Modules\\AADSyncAgent</span></span>

> <span data-ttu-id="0e3d2-399">import-module AADSyncAgent.psd1</span><span class="sxs-lookup"><span data-stu-id="0e3d2-399">import-module AADSyncAgent.psd1</span></span>

<span data-ttu-id="0e3d2-400">**Kommandot #2**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-400">**Command #2**</span></span>

> <span data-ttu-id="0e3d2-401">Lägg till ADSyncAgentActiveDirectoryConfiguration</span><span class="sxs-lookup"><span data-stu-id="0e3d2-401">Add-ADSyncAgentActiveDirectoryConfiguration</span></span>

* <span data-ttu-id="0e3d2-402">Indata: För ”katalognamn” ange hello AD skogsnamnet som har angetts i del \#2</span><span class="sxs-lookup"><span data-stu-id="0e3d2-402">Input: For "Directory Name", enter hello AD Forest name, as entered in part \#2</span></span>
* <span data-ttu-id="0e3d2-403">Indata: Admin användarnamn och lösenord för Active Directory-skog</span><span class="sxs-lookup"><span data-stu-id="0e3d2-403">Input: Admin username and password for Active Directory forest</span></span>

<span data-ttu-id="0e3d2-404">**Kommandot #3**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-404">**Command #3**</span></span>

> <span data-ttu-id="0e3d2-405">Lägg till ADSyncAgentAzureActiveDirectoryConfiguration</span><span class="sxs-lookup"><span data-stu-id="0e3d2-405">Add-ADSyncAgentAzureActiveDirectoryConfiguration</span></span>

* <span data-ttu-id="0e3d2-406">Indata: Global administratörsanvändarnamnet och lösenordet för din Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="0e3d2-406">Input: Global admin username and password for your Azure AD tenant</span></span>

<span data-ttu-id="0e3d2-407">**Kommandot #4**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-407">**Command #4**</span></span>

> <span data-ttu-id="0e3d2-408">Get-AdSyncAgentProvisioningTasks</span><span class="sxs-lookup"><span data-stu-id="0e3d2-408">Get-AdSyncAgentProvisioningTasks</span></span>

* <span data-ttu-id="0e3d2-409">Åtgärd: Kontrollera data returneras.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-409">Action: Confirm data is returned.</span></span> <span data-ttu-id="0e3d2-410">Det här kommandot upptäcker automatiskt Workday etablering appar i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-410">This command automatically discovers Workday provisioning apps in your Azure AD tenant.</span></span> <span data-ttu-id="0e3d2-411">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-411">Example output:</span></span>

> <span data-ttu-id="0e3d2-412">Namn: Min AD-skog</span><span class="sxs-lookup"><span data-stu-id="0e3d2-412">Name          : My AD Forest</span></span>
>
> <span data-ttu-id="0e3d2-413">Aktiverad: True</span><span class="sxs-lookup"><span data-stu-id="0e3d2-413">Enabled       : True</span></span>
>
> <span data-ttu-id="0e3d2-414">DirectoryName: mydomain.contoso.com</span><span class="sxs-lookup"><span data-stu-id="0e3d2-414">DirectoryName : mydomain.contoso.com</span></span>
>
> <span data-ttu-id="0e3d2-415">Trovärdig: False</span><span class="sxs-lookup"><span data-stu-id="0e3d2-415">Credentialed  : False</span></span>
>
> <span data-ttu-id="0e3d2-416">ID: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203</span><span class="sxs-lookup"><span data-stu-id="0e3d2-416">Identifier    : WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203</span></span>

<span data-ttu-id="0e3d2-417">**Kommandot #5**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-417">**Command #5**</span></span>

> <span data-ttu-id="0e3d2-418">Start-AdSyncAgentSynchronization-automatisk</span><span class="sxs-lookup"><span data-stu-id="0e3d2-418">Start-AdSyncAgentSynchronization -Automatic</span></span>

<span data-ttu-id="0e3d2-419">**Kommandot #6**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-419">**Command #6**</span></span>

> <span data-ttu-id="0e3d2-420">net stop aadsyncagent</span><span class="sxs-lookup"><span data-stu-id="0e3d2-420">net stop aadsyncagent</span></span>

<span data-ttu-id="0e3d2-421">**Kommandot #7**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-421">**Command #7**</span></span>

> <span data-ttu-id="0e3d2-422">net start aadsyncagent</span><span class="sxs-lookup"><span data-stu-id="0e3d2-422">net start aadsyncagent</span></span>

### <a name="part-4-start-hello-service"></a><span data-ttu-id="0e3d2-423">Del 4: Start hello service</span><span class="sxs-lookup"><span data-stu-id="0e3d2-423">Part 4: Start hello service</span></span>
<span data-ttu-id="0e3d2-424">När du delar 1-3 har slutförts, kan du starta hello etableras i hello Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-424">Once parts 1-3 have been completed, you can start hello provisioning service back in hello Azure Management Portal.</span></span>

1.  <span data-ttu-id="0e3d2-425">I hello **etablering** fliken, ange hello **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-425">In hello **Provisioning** tab, set hello **Provisioning Status** to **On**.</span></span>

2. <span data-ttu-id="0e3d2-426">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-426">Click **Save**.</span></span>

3. <span data-ttu-id="0e3d2-427">Detta startar hello inledande synkronisering, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-427">This will start hello initial sync, which can take a variable number of hours depending on how many users are in Workday.</span></span>

4. <span data-ttu-id="0e3d2-428">Enskilda sync händelser, t.ex vilka användare läses från Workday och sedan senare lagts till eller uppdaterats tooActive Directory, kan visas i hello **granskningsloggar** fliken. **[Se hello etablering reporting vägledning för detaljerade anvisningar om hur tooread hello granskningsloggar](active-directory-saas-provisioning-reporting.md)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-428">Individual sync events such as what users are being read out of  Workday, and then subsequently added or updated tooActive Directory,  can be viewed in hello **Audit Logs** tab. **[See hello provisioning reporting guide for detailed instructions on how tooread hello audit logs](active-directory-saas-provisioning-reporting.md)**</span></span>

5.  <span data-ttu-id="0e3d2-429">hello Windows programlogg hello agentdator visas alla åtgärder som utförs via hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-429">hello Windows Application log on hello agent machine will show all operations performed via hello agent.</span></span>

6. <span data-ttu-id="0e3d2-430">En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-430">One completed, it will write an audit summary report in the  **Provisioning** tab, as shown below.</span></span>

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-tooazure-active-directory"></a><span data-ttu-id="0e3d2-432">Konfigurera användaretablering tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e3d2-432">Configuring user provisioning tooAzure Active Directory</span></span>
<span data-ttu-id="0e3d2-433">Hur du konfigurerar etablering tooAzure Active Directory beror på dina etablering krav som anges i hello nedan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-433">How you configure provisioning tooAzure Active Directory will depend on your provisioning requirements, as detailed in hello table below.</span></span>

| <span data-ttu-id="0e3d2-434">Scenario</span><span class="sxs-lookup"><span data-stu-id="0e3d2-434">Scenario</span></span> | <span data-ttu-id="0e3d2-435">Lösning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-435">Solution</span></span> |
| -------- | -------- |
| <span data-ttu-id="0e3d2-436">**Användare behöver toobe etableras tooActive Directory och Azure AD**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-436">**Users need toobe provisioned tooActive Directory and Azure AD**</span></span> | <span data-ttu-id="0e3d2-437">Använd  **[AAD Connect](connect/active-directory-aadconnect.md)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-437">Use **[AAD Connect](connect/active-directory-aadconnect.md)**</span></span> |
| <span data-ttu-id="0e3d2-438">**Användare behöver toobe etableras tooActive Directory endast**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-438">**Users need toobe provisioned tooActive Directory only**</span></span> | <span data-ttu-id="0e3d2-439">Använd  **[AAD Connect](connect/active-directory-aadconnect.md)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-439">Use **[AAD Connect](connect/active-directory-aadconnect.md)**</span></span> |
| <span data-ttu-id="0e3d2-440">**Användare behöver toobe etableras tooAzure AD endast (moln endast)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-440">**Users need toobe provisioned tooAzure AD only (cloud only)**</span></span> | <span data-ttu-id="0e3d2-441">Använd hello **Workday tooAzure Active Directory-etablering** app i hello app-galleriet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-441">Use hello **Workday tooAzure Active Directory provisioning** app in hello app gallery</span></span> |

<span data-ttu-id="0e3d2-442">Anvisningar om hur du konfigurerar Azure AD Connect finns hello [dokumentation för Azure AD Connect](connect/active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-442">For instructions on setting up Azure AD Connect, see hello [Azure AD Connect documentation](connect/active-directory-aadconnect.md).</span></span>

<span data-ttu-id="0e3d2-443">hello följande avsnitt beskrivs hur du konfigurerar en anslutning mellan Workday och Azure AD tooprovision endast molnbaserad användare.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-443">hello following sections describe setting up a connection between Workday and Azure AD tooprovision cloud-only users.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0e3d2-444">Följ bara hello proceduren nedan om du har endast molnbaserad användare som behöver toobe etableras tooAzure AD och inte lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-444">Only follow hello procedure below if you have cloud-only users that need toobe provisioned tooAzure AD and not on-premises Active Directory.</span></span>

### <a name="part-1-adding-hello-azure-ad-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a><span data-ttu-id="0e3d2-445">Del 1: Lägga till hello Azure AD etablering connector appen och skapa hello anslutning tooWorkday</span><span class="sxs-lookup"><span data-stu-id="0e3d2-445">Part 1: Adding hello Azure AD provisioning connector app and creating hello connection tooWorkday</span></span>

<span data-ttu-id="0e3d2-446">**tooconfigure Workday tooAzure Active Directory-etablering för endast molnbaserad användare:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-446">**tooconfigure Workday tooAzure Active Directory provisioning for cloud-only users:**</span></span>

1.  <span data-ttu-id="0e3d2-447">Gå för<https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-447">Go too<https://portal.azure.com>.</span></span>

2.  <span data-ttu-id="0e3d2-448">Välj i hello vänstra navigeringsfältet **Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-448">In hello left navigation bar, select **Azure Active Directory**</span></span>

3.  <span data-ttu-id="0e3d2-449">Välj **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-449">Select **Enterprise Applications**, then **All Applications**.</span></span>

4.  <span data-ttu-id="0e3d2-450">Välj **lägga till ett program**, och välj sedan hello **alla** kategori.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-450">Select **Add an application**, and then select hello **All** category.</span></span>

5.  <span data-ttu-id="0e3d2-451">Sök efter **Workday tooAzure AD etablering**, och Lägg till appen från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-451">Search for **Workday tooAzure AD provisioning**, and add that app from hello gallery.</span></span>

6.  <span data-ttu-id="0e3d2-452">När hello app har lagts till och hello app information visas, Välj **etablering**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-452">After hello app is added and hello app details screen is shown, select **Provisioning**</span></span>

7.  <span data-ttu-id="0e3d2-453">Ändra hello **etablering** **läge** för**automatisk**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-453">Change hello **Provisioning** **Mode** too**Automatic**</span></span>

8.  <span data-ttu-id="0e3d2-454">Fullständig hello **administratörsautentiseringsuppgifter** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-454">Complete hello **Admin Credentials** section as follows:</span></span>

   * <span data-ttu-id="0e3d2-455">**Admin Username** – ange hello användarnamnet för hello systemkontot för Workday-integrering med hello klient domännamnet tillagt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-455">**Admin Username** – Enter hello username of hello Workday integration system account, with hello tenant domain name appended.</span></span> <span data-ttu-id="0e3d2-456">Ska se ut ungefär:username@contoso4</span><span class="sxs-lookup"><span data-stu-id="0e3d2-456">Should look something like: username@contoso4</span></span>

   * <span data-ttu-id="0e3d2-457">**Administratörslösenordet –** ange hello lösenord för hello systemkontot för Workday-integrering</span><span class="sxs-lookup"><span data-stu-id="0e3d2-457">**Admin password –** Enter hello password of hello Workday    integration system account</span></span>

   * <span data-ttu-id="0e3d2-458">**Klient URL –** ange hello URL toohello Workday web services-slutpunkten för din klient.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-458">**Tenant URL –** Enter hello URL toohello Workday web services endpoint for your tenant.</span></span> <span data-ttu-id="0e3d2-459">Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med hello rätt miljö sträng (vid behov).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-459">This should look like: https://wd3-impl-services1.workday.com/ccx/service/contoso4, where contoso4 is replaced with your correct tenant name and wd3-impl is replaced with hello correct environment string (if necessary).</span></span>

   * <span data-ttu-id="0e3d2-460">**E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-460">**Notification Email –** Enter your email address, and check the    “send email if failure occurs” checkbox.</span></span>

   * <span data-ttu-id="0e3d2-461">Klicka på hello **Testanslutningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-461">Click hello **Test Connection** button.</span></span>

   * <span data-ttu-id="0e3d2-462">Om anslutningstestet hello lyckas klickar du på hello **spara** knappen hello överst.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-462">If hello connection test succeeds, click hello **Save** button at hello top.</span></span> <span data-ttu-id="0e3d2-463">Om det misslyckas, kontrollera att hello Workday-URL och autentiseringsuppgifter är giltiga i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-463">If it fails, double-check that hello Workday URL and credentials are valid in Workday.</span></span>


### <a name="part-2-configure-attribute-mappings"></a><span data-ttu-id="0e3d2-464">Del 2: Konfigurera attributmappning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-464">Part 2: Configure attribute mappings</span></span> 

<span data-ttu-id="0e3d2-465">I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Azure Active Directory för endast molnbaserad användare.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-465">In this section, you will configure how user data flows from Workday to Azure Active Directory for cloud-only users.</span></span>

1.  <span data-ttu-id="0e3d2-466">Hello etablering på fliken **mappningar**, klickar du på **synkronisera arbetare tooAzure AD**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-466">On hello Provisioning tab under **Mappings**, click **Synchronize Workers tooAzure AD**.</span></span>

2.   <span data-ttu-id="0e3d2-467">I hello **källa Objektområde** fält, kan du välja vilka uppsättningar med användare i Workday ska vara i omfånget för att etablera tooAzure AD, genom att definiera en uppsättning attributbaserad filter.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-467">In hello **Source Object Scope** field, you can select which sets of users in Workday should be in scope for provisioning tooAzure AD, by defining a set of attribute-based filters.</span></span> <span data-ttu-id="0e3d2-468">hello standardscope är ”alla användare i Workday”.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-468">hello default scope is “all users in Workday”.</span></span> <span data-ttu-id="0e3d2-469">Exempel filter:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-469">Example filters:</span></span>

   * <span data-ttu-id="0e3d2-470">Exempel: Scope toousers med Worker-ID: N mellan 1000000 och 2000000</span><span class="sxs-lookup"><span data-stu-id="0e3d2-470">Example: Scope toousers with Worker IDs between 1000000 and   2000000</span></span>

      * <span data-ttu-id="0e3d2-471">Attributet: WorkerID</span><span class="sxs-lookup"><span data-stu-id="0e3d2-471">Attribute: WorkerID</span></span>

      * <span data-ttu-id="0e3d2-472">Operatorn: REGEX-matchning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-472">Operator: REGEX Match</span></span>

      * <span data-ttu-id="0e3d2-473">Värde: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span><span class="sxs-lookup"><span data-stu-id="0e3d2-473">Value: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span></span>

   * <span data-ttu-id="0e3d2-474">Exempel: Endast contingent arbetare och inte fast anställda</span><span class="sxs-lookup"><span data-stu-id="0e3d2-474">Example: Only contingent workers and not regular employees</span></span>

      * <span data-ttu-id="0e3d2-475">Attributet: ContingentID</span><span class="sxs-lookup"><span data-stu-id="0e3d2-475">Attribute: ContingentID</span></span>

      * <span data-ttu-id="0e3d2-476">Operatorn: Inte är NULL</span><span class="sxs-lookup"><span data-stu-id="0e3d2-476">Operator: IS NOT NULL</span></span>

3.  <span data-ttu-id="0e3d2-477">I hello **mål objektåtgärder** fält du kan globalt filtrera vilka åtgärder tillåts toobe utförs på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-477">In hello **Target Object Actions** field, you can globally filter what actions are allowed toobe performed on Azure AD.</span></span> <span data-ttu-id="0e3d2-478">**Skapa** och **uppdatering** är de vanligaste.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-478">**Create** and **Update** are most common.</span></span>

4.  <span data-ttu-id="0e3d2-479">I hello **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappa tooActive katalogattribut.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-479">In hello **Attribute mappings** section, you can define how individual Workday attributes map tooActive Directory attributes.</span></span>

5. <span data-ttu-id="0e3d2-480">Klicka på ett befintligt attribut mappning tooupdate den, eller klicka **Lägg till ny mappning** längst hello hello skärmen tooadd nya mappningar.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-480">Click on an existing attribute mapping tooupdate it, or click **Add new mapping** at hello bottom of hello screen tooadd new mappings.</span></span> <span data-ttu-id="0e3d2-481">En enskild attributmappning stöder dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-481">An individual attribute mapping supports these properties:</span></span>

   * <span data-ttu-id="0e3d2-482">**Avbildningstyp**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-482">**Mapping Type**</span></span>

      * <span data-ttu-id="0e3d2-483">**Direkt** – skriver hello värdet för hello Workday attributet toohello AD-attributet, utan ändringar</span><span class="sxs-lookup"><span data-stu-id="0e3d2-483">**Direct** – Writes hello value of hello Workday attribute         toohello AD attribute, with no changes</span></span>

      * <span data-ttu-id="0e3d2-484">**Konstant** -skriva ett statiska, konstant strängvärde till hello AD-attribut</span><span class="sxs-lookup"><span data-stu-id="0e3d2-484">**Constant** - Write a static, constant string value to         hello AD attribute</span></span>

      * <span data-ttu-id="0e3d2-485">**Uttrycket** – kan du toowrite ett anpassat värde till hello AD-attribut, baserat på en eller flera Workday-attribut.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-485">**Expression** – Allows you toowrite a custom value to hello AD attribute, based on one or more Workday attributes.</span></span> <span data-ttu-id="0e3d2-486">[Mer information finns i den här artikeln på uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-486">[For more info, see this article on expressions](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>

   * <span data-ttu-id="0e3d2-487">**Källattributet** -hello användarattribut från Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-487">**Source attribute** - hello user attribute from Workday.</span></span>

   * <span data-ttu-id="0e3d2-488">**Standardvärde** – det är valfritt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-488">**Default value** – Optional.</span></span> <span data-ttu-id="0e3d2-489">Om hello källattribut har ett tomt värde, skrivs hello mappningen det här värdet i stället.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-489">If hello source attribute has an empty value, hello mapping will write this value instead.</span></span>
            <span data-ttu-id="0e3d2-490">De vanligaste konfigurationen är tooleave detta tomt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-490">Most common configuration is tooleave this blank.</span></span>

   * <span data-ttu-id="0e3d2-491">**Målattribut** – hello användarattribut i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-491">**Target attribute** – hello user attribute in Azure AD.</span></span>

   * <span data-ttu-id="0e3d2-492">**Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen används toouniquely identifiera användare mellan Workday och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-492">**Match objects using this attribute** – Whether or not this mapping should be used toouniquely identify users between Workday and Azure AD.</span></span> <span data-ttu-id="0e3d2-493">Detta anges normalt Worker ID-fältet för Workday som vanligtvis är mappad till hello anställnings-ID-attributet (nya) eller ett tillägg-attributet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-493">This is typically set on the Worker ID field for Workday, which is typically mapped to hello Employee ID attribute (new) or an extension attribute in Azure AD.</span></span>

   * <span data-ttu-id="0e3d2-494">**Matchar prioritet** – flera matchande attribut kan anges.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-494">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="0e3d2-495">När det finns flera, utvärderas de i den ordning som anges av det här fältet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-495">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="0e3d2-496">När en matchning hittas matchar ingen ytterligare attribut utvärderas.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-496">As soon as a match is found, no further matching attributes are evaluated.</span></span>

   * <span data-ttu-id="0e3d2-497">**Använd den här mappningen**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-497">**Apply this mapping**</span></span>

     * <span data-ttu-id="0e3d2-498">**Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e3d2-498">**Always** – Apply this mapping on both user creation          and update actions</span></span>

     * <span data-ttu-id="0e3d2-499">**Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e3d2-499">**Only during creation** - Apply this mapping only on          user creation actions</span></span>

6. <span data-ttu-id="0e3d2-500">toosave kopplingar, klicka på **spara** hello överst i avsnittet attributmappning.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-500">toosave your mappings, click **Save** at hello top of the      Attribute Mapping section.</span></span>

### <a name="part-3-start-hello-service"></a><span data-ttu-id="0e3d2-501">Del 3: Start hello service</span><span class="sxs-lookup"><span data-stu-id="0e3d2-501">Part 3: Start hello service</span></span>
<span data-ttu-id="0e3d2-502">När du delar 1 – 2 har slutförts, kan du starta hello etableras.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-502">Once parts 1-2 have been completed, you can start hello provisioning service.</span></span>

1.  <span data-ttu-id="0e3d2-503">I hello **etablering** fliken, ange hello **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-503">In hello **Provisioning** tab, set hello **Provisioning Status** to **On**.</span></span>

2. <span data-ttu-id="0e3d2-504">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-504">Click **Save**.</span></span>

3. <span data-ttu-id="0e3d2-505">Detta startar hello inledande synkronisering, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-505">This will start hello initial sync, which can take a variable number  of hours depending on how many users are in Workday.</span></span>

4. <span data-ttu-id="0e3d2-506">Enskilda sync händelser kan visas i hello **granskningsloggar** fliken. **[Se hello etablering reporting vägledning för detaljerade anvisningar om hur tooread hello granskningsloggar](active-directory-saas-provisioning-reporting.md)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-506">Individual sync events can be viewed in hello **Audit Logs** tab. **[See hello provisioning reporting guide for detailed instructions on how tooread hello audit logs](active-directory-saas-provisioning-reporting.md)**</span></span>

5. <span data-ttu-id="0e3d2-507">En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-507">One completed, it will write an audit summary report in the  **Provisioning** tab, as shown below.</span></span>


## <a name="configuring-writeback-of-email-addresses-tooworkday"></a><span data-ttu-id="0e3d2-508">Konfigurera tillbakaskrivning av e-postadresser tooWorkday</span><span class="sxs-lookup"><span data-stu-id="0e3d2-508">Configuring writeback of email addresses tooWorkday</span></span>
<span data-ttu-id="0e3d2-509">Följ dessa instruktioner tooconfigure tillbakaskrivning av användare e-postadresser från Azure Active Directory tooWorkday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-509">Follow these instructions tooconfigure writeback of user email addresses from Azure Active Directory tooWorkday.</span></span>

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a><span data-ttu-id="0e3d2-510">Del 1: Lägga till hello etablering connector appen och skapa hello anslutning tooWorkday</span><span class="sxs-lookup"><span data-stu-id="0e3d2-510">Part 1: Adding hello provisioning connector app and creating hello connection tooWorkday</span></span>

<span data-ttu-id="0e3d2-511">**tooconfigure Workday tooActive Directory etablering:**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-511">**tooconfigure Workday tooActive Directory provisioning:**</span></span>

1.  <span data-ttu-id="0e3d2-512">Gå för<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="0e3d2-512">Go too<https://portal.azure.com></span></span>

2.  <span data-ttu-id="0e3d2-513">Välj i hello vänstra navigeringsfältet **Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-513">In hello left navigation bar, select **Azure Active Directory**</span></span>

3.  <span data-ttu-id="0e3d2-514">Välj **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-514">Select **Enterprise Applications**, then **All Applications**.</span></span>

4.  <span data-ttu-id="0e3d2-515">Välj **lägga till ett program**och välj hello **alla** kategori.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-515">Select **Add an application**, then select hello **All** category.</span></span>

5.  <span data-ttu-id="0e3d2-516">Sök efter **Workday tillbakaskrivning**, och Lägg till appen från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-516">Search for **Workday Writeback**, and add that app from hello gallery.</span></span>

6.  <span data-ttu-id="0e3d2-517">När hello app har lagts till och hello app information visas, Välj **etablering**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-517">After hello app is added and hello app details screen is shown, select **Provisioning**</span></span>

7.  <span data-ttu-id="0e3d2-518">Ändra hello **etablering** **läge** för**automatisk**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-518">Change hello **Provisioning** **Mode** too**Automatic**</span></span>

8.  <span data-ttu-id="0e3d2-519">Fullständig hello **administratörsautentiseringsuppgifter** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0e3d2-519">Complete hello **Admin Credentials** section as follows:</span></span>

   * <span data-ttu-id="0e3d2-520">**Admin Username** – ange hello användarnamnet för hello systemkontot för Workday-integrering med hello klient domännamnet tillagt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-520">**Admin Username** – Enter hello username of hello Workday integration system account, with hello tenant domain name appended.</span></span> <span data-ttu-id="0e3d2-521">Ska se ut ungefär:username@contoso4</span><span class="sxs-lookup"><span data-stu-id="0e3d2-521">Should look something like: username@contoso4</span></span>

   * <span data-ttu-id="0e3d2-522">**Administratörslösenordet –** ange hello lösenord för hello systemkontot för Workday-integrering</span><span class="sxs-lookup"><span data-stu-id="0e3d2-522">**Admin password –** Enter hello password of hello Workday    integration system account</span></span>

   * <span data-ttu-id="0e3d2-523">**Klient URL –** ange hello URL toohello Workday web services-slutpunkten för din klient.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-523">**Tenant URL –** Enter hello URL toohello Workday web services endpoint for your tenant.</span></span> <span data-ttu-id="0e3d2-524">Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med hello rätt miljö sträng (vid behov).</span><span class="sxs-lookup"><span data-stu-id="0e3d2-524">This should look like: https://wd3-impl-services1.workday.com/ccx/service/contoso4, where contoso4 is replaced with your correct tenant name and wd3-impl is replaced with hello correct environment string (if necessary).</span></span>

   * <span data-ttu-id="0e3d2-525">**E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-525">**Notification Email –** Enter your email address, and check the    “send email if failure occurs” checkbox.</span></span>

   * <span data-ttu-id="0e3d2-526">Klicka på hello **Testanslutningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-526">Click hello **Test Connection** button.</span></span> <span data-ttu-id="0e3d2-527">Om anslutningstestet hello lyckas klickar du på hello **spara** knappen hello överst.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-527">If hello connection test succeeds, click hello **Save** button at hello top.</span></span> <span data-ttu-id="0e3d2-528">Om det misslyckas, kontrollera att hello Workday-URL och autentiseringsuppgifter är giltiga i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-528">If it fails, double-check that hello Workday URL and credentials are valid in Workday.</span></span>


### <a name="part-2-configure-attribute-mappings"></a><span data-ttu-id="0e3d2-529">Del 2: Konfigurera attributmappning</span><span class="sxs-lookup"><span data-stu-id="0e3d2-529">Part 2: Configure attribute mappings</span></span> 


<span data-ttu-id="0e3d2-530">I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-530">In this section, you will configure how user data flows from Workday to Active Directory.</span></span>

1.  <span data-ttu-id="0e3d2-531">Hello etablering på fliken **mappningar**, klickar du på **synkronisera Azure AD-användare tooWorkday**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-531">On hello Provisioning tab under **Mappings**, click **Synchronize Azure AD Users tooWorkday**.</span></span>

2.  <span data-ttu-id="0e3d2-532">I hello **källa Objektområde** fält du kan du filtrera vilka uppsättningar med användare i Azure Active Directory ska ha sina e-postadresser skrivs tillbaka tooWorkday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-532">In hello **Source Object Scope** field, you can optionally filter which sets of users in Azure Active Directory should have their email addresses written back tooWorkday.</span></span> <span data-ttu-id="0e3d2-533">hello standardscope är ”alla användare i Azure AD”.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-533">hello default scope is “all users in Azure AD”.</span></span> 

3.  <span data-ttu-id="0e3d2-534">I hello **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappa tooActive katalogattribut.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-534">In hello **Attribute mappings** section, you can define how individual Workday attributes map tooActive Directory attributes.</span></span> <span data-ttu-id="0e3d2-535">Det finns en mappning för hello e-postadress som standard.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-535">There is a mapping for hello email address by default.</span></span> <span data-ttu-id="0e3d2-536">Hello matchande ID måste vara uppdaterade toomatch användare i Azure AD med sina motsvarande poster i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-536">However, hello matching ID must be updated toomatch users in Azure AD with their corresponding entries in Workday.</span></span> <span data-ttu-id="0e3d2-537">En populär matchande metod är toosynchronize hello Workday worker-ID eller medarbetare ID tooextensionAttribute1-15 i Azure AD och sedan använda det här attributet toomatch Azure AD-användare i Workday.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-537">A popular matching method is toosynchronize hello Workday worker ID or employee ID tooextensionAttribute1-15 in Azure AD, and then use this attribute in Azure AD toomatch users back in Workday.</span></span>

4.  <span data-ttu-id="0e3d2-538">toosave kopplingar, klicka på **spara** hello överst i hello attributmappning avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-538">toosave your mappings, click **Save** at hello top of hello Attribute Mapping section.</span></span>

### <a name="part-3-start-hello-service"></a><span data-ttu-id="0e3d2-539">Del 3: Start hello service</span><span class="sxs-lookup"><span data-stu-id="0e3d2-539">Part 3: Start hello service</span></span>
<span data-ttu-id="0e3d2-540">När du delar 1 – 2 har slutförts, kan du starta hello etableras.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-540">Once parts 1-2 have been completed, you can start hello provisioning service.</span></span>

1.  <span data-ttu-id="0e3d2-541">I hello **etablering** fliken, ange hello **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-541">In hello **Provisioning** tab, set hello **Provisioning Status** to **On**.</span></span>

2. <span data-ttu-id="0e3d2-542">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-542">Click **Save**.</span></span>

3. <span data-ttu-id="0e3d2-543">Detta startar hello inledande synkronisering, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-543">This will start hello initial sync, which can take a variable number  of hours depending on how many users are in Workday.</span></span>

4. <span data-ttu-id="0e3d2-544">Enskilda sync händelser kan visas i hello **granskningsloggar** fliken. **[Se hello etablering reporting vägledning för detaljerade anvisningar om hur tooread hello granskningsloggar](active-directory-saas-provisioning-reporting.md)**</span><span class="sxs-lookup"><span data-stu-id="0e3d2-544">Individual sync events can be viewed in hello **Audit Logs** tab. **[See hello provisioning reporting guide for detailed instructions on how tooread hello audit logs](active-directory-saas-provisioning-reporting.md)**</span></span>

5. <span data-ttu-id="0e3d2-545">En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-545">One completed, it will write an audit summary report in the  **Provisioning** tab, as shown below.</span></span>

## <a name="known-issues"></a><span data-ttu-id="0e3d2-546">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0e3d2-546">Known issues</span></span>

* <span data-ttu-id="0e3d2-547">**Granskningsloggar i europeiska språk** - hello-versionen av den här tekniska förhandsversionen är ett känt problem med hello [granskningsloggar](active-directory-saas-provisioning-reporting.md) för hello Workday connector appar visas inte i hello [Azure-portalen](https://portal.azure.com) om hello Azure AD-klient finns i ett Europeiska datacenter.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-547">**Audit logs in European locales** - As of hello release of this technical preview, there is a known issue with hello [audit logs](active-directory-saas-provisioning-reporting.md) for hello Workday connector apps not appearing in hello [Azure portal](https://portal.azure.com) if hello Azure AD tenant resides in a European data center.</span></span> <span data-ttu-id="0e3d2-548">En korrigering för problemet är kommande.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-548">A fix for this issue is forthcoming.</span></span> <span data-ttu-id="0e3d2-549">Kontrollera blanksteg igen hello nära framtiden för uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="0e3d2-549">Please check this space again in hello near future for updates.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0e3d2-550">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0e3d2-550">Additional resources</span></span>
* [<span data-ttu-id="0e3d2-551">Självstudier: Konfigurera enkel inloggning mellan Workday och Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e3d2-551">Tutorial: Configuring single sign-on between Workday and Azure Active Directory</span></span>](active-directory-saas-workday-tutorial.md)
* [<span data-ttu-id="0e3d2-552">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e3d2-552">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0e3d2-553">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0e3d2-553">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="0e3d2-554">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e3d2-554">Next steps</span></span>

* [<span data-ttu-id="0e3d2-555">Lär dig hur tooreview loggar och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="0e3d2-555">Learn how tooreview logs and get reports on provisioning activity</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
