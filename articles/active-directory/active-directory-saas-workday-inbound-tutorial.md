---
title: "Självstudier: Konfigurera Workday för automatisk användaretablering med lokala Active Directory och Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du använder Workday som datakälla identitet för Active Directory och Azure Active Directory."
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
ms.openlocfilehash: f9cc94ca1fc44d10af19debab49435b265bf6e7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a><span data-ttu-id="62300-103">Självstudier: Konfigurera Workday för automatisk användaretablering med lokala Active Directory och Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62300-103">Tutorial: Configure Workday for automatic user provisioning with on-premises Active Directory and Azure Active Directory</span></span>
<span data-ttu-id="62300-104">Syftet med den här kursen är att visa de steg som du behöver utföra för att importera personer från Workday till Azure Active Directory- och Active Directory-med valfritt tillbakaskrivning av vissa attribut till Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-104">The objective of this tutorial is to show you the steps you need to perform to import people from Workday into both Active Directory and Azure Active Directory, with optional writeback of some attributes to Workday.</span></span> 



## <a name="overview"></a><span data-ttu-id="62300-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="62300-105">Overview</span></span>

<span data-ttu-id="62300-106">Den [Azure Active Directory-användare som etableras](active-directory-saas-app-provisioning.md) kan integreras med den [Workday personal API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) för att kunna etablera användarkonton.</span><span class="sxs-lookup"><span data-stu-id="62300-106">The [Azure Active Directory user provisioning service](active-directory-saas-app-provisioning.md) integrates with the [Workday Human Resources API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) in order to provision user accounts.</span></span> <span data-ttu-id="62300-107">Azure AD använder den här anslutningen för att aktivera etablering arbetsflöden följande användare:</span><span class="sxs-lookup"><span data-stu-id="62300-107">Azure AD uses this connection to enable the following user provisioning workflows:</span></span>

* <span data-ttu-id="62300-108">**Användare i Active Directory-etablering** -Synkronisera markerade uppsättningar med användare från Workday till en eller flera Active Directory-skogar.</span><span class="sxs-lookup"><span data-stu-id="62300-108">**Provisioning users to Active Directory** - Synchronize selected sets of users from Workday into one or more Active Directory forests.</span></span> 

* <span data-ttu-id="62300-109">**Endast molnbaserad användare till Azure Active Directory-etablering** -Hybrid-användare som finns i både Active Directory och Azure Active Directory kan etableras till senare med [AAD Connect](connect/active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="62300-109">**Provisioning cloud-only users to Azure Active Directory** - Hybrid users who exist in both Active Directory and Azure Active Directory can be provisioned into the latter using [AAD Connect](connect/active-directory-aadconnect.md).</span></span> <span data-ttu-id="62300-110">Användare som är molnbaserad kan dock etableras direkt från Workday till Azure Active Directory med Azure AD-tjänsten för användaretablering.</span><span class="sxs-lookup"><span data-stu-id="62300-110">However, users that are cloud-only can be provisioned directly from Workday to Azure Active Directory using the Azure AD user provisioning service.</span></span>

* <span data-ttu-id="62300-111">**Tillbakaskrivning av e-post-adresser till Workday** -Azure AD etableras kan skriva vald användarattribut för Azure AD tillbaka till Workday, till exempel e-postadress.</span><span class="sxs-lookup"><span data-stu-id="62300-111">**Writeback of email addresses to Workday** - the Azure AD user provisioning service can write selected Azure AD user attributes back to Workday, such as the email address.</span></span>

### <a name="scenarios-covered"></a><span data-ttu-id="62300-112">Scenarier som tas upp</span><span class="sxs-lookup"><span data-stu-id="62300-112">Scenarios covered</span></span>

<span data-ttu-id="62300-113">Arbetsflöden som stöds av Azure AD-användaren etablering tjänsten Workday användaretablering kan automatisering av följande personal och identitet livscykel scenarier:</span><span class="sxs-lookup"><span data-stu-id="62300-113">The Workday user provisioning workflows supported by the Azure AD user provisioning service enables automation of the following human resources and identity lifecycle management scenarios:</span></span>

* <span data-ttu-id="62300-114">**Uthyrning nyanställda** – när en ny medarbetare har lagts till i Workday, ett konto skapas automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md), vid skrivning baksidan av e-postadressen till Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-114">**Hiring new employees** - When a new employee is added to Workday, a user account will be automatically created in Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md), with write back of the email address to Workday.</span></span>

* <span data-ttu-id="62300-115">**Medarbetare-attribut och profilen uppdateringar** – när en anställd uppdateras i Workday (till exempel deras namn, avdelning eller manager), sitt användarkonto uppdateras automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="62300-115">**Employee attribute and profile updates** - When an employee record is updated in Workday (such as their name, title, or manager), their user account will be automatically updated in Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md).</span></span>

* <span data-ttu-id="62300-116">**Medarbetare uppsägningar** – när en anställd avslutas i arbetsdagen, sitt användarkonto inaktiveras automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="62300-116">**Employee terminations** - When an employee is terminated in Workday, their user account is automatically disabled in Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md).</span></span>

* <span data-ttu-id="62300-117">**Medarbetare nytt anlitar** – när en anställd rehired i Workday, sina gamla kontot kan automatiskt återaktivera eller etablerats igen (beroende på dina inställningar) till Active Directory, Azure Active Directory, och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="62300-117">**Employee re-hires** - When an employee is rehired in Workday, their old account can be automatically reactivated or re-provisioned (depending on your preference) to Active Directory, Azure Active Directory, and optionally Office 365 and [other SaaS applications supported by Azure AD](active-directory-saas-app-provisioning.md).</span></span>


## <a name="planning-your-solution"></a><span data-ttu-id="62300-118">Planerar din lösning</span><span class="sxs-lookup"><span data-stu-id="62300-118">Planning your solution</span></span>

<span data-ttu-id="62300-119">Kontrollera krav nedan och Läs följande riktlinjer om hur du matchar din aktuella Active Directory-arkitektur och krav för användaretablering med solution(s) som tillhandahålls av Azure Active Directory innan du börjar din Workday-integrering.</span><span class="sxs-lookup"><span data-stu-id="62300-119">Before beginning your Workday integration, check the prerequisites below and read the following guidance on how to match your current Active Directory architecture and user provisioning requirements with the solution(s) provided by Azure Active Directory.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="62300-120">Krav</span><span class="sxs-lookup"><span data-stu-id="62300-120">Prerequisites</span></span>

<span data-ttu-id="62300-121">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="62300-121">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="62300-122">En giltig Azure AD Premium P1-prenumeration med global administratörsbehörighet</span><span class="sxs-lookup"><span data-stu-id="62300-122">A valid Azure AD Premium P1 subscription with global administrator access</span></span>
* <span data-ttu-id="62300-123">En klient för implementering av Workday för testning och integrering</span><span class="sxs-lookup"><span data-stu-id="62300-123">A Workday implementation tenant for testing and integration purposes</span></span>
* <span data-ttu-id="62300-124">Administratörsbehörighet i Workday för att skapa en användare för integration av system och göra ändringar på testdata medarbetare för testning</span><span class="sxs-lookup"><span data-stu-id="62300-124">Administrator permissions in Workday to create a system integration user, and make changes to test employee data for testing purposes</span></span>
* <span data-ttu-id="62300-125">För användaretablering till Active Directory, en domänansluten server som kör tjänsten Windows 2012 eller senare krävs för att värden i [lokalt lösenordssynkroniseringsagenten](https://go.microsoft.com/fwlink/?linkid=847801)</span><span class="sxs-lookup"><span data-stu-id="62300-125">For user provisioning to Active Directory, a domain-joined server running Windows Service 2012 or greater is required to host the [on-premises synchronization agent](https://go.microsoft.com/fwlink/?linkid=847801)</span></span>
* <span data-ttu-id="62300-126">[Azure AD Connect](connect/active-directory-aadconnect.md) för synkronisering mellan Active Directory och Azure AD</span><span class="sxs-lookup"><span data-stu-id="62300-126">[Azure AD Connect](connect/active-directory-aadconnect.md) for synchronizing between Active Directory and Azure AD</span></span>

> [!NOTE]
> <span data-ttu-id="62300-127">Om Azure AD-klienten finns i Europa, finns det [kända problem](#known-issues) nedan.</span><span class="sxs-lookup"><span data-stu-id="62300-127">If your Azure AD tenant is located in Europe, please see the [Known issues](#known-issues) section below.</span></span>


### <a name="solution-architecture"></a><span data-ttu-id="62300-128">Lösningsarkitektur</span><span class="sxs-lookup"><span data-stu-id="62300-128">Solution architecture</span></span>

<span data-ttu-id="62300-129">Azure AD innehåller en omfattande uppsättning etablering kopplingar för att lösa etablering och livscykel Identitetshantering från Workday till Active Directory, Azure AD, SaaS-appar och framåt.</span><span class="sxs-lookup"><span data-stu-id="62300-129">Azure AD provides a rich set of provisioning connectors to help you solve provisioning and identity lifecycle management from Workday to Active Directory, Azure AD, SaaS apps, and beyond.</span></span> <span data-ttu-id="62300-130">Vilka funktioner du använder och hur du ställer in lösningen varierar beroende på din organisations miljö och behov.</span><span class="sxs-lookup"><span data-stu-id="62300-130">Which features you will use and how you set up the solution will vary depending on your organization's environment and requirements.</span></span> <span data-ttu-id="62300-131">Går igenom hur många av följande är närvarande och distribuerade i din organisation som ett första steg:</span><span class="sxs-lookup"><span data-stu-id="62300-131">As a first step, take stock of how many of the following are present and deployed in your organization:</span></span>

* <span data-ttu-id="62300-132">Hur många Active Directory-skogar finns i använda?</span><span class="sxs-lookup"><span data-stu-id="62300-132">How many Active Directory Forests are in use?</span></span>
* <span data-ttu-id="62300-133">Hur många Active Directory-domäner finns i använda?</span><span class="sxs-lookup"><span data-stu-id="62300-133">How many Active Directory Domains are in use?</span></span>
* <span data-ttu-id="62300-134">Hur många Active Directory organisationsenheter (OU) används?</span><span class="sxs-lookup"><span data-stu-id="62300-134">How many Active Directory Organizational Units (OUs) are in use?</span></span>
* <span data-ttu-id="62300-135">Hur många Azure Active Directory-klienter finns i använda?</span><span class="sxs-lookup"><span data-stu-id="62300-135">How many Azure Active Directory tenants are in use?</span></span>
* <span data-ttu-id="62300-136">Finns det användare som behöver etableras både Active Directory och Azure Active Directory (t.ex. ”hybrid” användare)?</span><span class="sxs-lookup"><span data-stu-id="62300-136">Are there users who need to be provisioned to both Active Directory and Azure Active Directory (e.g. "hybrid" users)?</span></span>
* <span data-ttu-id="62300-137">Finns det användare som ska etableras på Azure Active Directory, men inte Active Directory (t.ex. ”endast molnbaserad” användare)?</span><span class="sxs-lookup"><span data-stu-id="62300-137">Are there users who need to be provisioned to Azure Active Directory, but not Active Directory (e.g. "cloud-only" users)?</span></span>
* <span data-ttu-id="62300-138">Behöver användare e-postadresser som ska skrivas tillbaka till Workday?</span><span class="sxs-lookup"><span data-stu-id="62300-138">Do user email addresses need to be written back to Workday?</span></span>

<span data-ttu-id="62300-139">När du har svaren på dessa frågor kan planera du din arbetsdag etablering distributionen genom att följa anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="62300-139">Once you have answers to these questions, you can plan your Workday provisioning deployment by following the guidance below.</span></span>

#### <a name="using-provisioning-connector-apps"></a><span data-ttu-id="62300-140">Använda etablering connector appar</span><span class="sxs-lookup"><span data-stu-id="62300-140">Using provisioning connector apps</span></span>

<span data-ttu-id="62300-141">Azure Active Directory stöder förintegrerade etablering kopplingar för Workday och många andra SaaS-program.</span><span class="sxs-lookup"><span data-stu-id="62300-141">Azure Active Directory supports pre-integrated provisioning connectors for Workday and a large number of other SaaS applications.</span></span> 

<span data-ttu-id="62300-142">En allokering kontakt med en enda källsystemet API-gränssnitt och hjälper etableringsdata till ett enda mål-system.</span><span class="sxs-lookup"><span data-stu-id="62300-142">A single provisioning connector interfaces with the API of a single source system, and helps provision data to a single target system.</span></span> <span data-ttu-id="62300-143">De flesta etablering kopplingar som har stöd för Azure AD är för ett enda käll- och system (t.ex. Azure AD att ServiceNow) och kan konfigureras genom att lägga till appen i fråga från appgalleriet för Azure AD (t.ex. ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="62300-143">Most provisioning connectors that Azure AD supports are for a single source and target system (e.g. Azure AD to ServiceNow), and can be setup by simply adding the app in question from the Azure AD app gallery (e.g. ServiceNow).</span></span> 

<span data-ttu-id="62300-144">Det finns en 1: 1-relation mellan etablering koppling instanser och app-instanser i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="62300-144">There is a one-to-one relationship between provisioning connector instances and app instances in Azure AD:</span></span>

| <span data-ttu-id="62300-145">Källsystemet</span><span class="sxs-lookup"><span data-stu-id="62300-145">Source System</span></span> | <span data-ttu-id="62300-146">Målsystem</span><span class="sxs-lookup"><span data-stu-id="62300-146">Target System</span></span> |
| ---------- | ---------- | 
| <span data-ttu-id="62300-147">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="62300-147">Azure AD tenant</span></span> | <span data-ttu-id="62300-148">SaaS-program</span><span class="sxs-lookup"><span data-stu-id="62300-148">SaaS application</span></span> |


<span data-ttu-id="62300-149">När du arbetar med Workday och Active Directory, finns det dock flera käll- och system för att ses som:</span><span class="sxs-lookup"><span data-stu-id="62300-149">However, when working with Workday and Active Directory, there are multiple source and target systems to be considered:</span></span>

| <span data-ttu-id="62300-150">Källsystemet</span><span class="sxs-lookup"><span data-stu-id="62300-150">Source System</span></span> | <span data-ttu-id="62300-151">Målsystem</span><span class="sxs-lookup"><span data-stu-id="62300-151">Target System</span></span> | <span data-ttu-id="62300-152">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="62300-152">Notes</span></span> |
| ---------- | ---------- | ---------- |
| <span data-ttu-id="62300-153">Arbetsdagar</span><span class="sxs-lookup"><span data-stu-id="62300-153">Workday</span></span> | <span data-ttu-id="62300-154">Active Directory-skog</span><span class="sxs-lookup"><span data-stu-id="62300-154">Active Directory Forest</span></span> | <span data-ttu-id="62300-155">Varje skog behandlas som ett distinkta målsystem</span><span class="sxs-lookup"><span data-stu-id="62300-155">Each forest is treated as a distinct target system</span></span> |
| <span data-ttu-id="62300-156">Arbetsdagar</span><span class="sxs-lookup"><span data-stu-id="62300-156">Workday</span></span> | <span data-ttu-id="62300-157">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="62300-157">Azure AD tenant</span></span> | <span data-ttu-id="62300-158">Som krävs för endast molnbaserad användare</span><span class="sxs-lookup"><span data-stu-id="62300-158">As required for cloud-only users</span></span> |
| <span data-ttu-id="62300-159">Active Directory-skog</span><span class="sxs-lookup"><span data-stu-id="62300-159">Active Directory Forest</span></span> | <span data-ttu-id="62300-160">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="62300-160">Azure AD tenant</span></span> | <span data-ttu-id="62300-161">Detta flöde hanteras av AAD Connect idag</span><span class="sxs-lookup"><span data-stu-id="62300-161">This flow is handled by AAD Connect today</span></span> |
| <span data-ttu-id="62300-162">Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="62300-162">Azure AD tenant</span></span> | <span data-ttu-id="62300-163">Arbetsdagar</span><span class="sxs-lookup"><span data-stu-id="62300-163">Workday</span></span> | <span data-ttu-id="62300-164">För tillbakaskrivning av e-postadresser</span><span class="sxs-lookup"><span data-stu-id="62300-164">For writeback of email addresses</span></span> |

<span data-ttu-id="62300-165">För att underlätta dessa flera olika arbetsflöden på flera datorer för käll- och innehåller Azure AD flera etablering connector-appar som du kan lägga till från appgalleriet för Azure AD:</span><span class="sxs-lookup"><span data-stu-id="62300-165">To facilitate these multiple workflows to multiple source and target systems, Azure AD provides multiple provisioning connector apps that you can add from the Azure AD app gallery:</span></span>

![AAD App-galleriet](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* <span data-ttu-id="62300-167">**Arbetsdagar Active Directory-etablering** -den här appen förenklar konto användarförsörjning från Workday till en Active Directory-skog.</span><span class="sxs-lookup"><span data-stu-id="62300-167">**Workday to Active Directory Provisioning** - This app facilitates user account provisioning from Workday to a single Active Directory forest.</span></span> <span data-ttu-id="62300-168">Om du har flera skogar, kan du lägga till en instans av den här appen från Azure AD app-galleriet för varje Active Directory-skog måste du etablera till.</span><span class="sxs-lookup"><span data-stu-id="62300-168">If you have multiple forests, you can add one instance of this app from the Azure AD app gallery for each Active Directory forest you need to provision to.</span></span>

* <span data-ttu-id="62300-169">**Arbetsdagar till Azure AD-etablering** - medan AAD Connect är ett verktyg som ska användas för att synkronisera Active Directory användare till Azure Active Directory, den här appen kan användas för att underlätta etablering av endast molnbaserad användare från Workday till en enda Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="62300-169">**Workday to Azure AD Provisioning** - While AAD Connect is the tool that should be used to synchronize Active Directory users to Azure Active Directory, this app can be used to facilitate provisioning of cloud-only users from Workday to a single Azure Active Directory tenant.</span></span>

* <span data-ttu-id="62300-170">**Arbetsdagar tillbakaskrivning** -den här appen förenklar tillbakaskrivning av användarens e-postadresser från Azure Active Directory till Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-170">**Workday Writeback** - This app facilitates writeback of user's email addresses from Azure Active Directory to Workday.</span></span>

> [!TIP]
> <span data-ttu-id="62300-171">Vanliga ”Workday” appen används för att konfigurera enkel inloggning mellan Workday och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-171">The regular "Workday" app is used for setting up single sign-on between Workday and Azure Active Directory.</span></span> 

<span data-ttu-id="62300-172">Hur du skapar och konfigurerar apparna särskilda etablering connector är föremål för de återstående avsnitten i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="62300-172">How to set up and configure these special provisioning connector apps is the subject of the remaining sections of this tutorial.</span></span> <span data-ttu-id="62300-173">Vilka appar som du vill konfigurera beror på vilka system måste du etablera till, och hur många Active Directory-skogar och Azure AD klienter finns i din miljö.</span><span class="sxs-lookup"><span data-stu-id="62300-173">Which apps you choose to configure will depend on which systems you need to provision to, and how many Active Directory Forests and Azure AD tenants are in your environment.</span></span>

![Översikt](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a><span data-ttu-id="62300-175">Konfigurera en systemanvändare integrering i Workday</span><span class="sxs-lookup"><span data-stu-id="62300-175">Configure a system integration user in Workday</span></span>
<span data-ttu-id="62300-176">Ett vanligt krav på alla Workday etablering kopplingar är de kräver autentiseringsuppgifter för ett Workday-integrering systemkonto att ansluta till Workday personal API.</span><span class="sxs-lookup"><span data-stu-id="62300-176">A common requirement of all the Workday provisioning connectors is they require credentials for a Workday system integration account to connect to the Workday Human Resources API.</span></span> <span data-ttu-id="62300-177">Det här avsnittet beskrivs hur du skapar ett konto för system integrator i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-177">This section describes how to create a system integrator account in Workday.</span></span>

> [!NOTE]
> <span data-ttu-id="62300-178">Det är möjligt att kringgå den här proceduren och i stället använda ett globalt administratörskonto för Workday som systemkonto för integrering.</span><span class="sxs-lookup"><span data-stu-id="62300-178">It is possible to bypass this procedure and instead use a Workday global administrator account as the system integration account.</span></span> <span data-ttu-id="62300-179">Detta fungerar bra för demonstrationer, men det rekommenderas inte för Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="62300-179">This may work fine for demos, but is not recommended for production deployments.</span></span>

### <a name="create-an-integration-system-user"></a><span data-ttu-id="62300-180">Skapa en integration system-användare</span><span class="sxs-lookup"><span data-stu-id="62300-180">Create an integration system user</span></span>

<span data-ttu-id="62300-181">**Skapa en integration systemanvändare:**</span><span class="sxs-lookup"><span data-stu-id="62300-181">**To create an integration system user:**</span></span>

1. <span data-ttu-id="62300-182">Logga in på din Workday-klient med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="62300-182">Sign into your Workday tenant using an administrator account.</span></span> <span data-ttu-id="62300-183">I den **Workday arbetsstationen**, ange skapa användare i sökrutan och klicka sedan på **skapa Integration systemanvändare**.</span><span class="sxs-lookup"><span data-stu-id="62300-183">In the **Workday Workbench**, enter create user in the search box, and then click **Create Integration System User**.</span></span> 
   
    <span data-ttu-id="62300-184">![Skapa användare](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="62300-184">![Create user](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Create user")</span></span>
2. <span data-ttu-id="62300-185">Slutför den **skapa Integration systemanvändare** aktiviteten genom att ange ett användarnamn och lösenord för en ny Integration systemanvändare.</span><span class="sxs-lookup"><span data-stu-id="62300-185">Complete the **Create Integration System User** task by supplying a user name and password for a new Integration System User.</span></span>  
 * <span data-ttu-id="62300-186">Lämna den **kräver nya lösenord vid nästa inloggning** alternativet är avmarkerat, eftersom den här användaren loggar in via programmering.</span><span class="sxs-lookup"><span data-stu-id="62300-186">Leave the **Require New Password at Next Sign In** option unchecked, because this user will be logging on programmatically.</span></span> 
 * <span data-ttu-id="62300-187">Lämna den **antal minuter för sessionens Timeout** med standardvärdet 0, vilket förhindrar att användarens sessioner timeout för tidigt.</span><span class="sxs-lookup"><span data-stu-id="62300-187">Leave the **Session Timeout Minutes** with its default value of 0, which will prevent the user’s sessions from timing out prematurely.</span></span> 
   
    <span data-ttu-id="62300-188">![Skapa Integration systemanvändare](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "skapa Integration systemanvändare")</span><span class="sxs-lookup"><span data-stu-id="62300-188">![Create Integration System User](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Create Integration System User")</span></span>

### <a name="create-a-security-group"></a><span data-ttu-id="62300-189">Skapa en säkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="62300-189">Create a security group</span></span>
<span data-ttu-id="62300-190">Du måste skapa en säkerhetsgrupp för obegränsad integration system och tilldela användaren till den.</span><span class="sxs-lookup"><span data-stu-id="62300-190">You need to create an unconstrained integration system security group and assign the user to it.</span></span>

<span data-ttu-id="62300-191">**Skapa en säkerhetsgrupp:**</span><span class="sxs-lookup"><span data-stu-id="62300-191">**To create a security group:**</span></span>

1. <span data-ttu-id="62300-192">Ange skapa säkerhetsgrupp i sökrutan och klicka sedan på **skapa säkerhetsgruppen**.</span><span class="sxs-lookup"><span data-stu-id="62300-192">Enter create security group in the search box, and then click **Create Security Group**.</span></span> 
   
    <span data-ttu-id="62300-193">![CreateSecurity grupp](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity grupp")</span><span class="sxs-lookup"><span data-stu-id="62300-193">![CreateSecurity Group](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity Group")</span></span>
2. <span data-ttu-id="62300-194">Slutför den **skapa säkerhetsgrupp** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="62300-194">Complete the **Create Security Group** task.</span></span>  
3. <span data-ttu-id="62300-195">Välj Integration Systemsäkerhetsgrupp – obegränsad från den **typ av innehavare säkerhetsgrupp** listrutan.</span><span class="sxs-lookup"><span data-stu-id="62300-195">Select Integration System Security Group—Unconstrained from the **Type of Tenanted Security Group** dropdown.</span></span>
4. <span data-ttu-id="62300-196">Skapa en säkerhetsgrupp som medlemmar uttryckligen läggs.</span><span class="sxs-lookup"><span data-stu-id="62300-196">Create a security group to which members will be explicitly added.</span></span> 
   
    <span data-ttu-id="62300-197">![CreateSecurity grupp](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity grupp")</span><span class="sxs-lookup"><span data-stu-id="62300-197">![CreateSecurity Group](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity Group")</span></span>

### <a name="assign-the-integration-system-user-to-the-security-group"></a><span data-ttu-id="62300-198">Tilldela säkerhetsgruppen systemanvändaren integrering</span><span class="sxs-lookup"><span data-stu-id="62300-198">Assign the integration system user to the security group</span></span>

<span data-ttu-id="62300-199">**Så här tilldelar systemanvändaren integrering:**</span><span class="sxs-lookup"><span data-stu-id="62300-199">**To assign the integration system user:**</span></span>

1. <span data-ttu-id="62300-200">Ange redigera säkerhetsgrupp i sökrutan och klicka sedan på **redigera säkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="62300-200">Enter edit security group in the search box, and then click **Edit Security Group**.</span></span> 
   
    <span data-ttu-id="62300-201">![Redigera säkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "redigera säkerhetsgrupp")</span><span class="sxs-lookup"><span data-stu-id="62300-201">![Edit Security Group](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Edit Security Group")</span></span>
2. <span data-ttu-id="62300-202">Söka efter och välj den nya säkerhetsgruppen för integrering av namn.</span><span class="sxs-lookup"><span data-stu-id="62300-202">Search for, and select the new integration security group by name.</span></span> 
   
    <span data-ttu-id="62300-203">![Redigera säkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "redigera säkerhetsgrupp")</span><span class="sxs-lookup"><span data-stu-id="62300-203">![Edit Security Group](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Edit Security Group")</span></span>
3. <span data-ttu-id="62300-204">Lägg till den nya integration systemanvändaren till den nya säkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="62300-204">Add the new integration system user to the new security group.</span></span> 
   
    <span data-ttu-id="62300-205">![Systemsäkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Systemsäkerhetsgrupp")</span><span class="sxs-lookup"><span data-stu-id="62300-205">![System Security Group](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "System Security Group")</span></span>  

### <a name="configure-security-group-options"></a><span data-ttu-id="62300-206">Konfigurera alternativ för säkerhet</span><span class="sxs-lookup"><span data-stu-id="62300-206">Configure security group options</span></span>
<span data-ttu-id="62300-207">I det här steget beviljar du ny grupp säkerhetsbehörigheter **hämta** och **placera** åtgärder på objekt som skyddas av säkerhetsprinciper för följande domän:</span><span class="sxs-lookup"><span data-stu-id="62300-207">In this step, you grant to the new security group permissions for **Get** and **Put** operations on the objects secured by the following domain security policies:</span></span>

* <span data-ttu-id="62300-208">Externa konto-etablering</span><span class="sxs-lookup"><span data-stu-id="62300-208">External Account Provisioning</span></span>
* <span data-ttu-id="62300-209">Worker Data: Public Worker rapporter</span><span class="sxs-lookup"><span data-stu-id="62300-209">Worker Data: Public Worker Reports</span></span>
* <span data-ttu-id="62300-210">Worker Data: Alla positioner</span><span class="sxs-lookup"><span data-stu-id="62300-210">Worker Data: All Positions</span></span>
* <span data-ttu-id="62300-211">Worker Data: Current bemanning Information</span><span class="sxs-lookup"><span data-stu-id="62300-211">Worker Data: Current Staffing Information</span></span>
* <span data-ttu-id="62300-212">Worker Data: Business titel på Worker profil</span><span class="sxs-lookup"><span data-stu-id="62300-212">Worker Data: Business Title on Worker Profile</span></span>

<span data-ttu-id="62300-213">**Konfigurera alternativ för säkerhet:**</span><span class="sxs-lookup"><span data-stu-id="62300-213">**To configure security group options:**</span></span>

1. <span data-ttu-id="62300-214">Ange säkerhetsprinciper för domänen i sökrutan och klicka på länken **domän säkerhetsprinciper för funktionsområde**.</span><span class="sxs-lookup"><span data-stu-id="62300-214">Enter domain security policies in the search box, and then click on the link **Domain Security Policies for Functional Area**.</span></span>  
   
    <span data-ttu-id="62300-215">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="62300-215">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domain Security Policies")</span></span>  
2. <span data-ttu-id="62300-216">Sök efter system och välj den **System** funktionsområde.</span><span class="sxs-lookup"><span data-stu-id="62300-216">Search for system and select the **System** functional area.</span></span>  <span data-ttu-id="62300-217">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62300-217">Click **OK**.</span></span>  
   
    <span data-ttu-id="62300-218">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="62300-218">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domain Security Policies")</span></span>  
3. <span data-ttu-id="62300-219">I listan över säkerhetsprinciper för de funktionella området System, expanderar **säkerhetsadministration** och välj säkerhetsprincip för domän **externa Kontoetablering**.</span><span class="sxs-lookup"><span data-stu-id="62300-219">In the list of security policies for the System functional area, expand **Security Administration** and select the domain security policy **External Account Provisioning**.</span></span>  
   
    <span data-ttu-id="62300-220">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="62300-220">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domain Security Policies")</span></span>  
4. <span data-ttu-id="62300-221">Klicka på **Redigera behörigheter**, och klicka sedan på den **Redigera behörigheter**dialogrutan sidan lägger till den nya säkerhetsgruppen i listan säkerhetsgrupper med **hämta** och **placera**  integrering behörigheter.</span><span class="sxs-lookup"><span data-stu-id="62300-221">Click **Edit Permissions**, and then, on the **Edit Permissions**dialog page, add the new security group to the list of security groups with **Get** and **Put** integration permissions.</span></span> 
   
    <span data-ttu-id="62300-222">![Redigera behörighet](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "redigera behörighet")</span><span class="sxs-lookup"><span data-stu-id="62300-222">![Edit Permission](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Edit Permission")</span></span>  
5. <span data-ttu-id="62300-223">Upprepa steg 1 ovan för att återgå till skärmen för att välja huvudområden och nu, Sök efter personal, Välj den **bemanning funktionsområde** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62300-223">Repeat step 1 above to return to the screen for selecting functional areas, and this time, search for staffing, select the **Staffing functional area** and click **OK**.</span></span>
   
    <span data-ttu-id="62300-224">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="62300-224">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domain Security Policies")</span></span>  
6. <span data-ttu-id="62300-225">I listan över säkerhetsprinciper för funktionsområdet Staffing, expanderar **Worker Data: Staffing** och upprepa ovanstående steg 4 för varje återstående säkerhetsprinciper:</span><span class="sxs-lookup"><span data-stu-id="62300-225">In the list of security policies for the Staffing functional area, expand **Worker Data: Staffing** and repeat step 4 above for each of these remaining security policies:</span></span>

   * <span data-ttu-id="62300-226">Worker Data: Public Worker rapporter</span><span class="sxs-lookup"><span data-stu-id="62300-226">Worker Data: Public Worker Reports</span></span>
   * <span data-ttu-id="62300-227">Worker Data: Alla positioner</span><span class="sxs-lookup"><span data-stu-id="62300-227">Worker Data: All Positions</span></span>
   * <span data-ttu-id="62300-228">Worker Data: Current bemanning Information</span><span class="sxs-lookup"><span data-stu-id="62300-228">Worker Data: Current Staffing Information</span></span>
   * <span data-ttu-id="62300-229">Worker Data: Business titel på Worker profil</span><span class="sxs-lookup"><span data-stu-id="62300-229">Worker Data: Business Title on Worker Profile</span></span>
   
7. <span data-ttu-id="62300-230">Upprepa steg 1, ovan, för att återgå till skärmen för att välja funktionella områden och den här tiden kan söka efter **kontaktinformation**, Välj funktionsområdet Staffing och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62300-230">Repeat step 1, above, to return to the screen for selecting  functional areas, and this time, search for **Contact Information**,  select the Staffing functional area, and click **OK**.</span></span>

8.  <span data-ttu-id="62300-231">I listan över säkerhetsprinciper för funktionsområdet Staffing, expanderar **Worker Data: Kontakta arbetsinformation**, och upprepa steg 4 ovan för säkerhetsprinciper nedan:</span><span class="sxs-lookup"><span data-stu-id="62300-231">In the list of security policies for the Staffing functional area, expand **Worker Data: Work Contact Information**, and repeat step 4 above for the security policies below:</span></span>

    * <span data-ttu-id="62300-232">Worker Data: E-post</span><span class="sxs-lookup"><span data-stu-id="62300-232">Worker Data: Work Email</span></span>

    <span data-ttu-id="62300-233">![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "säkerhetsprinciper för domänen")</span><span class="sxs-lookup"><span data-stu-id="62300-233">![Domain Security Policies](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domain Security Policies")</span></span>  
    
### <a name="activate-security-policy-changes"></a><span data-ttu-id="62300-234">Aktivera ändringar i säkerhet</span><span class="sxs-lookup"><span data-stu-id="62300-234">Activate security policy changes</span></span>

<span data-ttu-id="62300-235">**Att aktivera ändringar i säkerhet:**</span><span class="sxs-lookup"><span data-stu-id="62300-235">**To activate security policy changes:**</span></span>

1. <span data-ttu-id="62300-236">Ange aktivera i sökrutan och klicka på länken **aktivera väntande ändringar i säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="62300-236">Enter activate in the search box, and then click on the link **Activate Pending Security Policy Changes**.</span></span> 
   
    <span data-ttu-id="62300-237">![Aktivera](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "aktivera")</span><span class="sxs-lookup"><span data-stu-id="62300-237">![Activate](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Activate")</span></span> 
2. <span data-ttu-id="62300-238">Börja aktiviteten aktivera väntande ändringar i säkerhet genom att ange en kommentar för granskningsändamål och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62300-238">Begin the Activate Pending Security Policy Changes task by entering a comment for auditing purposes, and then click **OK**.</span></span> 
   
    <span data-ttu-id="62300-239">![Aktivera väntande säkerhet](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "aktivera väntande säkerhet")</span><span class="sxs-lookup"><span data-stu-id="62300-239">![Activate Pending Security](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Activate Pending Security")</span></span>   
3. <span data-ttu-id="62300-240">Slutföra uppgiften på nästa skärm genom att markera kryssrutan **Bekräfta**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62300-240">Complete the task on the next screen by checking the checkbox **Confirm**, and then click **OK**.</span></span> 
   
    <span data-ttu-id="62300-241">![Aktivera väntande säkerhet](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "aktivera väntande säkerhet")</span><span class="sxs-lookup"><span data-stu-id="62300-241">![Activate Pending Security](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Activate Pending Security")</span></span>  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a><span data-ttu-id="62300-242">Konfigurering av användarförsörjning från Workday till Active Directory</span><span class="sxs-lookup"><span data-stu-id="62300-242">Configuring user provisioning from Workday to Active Directory</span></span>
<span data-ttu-id="62300-243">Följ instruktionerna för att konfigurera användarkonto etablering från Workday till varje Active Directory-skog som du behöver etablering till.</span><span class="sxs-lookup"><span data-stu-id="62300-243">Follow these instructions to configure user account provisioning from Workday to each Active Directory forest that you require provisioning to.</span></span>

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a><span data-ttu-id="62300-244">Del 1: Lägga till appen etablering koppling och skapa anslutningen till Workday</span><span class="sxs-lookup"><span data-stu-id="62300-244">Part 1: Adding the provisioning connector app and creating the connection to Workday</span></span>

<span data-ttu-id="62300-245">**Konfigurera Workday för Active Directory-etablering:**</span><span class="sxs-lookup"><span data-stu-id="62300-245">**To configure Workday to Active Directory provisioning:**</span></span>

1.  <span data-ttu-id="62300-246">Gå till <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="62300-246">Go to <https://portal.azure.com></span></span>

2.  <span data-ttu-id="62300-247">I det vänstra navigeringsfältet väljer **Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="62300-247">In the left navigation bar, select **Azure Active Directory**</span></span>

3.  <span data-ttu-id="62300-248">Välj **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="62300-248">Select **Enterprise Applications**, then **All Applications**.</span></span>

4.  <span data-ttu-id="62300-249">Välj **lägga till ett program**, och välj den **alla** kategori.</span><span class="sxs-lookup"><span data-stu-id="62300-249">Select **Add an application**, and select the **All** category.</span></span>

5.  <span data-ttu-id="62300-250">Sök efter **Workday etablering till Active Directory**, och Lägg till appen från galleriet.</span><span class="sxs-lookup"><span data-stu-id="62300-250">Search for **Workday Provisioning to Active Directory**, and add that app from the gallery.</span></span>

6.  <span data-ttu-id="62300-251">När appen har lagts till och skärmen app information är visas, väljer **etablering**</span><span class="sxs-lookup"><span data-stu-id="62300-251">After the app is added and the app details screen is shown, select **Provisioning**</span></span>

7.  <span data-ttu-id="62300-252">Ändra den **etablering** **läge** till **automatisk**</span><span class="sxs-lookup"><span data-stu-id="62300-252">Change the **Provisioning** **Mode** to **Automatic**</span></span>

8.  <span data-ttu-id="62300-253">Slutför den **administratörsautentiseringsuppgifter** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="62300-253">Complete the **Admin Credentials** section as follows:</span></span>

   * <span data-ttu-id="62300-254">**Admin Username** – ange användarnamnet för system-kontot Workday-integrering med klienten domännamnet tillagt.</span><span class="sxs-lookup"><span data-stu-id="62300-254">**Admin Username** – Enter the username of the Workday integration system account, with the tenant domain name appended.</span></span> <span data-ttu-id="62300-255">**Ska se ut ungefär:username@contoso4**</span><span class="sxs-lookup"><span data-stu-id="62300-255">**Should look something like: username@contoso4**</span></span>

   * <span data-ttu-id="62300-256">**Administratörslösenordet –** ange lösenordet för kontot Workday-integrering system</span><span class="sxs-lookup"><span data-stu-id="62300-256">**Admin password –** Enter the password of the Workday    integration system account</span></span>

   * <span data-ttu-id="62300-257">**Klient URL –** anger du Webbadressen till Workday web services-slutpunkten för din klient.</span><span class="sxs-lookup"><span data-stu-id="62300-257">**Tenant URL –** Enter the URL to the Workday web services endpoint for your tenant.</span></span> <span data-ttu-id="62300-258">Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med rätt miljö-sträng.</span><span class="sxs-lookup"><span data-stu-id="62300-258">This should look like: https://wd3-impl-services1.workday.com/ccx/service/contoso4, where contoso4 is replaced with your correct tenant name and wd3-impl is replaced with the correct environment string.</span></span>

   * <span data-ttu-id="62300-259">**Active Directory-skogar -** ”Name” för din Active Directory-skog, som returneras av powershell-kommandot Get-ADForest.</span><span class="sxs-lookup"><span data-stu-id="62300-259">**Active Directory Forest -** The “Name” of your Active Directory forest, as returned by the Get-ADForest powershell commandlet.</span></span> <span data-ttu-id="62300-260">Detta är vanligtvis en sträng som: *contoso.com*</span><span class="sxs-lookup"><span data-stu-id="62300-260">This is typically a string like: *contoso.com*</span></span>

   * <span data-ttu-id="62300-261">**Active Directory-behållare -** teckensträngen behållare, som innehåller alla användare i din AD-skog.</span><span class="sxs-lookup"><span data-stu-id="62300-261">**Active Directory Container -** Enter the container string that contains all users in your AD forest.</span></span> <span data-ttu-id="62300-262">Exempel: *OU = standardanvändare, OU = Users, DC = contoso, DC = test*</span><span class="sxs-lookup"><span data-stu-id="62300-262">Example: *OU=Standard Users,OU=Users,DC=contoso,DC=test*</span></span>

   * <span data-ttu-id="62300-263">**E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.</span><span class="sxs-lookup"><span data-stu-id="62300-263">**Notification Email –** Enter your email address, and check the    “send email if failure occurs” checkbox.</span></span>

   * <span data-ttu-id="62300-264">Klicka på den **Testanslutningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="62300-264">Click the **Test Connection** button.</span></span> <span data-ttu-id="62300-265">Om Anslutningstestet lyckas klickar du på den **spara** längst upp.</span><span class="sxs-lookup"><span data-stu-id="62300-265">If the connection test succeeds, click the **Save** button at the top.</span></span> <span data-ttu-id="62300-266">Om det misslyckas, kan du kontrollera att Workday-autentiseringsuppgifter är giltiga i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-266">If it fails, double-check that the Workday credentials are valid in Workday.</span></span> 

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a><span data-ttu-id="62300-268">Del 2: Konfigurera attributmappning</span><span class="sxs-lookup"><span data-stu-id="62300-268">Part 2: Configure attribute mappings</span></span> 

<span data-ttu-id="62300-269">I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-269">In this section, you will configure how user data flows from Workday to Active Directory.</span></span>

1.  <span data-ttu-id="62300-270">På fliken etablering under **mappningar**, klickar du på **synkronisera Workday anställda för OnPremises**.</span><span class="sxs-lookup"><span data-stu-id="62300-270">On the Provisioning tab under **Mappings**, click **Synchronize Workday Workers to OnPremises**.</span></span>

2.  <span data-ttu-id="62300-271">I den **källa Objektområde** fält, kan du välja vilka uppsättningar med användare i Workday ska vara i omfånget för etablering i AD, genom att definiera en uppsättning attributbaserad filter.</span><span class="sxs-lookup"><span data-stu-id="62300-271">In the **Source Object Scope** field, you can select which sets of users in Workday should be in scope for provisioning to AD, by defining a set of attribute-based filters.</span></span> <span data-ttu-id="62300-272">Standardvärde är ”alla användare i Workday”.</span><span class="sxs-lookup"><span data-stu-id="62300-272">The default scope is “all users in Workday”.</span></span> <span data-ttu-id="62300-273">Exempel filter:</span><span class="sxs-lookup"><span data-stu-id="62300-273">Example filters:</span></span>

   * <span data-ttu-id="62300-274">Exempel: Omfång för användare med Worker-ID: N mellan 1000000 och 2000000</span><span class="sxs-lookup"><span data-stu-id="62300-274">Example: Scope to users with Worker IDs between 1000000 and    2000000</span></span>

      * <span data-ttu-id="62300-275">Attributet: WorkerID</span><span class="sxs-lookup"><span data-stu-id="62300-275">Attribute: WorkerID</span></span>

      * <span data-ttu-id="62300-276">Operatorn: REGEX-matchning</span><span class="sxs-lookup"><span data-stu-id="62300-276">Operator: REGEX Match</span></span>

      * <span data-ttu-id="62300-277">Värde: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span><span class="sxs-lookup"><span data-stu-id="62300-277">Value: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span></span>

   * <span data-ttu-id="62300-278">Exempel: Endast anställda och inte contingent personer</span><span class="sxs-lookup"><span data-stu-id="62300-278">Example: Only employees and not contingent workers</span></span> 

      * <span data-ttu-id="62300-279">Attributet: EmployeeID</span><span class="sxs-lookup"><span data-stu-id="62300-279">Attribute: EmployeeID</span></span>

      * <span data-ttu-id="62300-280">Operatorn: Inte är NULL</span><span class="sxs-lookup"><span data-stu-id="62300-280">Operator: IS NOT NULL</span></span>

3.  <span data-ttu-id="62300-281">I den **mål objektåtgärder** fält du kan globalt filtrera vilka åtgärder ska kunna utföras på Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-281">In the **Target Object Actions** field, you can globally filter what actions are allowed to be performed on Active Directory.</span></span> <span data-ttu-id="62300-282">**Skapa** och **uppdatering** är de vanligaste.</span><span class="sxs-lookup"><span data-stu-id="62300-282">**Create** and **Update** are most common.</span></span>

4.  <span data-ttu-id="62300-283">I den **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappar till Active Directory-attribut.</span><span class="sxs-lookup"><span data-stu-id="62300-283">In the **Attribute mappings** section, you can define how individual Workday attributes map to Active Directory attributes.</span></span>

5. <span data-ttu-id="62300-284">Klicka på en befintlig attributmappning att uppdatera det, eller klicka på **Lägg till ny mappning** längst ned på skärmen för att lägga till nya mappningar.</span><span class="sxs-lookup"><span data-stu-id="62300-284">Click on an existing attribute mapping to update it, or click **Add new mapping** at the bottom of the screen to add new mappings.</span></span> <span data-ttu-id="62300-285">En enskild attributmappning stöder dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="62300-285">An individual attribute mapping supports these properties:</span></span>

      * <span data-ttu-id="62300-286">**Avbildningstyp**</span><span class="sxs-lookup"><span data-stu-id="62300-286">**Mapping Type**</span></span>

         * <span data-ttu-id="62300-287">**Direkt** – skriver värdet för Workday-attribut till attributet AD utan ändringar</span><span class="sxs-lookup"><span data-stu-id="62300-287">**Direct** – Writes the value of the Workday attribute      to the AD attribute, with no changes</span></span>

         * <span data-ttu-id="62300-288">**Konstant** -skriva ett statiska, konstant strängvärde till AD-attribut</span><span class="sxs-lookup"><span data-stu-id="62300-288">**Constant** - Write a static, constant string value to      the AD attribute</span></span>

         * <span data-ttu-id="62300-289">**Uttrycket** – du kan skriva ett anpassat värde till AD-attributet baserat på en eller flera Workday-attribut.</span><span class="sxs-lookup"><span data-stu-id="62300-289">**Expression** – Allows you to write a custom value to the AD attribute, based on one or more Workday attributes.</span></span> <span data-ttu-id="62300-290">[Mer information finns i den här artikeln på uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="62300-290">[For more info, see this article on expressions](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>

      * <span data-ttu-id="62300-291">**Källattributet** -användarattribut från Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-291">**Source attribute** - The user attribute from Workday.</span></span>

      * <span data-ttu-id="62300-292">**Standardvärde** – det är valfritt.</span><span class="sxs-lookup"><span data-stu-id="62300-292">**Default value** – Optional.</span></span> <span data-ttu-id="62300-293">Om källattributet har ett tomt värde, skrivs mappningen det här värdet i stället.</span><span class="sxs-lookup"><span data-stu-id="62300-293">If the source attribute has an empty value, the mapping will write this value instead.</span></span>
            <span data-ttu-id="62300-294">De vanligaste konfigurationen är att du lämnar fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="62300-294">Most common configuration is to leave this blank.</span></span>

      * <span data-ttu-id="62300-295">**Målattribut** – användarattribut i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-295">**Target attribute** – The user attribute in Active     Directory.</span></span>

      * <span data-ttu-id="62300-296">**Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen som ska användas för att unikt identifiera användarna mellan Workday och Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-296">**Match objects using this attribute** – Whether or not this mapping should be used to uniquely identify users between Workday and Active Directory.</span></span> <span data-ttu-id="62300-297">Detta anges normalt Worker ID-fältet för Workday som vanligtvis är mappad till en medarbetare ID-attribut i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-297">This is typically set on the Worker ID field for Workday, which is typically mapped to one of the Employee ID attributes in Active Directory.</span></span>

      * <span data-ttu-id="62300-298">**Matchar prioritet** – flera matchande attribut kan anges.</span><span class="sxs-lookup"><span data-stu-id="62300-298">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="62300-299">När det finns flera, utvärderas de i den ordning som anges av det här fältet.</span><span class="sxs-lookup"><span data-stu-id="62300-299">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="62300-300">När en matchning hittas matchar ingen ytterligare attribut utvärderas.</span><span class="sxs-lookup"><span data-stu-id="62300-300">As soon as a match is found, no further matching attributes are evaluated.</span></span>

      * <span data-ttu-id="62300-301">**Använd den här mappningen**</span><span class="sxs-lookup"><span data-stu-id="62300-301">**Apply this mapping**</span></span>
       
         * <span data-ttu-id="62300-302">**Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder</span><span class="sxs-lookup"><span data-stu-id="62300-302">**Always** – Apply this mapping on both user creation      and update actions</span></span>

         * <span data-ttu-id="62300-303">**Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder</span><span class="sxs-lookup"><span data-stu-id="62300-303">**Only during creation** - Apply this mapping only on      user creation actions</span></span>

6. <span data-ttu-id="62300-304">Om du vill spara dina mappningar klickar du på **spara** överst i avsnittet attributmappning.</span><span class="sxs-lookup"><span data-stu-id="62300-304">To save your mappings, click **Save** at the top of the      Attribute Mapping section.</span></span>

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

<span data-ttu-id="62300-306">**Här följer några exempel attributmappning mellan Workday och Active Directory med vissa vanliga uttryck**</span><span class="sxs-lookup"><span data-stu-id="62300-306">**Below are some example attribute mappings between Workday and Active Directory, with some common expressions**</span></span>

-   <span data-ttu-id="62300-307">Det uttryck som mappar till parentDistinguishedName AD-attributet kan användas för att etablera en användare till en viss Organisationsenhet baserat på en eller flera Workday källa attribut.</span><span class="sxs-lookup"><span data-stu-id="62300-307">The expression that maps to the parentDistinguishedName AD attribute can be used to provision a user to a specific OU based on one or more Workday source attributes.</span></span> <span data-ttu-id="62300-308">Det här exemplet placerar användare i olika organisationsenheter beroende på deras stad data i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-308">This example places users in different OUs depending on their city data in Workday.</span></span>

-   <span data-ttu-id="62300-309">Det uttryck som mappar till attributet userPrincipalName AD skapa ett UPN för firstName.LastName@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="62300-309">The expression that maps to the userPrincipalName AD attribute create a UPN of firstName.LastName@contoso.com.</span></span> <span data-ttu-id="62300-310">Den ersätter också ogiltiga specialtecken.</span><span class="sxs-lookup"><span data-stu-id="62300-310">It also replaces illegal special characters.</span></span>

-   [<span data-ttu-id="62300-311">Det finns dokumentationen om hur du skriver här uttryck</span><span class="sxs-lookup"><span data-stu-id="62300-311">There is documentation on writing expressions here</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| <span data-ttu-id="62300-312">ARBETSDAGAR ATTRIBUT</span><span class="sxs-lookup"><span data-stu-id="62300-312">WORKDAY ATTRIBUTE</span></span> | <span data-ttu-id="62300-313">ACTIVE DIRECTORY-ATTRIBUT</span><span class="sxs-lookup"><span data-stu-id="62300-313">ACTIVE DIRECTORY ATTRIBUTE</span></span> |  <span data-ttu-id="62300-314">MATCHANDE ID?</span><span class="sxs-lookup"><span data-stu-id="62300-314">MATCHING ID?</span></span> | <span data-ttu-id="62300-315">SKAPA / UPPDATERA</span><span class="sxs-lookup"><span data-stu-id="62300-315">CREATE / UPDATE</span></span> |
| ---------- | ---------- | ---------- | ---------- |
|  <span data-ttu-id="62300-316">**WorkerID**</span><span class="sxs-lookup"><span data-stu-id="62300-316">**WorkerID**</span></span>  |  <span data-ttu-id="62300-317">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="62300-317">EmployeeID</span></span> | <span data-ttu-id="62300-318">**Ja**</span><span class="sxs-lookup"><span data-stu-id="62300-318">**Yes**</span></span> | <span data-ttu-id="62300-319">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="62300-319">Written on create only</span></span> | 
|  <span data-ttu-id="62300-320">**Namnet**</span><span class="sxs-lookup"><span data-stu-id="62300-320">**Municipality**</span></span>   |   <span data-ttu-id="62300-321">L</span><span class="sxs-lookup"><span data-stu-id="62300-321">l</span></span>   |     | <span data-ttu-id="62300-322">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-322">Create + update</span></span> |
|  <span data-ttu-id="62300-323">**Företag**</span><span class="sxs-lookup"><span data-stu-id="62300-323">**Company**</span></span>         | <span data-ttu-id="62300-324">Företag</span><span class="sxs-lookup"><span data-stu-id="62300-324">company</span></span>   |     |  <span data-ttu-id="62300-325">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-325">Create + update</span></span> |
|  <span data-ttu-id="62300-326">**CountryReferenceTwoLetter**</span><span class="sxs-lookup"><span data-stu-id="62300-326">**CountryReferenceTwoLetter**</span></span>      |   <span data-ttu-id="62300-327">CO</span><span class="sxs-lookup"><span data-stu-id="62300-327">co</span></span> |     |   <span data-ttu-id="62300-328">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-328">Create + update</span></span> |
| <span data-ttu-id="62300-329">**CountryReferenceTwoLetter**</span><span class="sxs-lookup"><span data-stu-id="62300-329">**CountryReferenceTwoLetter**</span></span>    |  <span data-ttu-id="62300-330">C</span><span class="sxs-lookup"><span data-stu-id="62300-330">c</span></span>  |     |         <span data-ttu-id="62300-331">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-331">Create + update</span></span> |
| <span data-ttu-id="62300-332">**SupervisoryOrganization**</span><span class="sxs-lookup"><span data-stu-id="62300-332">**SupervisoryOrganization**</span></span>  | <span data-ttu-id="62300-333">Avdelning</span><span class="sxs-lookup"><span data-stu-id="62300-333">department</span></span>  |     |  <span data-ttu-id="62300-334">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-334">Create + update</span></span> |
|  <span data-ttu-id="62300-335">**PreferredNameData**</span><span class="sxs-lookup"><span data-stu-id="62300-335">**PreferredNameData**</span></span>  |  <span data-ttu-id="62300-336">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="62300-336">displayName</span></span> |     |   <span data-ttu-id="62300-337">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-337">Create + update</span></span> |
| <span data-ttu-id="62300-338">**EmployeeID**</span><span class="sxs-lookup"><span data-stu-id="62300-338">**EmployeeID**</span></span>    |  <span data-ttu-id="62300-339">CN</span><span class="sxs-lookup"><span data-stu-id="62300-339">cn</span></span>    |   |   <span data-ttu-id="62300-340">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="62300-340">Written on create only</span></span> |
| <span data-ttu-id="62300-341">**Fax**</span><span class="sxs-lookup"><span data-stu-id="62300-341">**Fax**</span></span>      | <span data-ttu-id="62300-342">facsimileTelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="62300-342">facsimileTelephoneNumber</span></span>     |     |    <span data-ttu-id="62300-343">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-343">Create + update</span></span> |
| <span data-ttu-id="62300-344">**Förnamn**</span><span class="sxs-lookup"><span data-stu-id="62300-344">**FirstName**</span></span>   | <span data-ttu-id="62300-345">givenName</span><span class="sxs-lookup"><span data-stu-id="62300-345">givenName</span></span>       |     |    <span data-ttu-id="62300-346">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-346">Create + update</span></span> |
| <span data-ttu-id="62300-347">**Växel (\[Active\],, ”0”, ”True”, ”1”)**</span><span class="sxs-lookup"><span data-stu-id="62300-347">**Switch(\[Active\], , "0", "True", "1",)**</span></span> |  <span data-ttu-id="62300-348">AccountDisabled</span><span class="sxs-lookup"><span data-stu-id="62300-348">accountDisabled</span></span>      |     | <span data-ttu-id="62300-349">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-349">Create + update</span></span> |
| <span data-ttu-id="62300-350">**Mobile**</span><span class="sxs-lookup"><span data-stu-id="62300-350">**Mobile**</span></span>  |    <span data-ttu-id="62300-351">mobila</span><span class="sxs-lookup"><span data-stu-id="62300-351">mobile</span></span>       |     |       <span data-ttu-id="62300-352">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="62300-352">Written on create only</span></span> |
| <span data-ttu-id="62300-353">**E-postadress**</span><span class="sxs-lookup"><span data-stu-id="62300-353">**EmailAddress**</span></span>    | <span data-ttu-id="62300-354">E-post</span><span class="sxs-lookup"><span data-stu-id="62300-354">mail</span></span>    |     |     <span data-ttu-id="62300-355">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-355">Create + update</span></span> |
| <span data-ttu-id="62300-356">**ManagerReference**</span><span class="sxs-lookup"><span data-stu-id="62300-356">**ManagerReference**</span></span>   | <span data-ttu-id="62300-357">Manager</span><span class="sxs-lookup"><span data-stu-id="62300-357">manager</span></span>  |     |  <span data-ttu-id="62300-358">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-358">Create + update</span></span> |
| <span data-ttu-id="62300-359">**WorkSpaceReference**</span><span class="sxs-lookup"><span data-stu-id="62300-359">**WorkSpaceReference**</span></span> | <span data-ttu-id="62300-360">physicalDeliveryOfficeName</span><span class="sxs-lookup"><span data-stu-id="62300-360">physicalDeliveryOfficeName</span></span>    |     |  <span data-ttu-id="62300-361">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-361">Create + update</span></span> |
| <span data-ttu-id="62300-362">**Postnummer**</span><span class="sxs-lookup"><span data-stu-id="62300-362">**PostalCode**</span></span>  |   <span data-ttu-id="62300-363">Postnummer</span><span class="sxs-lookup"><span data-stu-id="62300-363">postalCode</span></span>  |     | <span data-ttu-id="62300-364">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-364">Create + update</span></span> |
| <span data-ttu-id="62300-365">**LocalReference**</span><span class="sxs-lookup"><span data-stu-id="62300-365">**LocalReference**</span></span> |  <span data-ttu-id="62300-366">preferredLanguage</span><span class="sxs-lookup"><span data-stu-id="62300-366">preferredLanguage</span></span>  |     |  <span data-ttu-id="62300-367">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-367">Create + update</span></span> |
| <span data-ttu-id="62300-368">** Ersätta (Mid (Ersätt (\[EmployeeID\]”, (\[ \\ \\ / \\ \\ \\ \\ \\ \\ \[\\\\\]\\\\:\\\\;\\\\</span><span class="sxs-lookup"><span data-stu-id="62300-368">**Replace(Mid(Replace(\[EmployeeID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\</span></span>|<span data-ttu-id="62300-369">\\\\=\\\\,\\\\+\\\\\*\\\\?\\ \\ &lt; \\ \\ &gt; \]) ””, ”,), 1, 20)”, ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**</span><span class="sxs-lookup"><span data-stu-id="62300-369">\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**</span></span>      |    <span data-ttu-id="62300-370">SAMAccountName</span><span class="sxs-lookup"><span data-stu-id="62300-370">sAMAccountName</span></span>            |     |         <span data-ttu-id="62300-371">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="62300-371">Written on create only</span></span> |
| <span data-ttu-id="62300-372">**Efternamn**</span><span class="sxs-lookup"><span data-stu-id="62300-372">**LastName**</span></span>   |   <span data-ttu-id="62300-373">SN</span><span class="sxs-lookup"><span data-stu-id="62300-373">sn</span></span>   |     |  <span data-ttu-id="62300-374">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-374">Create + update</span></span> |
| <span data-ttu-id="62300-375">**CountryRegionReference**</span><span class="sxs-lookup"><span data-stu-id="62300-375">**CountryRegionReference**</span></span> |  <span data-ttu-id="62300-376">St</span><span class="sxs-lookup"><span data-stu-id="62300-376">st</span></span>     |     | <span data-ttu-id="62300-377">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-377">Create + update</span></span> |
| <span data-ttu-id="62300-378">**AddressLineData**</span><span class="sxs-lookup"><span data-stu-id="62300-378">**AddressLineData**</span></span>    |  <span data-ttu-id="62300-379">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="62300-379">streetAddress</span></span>  |     |   <span data-ttu-id="62300-380">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-380">Create + update</span></span> |
| <span data-ttu-id="62300-381">**PrimaryWorkTelephone**</span><span class="sxs-lookup"><span data-stu-id="62300-381">**PrimaryWorkTelephone**</span></span>  |  <span data-ttu-id="62300-382">telephoneNumber</span><span class="sxs-lookup"><span data-stu-id="62300-382">telephoneNumber</span></span>   |     | <span data-ttu-id="62300-383">Skrivas i Skapa endast</span><span class="sxs-lookup"><span data-stu-id="62300-383">Written on create only</span></span> |
| <span data-ttu-id="62300-384">**BusinessTitle**</span><span class="sxs-lookup"><span data-stu-id="62300-384">**BusinessTitle**</span></span>   |  <span data-ttu-id="62300-385">Rubrik</span><span class="sxs-lookup"><span data-stu-id="62300-385">title</span></span>     |     |  <span data-ttu-id="62300-386">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-386">Create + update</span></span> |
| <span data-ttu-id="62300-387">**Ansluta (”@”, Ersätt (Ersätt (ersätta (Ersätt (ersätta (Ersätt (ersätta (ersätta (Ersätt ((Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt ( Ersätt (ansluta till (””., [Förnamn], [Efternamn]), ”([Øø])”, ”Outlook Express”,), ”[Ææ]”, ”ae”,), ”([äãàâãåáąÄÃÀÂÃÅÁĄA])”, ”a”,), ”[B]”, ”b”,), ”([CçčćÇČĆ])”, ”c”,), ”([ďĎD])”, ”d”,), ”([ëèéêęěËÈÉÊĘĚE])”, ”e”,), ”[F]”, ”f”,), ”([G])” ”g”,), ”[H]”, ”h”,), ”([ïîìíÏÎÌÍI])”, ”i”,), ”[J]”, ”j”,), ”([K])”, ”k”,), ”([ľłŁĽL])”, ”l”,), ”([M])”, ”m”,), ”([ñńňÑŃŇN])”, ”n”,), ”([öòőõôóÖÒŐÕÔÓO])”, ”o”,), ”([P])”, ”p”,), ”([Q])”, ”q”,),  ”([ŘŘR])”, ”r”,), ”([ßšśŠŚS])”, ”s”,), ”([TŤť])”, ”t”,), ”([üùûúůűÜÙÛÚŮŰU])”, ”u”,), ”([V])”, ”v”,), ”([B])”, ”b”,), ”([ýÿýŸÝY])”, ”y”,), ”([źžżŹŽŻZ])”, ”z”,) ”,”, ””,), ”contoso.com”)**</span><span class="sxs-lookup"><span data-stu-id="62300-387">**Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])", , "m", , ), , "([ñńňÑŃŇN])", , "n", , ), , "([öòőõôóÖÒŐÕÔÓO])", , "o", , ), , "([P])", , "p", , ), , "([Q])", , "q", , ), , "([řŘR])", , "r", , ), , "([ßšśŠŚS])", , "s", , ), , "([TŤť])", , "t", , ), , "([üùûúůűÜÙÛÚŮŰU])", , "u", , ), , "([V])", , "v", , ), , "([W])", , "w", , ), , "([ýÿýŸÝY])", , "y", , ), , "([źžżŹŽŻZ])", , "z", , ), " ", , , "", , ), "contoso.com")**</span></span>   | <span data-ttu-id="62300-388">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="62300-388">userPrincipalName</span></span>     |     | <span data-ttu-id="62300-389">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-389">Create + update</span></span>                                                   
| <span data-ttu-id="62300-390">**Växel (\[namnet\]”, OU standardanvändare, OU = = användare, OU = standard, OU = platser, DC = contoso, DC = com”, ”Dallas” ”, OU standardanvändare, OU = = användare, OU Dallas, OU = platser, DC = = contoso, DC = com”, ”Austin” ”OU standardanvändare, OU = = Användare, OU Austin, OU = platser, DC = = contoso, DC = com ”,” Seattle ””, OU standardanvändare, OU = = användare, OU Seattle, OU = = platser, DC = contoso, DC = com ”,” London ””, OU standardanvändare, OU = = användare, OU London, OU = platser, DC = = contoso, DC = com ”)**</span><span class="sxs-lookup"><span data-stu-id="62300-390">**Switch(\[Municipality\], "OU=Standard Users,OU=Users,OU=Default,OU=Locations,DC=contoso,DC=com", "Dallas", "OU=Standard Users,OU=Users,OU=Dallas,OU=Locations,DC=contoso,DC=com", "Austin", "OU=Standard Users,OU=Users,OU=Austin,OU=Locations,DC=contoso,DC=com", "Seattle", "OU=Standard Users,OU=Users,OU=Seattle,OU=Locations,DC=contoso,DC=com", “London", "OU=Standard Users,OU=Users,OU=London,OU=Locations,DC=contoso,DC=com")**</span></span>  | <span data-ttu-id="62300-391">parentDistinguishedName</span><span class="sxs-lookup"><span data-stu-id="62300-391">parentDistinguishedName</span></span>     |     |  <span data-ttu-id="62300-392">Skapa och uppdatera</span><span class="sxs-lookup"><span data-stu-id="62300-392">Create + update</span></span> |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a><span data-ttu-id="62300-393">Del 3: Konfigurera lokal synkronisering agent</span><span class="sxs-lookup"><span data-stu-id="62300-393">Part 3: Configure the on-premises synchronization agent</span></span>

<span data-ttu-id="62300-394">För att kunna etablera till Active Directory lokalt, måste en agent installeras på en domänansluten server i önskan Active Directory-skog.</span><span class="sxs-lookup"><span data-stu-id="62300-394">In order to provision to Active Directory on-premises, an agent must be installed on a domain-joined server in the desire Active Directory forest.</span></span> <span data-ttu-id="62300-395">Domänadministratörer (eller företagsadministratör) autentiseringsuppgifter krävs för att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="62300-395">Domain admin (or Enterprise admin) credentials are required to complete the procedure.</span></span>

<span data-ttu-id="62300-396">**[Du kan hämta den lokala lösenordssynkroniseringsagenten här](https://go.microsoft.com/fwlink/?linkid=847801)**</span><span class="sxs-lookup"><span data-stu-id="62300-396">**[You can download the on-premises synchronization agent here](https://go.microsoft.com/fwlink/?linkid=847801)**</span></span>

<span data-ttu-id="62300-397">När du har installerat agenten, kör Powershell-kommandona nedan att konfigurera agenten för din miljö.</span><span class="sxs-lookup"><span data-stu-id="62300-397">After installing agent, run the Powershell commands below to configure the agent for your environment.</span></span>

<span data-ttu-id="62300-398">**Kommandot #1**</span><span class="sxs-lookup"><span data-stu-id="62300-398">**Command #1**</span></span>

> <span data-ttu-id="62300-399">CD C:\\programfiler\\Microsoft Azure Active Directory-synkronisering Agent\\moduler\\AADSyncAgent</span><span class="sxs-lookup"><span data-stu-id="62300-399">cd C:\\Program Files\\Microsoft Azure Active Directory Synchronization Agent\\Modules\\AADSyncAgent</span></span>

> <span data-ttu-id="62300-400">import-module AADSyncAgent.psd1</span><span class="sxs-lookup"><span data-stu-id="62300-400">import-module AADSyncAgent.psd1</span></span>

<span data-ttu-id="62300-401">**Kommandot #2**</span><span class="sxs-lookup"><span data-stu-id="62300-401">**Command #2**</span></span>

> <span data-ttu-id="62300-402">Lägg till ADSyncAgentActiveDirectoryConfiguration</span><span class="sxs-lookup"><span data-stu-id="62300-402">Add-ADSyncAgentActiveDirectoryConfiguration</span></span>

* <span data-ttu-id="62300-403">Indata: För ”katalognamn”, ange namnet på AD-skog, som har angetts i del \#2</span><span class="sxs-lookup"><span data-stu-id="62300-403">Input: For "Directory Name", enter the AD Forest name, as entered in part \#2</span></span>
* <span data-ttu-id="62300-404">Indata: Admin användarnamn och lösenord för Active Directory-skog</span><span class="sxs-lookup"><span data-stu-id="62300-404">Input: Admin username and password for Active Directory forest</span></span>

<span data-ttu-id="62300-405">**Kommandot #3**</span><span class="sxs-lookup"><span data-stu-id="62300-405">**Command #3**</span></span>

> <span data-ttu-id="62300-406">Lägg till ADSyncAgentAzureActiveDirectoryConfiguration</span><span class="sxs-lookup"><span data-stu-id="62300-406">Add-ADSyncAgentAzureActiveDirectoryConfiguration</span></span>

* <span data-ttu-id="62300-407">Indata: Global administratörsanvändarnamnet och lösenordet för din Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="62300-407">Input: Global admin username and password for your Azure AD tenant</span></span>

<span data-ttu-id="62300-408">**Kommandot #4**</span><span class="sxs-lookup"><span data-stu-id="62300-408">**Command #4**</span></span>

> <span data-ttu-id="62300-409">Get-AdSyncAgentProvisioningTasks</span><span class="sxs-lookup"><span data-stu-id="62300-409">Get-AdSyncAgentProvisioningTasks</span></span>

* <span data-ttu-id="62300-410">Åtgärd: Kontrollera data returneras.</span><span class="sxs-lookup"><span data-stu-id="62300-410">Action: Confirm data is returned.</span></span> <span data-ttu-id="62300-411">Det här kommandot upptäcker automatiskt Workday etablering appar i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="62300-411">This command automatically discovers Workday provisioning apps in your Azure AD tenant.</span></span> <span data-ttu-id="62300-412">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="62300-412">Example output:</span></span>

> <span data-ttu-id="62300-413">Namn: Min AD-skog</span><span class="sxs-lookup"><span data-stu-id="62300-413">Name          : My AD Forest</span></span>
>
> <span data-ttu-id="62300-414">Aktiverad: True</span><span class="sxs-lookup"><span data-stu-id="62300-414">Enabled       : True</span></span>
>
> <span data-ttu-id="62300-415">DirectoryName: mydomain.contoso.com</span><span class="sxs-lookup"><span data-stu-id="62300-415">DirectoryName : mydomain.contoso.com</span></span>
>
> <span data-ttu-id="62300-416">Trovärdig: False</span><span class="sxs-lookup"><span data-stu-id="62300-416">Credentialed  : False</span></span>
>
> <span data-ttu-id="62300-417">ID: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203</span><span class="sxs-lookup"><span data-stu-id="62300-417">Identifier    : WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203</span></span>

<span data-ttu-id="62300-418">**Kommandot #5**</span><span class="sxs-lookup"><span data-stu-id="62300-418">**Command #5**</span></span>

> <span data-ttu-id="62300-419">Start-AdSyncAgentSynchronization-automatisk</span><span class="sxs-lookup"><span data-stu-id="62300-419">Start-AdSyncAgentSynchronization -Automatic</span></span>

<span data-ttu-id="62300-420">**Kommandot #6**</span><span class="sxs-lookup"><span data-stu-id="62300-420">**Command #6**</span></span>

> <span data-ttu-id="62300-421">net stop aadsyncagent</span><span class="sxs-lookup"><span data-stu-id="62300-421">net stop aadsyncagent</span></span>

<span data-ttu-id="62300-422">**Kommandot #7**</span><span class="sxs-lookup"><span data-stu-id="62300-422">**Command #7**</span></span>

> <span data-ttu-id="62300-423">net start aadsyncagent</span><span class="sxs-lookup"><span data-stu-id="62300-423">net start aadsyncagent</span></span>

### <a name="part-4-start-the-service"></a><span data-ttu-id="62300-424">Del 4: Starta tjänsten</span><span class="sxs-lookup"><span data-stu-id="62300-424">Part 4: Start the service</span></span>
<span data-ttu-id="62300-425">När delarna 1-3 har slutförts, kan du starta tjänsten etablering tillbaka i Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="62300-425">Once parts 1-3 have been completed, you can start the provisioning service back in the Azure Management Portal.</span></span>

1.  <span data-ttu-id="62300-426">I den **etablering** ställer du in den **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="62300-426">In the **Provisioning** tab, set the **Provisioning Status** to **On**.</span></span>

2. <span data-ttu-id="62300-427">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="62300-427">Click **Save**.</span></span>

3. <span data-ttu-id="62300-428">Detta startar den första synkroniseringen, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.</span><span class="sxs-lookup"><span data-stu-id="62300-428">This will start the initial sync, which can take a variable number of hours depending on how many users are in Workday.</span></span>

4. <span data-ttu-id="62300-429">Enskilda sync händelser, t.ex vilka användare att läsa från Workday och senare läggs eller uppdateras till Active Directory, kan visas i den **granskningsloggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="62300-429">Individual sync events such as what users are being read out of Workday, and then subsequently added or updated to Active Directory, can be viewed in the **Audit Logs** tab.</span></span> <span data-ttu-id="62300-430">**[Finns i guiden för etablering reporting detaljerade anvisningar om hur du tolkar granskningsloggarna](active-directory-saas-provisioning-reporting.md)**</span><span class="sxs-lookup"><span data-stu-id="62300-430">**[See the provisioning reporting guide for detailed instructions on how to read the audit logs](active-directory-saas-provisioning-reporting.md)**</span></span>

5.  <span data-ttu-id="62300-431">Windows programloggen på agentdatorn visas alla åtgärder som utförs via agenten.</span><span class="sxs-lookup"><span data-stu-id="62300-431">The Windows Application log on the agent machine will show all operations performed via the agent.</span></span>

6. <span data-ttu-id="62300-432">En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="62300-432">One completed, it will write an audit summary report in the  **Provisioning** tab, as shown below.</span></span>

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a><span data-ttu-id="62300-434">Konfigurering av användarförsörjning till Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62300-434">Configuring user provisioning to Azure Active Directory</span></span>
<span data-ttu-id="62300-435">Hur du konfigurerar etablering till Azure Active Directory beror på dina etablering krav som anges i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="62300-435">How you configure provisioning to Azure Active Directory will depend on your provisioning requirements, as detailed in the table below.</span></span>

| <span data-ttu-id="62300-436">Scenario</span><span class="sxs-lookup"><span data-stu-id="62300-436">Scenario</span></span> | <span data-ttu-id="62300-437">Lösning</span><span class="sxs-lookup"><span data-stu-id="62300-437">Solution</span></span> |
| -------- | -------- |
| <span data-ttu-id="62300-438">**Användarna behöver för att etableras till Active Directory och Azure AD**</span><span class="sxs-lookup"><span data-stu-id="62300-438">**Users need to be provisioned to Active Directory and Azure AD**</span></span> | <span data-ttu-id="62300-439">Använd  **[AAD Connect](connect/active-directory-aadconnect.md)**</span><span class="sxs-lookup"><span data-stu-id="62300-439">Use **[AAD Connect](connect/active-directory-aadconnect.md)**</span></span> |
| <span data-ttu-id="62300-440">**Användarna behöver för att etableras endast till Active Directory**</span><span class="sxs-lookup"><span data-stu-id="62300-440">**Users need to be provisioned to Active Directory only**</span></span> | <span data-ttu-id="62300-441">Använd  **[AAD Connect](connect/active-directory-aadconnect.md)**</span><span class="sxs-lookup"><span data-stu-id="62300-441">Use **[AAD Connect](connect/active-directory-aadconnect.md)**</span></span> |
| <span data-ttu-id="62300-442">**Användarna behöver för att etableras till Azure AD endast (moln)**</span><span class="sxs-lookup"><span data-stu-id="62300-442">**Users need to be provisioned to Azure AD only (cloud only)**</span></span> | <span data-ttu-id="62300-443">Använd den **arbetsdagar Azure Active Directory-etablering** app i appgalleriet</span><span class="sxs-lookup"><span data-stu-id="62300-443">Use the **Workday to Azure Active Directory provisioning** app in the app gallery</span></span> |

<span data-ttu-id="62300-444">Anvisningar om hur du konfigurerar Azure AD Connect finns i [dokumentation för Azure AD Connect](connect/active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="62300-444">For instructions on setting up Azure AD Connect, see the [Azure AD Connect documentation](connect/active-directory-aadconnect.md).</span></span>

<span data-ttu-id="62300-445">I följande avsnitt beskrivs hur du konfigurerar en anslutning mellan Workday och Azure AD för att etablera endast molnbaserad användare.</span><span class="sxs-lookup"><span data-stu-id="62300-445">The following sections describe setting up a connection between Workday and Azure AD to provision cloud-only users.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62300-446">Endast följer du anvisningarna nedan om du har endast molnbaserad användare som behöver etableras till Azure AD och inte lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-446">Only follow the procedure below if you have cloud-only users that need to be provisioned to Azure AD and not on-premises Active Directory.</span></span>

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a><span data-ttu-id="62300-447">Del 1: Lägga till Azure AD-appar för etablering koppling och skapa anslutningen till Workday</span><span class="sxs-lookup"><span data-stu-id="62300-447">Part 1: Adding the Azure AD provisioning connector app and creating the connection to Workday</span></span>

<span data-ttu-id="62300-448">**Konfigurera Workday till Azure Active Directory-etablering för endast molnbaserad användare:**</span><span class="sxs-lookup"><span data-stu-id="62300-448">**To configure Workday to Azure Active Directory provisioning for cloud-only users:**</span></span>

1.  <span data-ttu-id="62300-449">Gå till <https://portal.azure.com>.</span><span class="sxs-lookup"><span data-stu-id="62300-449">Go to <https://portal.azure.com>.</span></span>

2.  <span data-ttu-id="62300-450">I det vänstra navigeringsfältet väljer **Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="62300-450">In the left navigation bar, select **Azure Active Directory**</span></span>

3.  <span data-ttu-id="62300-451">Välj **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="62300-451">Select **Enterprise Applications**, then **All Applications**.</span></span>

4.  <span data-ttu-id="62300-452">Välj **lägga till ett program**, och välj sedan den **alla** kategori.</span><span class="sxs-lookup"><span data-stu-id="62300-452">Select **Add an application**, and then select the **All** category.</span></span>

5.  <span data-ttu-id="62300-453">Sök efter **Workday till Azure AD-etablering**, och Lägg till appen från galleriet.</span><span class="sxs-lookup"><span data-stu-id="62300-453">Search for **Workday to Azure AD provisioning**, and add that app from the gallery.</span></span>

6.  <span data-ttu-id="62300-454">När appen har lagts till och skärmen app information är visas, väljer **etablering**</span><span class="sxs-lookup"><span data-stu-id="62300-454">After the app is added and the app details screen is shown, select **Provisioning**</span></span>

7.  <span data-ttu-id="62300-455">Ändra den **etablering** **läge** till **automatisk**</span><span class="sxs-lookup"><span data-stu-id="62300-455">Change the **Provisioning** **Mode** to **Automatic**</span></span>

8.  <span data-ttu-id="62300-456">Slutför den **administratörsautentiseringsuppgifter** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="62300-456">Complete the **Admin Credentials** section as follows:</span></span>

   * <span data-ttu-id="62300-457">**Admin Username** – ange användarnamnet för system-kontot Workday-integrering med klienten domännamnet tillagt.</span><span class="sxs-lookup"><span data-stu-id="62300-457">**Admin Username** – Enter the username of the Workday integration system account, with the tenant domain name appended.</span></span> <span data-ttu-id="62300-458">Ska se ut ungefär:username@contoso4</span><span class="sxs-lookup"><span data-stu-id="62300-458">Should look something like: username@contoso4</span></span>

   * <span data-ttu-id="62300-459">**Administratörslösenordet –** ange lösenordet för kontot Workday-integrering system</span><span class="sxs-lookup"><span data-stu-id="62300-459">**Admin password –** Enter the password of the Workday    integration system account</span></span>

   * <span data-ttu-id="62300-460">**Klient URL –** anger du Webbadressen till Workday web services-slutpunkten för din klient.</span><span class="sxs-lookup"><span data-stu-id="62300-460">**Tenant URL –** Enter the URL to the Workday web services endpoint for your tenant.</span></span> <span data-ttu-id="62300-461">Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med rätt miljö-sträng (vid behov).</span><span class="sxs-lookup"><span data-stu-id="62300-461">This should look like: https://wd3-impl-services1.workday.com/ccx/service/contoso4, where contoso4 is replaced with your correct tenant name and wd3-impl is replaced with the correct environment string (if necessary).</span></span>

   * <span data-ttu-id="62300-462">**E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.</span><span class="sxs-lookup"><span data-stu-id="62300-462">**Notification Email –** Enter your email address, and check the    “send email if failure occurs” checkbox.</span></span>

   * <span data-ttu-id="62300-463">Klicka på den **Testanslutningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="62300-463">Click the **Test Connection** button.</span></span>

   * <span data-ttu-id="62300-464">Om Anslutningstestet lyckas klickar du på den **spara** längst upp.</span><span class="sxs-lookup"><span data-stu-id="62300-464">If the connection test succeeds, click the **Save** button at the top.</span></span> <span data-ttu-id="62300-465">Om det misslyckas, kan du kontrollera att Workday-URL och autentiseringsuppgifter är giltiga i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-465">If it fails, double-check that the Workday URL and credentials are valid in Workday.</span></span>


### <a name="part-2-configure-attribute-mappings"></a><span data-ttu-id="62300-466">Del 2: Konfigurera attributmappning</span><span class="sxs-lookup"><span data-stu-id="62300-466">Part 2: Configure attribute mappings</span></span> 

<span data-ttu-id="62300-467">I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Azure Active Directory för endast molnbaserad användare.</span><span class="sxs-lookup"><span data-stu-id="62300-467">In this section, you will configure how user data flows from Workday to Azure Active Directory for cloud-only users.</span></span>

1.  <span data-ttu-id="62300-468">På fliken etablering under **mappningar**, klickar du på **synkronisera arbetare till Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="62300-468">On the Provisioning tab under **Mappings**, click **Synchronize Workers to Azure AD**.</span></span>

2.   <span data-ttu-id="62300-469">I den **källa Objektområde** fält, kan du välja vilka uppsättningar med användare i Workday ska vara i omfånget för etablering i Azure AD genom att definiera en uppsättning attributbaserad filter.</span><span class="sxs-lookup"><span data-stu-id="62300-469">In the **Source Object Scope** field, you can select which sets of users in Workday should be in scope for provisioning to Azure AD, by defining a set of attribute-based filters.</span></span> <span data-ttu-id="62300-470">Standardvärde är ”alla användare i Workday”.</span><span class="sxs-lookup"><span data-stu-id="62300-470">The default scope is “all users in Workday”.</span></span> <span data-ttu-id="62300-471">Exempel filter:</span><span class="sxs-lookup"><span data-stu-id="62300-471">Example filters:</span></span>

   * <span data-ttu-id="62300-472">Exempel: Omfång för användare med Worker-ID: N mellan 1000000 och 2000000</span><span class="sxs-lookup"><span data-stu-id="62300-472">Example: Scope to users with Worker IDs between 1000000 and   2000000</span></span>

      * <span data-ttu-id="62300-473">Attributet: WorkerID</span><span class="sxs-lookup"><span data-stu-id="62300-473">Attribute: WorkerID</span></span>

      * <span data-ttu-id="62300-474">Operatorn: REGEX-matchning</span><span class="sxs-lookup"><span data-stu-id="62300-474">Operator: REGEX Match</span></span>

      * <span data-ttu-id="62300-475">Värde: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span><span class="sxs-lookup"><span data-stu-id="62300-475">Value: (1[0-9][0-9][0-9][0-9][0-9][0-9])</span></span>

   * <span data-ttu-id="62300-476">Exempel: Endast contingent arbetare och inte fast anställda</span><span class="sxs-lookup"><span data-stu-id="62300-476">Example: Only contingent workers and not regular employees</span></span>

      * <span data-ttu-id="62300-477">Attributet: ContingentID</span><span class="sxs-lookup"><span data-stu-id="62300-477">Attribute: ContingentID</span></span>

      * <span data-ttu-id="62300-478">Operatorn: Inte är NULL</span><span class="sxs-lookup"><span data-stu-id="62300-478">Operator: IS NOT NULL</span></span>

3.  <span data-ttu-id="62300-479">I den **mål objektåtgärder** fält du kan globalt filtrera vilka åtgärder ska kunna utföras på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62300-479">In the **Target Object Actions** field, you can globally filter what actions are allowed to be performed on Azure AD.</span></span> <span data-ttu-id="62300-480">**Skapa** och **uppdatering** är de vanligaste.</span><span class="sxs-lookup"><span data-stu-id="62300-480">**Create** and **Update** are most common.</span></span>

4.  <span data-ttu-id="62300-481">I den **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappar till Active Directory-attribut.</span><span class="sxs-lookup"><span data-stu-id="62300-481">In the **Attribute mappings** section, you can define how individual Workday attributes map to Active Directory attributes.</span></span>

5. <span data-ttu-id="62300-482">Klicka på en befintlig attributmappning att uppdatera det, eller klicka på **Lägg till ny mappning** längst ned på skärmen för att lägga till nya mappningar.</span><span class="sxs-lookup"><span data-stu-id="62300-482">Click on an existing attribute mapping to update it, or click **Add new mapping** at the bottom of the screen to add new mappings.</span></span> <span data-ttu-id="62300-483">En enskild attributmappning stöder dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="62300-483">An individual attribute mapping supports these properties:</span></span>

   * <span data-ttu-id="62300-484">**Avbildningstyp**</span><span class="sxs-lookup"><span data-stu-id="62300-484">**Mapping Type**</span></span>

      * <span data-ttu-id="62300-485">**Direkt** – skriver värdet för Workday-attribut till attributet AD utan ändringar</span><span class="sxs-lookup"><span data-stu-id="62300-485">**Direct** – Writes the value of the Workday attribute         to the AD attribute, with no changes</span></span>

      * <span data-ttu-id="62300-486">**Konstant** -skriva ett statiska, konstant strängvärde till AD-attribut</span><span class="sxs-lookup"><span data-stu-id="62300-486">**Constant** - Write a static, constant string value to         the AD attribute</span></span>

      * <span data-ttu-id="62300-487">**Uttrycket** – du kan skriva ett anpassat värde till AD-attributet baserat på en eller flera Workday-attribut.</span><span class="sxs-lookup"><span data-stu-id="62300-487">**Expression** – Allows you to write a custom value to the AD attribute, based on one or more Workday attributes.</span></span> <span data-ttu-id="62300-488">[Mer information finns i den här artikeln på uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="62300-488">[For more info, see this article on expressions](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>

   * <span data-ttu-id="62300-489">**Källattributet** -användarattribut från Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-489">**Source attribute** - The user attribute from Workday.</span></span>

   * <span data-ttu-id="62300-490">**Standardvärde** – det är valfritt.</span><span class="sxs-lookup"><span data-stu-id="62300-490">**Default value** – Optional.</span></span> <span data-ttu-id="62300-491">Om källattributet har ett tomt värde, skrivs mappningen det här värdet i stället.</span><span class="sxs-lookup"><span data-stu-id="62300-491">If the source attribute has an empty value, the mapping will write this value instead.</span></span>
            <span data-ttu-id="62300-492">De vanligaste konfigurationen är att du lämnar fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="62300-492">Most common configuration is to leave this blank.</span></span>

   * <span data-ttu-id="62300-493">**Målattribut** – användarattribut i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62300-493">**Target attribute** – The user attribute in Azure AD.</span></span>

   * <span data-ttu-id="62300-494">**Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen som ska användas för att unikt identifiera användarna mellan Workday och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62300-494">**Match objects using this attribute** – Whether or not this mapping should be used to uniquely identify users between Workday and Azure AD.</span></span> <span data-ttu-id="62300-495">Detta anges normalt Worker ID-fältet för Workday som vanligtvis är mappad till attributet anställnings-ID (nya) eller ett attribut för tillägget i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62300-495">This is typically set on the Worker ID field for Workday, which is typically mapped to the Employee ID attribute (new) or an extension attribute in Azure AD.</span></span>

   * <span data-ttu-id="62300-496">**Matchar prioritet** – flera matchande attribut kan anges.</span><span class="sxs-lookup"><span data-stu-id="62300-496">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="62300-497">När det finns flera, utvärderas de i den ordning som anges av det här fältet.</span><span class="sxs-lookup"><span data-stu-id="62300-497">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="62300-498">När en matchning hittas matchar ingen ytterligare attribut utvärderas.</span><span class="sxs-lookup"><span data-stu-id="62300-498">As soon as a match is found, no further matching attributes are evaluated.</span></span>

   * <span data-ttu-id="62300-499">**Använd den här mappningen**</span><span class="sxs-lookup"><span data-stu-id="62300-499">**Apply this mapping**</span></span>

     * <span data-ttu-id="62300-500">**Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder</span><span class="sxs-lookup"><span data-stu-id="62300-500">**Always** – Apply this mapping on both user creation          and update actions</span></span>

     * <span data-ttu-id="62300-501">**Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder</span><span class="sxs-lookup"><span data-stu-id="62300-501">**Only during creation** - Apply this mapping only on          user creation actions</span></span>

6. <span data-ttu-id="62300-502">Om du vill spara dina mappningar klickar du på **spara** överst i avsnittet attributmappning.</span><span class="sxs-lookup"><span data-stu-id="62300-502">To save your mappings, click **Save** at the top of the      Attribute Mapping section.</span></span>

### <a name="part-3-start-the-service"></a><span data-ttu-id="62300-503">Del 3: Starta tjänsten</span><span class="sxs-lookup"><span data-stu-id="62300-503">Part 3: Start the service</span></span>
<span data-ttu-id="62300-504">När delar 1 – 2 har slutförts, kan du starta tjänsten etablering.</span><span class="sxs-lookup"><span data-stu-id="62300-504">Once parts 1-2 have been completed, you can start the provisioning service.</span></span>

1.  <span data-ttu-id="62300-505">I den **etablering** ställer du in den **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="62300-505">In the **Provisioning** tab, set the **Provisioning Status** to **On**.</span></span>

2. <span data-ttu-id="62300-506">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="62300-506">Click **Save**.</span></span>

3. <span data-ttu-id="62300-507">Detta startar den första synkroniseringen, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.</span><span class="sxs-lookup"><span data-stu-id="62300-507">This will start the initial sync, which can take a variable number  of hours depending on how many users are in Workday.</span></span>

4. <span data-ttu-id="62300-508">Enskilda sync händelser kan visas i den **granskningsloggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="62300-508">Individual sync events can be viewed in the **Audit Logs** tab.</span></span> <span data-ttu-id="62300-509">**[Finns i guiden för etablering reporting detaljerade anvisningar om hur du tolkar granskningsloggarna](active-directory-saas-provisioning-reporting.md)**</span><span class="sxs-lookup"><span data-stu-id="62300-509">**[See the provisioning reporting guide for detailed instructions on how to read the audit logs](active-directory-saas-provisioning-reporting.md)**</span></span>

5. <span data-ttu-id="62300-510">En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="62300-510">One completed, it will write an audit summary report in the  **Provisioning** tab, as shown below.</span></span>


## <a name="configuring-writeback-of-email-addresses-to-workday"></a><span data-ttu-id="62300-511">Konfigurera tillbakaskrivning av e-postadresser till Workday</span><span class="sxs-lookup"><span data-stu-id="62300-511">Configuring writeback of email addresses to Workday</span></span>
<span data-ttu-id="62300-512">Följ dessa instruktioner för att konfigurera tillbakaskrivning av användare e-postadresser från Azure Active Directory till Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-512">Follow these instructions to configure writeback of user email addresses from Azure Active Directory to Workday.</span></span>

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a><span data-ttu-id="62300-513">Del 1: Lägga till appen etablering koppling och skapa anslutningen till Workday</span><span class="sxs-lookup"><span data-stu-id="62300-513">Part 1: Adding the provisioning connector app and creating the connection to Workday</span></span>

<span data-ttu-id="62300-514">**Konfigurera Workday för Active Directory-etablering:**</span><span class="sxs-lookup"><span data-stu-id="62300-514">**To configure Workday to Active Directory provisioning:**</span></span>

1.  <span data-ttu-id="62300-515">Gå till <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="62300-515">Go to <https://portal.azure.com></span></span>

2.  <span data-ttu-id="62300-516">I det vänstra navigeringsfältet väljer **Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="62300-516">In the left navigation bar, select **Azure Active Directory**</span></span>

3.  <span data-ttu-id="62300-517">Välj **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="62300-517">Select **Enterprise Applications**, then **All Applications**.</span></span>

4.  <span data-ttu-id="62300-518">Välj **lägga till ett program**och välj den **alla** kategori.</span><span class="sxs-lookup"><span data-stu-id="62300-518">Select **Add an application**, then select the **All** category.</span></span>

5.  <span data-ttu-id="62300-519">Sök efter **Workday tillbakaskrivning**, och Lägg till appen från galleriet.</span><span class="sxs-lookup"><span data-stu-id="62300-519">Search for **Workday Writeback**, and add that app from the gallery.</span></span>

6.  <span data-ttu-id="62300-520">När appen har lagts till och skärmen app information är visas, väljer **etablering**</span><span class="sxs-lookup"><span data-stu-id="62300-520">After the app is added and the app details screen is shown, select **Provisioning**</span></span>

7.  <span data-ttu-id="62300-521">Ändra den **etablering** **läge** till **automatisk**</span><span class="sxs-lookup"><span data-stu-id="62300-521">Change the **Provisioning** **Mode** to **Automatic**</span></span>

8.  <span data-ttu-id="62300-522">Slutför den **administratörsautentiseringsuppgifter** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="62300-522">Complete the **Admin Credentials** section as follows:</span></span>

   * <span data-ttu-id="62300-523">**Admin Username** – ange användarnamnet för system-kontot Workday-integrering med klienten domännamnet tillagt.</span><span class="sxs-lookup"><span data-stu-id="62300-523">**Admin Username** – Enter the username of the Workday integration system account, with the tenant domain name appended.</span></span> <span data-ttu-id="62300-524">Ska se ut ungefär:username@contoso4</span><span class="sxs-lookup"><span data-stu-id="62300-524">Should look something like: username@contoso4</span></span>

   * <span data-ttu-id="62300-525">**Administratörslösenordet –** ange lösenordet för kontot Workday-integrering system</span><span class="sxs-lookup"><span data-stu-id="62300-525">**Admin password –** Enter the password of the Workday    integration system account</span></span>

   * <span data-ttu-id="62300-526">**Klient URL –** anger du Webbadressen till Workday web services-slutpunkten för din klient.</span><span class="sxs-lookup"><span data-stu-id="62300-526">**Tenant URL –** Enter the URL to the Workday web services endpoint for your tenant.</span></span> <span data-ttu-id="62300-527">Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med rätt miljö-sträng (vid behov).</span><span class="sxs-lookup"><span data-stu-id="62300-527">This should look like: https://wd3-impl-services1.workday.com/ccx/service/contoso4, where contoso4 is replaced with your correct tenant name and wd3-impl is replaced with the correct environment string (if necessary).</span></span>

   * <span data-ttu-id="62300-528">**E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.</span><span class="sxs-lookup"><span data-stu-id="62300-528">**Notification Email –** Enter your email address, and check the    “send email if failure occurs” checkbox.</span></span>

   * <span data-ttu-id="62300-529">Klicka på den **Testanslutningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="62300-529">Click the **Test Connection** button.</span></span> <span data-ttu-id="62300-530">Om Anslutningstestet lyckas klickar du på den **spara** längst upp.</span><span class="sxs-lookup"><span data-stu-id="62300-530">If the connection test succeeds, click the **Save** button at the top.</span></span> <span data-ttu-id="62300-531">Om det misslyckas, kan du kontrollera att Workday-URL och autentiseringsuppgifter är giltiga i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-531">If it fails, double-check that the Workday URL and credentials are valid in Workday.</span></span>


### <a name="part-2-configure-attribute-mappings"></a><span data-ttu-id="62300-532">Del 2: Konfigurera attributmappning</span><span class="sxs-lookup"><span data-stu-id="62300-532">Part 2: Configure attribute mappings</span></span> 


<span data-ttu-id="62300-533">I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Active Directory.</span><span class="sxs-lookup"><span data-stu-id="62300-533">In this section, you will configure how user data flows from Workday to Active Directory.</span></span>

1.  <span data-ttu-id="62300-534">På fliken etablering under **mappningar**, klickar du på **synkronisera Azure AD-användare Workday**.</span><span class="sxs-lookup"><span data-stu-id="62300-534">On the Provisioning tab under **Mappings**, click **Synchronize Azure AD Users to Workday**.</span></span>

2.  <span data-ttu-id="62300-535">I den **källa Objektområde** fält du kan du filtrera vilka uppsättningar med användare i Azure Active Directory ska ha sina e-postadresser skrivs tillbaka till Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-535">In the **Source Object Scope** field, you can optionally filter which sets of users in Azure Active Directory should have their email addresses written back to Workday.</span></span> <span data-ttu-id="62300-536">Standardvärde är ”alla användare i Azure AD”.</span><span class="sxs-lookup"><span data-stu-id="62300-536">The default scope is “all users in Azure AD”.</span></span> 

3.  <span data-ttu-id="62300-537">I den **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappar till Active Directory-attribut.</span><span class="sxs-lookup"><span data-stu-id="62300-537">In the **Attribute mappings** section, you can define how individual Workday attributes map to Active Directory attributes.</span></span> <span data-ttu-id="62300-538">Det finns en mappning för e-postadressen som standard.</span><span class="sxs-lookup"><span data-stu-id="62300-538">There is a mapping for the email address by default.</span></span> <span data-ttu-id="62300-539">Det matchande ID måste dock uppdateras för att matcha användare i Azure AD med sina motsvarande poster i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-539">However, the matching ID must be updated to match users in Azure AD with their corresponding entries in Workday.</span></span> <span data-ttu-id="62300-540">En populär matchande metod är att synkronisera Workday worker-ID eller anställnings-ID-extensionAttribute1 15 i Azure AD och sedan använda det här attributet i Azure AD för att matcha användare i Workday.</span><span class="sxs-lookup"><span data-stu-id="62300-540">A popular matching method is to synchronize the Workday worker ID or employee ID to extensionAttribute1-15 in Azure AD, and then use this attribute in Azure AD to match users back in Workday.</span></span>

4.  <span data-ttu-id="62300-541">Om du vill spara dina mappningar klickar du på **spara** överst i avsnittet attributmappning.</span><span class="sxs-lookup"><span data-stu-id="62300-541">To save your mappings, click **Save** at the top of the Attribute Mapping section.</span></span>

### <a name="part-3-start-the-service"></a><span data-ttu-id="62300-542">Del 3: Starta tjänsten</span><span class="sxs-lookup"><span data-stu-id="62300-542">Part 3: Start the service</span></span>
<span data-ttu-id="62300-543">När delar 1 – 2 har slutförts, kan du starta tjänsten etablering.</span><span class="sxs-lookup"><span data-stu-id="62300-543">Once parts 1-2 have been completed, you can start the provisioning service.</span></span>

1.  <span data-ttu-id="62300-544">I den **etablering** ställer du in den **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="62300-544">In the **Provisioning** tab, set the **Provisioning Status** to **On**.</span></span>

2. <span data-ttu-id="62300-545">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="62300-545">Click **Save**.</span></span>

3. <span data-ttu-id="62300-546">Detta startar den första synkroniseringen, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.</span><span class="sxs-lookup"><span data-stu-id="62300-546">This will start the initial sync, which can take a variable number  of hours depending on how many users are in Workday.</span></span>

4. <span data-ttu-id="62300-547">Enskilda sync händelser kan visas i den **granskningsloggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="62300-547">Individual sync events can be viewed in the **Audit Logs** tab.</span></span> <span data-ttu-id="62300-548">**[Finns i guiden för etablering reporting detaljerade anvisningar om hur du tolkar granskningsloggarna](active-directory-saas-provisioning-reporting.md)**</span><span class="sxs-lookup"><span data-stu-id="62300-548">**[See the provisioning reporting guide for detailed instructions on how to read the audit logs](active-directory-saas-provisioning-reporting.md)**</span></span>

5. <span data-ttu-id="62300-549">En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="62300-549">One completed, it will write an audit summary report in the  **Provisioning** tab, as shown below.</span></span>

## <a name="known-issues"></a><span data-ttu-id="62300-550">Kända problem</span><span class="sxs-lookup"><span data-stu-id="62300-550">Known issues</span></span>

* <span data-ttu-id="62300-551">**Granskningsloggar i europeiska språk** – av versionen av den här tekniska förhandsversionen är ett känt problem med den [granskningsloggar](active-directory-saas-provisioning-reporting.md) för Workday connector appar som inte visas i den [Azure-portalen](https://portal.azure.com) Om Azure AD-klienten finns i ett Europeiska datacenter.</span><span class="sxs-lookup"><span data-stu-id="62300-551">**Audit logs in European locales** - As of the release of this technical preview, there is a known issue with the [audit logs](active-directory-saas-provisioning-reporting.md) for the Workday connector apps not appearing in the [Azure portal](https://portal.azure.com) if the Azure AD tenant resides in a European data center.</span></span> <span data-ttu-id="62300-552">En korrigering för problemet är kommande.</span><span class="sxs-lookup"><span data-stu-id="62300-552">A fix for this issue is forthcoming.</span></span> <span data-ttu-id="62300-553">Kontrollera diskutrymme igen i den nära framtiden för uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="62300-553">Please check this space again in the near future for updates.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="62300-554">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="62300-554">Additional resources</span></span>
* [<span data-ttu-id="62300-555">Självstudier: Konfigurera enkel inloggning mellan Workday och Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62300-555">Tutorial: Configuring single sign-on between Workday and Azure Active Directory</span></span>](active-directory-saas-workday-tutorial.md)
* [<span data-ttu-id="62300-556">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62300-556">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62300-557">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="62300-557">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="62300-558">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62300-558">Next steps</span></span>

* [<span data-ttu-id="62300-559">Lär dig hur du granska loggarna och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="62300-559">Learn how to review logs and get reports on provisioning activity</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
