---
title: "Vanliga frågor och svar om Azure Active Directory | Microsoft Docs"
description: "I det här avsnittet med vanliga frågor och svar om Azure Active Directory får du svar på frågor om hur du får åtkomst till Azure och Azure Active Directory, om lösenordshantering samt om åtkomsten till program."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/16/2017
ms.author: markvi
ms.openlocfilehash: 8d4460b3059558de2253c6f6a2d2fc8e7564d6d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-faq"></a><span data-ttu-id="38248-103">Vanliga frågor och svar om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="38248-103">Azure Active Directory FAQ</span></span>
<span data-ttu-id="38248-104">Azure Active Directory (Azure AD) är en omfattande IDaaS-lösning (Identity as a Service) som omfattar alla aspekter relaterade till identiteter, åtkomsthantering och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="38248-104">Azure Active Directory (Azure AD) is a comprehensive identity as a service (IDaaS) solution that spans all aspects of identity, access management, and security.</span></span>

<span data-ttu-id="38248-105">Mer information finns i [Vad är Azure Active Directory?](active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="38248-105">For more information, see [What is Azure Active Directory?](active-directory-whatis.md).</span></span>


## <a name="access-azure-and-azure-active-directory"></a><span data-ttu-id="38248-106">Kom åt Azure och Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="38248-106">Access Azure and Azure Active Directory</span></span>
<span data-ttu-id="38248-107">**F: Varför visas ett meddelande om att inga prenumerationer hittades när jag försöker komma åt Azure AD på den klassiska Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="38248-107">**Q: Why do I get “No subscriptions found” when I try to access Azure AD in the Azure classic portal?**</span></span>

<span data-ttu-id="38248-108">**S:** För att komma åt den klassiska Azure-portalen behöver varje användare ha behörighet med en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="38248-108">**A:** To access the Azure classic portal, each user needs permissions with an Azure subscription.</span></span> <span data-ttu-id="38248-109">Om du har en betald Office 365- eller Azure AD-prenumeration går du till [http://aka.ms/accessAAD](http://aka.ms/accessAAD) och följer steget för engångsaktiveringen.</span><span class="sxs-lookup"><span data-stu-id="38248-109">If you have a paid Office 365 or Azure AD subscription, go to [http://aka.ms/accessAAD](http://aka.ms/accessAAD) for a one-time activation step.</span></span> <span data-ttu-id="38248-110">Annars måste du aktivera ett kostnadsfritt [Azure-konto](https://azure.microsoft.com/pricing/free-trial/) eller en betald prenumeration.</span><span class="sxs-lookup"><span data-stu-id="38248-110">Otherwise, you will need to activate a free [Azure account](https://azure.microsoft.com/pricing/free-trial/) or a paid subscription.</span></span>

<span data-ttu-id="38248-111">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="38248-111">For more information, see:</span></span>

* [<span data-ttu-id="38248-112">Hur Azure-prenumerationer är associerade med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="38248-112">How Azure subscriptions are associated with Azure Active Directory</span></span>](active-directory-how-subscriptions-associated-directory.md)
* [<span data-ttu-id="38248-113">Hantera katalogen för din Office 365-prenumeration i Azure</span><span class="sxs-lookup"><span data-stu-id="38248-113">Manage the directory for your Office 365 subscription in Azure</span></span>](active-directory-manage-o365-subscription.md)

- - -
<span data-ttu-id="38248-114">**F: Hur är Azure AD, Office 365 och Azure kopplade till varandra?**</span><span class="sxs-lookup"><span data-stu-id="38248-114">**Q: What’s the relationship between Azure AD, Office 365, and Azure?**</span></span>

<span data-ttu-id="38248-115">**S:** Azure AD tillhandahåller gemensamma identitets- och åtkomstfunktioner för alla webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="38248-115">**A:** Azure AD provides you with common identity and access capabilities to all web services.</span></span> <span data-ttu-id="38248-116">Oavsett om du använder Office 365, Microsoft Azure, Intune eller andra tjänster använder du redan Azure AD för hjälp med inloggnings- och åtkomsthantering för alla dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="38248-116">Whether you are using Office 365, Microsoft Azure, Intune, or others, you're already using Azure AD to help turn on sign-on and access management for all these services.</span></span>

<span data-ttu-id="38248-117">Alla användare som har ställts in för att använda webbtjänster definieras som användarkonton i en eller flera Azure AD-instanser.</span><span class="sxs-lookup"><span data-stu-id="38248-117">All users who are set up to use web services are defined as user accounts in one or more Azure AD instances.</span></span> <span data-ttu-id="38248-118">Du kan ställa in dessa konton för kostnadsfria Azure AD-funktioner, till exempel programåtkomst i molnet.</span><span class="sxs-lookup"><span data-stu-id="38248-118">You can set up these accounts for free Azure AD capabilities like cloud application access.</span></span>

<span data-ttu-id="38248-119">Azure AD-betaltjänsterna som Enterprise Mobility + Security kompletterar andra webbtjänster som Office 365 och Microsoft Azure med heltäckande hanterings- och säkerhetslösningar i företagsklass.</span><span class="sxs-lookup"><span data-stu-id="38248-119">Azure AD paid services like Enterprise Mobility + Security complement other web services like Office 365 and Microsoft Azure with comprehensive enterprise-scale management and security solutions.</span></span>
- - -
<span data-ttu-id="38248-120">**F: Varför kan jag logga in i Azure Portal, men inte den klassiska Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="38248-120">**Q:  Why can I sign in to the Azure portal but not the Azure classic portal?**</span></span>

<span data-ttu-id="38248-121">**S:** Azure Portal kräver ingen giltig prenumeration, men den klassiska portalen kräver att du har en giltig prenumeration.</span><span class="sxs-lookup"><span data-stu-id="38248-121">**A:**  The Azure portal does not require a valid subscription, and the classic portal does require a valid subscription.</span></span>  <span data-ttu-id="38248-122">Om du inte har någon prenumeration kan du inte logga in på den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="38248-122">If you do not have a subscription, you can't sign in to the classic portal.</span></span>
- - -
<span data-ttu-id="38248-123">**F: Vad är skillnaden mellan prenumerationsadministratör och katalogadministratör?**</span><span class="sxs-lookup"><span data-stu-id="38248-123">**Q:  What are the differences between Subscription Administrator and Directory Administrator?**</span></span>

<span data-ttu-id="38248-124">**S:** Som standard tilldelas du rollen som prenumerationsadministratör när du registrerar dig för Azure.</span><span class="sxs-lookup"><span data-stu-id="38248-124">**A:** By default, you are assigned the Subscription Administrator role when you sign up for Azure.</span></span> <span data-ttu-id="38248-125">En prenumerationsadministratör kan använda antingen ett Microsoft-konto eller ett arbets- eller skolkonto från den katalog som Azure-prenumerationen är associerad med.</span><span class="sxs-lookup"><span data-stu-id="38248-125">A subscription admin can use either a Microsoft account or a work or school account from the directory that the Azure subscription is associated with.</span></span>  <span data-ttu-id="38248-126">Den här rollen har behörighet att hantera tjänster på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="38248-126">This role is authorized to manage services in the Azure portal.</span></span>

<span data-ttu-id="38248-127">Om andra behöver logga in och komma åt tjänster med samma prenumeration kan du lägga till dem som medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="38248-127">If others need to sign in and access services by using the same subscription, you can add them as co-admins.</span></span> <span data-ttu-id="38248-128">Den här rollen har samma åtkomstbehörigheter som tjänstadministratören, men kan inte ändra associationen mellan prenumerationer och Azure-kataloger.</span><span class="sxs-lookup"><span data-stu-id="38248-128">This role has the same access privileges as the service admin, but can’t change the association of subscriptions to Azure directories.</span></span>  <span data-ttu-id="38248-129">Ytterligare information om prenumerationsadministratörer finns i [How to add or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) (Så här lägger du till eller ändrar Azure-administratörsroller) och [Hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="38248-129">For additional information on subscription admins, see [How to add or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) and [How Azure subscriptions are associated with Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span></span>


<span data-ttu-id="38248-130">Azure AD har en annan uppsättning administratörsroller för att hantera katalog- och identitetsrelaterade funktioner.</span><span class="sxs-lookup"><span data-stu-id="38248-130">Azure AD has a different set of admin roles to manage the directory and identity-related features.</span></span>  <span data-ttu-id="38248-131">De här administratörerna har åtkomst till olika funktioner i Azure Portal eller den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="38248-131">These admins will have access to various features in the Azure portal or the Azure classic portal.</span></span> <span data-ttu-id="38248-132">Administratörens roll avgör vad de kan göra, som att skapa eller redigera användare, tilldela administrativa roller till andra, återställa användarlösenord, hantera användarlicenser eller hantera domäner.</span><span class="sxs-lookup"><span data-stu-id="38248-132">The admin's role determines what they can do, like create or edit users, assign administrative roles to others, reset user passwords, manage user licenses, or manage domains.</span></span>  <span data-ttu-id="38248-133">Mer information om Azure AD-katalogadministratörer och deras roller finns i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="38248-133">For additional information on Azure AD directory admins and their roles, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="38248-134">Dessutom kompletterar Azure AD-betaltjänster som Enterprise Mobility + Security andra webbtjänster, som Office 365 och Microsoft Azure, med heltäckande hanterings- och säkerhetslösningar i företagsklass.</span><span class="sxs-lookup"><span data-stu-id="38248-134">Additionally, Azure AD paid services like Enterprise Mobility + Security complement other web services, such as Office 365 and Microsoft Azure, with comprehensive enterprise-scale management and security solutions.</span></span>

- - -
<span data-ttu-id="38248-135">**F: Finns det någon rapport som visar när mina Azure AD-användarlicenser upphör att gälla?**</span><span class="sxs-lookup"><span data-stu-id="38248-135">**Q: Is there a report that shows when my Azure AD user licenses will expire?**</span></span>

<span data-ttu-id="38248-136">**S:** Nej.</span><span class="sxs-lookup"><span data-stu-id="38248-136">**A:** No.</span></span>  <span data-ttu-id="38248-137">Det här är inte tillgängligt för närvarande.</span><span class="sxs-lookup"><span data-stu-id="38248-137">This is not currently available.</span></span>

- - -

## <a name="get-started-with-hybrid-azure-ad"></a><span data-ttu-id="38248-138">Kom igång med en Azure AD-hybridlösning</span><span class="sxs-lookup"><span data-stu-id="38248-138">Get started with Hybrid Azure AD</span></span>


<span data-ttu-id="38248-139">**F: Hur lämnar jag en klient när jag har lagts till som medarbetare?**</span><span class="sxs-lookup"><span data-stu-id="38248-139">**Q: How do I leave a tenant when I am added as a collaborator?**</span></span>

<span data-ttu-id="38248-140">**A:** När du har lagts till som medarbetare till en annan organisations klient kan du använda "klientväxlaren" i det övre högra hörnet för att växla mellan klienter.</span><span class="sxs-lookup"><span data-stu-id="38248-140">**A:** When you are added to another organization's tenant as a collaborator, you can use the "tenant switcher" in the upper right to switch between tenants.</span></span>  <span data-ttu-id="38248-141">För närvarande går det inte att lämna organisationen som bjuder in och Microsoft arbetar med att tillhandahålla den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="38248-141">Currently, there is no way to leave the inviting organization, and Microsoft is working on providing this functionality.</span></span>  <span data-ttu-id="38248-142">Tills den funktionen är tillgänglig kan du be organisationen som bjuder in att ta bort dig från deras klient.</span><span class="sxs-lookup"><span data-stu-id="38248-142">Until this feature is available, you can ask the inviting organization to remove you from their tenant.</span></span>
- - -
<span data-ttu-id="38248-143">**F: Hur kan jag ansluta min lokala katalog till Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="38248-143">**Q: How can I connect my on-premises directory to Azure AD?**</span></span>

<span data-ttu-id="38248-144">**S:** Du kan ansluta din lokala katalog till Azure AD med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="38248-144">**A:** You can connect your on-premises directory to Azure AD by using Azure AD Connect.</span></span>

<span data-ttu-id="38248-145">Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="38248-145">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="38248-146">**F: Hur konfigurerar jag enkel inloggning (SSO) mellan min lokala katalog och mina molnprogram?**</span><span class="sxs-lookup"><span data-stu-id="38248-146">**Q: How do I set up SSO between my on-premises directory and my cloud applications?**</span></span>

<span data-ttu-id="38248-147">**S:** Du behöver bara konfigurera enkel inloggning (SSO) mellan din lokala katalog och AD Azure.</span><span class="sxs-lookup"><span data-stu-id="38248-147">**A:** You only need to set up single sign-on (SSO) between your on-premises directory and Azure AD.</span></span> <span data-ttu-id="38248-148">Så länge du kommer åt dina molnprogram via Azure AD ser tjänsten till att användarna autentiseras korrekt med deras lokala autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="38248-148">As long as you access your cloud applications through Azure AD, the service automatically drives your users to correctly authenticate with their on-premises credentials.</span></span>

<span data-ttu-id="38248-149">Du kan enkelt implementera enkel inloggning (SSO) från det lokala systemet med federationslösningar som Active Directory Federation Services (AD FS) eller genom att konfigurera hash-synkronisering av lösenord. Du kan enkelt distribuera båda alternativen med hjälp av Azure AD Connect-konfigurationsguiden.</span><span class="sxs-lookup"><span data-stu-id="38248-149">Implementing SSO from on-premises can be easily achieved with federation solutions such as Active Directory Federation Services (AD FS), or by configuring password hash sync. You can easily deploy both options by using the Azure AD Connect configuration wizard.</span></span>

<span data-ttu-id="38248-150">Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="38248-150">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="38248-151">**F: Tillhandahåller Azure AD en självbetjäningsportal för användare i organisationen?**</span><span class="sxs-lookup"><span data-stu-id="38248-151">**Q: Does Azure AD provide a self-service portal for users in my organization?**</span></span>

<span data-ttu-id="38248-152">**S:** Ja, Azure AD ger tillgång till [Azure AD-åtkomstpanelen](http://myapps.microsoft.com) för självbetjäning och programåtkomst.</span><span class="sxs-lookup"><span data-stu-id="38248-152">**A:** Yes, Azure AD provides you with the [Azure AD Access Panel](http://myapps.microsoft.com) for user self-service and application access.</span></span> <span data-ttu-id="38248-153">Om du är en Office 365-kund kan du hitta många av dessa funktioner på Office 365-portalen.</span><span class="sxs-lookup"><span data-stu-id="38248-153">If you are an Office 365 customer, you can find many of the same capabilities in the Office 365 portal.</span></span>

<span data-ttu-id="38248-154">Mer information finns i [Introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="38248-154">For more information, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

- - -
<span data-ttu-id="38248-155">**F: Kan Azure AD hjälpa mig att hantera min lokala infrastruktur?**</span><span class="sxs-lookup"><span data-stu-id="38248-155">**Q: Does Azure AD help me manage my on-premises infrastructure?**</span></span>

<span data-ttu-id="38248-156">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="38248-156">**A:** Yes.</span></span> <span data-ttu-id="38248-157">Azure AD Premium-versionen tillhandahåller Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="38248-157">The Azure AD Premium edition provides you with Azure AD Connect Health.</span></span> <span data-ttu-id="38248-158">Azure AD Connect Health hjälper dig att övervaka och få insyn i den lokala identitetsinfrastrukturen och synkroniseringstjänsterna.</span><span class="sxs-lookup"><span data-stu-id="38248-158">Azure AD Connect Health helps you monitor and gain insight into your on-premises identity infrastructure and the synchronization services.</span></span>  

<span data-ttu-id="38248-159">Mer information finns i [Övervaka den lokala identitetsinfrastrukturen och synkroniseringstjänster i molnet](active-directory-aadconnect-health.md).</span><span class="sxs-lookup"><span data-stu-id="38248-159">For more information, see [Monitor your on-premises identity infrastructure and synchronization services in the cloud](active-directory-aadconnect-health.md).</span></span>  

- - -
## <a name="password-management"></a><span data-ttu-id="38248-160">Lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="38248-160">Password management</span></span>
<span data-ttu-id="38248-161">**F: Kan jag använda tillbakaskrivning av lösenord i Azure AD utan lösenordssynkronisering? (Är det i det här scenariot möjligt att använda lösenordsåterställning via självbetjäning (SSPR) för Azure AD med tillbakaskrivning av lösenord och inte lagra lösenord i molnet?)**</span><span class="sxs-lookup"><span data-stu-id="38248-161">**Q: Can I use Azure AD password write-back without password sync? (In this scenario, is it possible to use Azure AD self-service password reset (SSPR) with password write-back and not store passwords in the cloud?)**</span></span>

<span data-ttu-id="38248-162">**S:** Du behöver inte synkronisera dina Active Directory-lösenord till Azure AD för att använda tillbakaskrivning.</span><span class="sxs-lookup"><span data-stu-id="38248-162">**A:** You do not need to synchronize your Active Directory passwords to Azure AD to enable write-back.</span></span> <span data-ttu-id="38248-163">I en federerad miljö använder enkel inloggning (SSO) i Azure AD den lokala katalogen för att autentisera användaren.</span><span class="sxs-lookup"><span data-stu-id="38248-163">In a federated environment, Azure AD single sign-on (SSO) relies on the on-premises directory to authenticate the user.</span></span> <span data-ttu-id="38248-164">I det här scenariot måste inte det lokala lösenordet spåras i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38248-164">This scenario does not require the on-premises password to be tracked in Azure AD.</span></span>

- - -
<span data-ttu-id="38248-165">**F: Hur lång tid tar det för ett lösenord att skrivas tillbaka till Active Directory lokalt?**</span><span class="sxs-lookup"><span data-stu-id="38248-165">**Q: How long does it take for a password to be written back to Active Directory on-premises?**</span></span>

<span data-ttu-id="38248-166">**S:** Tillbakaskrivningen av lösenord fungerar i realtid.</span><span class="sxs-lookup"><span data-stu-id="38248-166">**A:** Password write-back operates in real time.</span></span>

<span data-ttu-id="38248-167">Mer information finns i [Komma igång med lösenordshantering](active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="38248-167">For more information, see [Getting started with password management](active-directory-passwords-getting-started.md).</span></span>

- - -
<span data-ttu-id="38248-168">**F: Kan jag använda tillbakaskrivning av lösenord med lösenord som hanteras av en administratör?**</span><span class="sxs-lookup"><span data-stu-id="38248-168">**Q: Can I use password write-back with passwords that are managed by an admin?**</span></span>

<span data-ttu-id="38248-169">**S:** Ja, om du har aktiverat tillbakaskrivning av lösenord skrivs lösenordsåtgärder som utförs av en administratör tillbaka till din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="38248-169">**A:** Yes, if you have password write-back enabled, the password operations performed by an admin are written back to your on-premises environment.</span></span>  

<span data-ttu-id="38248-170">Fler svar på lösenordsrelaterade frågor finns i [Vanliga frågor och svar om lösenordshantering](active-directory-passwords-faq.md).</span><span class="sxs-lookup"><span data-stu-id="38248-170">For more answers to password-related questions, see [Password management frequently asked questions](active-directory-passwords-faq.md).</span></span>
- - -
<span data-ttu-id="38248-171">**F: Vad gör jag om jag inte kommer ihåg mitt befintliga Office 365-/Azure AD-lösenord när jag försöker ändra mitt lösenord?**</span><span class="sxs-lookup"><span data-stu-id="38248-171">**Q:  What can I do if I can't remember my existing Office 365/Azure AD password while trying to change my password?**</span></span>

<span data-ttu-id="38248-172">**S:** För den här typen av situation finns det ett par alternativ.</span><span class="sxs-lookup"><span data-stu-id="38248-172">**A:** For this type of situation, there are a couple of options.</span></span>  <span data-ttu-id="38248-173">Använd självbetjäning för återställning av lösenord (SSPR) om det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="38248-173">Use self-service password reset (SSPR) if it's available.</span></span>  <span data-ttu-id="38248-174">Huruvida SSPR fungerar eller ej beror på hur det är konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="38248-174">Whether SSPR works depends on how it's configured.</span></span>  <span data-ttu-id="38248-175">Mer information finns i avsnittet som beskriver [Hur portalen för lösenordsåterställning fungerar](active-directory-passwords-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="38248-175">For more information, see [How does the password reset portal work](active-directory-passwords-best-practices.md).</span></span>

<span data-ttu-id="38248-176">För Office 365-användare kan administratören återställa lösenordet med hjälp av stegen som beskrivs i [Återställa användares lösenord](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="38248-176">For Office 365 users, your admin can reset the password by using the steps outlined in [Reset user passwords](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span></span>

<span data-ttu-id="38248-177">För Azure AD-konton kan administratörer återställa lösenord med någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="38248-177">For Azure AD accounts, admins can reset passwords by using one of the following:</span></span>

- [<span data-ttu-id="38248-178">Återställa konton på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="38248-178">Reset accounts in the Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
- [<span data-ttu-id="38248-179">Återställa konton på den klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="38248-179">Reset accounts in the classic portal</span></span>](active-directory-create-users-reset-password.md)
- [<span data-ttu-id="38248-180">Använda PowerShell</span><span class="sxs-lookup"><span data-stu-id="38248-180">Using PowerShell</span></span>](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a><span data-ttu-id="38248-181">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="38248-181">Security</span></span>
<span data-ttu-id="38248-182">**Fråga: Låses konton efter ett visst antal misslyckade försök eller finns det en mer avancerad strategi?**</span><span class="sxs-lookup"><span data-stu-id="38248-182">**Q: Are accounts locked after a specific number of failed attempts or is there a more sophisticated strategy used?**</span></span></br>
<span data-ttu-id="38248-183">Vi använder en mer avancerad strategi för att låsa konton.</span><span class="sxs-lookup"><span data-stu-id="38248-183">We use a more sophisticated strategy to lock accounts.</span></span>  <span data-ttu-id="38248-184">Den är baserad på IP-adressen för begäran och det lösenord som anges.</span><span class="sxs-lookup"><span data-stu-id="38248-184">This is based on the IP of the request and the passwords entered.</span></span> <span data-ttu-id="38248-185">Låsningens varaktighet ökar också beroende på sannolikheten för att det rör sig om ett angrepp.</span><span class="sxs-lookup"><span data-stu-id="38248-185">The duration of the lockout also increases based on the likelihood that it is an attack.</span></span>  

<span data-ttu-id="38248-186">**Fråga: Vissa (vanliga) lösenord avvisas med meddelandet ”lösenordet har använts för många gånger”. Gäller detta lösenord som används i aktuell Active Directory?**</span><span class="sxs-lookup"><span data-stu-id="38248-186">**Q:  Certain (common) passwords get rejected with the messages ‘this password has been used to many times’, does this refer to passwords used in the current active directory?**</span></span></br>
<span data-ttu-id="38248-187">Det här gäller lösenord som är allmänt vanliga, till exempel varianter av "Lösenord" och "123456".</span><span class="sxs-lookup"><span data-stu-id="38248-187">This refers to passwords that are globally common, such as any variants of “Password” and “123456”.</span></span>

<span data-ttu-id="38248-188">**Fråga: Blockeras inloggningsbegäranden från misstänkta källor (botnät, Tor-slutpunkt) på en B2C-klient eller kräver detta att klienten har en Basic- eller Premium-utgåva?**</span><span class="sxs-lookup"><span data-stu-id="38248-188">**Q: Will a sign-in request from dubious sources (botnets, tor endpoint) be blocked in a B2C tenant or does this require a Basic or Premium edition tenant?**</span></span></br>
<span data-ttu-id="38248-189">Vi har en gateway som filtrerar begäranden och som ger ett visst skydd mot botnät. Den används för alla B2C-klienter.</span><span class="sxs-lookup"><span data-stu-id="38248-189">We do have a gateway that filters requests and provides some protection from botnets, and is applied for all B2C tenants.</span></span>

## <a name="application-access"></a><span data-ttu-id="38248-190">Programåtkomst</span><span class="sxs-lookup"><span data-stu-id="38248-190">Application access</span></span>
<span data-ttu-id="38248-191">**F: Var kan jag hitta en lista över program som redan är integrerade i Azure AD och deras funktioner?**</span><span class="sxs-lookup"><span data-stu-id="38248-191">**Q: Where can I find a list of applications that are pre-integrated with Azure AD and their capabilities?**</span></span>

<span data-ttu-id="38248-192">**S:** Azure AD har mer än 2 600 redan integrerade program från Microsoft, programtjänstleverantörer och partner.</span><span class="sxs-lookup"><span data-stu-id="38248-192">**A:** Azure AD has more than 2,600 pre-integrated applications from Microsoft, application service providers, and partners.</span></span> <span data-ttu-id="38248-193">Alla redan integrerade program stöder enkel inloggning (SSO).</span><span class="sxs-lookup"><span data-stu-id="38248-193">All pre-integrated applications support single sign-on (SSO).</span></span> <span data-ttu-id="38248-194">Med enkel inloggning kan du använda din organisations autentiseringsuppgifter för att komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="38248-194">SSO lets you use your organizational credentials to access your apps.</span></span> <span data-ttu-id="38248-195">Vissa program stöder även automatisk etablering och avetablering.</span><span class="sxs-lookup"><span data-stu-id="38248-195">Some of the applications also support automated provisioning and de-provisioning.</span></span>

<span data-ttu-id="38248-196">En fullständig lista över redan integrerade program finns på [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="38248-196">For a complete list of the pre-integrated applications, see the [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span></span>

- - -
<span data-ttu-id="38248-197">**F: Vad gör jag om jag inte hittar det program som jag behöver på Azure AD-Marketplace?**</span><span class="sxs-lookup"><span data-stu-id="38248-197">**Q: What if the application I need is not in the Azure AD marketplace?**</span></span>

<span data-ttu-id="38248-198">**S:** Med Azure AD Premium kan du lägga till och konfigurera de program du vill.</span><span class="sxs-lookup"><span data-stu-id="38248-198">**A:** With Azure AD Premium, you can add and configure any application that you want.</span></span> <span data-ttu-id="38248-199">Beroende på programmets funktioner och dina preferenser kan du konfigurera enkel inloggning (SSO) och automatisk etablering.</span><span class="sxs-lookup"><span data-stu-id="38248-199">Depending on your application’s capabilities and your preferences, you can configure SSO and automated provisioning.</span></span>  

<span data-ttu-id="38248-200">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="38248-200">For more information, see:</span></span>

* [<span data-ttu-id="38248-201">Konfigurera enkel inloggning för program som inte ingår i Azure Active Directory-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="38248-201">Configuring single sign-on to applications that are not in the Azure Active Directory application gallery</span></span>](active-directory-saas-custom-apps.md)
* [<span data-ttu-id="38248-202">Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program</span><span class="sxs-lookup"><span data-stu-id="38248-202">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)

- - -
<span data-ttu-id="38248-203">**F: Hur loggar användarna in i program med Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="38248-203">**Q: How do users sign in to applications by using Azure AD?**</span></span>

<span data-ttu-id="38248-204">**S:** Med Azure AD kan användarna visa och komma åt sina program på flera olika sätt, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="38248-204">**A:** Azure AD provides several ways for users to view and access their applications, such as:</span></span>

* <span data-ttu-id="38248-205">Azure AD-åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="38248-205">The Azure AD access panel</span></span>
* <span data-ttu-id="38248-206">Startprogrammet för Office 365</span><span class="sxs-lookup"><span data-stu-id="38248-206">The Office 365 application launcher</span></span>
* <span data-ttu-id="38248-207">Direkt inloggning till federerade appar</span><span class="sxs-lookup"><span data-stu-id="38248-207">Direct sign-in to federated apps</span></span>
* <span data-ttu-id="38248-208">Djuplänkar till federerade, lösenordsbaserade eller befintliga appar</span><span class="sxs-lookup"><span data-stu-id="38248-208">Deep links to federated, password-based, or existing apps</span></span>

<span data-ttu-id="38248-209">Mer information finns i [Distribuera Azure AD-integrerade program till användare](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="38248-209">For more information, see [Deploying Azure AD integrated applications to users](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>

- - -
<span data-ttu-id="38248-210">**F: På vilka sätt stöder Azure AD autentisering och enkel inloggning (SSO) i program?**</span><span class="sxs-lookup"><span data-stu-id="38248-210">**Q: What are the different ways Azure AD enables authentication and single sign-on to applications?**</span></span>

<span data-ttu-id="38248-211">**S:** Azure AD har stöd för många standardiserade protokoll för autentisering och auktorisering, till exempel SAML 2.0, OpenID Connect, OAuth 2.0 och WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="38248-211">**A:** Azure AD supports many standardized protocols for authentication and authorization, such as SAML 2.0, OpenID Connect, OAuth 2.0, and WS-Federation.</span></span> <span data-ttu-id="38248-212">Azure AD stöder också lösenordsvalv och automatisk inloggning för appar som endast har stöd för formulärbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="38248-212">Azure AD also supports password vaulting and automated sign-in capabilities for apps that only support forms-based authentication.</span></span>  

<span data-ttu-id="38248-213">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="38248-213">For more information, see:</span></span>

* [<span data-ttu-id="38248-214">Autentiseringsscenarier för Azure AD</span><span class="sxs-lookup"><span data-stu-id="38248-214">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)
* [<span data-ttu-id="38248-215">Active Directory-autentiseringsprotokoll</span><span class="sxs-lookup"><span data-stu-id="38248-215">Active Directory authentication protocols</span></span>](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [<span data-ttu-id="38248-216">Hur fungerar enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="38248-216">How does single sign-on with Azure Active Directory work?</span></span>](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
<span data-ttu-id="38248-217">**F: Kan jag lägga till program som jag kör lokalt?**</span><span class="sxs-lookup"><span data-stu-id="38248-217">**Q: Can I add applications I’m running on-premises?**</span></span>

<span data-ttu-id="38248-218">**S:** Azure AD Application Proxy ger enkel och säker åtkomst till lokala webbprogram som du väljer.</span><span class="sxs-lookup"><span data-stu-id="38248-218">**A:** Azure AD Application Proxy provides you with easy and secure access to on-premises web applications that you choose.</span></span> <span data-ttu-id="38248-219">Du kan komma åt dessa program på samma sätt som du kommer åt programvara som en tjänst (SaaS)-appar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38248-219">You can access these applications in the same way that you access your software as a service (SaaS) apps in Azure AD.</span></span> <span data-ttu-id="38248-220">Du behöver ingen VPN-anslutning och du behöver inte ändra din nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="38248-220">There is no need for a VPN or to change your network infrastructure.</span></span>  

<span data-ttu-id="38248-221">Mer information finns i [Ge säker fjärråtkomst till lokala program](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="38248-221">For more information, see [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md).</span></span>

- - -
<span data-ttu-id="38248-222">**F: Hur kräver jag Multi-Factor Authentication för användare som kommer åt ett visst program?**</span><span class="sxs-lookup"><span data-stu-id="38248-222">**Q: How do I require multi-factor authentication for users who access a particular application?**</span></span>

<span data-ttu-id="38248-223">**S:** Med villkorlig åtkomst i Azure AD kan du tilldela en unik åtkomstprincip för varje program.</span><span class="sxs-lookup"><span data-stu-id="38248-223">**A:** With Azure AD conditional access, you can assign a unique access policy for each application.</span></span> <span data-ttu-id="38248-224">Du kan välja att alltid kräva Multi-Factor Authentication eller bara kräva det när användarna inte är anslutna till det lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="38248-224">In your policy, you can require multi-factor authentication always, or when users are not connected to the local network.</span></span>  

<span data-ttu-id="38248-225">Mer information finns i [Skydda åtkomsten till Office 365 och andra appar som är anslutna till Azure Active Directory](active-directory-conditional-access.md).</span><span class="sxs-lookup"><span data-stu-id="38248-225">For more information, see [Securing access to Office 365 and other apps connected to Azure Active Directory](active-directory-conditional-access.md).</span></span>

- - -
<span data-ttu-id="38248-226">**F: Vad är automatisk användaretablering för SaaS-appar?**</span><span class="sxs-lookup"><span data-stu-id="38248-226">**Q: What is automated user provisioning for SaaS apps?**</span></span>

<span data-ttu-id="38248-227">**S:** Med Azure AD kan du automatisera genereringen, underhållet och borttagningen av användaridentiteter i många populära SaaS-appar i molnet.</span><span class="sxs-lookup"><span data-stu-id="38248-227">**A:** Use Azure AD to automate the creation, maintenance, and removal of user identities in many popular cloud SaaS apps.</span></span>

<span data-ttu-id="38248-228">Mer information finns i [Automatisera användaretablering och avetablering för SaaS-program med Azure Active Directory](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="38248-228">For more information, see [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](active-directory-saas-app-provisioning.md).</span></span>

- - -
<span data-ttu-id="38248-229">**F: Kan jag skapa en säker LDAP-anslutning med Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="38248-229">**Q:  Can I set up a secure LDAP connection with Azure AD?**</span></span>

<span data-ttu-id="38248-230">**S:** Nej.</span><span class="sxs-lookup"><span data-stu-id="38248-230">**A:**  No.</span></span> <span data-ttu-id="38248-231">Azure AD stöder inte LDAP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="38248-231">Azure AD does not support the LDAP protocol.</span></span>
