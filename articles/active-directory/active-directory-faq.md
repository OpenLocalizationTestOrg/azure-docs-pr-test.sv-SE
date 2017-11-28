---
title: "aaaAzure Active Directory vanliga frågor och svar | Microsoft Docs"
description: "Azure Active Directory vanliga frågor och svar svar på frågor om hur tooaccess Azure och Azure Active Directory, lösenordshantering och program åt."
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
ms.openlocfilehash: 63c30c4aeda4551bf02c6b968f98cded5a3b2c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-faq"></a><span data-ttu-id="360d3-103">Vanliga frågor och svar om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="360d3-103">Azure Active Directory FAQ</span></span>
<span data-ttu-id="360d3-104">Azure Active Directory (Azure AD) är en omfattande IDaaS-lösning (Identity as a Service) som omfattar alla aspekter relaterade till identiteter, åtkomsthantering och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="360d3-104">Azure Active Directory (Azure AD) is a comprehensive identity as a service (IDaaS) solution that spans all aspects of identity, access management, and security.</span></span>

<span data-ttu-id="360d3-105">Mer information finns i [Vad är Azure Active Directory?](active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-105">For more information, see [What is Azure Active Directory?](active-directory-whatis.md).</span></span>


## <a name="access-azure-and-azure-active-directory"></a><span data-ttu-id="360d3-106">Kom åt Azure och Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="360d3-106">Access Azure and Azure Active Directory</span></span>
<span data-ttu-id="360d3-107">**F: Varför visas ”inga prenumerationer hittades” när jag försöker tooaccess Azure AD i hello klassiska Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="360d3-107">**Q: Why do I get “No subscriptions found” when I try tooaccess Azure AD in hello Azure classic portal?**</span></span>

<span data-ttu-id="360d3-108">**S:** tooaccess Hej klassiska Azure-portalen, alla användare måste ha behörigheter med en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="360d3-108">**A:** tooaccess hello Azure classic portal, each user needs permissions with an Azure subscription.</span></span> <span data-ttu-id="360d3-109">Om du har en betald Office 365 eller Azure AD-prenumeration går för[http://aka.ms/accessAAD](http://aka.ms/accessAAD) för engångsaktiveringen.</span><span class="sxs-lookup"><span data-stu-id="360d3-109">If you have a paid Office 365 or Azure AD subscription, go too[http://aka.ms/accessAAD](http://aka.ms/accessAAD) for a one-time activation step.</span></span> <span data-ttu-id="360d3-110">I annat fall behöver du ett kostnadsfritt tooactivate [Azure-konto](https://azure.microsoft.com/pricing/free-trial/) eller en betald prenumeration.</span><span class="sxs-lookup"><span data-stu-id="360d3-110">Otherwise, you will need tooactivate a free [Azure account](https://azure.microsoft.com/pricing/free-trial/) or a paid subscription.</span></span>

<span data-ttu-id="360d3-111">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="360d3-111">For more information, see:</span></span>

* [<span data-ttu-id="360d3-112">Hur Azure-prenumerationer är associerade med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="360d3-112">How Azure subscriptions are associated with Azure Active Directory</span></span>](active-directory-how-subscriptions-associated-directory.md)
* [<span data-ttu-id="360d3-113">Hantera hello katalogen för din Office 365-prenumeration i Azure</span><span class="sxs-lookup"><span data-stu-id="360d3-113">Manage hello directory for your Office 365 subscription in Azure</span></span>](active-directory-manage-o365-subscription.md)

- - -
<span data-ttu-id="360d3-114">**F: Vad är hello förhållandet mellan Azure AD och Office 365 och Azure?**</span><span class="sxs-lookup"><span data-stu-id="360d3-114">**Q: What’s hello relationship between Azure AD, Office 365, and Azure?**</span></span>

<span data-ttu-id="360d3-115">**S:** Azure AD innehåller gemensamma identitets- och åtkomstfunktioner tooall webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="360d3-115">**A:** Azure AD provides you with common identity and access capabilities tooall web services.</span></span> <span data-ttu-id="360d3-116">Om du använder Office 365, Microsoft Azure, Intune, eller andra, du är redan använder Azure AD toohelp aktivera inloggning och åtkomsthantering för alla dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="360d3-116">Whether you are using Office 365, Microsoft Azure, Intune, or others, you're already using Azure AD toohelp turn on sign-on and access management for all these services.</span></span>

<span data-ttu-id="360d3-117">Alla användare som har ställts in toouse webbtjänster definieras som användarkonton i en eller flera Azure AD-instanser.</span><span class="sxs-lookup"><span data-stu-id="360d3-117">All users who are set up toouse web services are defined as user accounts in one or more Azure AD instances.</span></span> <span data-ttu-id="360d3-118">Du kan ställa in dessa konton för kostnadsfria Azure AD-funktioner, till exempel programåtkomst i molnet.</span><span class="sxs-lookup"><span data-stu-id="360d3-118">You can set up these accounts for free Azure AD capabilities like cloud application access.</span></span>

<span data-ttu-id="360d3-119">Azure AD-betaltjänsterna som Enterprise Mobility + Security kompletterar andra webbtjänster som Office 365 och Microsoft Azure med heltäckande hanterings- och säkerhetslösningar i företagsklass.</span><span class="sxs-lookup"><span data-stu-id="360d3-119">Azure AD paid services like Enterprise Mobility + Security complement other web services like Office 365 and Microsoft Azure with comprehensive enterprise-scale management and security solutions.</span></span>
- - -
<span data-ttu-id="360d3-120">**F: Varför kan logga in toohello Azure-portalen men inte hello klassiska Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="360d3-120">**Q:  Why can I sign in toohello Azure portal but not hello Azure classic portal?**</span></span>

<span data-ttu-id="360d3-121">**S:** hello Azure-portalen kräver inte en giltig prenumeration och hello klassiska portalen kräver en giltig prenumeration.</span><span class="sxs-lookup"><span data-stu-id="360d3-121">**A:**  hello Azure portal does not require a valid subscription, and hello classic portal does require a valid subscription.</span></span>  <span data-ttu-id="360d3-122">Om du inte har en prenumeration kan du logga in toohello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="360d3-122">If you do not have a subscription, you can't sign in toohello classic portal.</span></span>
- - -
<span data-ttu-id="360d3-123">**F: Vad är hello skillnader mellan administratör för prenumeration och Directory-administratör?**</span><span class="sxs-lookup"><span data-stu-id="360d3-123">**Q:  What are hello differences between Subscription Administrator and Directory Administrator?**</span></span>

<span data-ttu-id="360d3-124">**S:** som standard du tilldelas hello prenumeration administratörsroll när du registrerar dig för Azure.</span><span class="sxs-lookup"><span data-stu-id="360d3-124">**A:** By default, you are assigned hello Subscription Administrator role when you sign up for Azure.</span></span> <span data-ttu-id="360d3-125">En prenumerationsadministratör för kan använda ett Microsoft-konto eller ett arbets eller skolkonto från hello-katalog som hello Azure-prenumeration är associerad med.</span><span class="sxs-lookup"><span data-stu-id="360d3-125">A subscription admin can use either a Microsoft account or a work or school account from hello directory that hello Azure subscription is associated with.</span></span>  <span data-ttu-id="360d3-126">Den här rollen är auktoriserade toomanage tjänster i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="360d3-126">This role is authorized toomanage services in hello Azure portal.</span></span>

<span data-ttu-id="360d3-127">Om andra behöver toosign i och komma åt tjänster med hjälp av Hej samma prenumeration, du kan lägga till dem som medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="360d3-127">If others need toosign in and access services by using hello same subscription, you can add them as co-admins.</span></span> <span data-ttu-id="360d3-128">Den här rollen har hello samma behörighet som hello tjänstadministratör, men kan inte ändra hello associationen mellan prenumerationer tooAzure kataloger.</span><span class="sxs-lookup"><span data-stu-id="360d3-128">This role has hello same access privileges as hello service admin, but can’t change hello association of subscriptions tooAzure directories.</span></span>  <span data-ttu-id="360d3-129">Ytterligare information om prenumerationsadministratörer finns [hur tooadd eller ändra Azure-administratörsroller](../billing-add-change-azure-subscription-administrator.md) och [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-129">For additional information on subscription admins, see [How tooadd or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) and [How Azure subscriptions are associated with Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span></span>


<span data-ttu-id="360d3-130">Azure AD har en annan uppsättning administrativa roller toomanage hello katalog- och identitetsrelaterade funktioner.</span><span class="sxs-lookup"><span data-stu-id="360d3-130">Azure AD has a different set of admin roles toomanage hello directory and identity-related features.</span></span>  <span data-ttu-id="360d3-131">Dessa administratörer ska ha åtkomst toovarious funktioner i hello Azure-portalen eller hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="360d3-131">These admins will have access toovarious features in hello Azure portal or hello Azure classic portal.</span></span> <span data-ttu-id="360d3-132">Hej administratör roll avgör vad de kan göra, som att skapa eller redigera användare, Tilldela administratörsroller tooothers, återställa användarlösenord, hantera användarlicenser eller hantera domäner.</span><span class="sxs-lookup"><span data-stu-id="360d3-132">hello admin's role determines what they can do, like create or edit users, assign administrative roles tooothers, reset user passwords, manage user licenses, or manage domains.</span></span>  <span data-ttu-id="360d3-133">Mer information om Azure AD-katalogadministratörer och deras roller finns i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-133">For additional information on Azure AD directory admins and their roles, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="360d3-134">Dessutom kompletterar Azure AD-betaltjänster som Enterprise Mobility + Security andra webbtjänster, som Office 365 och Microsoft Azure, med heltäckande hanterings- och säkerhetslösningar i företagsklass.</span><span class="sxs-lookup"><span data-stu-id="360d3-134">Additionally, Azure AD paid services like Enterprise Mobility + Security complement other web services, such as Office 365 and Microsoft Azure, with comprehensive enterprise-scale management and security solutions.</span></span>

- - -
<span data-ttu-id="360d3-135">**F: Finns det någon rapport som visar när mina Azure AD-användarlicenser upphör att gälla?**</span><span class="sxs-lookup"><span data-stu-id="360d3-135">**Q: Is there a report that shows when my Azure AD user licenses will expire?**</span></span>

<span data-ttu-id="360d3-136">**S:** Nej.</span><span class="sxs-lookup"><span data-stu-id="360d3-136">**A:** No.</span></span>  <span data-ttu-id="360d3-137">Det här är inte tillgängligt för närvarande.</span><span class="sxs-lookup"><span data-stu-id="360d3-137">This is not currently available.</span></span>

- - -

## <a name="get-started-with-hybrid-azure-ad"></a><span data-ttu-id="360d3-138">Kom igång med en Azure AD-hybridlösning</span><span class="sxs-lookup"><span data-stu-id="360d3-138">Get started with Hybrid Azure AD</span></span>


<span data-ttu-id="360d3-139">**F: Hur lämnar jag en klient när jag har lagts till som medarbetare?**</span><span class="sxs-lookup"><span data-stu-id="360d3-139">**Q: How do I leave a tenant when I am added as a collaborator?**</span></span>

<span data-ttu-id="360d3-140">**S:** när du har lagts till tooanother organisationens klient som en samarbetspartner, kan du använda hello ”klient switcher” i hello övre högra tooswitch mellan klienter behålls.</span><span class="sxs-lookup"><span data-stu-id="360d3-140">**A:** When you are added tooanother organization's tenant as a collaborator, you can use hello "tenant switcher" in hello upper right tooswitch between tenants.</span></span>  <span data-ttu-id="360d3-141">Det finns inget sätt tooleave hello bjuda in organisation för närvarande och Microsoft arbetar på att tillhandahålla den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="360d3-141">Currently, there is no way tooleave hello inviting organization, and Microsoft is working on providing this functionality.</span></span>  <span data-ttu-id="360d3-142">Tills den här funktionen finns tillgänglig, kan du be hello bjuda in organisation tooremove du från klienten.</span><span class="sxs-lookup"><span data-stu-id="360d3-142">Until this feature is available, you can ask hello inviting organization tooremove you from their tenant.</span></span>
- - -
<span data-ttu-id="360d3-143">**F: hur kan jag ansluta min lokala katalog tooAzure AD?**</span><span class="sxs-lookup"><span data-stu-id="360d3-143">**Q: How can I connect my on-premises directory tooAzure AD?**</span></span>

<span data-ttu-id="360d3-144">**S:** kan du ansluta din lokala katalog tooAzure AD med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="360d3-144">**A:** You can connect your on-premises directory tooAzure AD by using Azure AD Connect.</span></span>

<span data-ttu-id="360d3-145">Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-145">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="360d3-146">**F: Hur konfigurerar jag enkel inloggning (SSO) mellan min lokala katalog och mina molnprogram?**</span><span class="sxs-lookup"><span data-stu-id="360d3-146">**Q: How do I set up SSO between my on-premises directory and my cloud applications?**</span></span>

<span data-ttu-id="360d3-147">**S:** behöver du bara tooset in enkel inloggning (SSO) mellan din lokala katalog och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="360d3-147">**A:** You only need tooset up single sign-on (SSO) between your on-premises directory and Azure AD.</span></span> <span data-ttu-id="360d3-148">Så länge som du kommer åt dina molnprogram via Azure AD enheter hello service automatiskt användarna toocorrectly autentiseras med deras lokala autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="360d3-148">As long as you access your cloud applications through Azure AD, hello service automatically drives your users toocorrectly authenticate with their on-premises credentials.</span></span>

<span data-ttu-id="360d3-149">Du kan enkelt implementera enkel inloggning (SSO) från det lokala systemet med federationslösningar som Active Directory Federation Services (AD FS) eller genom att konfigurera hash-synkronisering av lösenord. Du kan enkelt distribuera båda alternativen med hjälp av hello Azure AD Connect-konfigurationsguiden.</span><span class="sxs-lookup"><span data-stu-id="360d3-149">Implementing SSO from on-premises can be easily achieved with federation solutions such as Active Directory Federation Services (AD FS), or by configuring password hash sync. You can easily deploy both options by using hello Azure AD Connect configuration wizard.</span></span>

<span data-ttu-id="360d3-150">Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-150">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="360d3-151">**F: Tillhandahåller Azure AD en självbetjäningsportal för användare i organisationen?**</span><span class="sxs-lookup"><span data-stu-id="360d3-151">**Q: Does Azure AD provide a self-service portal for users in my organization?**</span></span>

<span data-ttu-id="360d3-152">**S:** Ja, Azure AD innehåller hello [Azure AD-åtkomstpanelen](http://myapps.microsoft.com) för självbetjäning och programåtkomst.</span><span class="sxs-lookup"><span data-stu-id="360d3-152">**A:** Yes, Azure AD provides you with hello [Azure AD Access Panel](http://myapps.microsoft.com) for user self-service and application access.</span></span> <span data-ttu-id="360d3-153">Om du är en Office 365-kund kan hitta du många av hello samma funktioner i hello Office 365-portalen.</span><span class="sxs-lookup"><span data-stu-id="360d3-153">If you are an Office 365 customer, you can find many of hello same capabilities in hello Office 365 portal.</span></span>

<span data-ttu-id="360d3-154">Mer information finns i [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-154">For more information, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

- - -
<span data-ttu-id="360d3-155">**F: Kan Azure AD hjälpa mig att hantera min lokala infrastruktur?**</span><span class="sxs-lookup"><span data-stu-id="360d3-155">**Q: Does Azure AD help me manage my on-premises infrastructure?**</span></span>

<span data-ttu-id="360d3-156">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="360d3-156">**A:** Yes.</span></span> <span data-ttu-id="360d3-157">hello Azure AD Premium edition ger Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="360d3-157">hello Azure AD Premium edition provides you with Azure AD Connect Health.</span></span> <span data-ttu-id="360d3-158">Azure AD Connect Health hjälper dig att övervaka och få insyn i din lokala identitet infrastruktur och hello synkroniseringstjänsterna.</span><span class="sxs-lookup"><span data-stu-id="360d3-158">Azure AD Connect Health helps you monitor and gain insight into your on-premises identity infrastructure and hello synchronization services.</span></span>  

<span data-ttu-id="360d3-159">Mer information finns i [övervaka din lokala identitet infrastruktur och synkroniseringstjänster i molnet hello](active-directory-aadconnect-health.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-159">For more information, see [Monitor your on-premises identity infrastructure and synchronization services in hello cloud](active-directory-aadconnect-health.md).</span></span>  

- - -
## <a name="password-management"></a><span data-ttu-id="360d3-160">Lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="360d3-160">Password management</span></span>
<span data-ttu-id="360d3-161">**F: Kan jag använda tillbakaskrivning av lösenord i Azure AD utan lösenordssynkronisering? (I det här fallet är det möjligt toouse Azure AD lösenordsåterställning via självbetjäning (SSPR) med lösenord återskrivning och inte lagra lösenord i molnet hello?)**</span><span class="sxs-lookup"><span data-stu-id="360d3-161">**Q: Can I use Azure AD password write-back without password sync? (In this scenario, is it possible toouse Azure AD self-service password reset (SSPR) with password write-back and not store passwords in hello cloud?)**</span></span>

<span data-ttu-id="360d3-162">**S:** behöver du inte toosynchronize din Active Directory lösenord tooAzure AD tooenable tillbakaskrivning.</span><span class="sxs-lookup"><span data-stu-id="360d3-162">**A:** You do not need toosynchronize your Active Directory passwords tooAzure AD tooenable write-back.</span></span> <span data-ttu-id="360d3-163">Förlitar sig på hello lokalt directory tooauthenticate hello användaren Azure AD enkel inloggning (SSO) i en federerad miljö.</span><span class="sxs-lookup"><span data-stu-id="360d3-163">In a federated environment, Azure AD single sign-on (SSO) relies on hello on-premises directory tooauthenticate hello user.</span></span> <span data-ttu-id="360d3-164">Det här scenariot kräver inte hello lokala lösenord toobe spåras i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="360d3-164">This scenario does not require hello on-premises password toobe tracked in Azure AD.</span></span>

- - -
<span data-ttu-id="360d3-165">**F: hur lång tid tar det för ett lösenord toobe skrivs tillbaka tooActive Directory lokalt?**</span><span class="sxs-lookup"><span data-stu-id="360d3-165">**Q: How long does it take for a password toobe written back tooActive Directory on-premises?**</span></span>

<span data-ttu-id="360d3-166">**S:** Tillbakaskrivningen av lösenord fungerar i realtid.</span><span class="sxs-lookup"><span data-stu-id="360d3-166">**A:** Password write-back operates in real time.</span></span>

<span data-ttu-id="360d3-167">Mer information finns i [Komma igång med lösenordshantering](active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-167">For more information, see [Getting started with password management](active-directory-passwords-getting-started.md).</span></span>

- - -
<span data-ttu-id="360d3-168">**F: Kan jag använda tillbakaskrivning av lösenord med lösenord som hanteras av en administratör?**</span><span class="sxs-lookup"><span data-stu-id="360d3-168">**Q: Can I use password write-back with passwords that are managed by an admin?**</span></span>

<span data-ttu-id="360d3-169">**S:** Ja, om du har tillbakaskrivning av lösenord aktiverat hello lösenordsåtgärder som utförs av en administratör skrivs tillbaka tooyour lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="360d3-169">**A:** Yes, if you have password write-back enabled, hello password operations performed by an admin are written back tooyour on-premises environment.</span></span>  

<span data-ttu-id="360d3-170">Fler svar toopassword-relaterade frågor finns i [vanliga frågor och svar om lösenordshantering](active-directory-passwords-faq.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-170">For more answers toopassword-related questions, see [Password management frequently asked questions](active-directory-passwords-faq.md).</span></span>
- - -
<span data-ttu-id="360d3-171">**F: Vad gör jag om jag inte kommer ihåg mina befintliga Office 365-/ Azure AD-lösenord uppstod toochange mitt lösenord?**</span><span class="sxs-lookup"><span data-stu-id="360d3-171">**Q:  What can I do if I can't remember my existing Office 365/Azure AD password while trying toochange my password?**</span></span>

<span data-ttu-id="360d3-172">**S:** För den här typen av situation finns det ett par alternativ.</span><span class="sxs-lookup"><span data-stu-id="360d3-172">**A:** For this type of situation, there are a couple of options.</span></span>  <span data-ttu-id="360d3-173">Använd självbetjäning för återställning av lösenord (SSPR) om det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="360d3-173">Use self-service password reset (SSPR) if it's available.</span></span>  <span data-ttu-id="360d3-174">Huruvida SSPR fungerar eller ej beror på hur det är konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="360d3-174">Whether SSPR works depends on how it's configured.</span></span>  <span data-ttu-id="360d3-175">Mer information finns i [hur hello lösenordsåterställning portal arbete](active-directory-passwords-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-175">For more information, see [How does hello password reset portal work](active-directory-passwords-best-practices.md).</span></span>

<span data-ttu-id="360d3-176">För Office 365-användare kan administratören återställa hello lösenord med hjälp av hello steg som beskrivs i [återställa användarlösenord](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="360d3-176">For Office 365 users, your admin can reset hello password by using hello steps outlined in [Reset user passwords](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span></span>

<span data-ttu-id="360d3-177">Administratörer kan återställa lösenord med hjälp av en av följande hello för Azure AD-konton:</span><span class="sxs-lookup"><span data-stu-id="360d3-177">For Azure AD accounts, admins can reset passwords by using one of hello following:</span></span>

- [<span data-ttu-id="360d3-178">Återställ konton i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="360d3-178">Reset accounts in hello Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
- [<span data-ttu-id="360d3-179">Återställ konton i hello klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="360d3-179">Reset accounts in hello classic portal</span></span>](active-directory-create-users-reset-password.md)
- [<span data-ttu-id="360d3-180">Använda PowerShell</span><span class="sxs-lookup"><span data-stu-id="360d3-180">Using PowerShell</span></span>](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a><span data-ttu-id="360d3-181">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="360d3-181">Security</span></span>
<span data-ttu-id="360d3-182">**Fråga: Låses konton efter ett visst antal misslyckade försök eller finns det en mer avancerad strategi?**</span><span class="sxs-lookup"><span data-stu-id="360d3-182">**Q: Are accounts locked after a specific number of failed attempts or is there a more sophisticated strategy used?**</span></span></br>
<span data-ttu-id="360d3-183">Vi använder en mer avancerad strategi toolock konton.</span><span class="sxs-lookup"><span data-stu-id="360d3-183">We use a more sophisticated strategy toolock accounts.</span></span>  <span data-ttu-id="360d3-184">Detta baseras på hello IP hello begäran och hello lösenorden.</span><span class="sxs-lookup"><span data-stu-id="360d3-184">This is based on hello IP of hello request and hello passwords entered.</span></span> <span data-ttu-id="360d3-185">hello varaktighet hello kontoutelåsning ökar baserat på hello sannolikheten att det är en attack.</span><span class="sxs-lookup"><span data-stu-id="360d3-185">hello duration of hello lockout also increases based on hello likelihood that it is an attack.</span></span>  

<span data-ttu-id="360d3-186">**F: vissa (common) lösenord hämta nekas med hello meddelanden 'lösenordet har använt toomany gånger', refererar toopasswords som används i hello aktuell active directory?**</span><span class="sxs-lookup"><span data-stu-id="360d3-186">**Q:  Certain (common) passwords get rejected with hello messages ‘this password has been used toomany times’, does this refer toopasswords used in hello current active directory?**</span></span></br>
<span data-ttu-id="360d3-187">Detta refererar toopasswords globalt vanliga som varianter av ”Password” och ”123456”.</span><span class="sxs-lookup"><span data-stu-id="360d3-187">This refers toopasswords that are globally common, such as any variants of “Password” and “123456”.</span></span>

<span data-ttu-id="360d3-188">**Fråga: Blockeras inloggningsbegäranden från misstänkta källor (botnät, Tor-slutpunkt) på en B2C-klient eller kräver detta att klienten har en Basic- eller Premium-utgåva?**</span><span class="sxs-lookup"><span data-stu-id="360d3-188">**Q: Will a sign-in request from dubious sources (botnets, tor endpoint) be blocked in a B2C tenant or does this require a Basic or Premium edition tenant?**</span></span></br>
<span data-ttu-id="360d3-189">Vi har en gateway som filtrerar begäranden och som ger ett visst skydd mot botnät. Den används för alla B2C-klienter.</span><span class="sxs-lookup"><span data-stu-id="360d3-189">We do have a gateway that filters requests and provides some protection from botnets, and is applied for all B2C tenants.</span></span>

## <a name="application-access"></a><span data-ttu-id="360d3-190">Programåtkomst</span><span class="sxs-lookup"><span data-stu-id="360d3-190">Application access</span></span>
<span data-ttu-id="360d3-191">**F: Var kan jag hitta en lista över program som redan är integrerade i Azure AD och deras funktioner?**</span><span class="sxs-lookup"><span data-stu-id="360d3-191">**Q: Where can I find a list of applications that are pre-integrated with Azure AD and their capabilities?**</span></span>

<span data-ttu-id="360d3-192">**S:** Azure AD har mer än 2 600 redan integrerade program från Microsoft, programtjänstleverantörer och partner.</span><span class="sxs-lookup"><span data-stu-id="360d3-192">**A:** Azure AD has more than 2,600 pre-integrated applications from Microsoft, application service providers, and partners.</span></span> <span data-ttu-id="360d3-193">Alla redan integrerade program stöder enkel inloggning (SSO).</span><span class="sxs-lookup"><span data-stu-id="360d3-193">All pre-integrated applications support single sign-on (SSO).</span></span> <span data-ttu-id="360d3-194">Enkel inloggning kan du använda din organisations autentiseringsuppgifter tooaccess dina appar.</span><span class="sxs-lookup"><span data-stu-id="360d3-194">SSO lets you use your organizational credentials tooaccess your apps.</span></span> <span data-ttu-id="360d3-195">Vissa av hello program stöder även Automatisk etablering och avetablering.</span><span class="sxs-lookup"><span data-stu-id="360d3-195">Some of hello applications also support automated provisioning and de-provisioning.</span></span>

<span data-ttu-id="360d3-196">En fullständig lista över hello redan integrerade program finns i hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="360d3-196">For a complete list of hello pre-integrated applications, see hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span></span>

- - -
<span data-ttu-id="360d3-197">**F: Vad händer om hello program som jag behöver är inte i hello Azure AD-marketplace?**</span><span class="sxs-lookup"><span data-stu-id="360d3-197">**Q: What if hello application I need is not in hello Azure AD marketplace?**</span></span>

<span data-ttu-id="360d3-198">**S:** Med Azure AD Premium kan du lägga till och konfigurera de program du vill.</span><span class="sxs-lookup"><span data-stu-id="360d3-198">**A:** With Azure AD Premium, you can add and configure any application that you want.</span></span> <span data-ttu-id="360d3-199">Beroende på programmets funktioner och dina preferenser kan du konfigurera enkel inloggning (SSO) och automatisk etablering.</span><span class="sxs-lookup"><span data-stu-id="360d3-199">Depending on your application’s capabilities and your preferences, you can configure SSO and automated provisioning.</span></span>  

<span data-ttu-id="360d3-200">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="360d3-200">For more information, see:</span></span>

* [<span data-ttu-id="360d3-201">Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="360d3-201">Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery</span></span>](active-directory-saas-custom-apps.md)
* [<span data-ttu-id="360d3-202">Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="360d3-202">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)

- - -
<span data-ttu-id="360d3-203">**F: hur användare loggar in tooapplications med hjälp av Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="360d3-203">**Q: How do users sign in tooapplications by using Azure AD?**</span></span>

<span data-ttu-id="360d3-204">**S:** Azure AD innehåller flera olika sätt för användarna tooview och komma åt sina program, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="360d3-204">**A:** Azure AD provides several ways for users tooview and access their applications, such as:</span></span>

* <span data-ttu-id="360d3-205">hello Azure AD-åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="360d3-205">hello Azure AD access panel</span></span>
* <span data-ttu-id="360d3-206">startprogram för hello Office 365</span><span class="sxs-lookup"><span data-stu-id="360d3-206">hello Office 365 application launcher</span></span>
* <span data-ttu-id="360d3-207">Direkt inloggning toofederated appar</span><span class="sxs-lookup"><span data-stu-id="360d3-207">Direct sign-in toofederated apps</span></span>
* <span data-ttu-id="360d3-208">Djup länkar toofederated, lösenordsbaserade eller befintliga appar</span><span class="sxs-lookup"><span data-stu-id="360d3-208">Deep links toofederated, password-based, or existing apps</span></span>

<span data-ttu-id="360d3-209">Mer information finns i [distribuera Azure AD-integrerade program toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="360d3-209">For more information, see [Deploying Azure AD integrated applications toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>

- - -
<span data-ttu-id="360d3-210">**F: Vad är hello olika sätt Azure AD aktiverar autentisering och enkel inloggning tooapplications?**</span><span class="sxs-lookup"><span data-stu-id="360d3-210">**Q: What are hello different ways Azure AD enables authentication and single sign-on tooapplications?**</span></span>

<span data-ttu-id="360d3-211">**S:** Azure AD har stöd för många standardiserade protokoll för autentisering och auktorisering, till exempel SAML 2.0, OpenID Connect, OAuth 2.0 och WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="360d3-211">**A:** Azure AD supports many standardized protocols for authentication and authorization, such as SAML 2.0, OpenID Connect, OAuth 2.0, and WS-Federation.</span></span> <span data-ttu-id="360d3-212">Azure AD stöder också lösenordsvalv och automatisk inloggning för appar som endast har stöd för formulärbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="360d3-212">Azure AD also supports password vaulting and automated sign-in capabilities for apps that only support forms-based authentication.</span></span>  

<span data-ttu-id="360d3-213">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="360d3-213">For more information, see:</span></span>

* [<span data-ttu-id="360d3-214">Autentiseringsscenarier för Azure AD</span><span class="sxs-lookup"><span data-stu-id="360d3-214">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)
* [<span data-ttu-id="360d3-215">Active Directory-autentiseringsprotokoll</span><span class="sxs-lookup"><span data-stu-id="360d3-215">Active Directory authentication protocols</span></span>](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [<span data-ttu-id="360d3-216">Hur fungerar enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="360d3-216">How does single sign-on with Azure Active Directory work?</span></span>](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
<span data-ttu-id="360d3-217">**F: Kan jag lägga till program som jag kör lokalt?**</span><span class="sxs-lookup"><span data-stu-id="360d3-217">**Q: Can I add applications I’m running on-premises?**</span></span>

<span data-ttu-id="360d3-218">**S:** Azure AD Application Proxy ger dig enkel och säker åtkomst tooon lokala webbprogram som du väljer.</span><span class="sxs-lookup"><span data-stu-id="360d3-218">**A:** Azure AD Application Proxy provides you with easy and secure access tooon-premises web applications that you choose.</span></span> <span data-ttu-id="360d3-219">Du kan komma åt dessa program i hello samma hur du kommer åt din programvara som en tjänst (SaaS)-appar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="360d3-219">You can access these applications in hello same way that you access your software as a service (SaaS) apps in Azure AD.</span></span> <span data-ttu-id="360d3-220">Det finns inget behov av en VPN- eller toochange nätverksinfrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="360d3-220">There is no need for a VPN or toochange your network infrastructure.</span></span>  

<span data-ttu-id="360d3-221">Mer information finns i [hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-221">For more information, see [How tooprovide secure remote access tooon-premises applications](active-directory-application-proxy-get-started.md).</span></span>

- - -
<span data-ttu-id="360d3-222">**F: Hur kräver jag Multi-Factor Authentication för användare som kommer åt ett visst program?**</span><span class="sxs-lookup"><span data-stu-id="360d3-222">**Q: How do I require multi-factor authentication for users who access a particular application?**</span></span>

<span data-ttu-id="360d3-223">**S:** Med villkorlig åtkomst i Azure AD kan du tilldela en unik åtkomstprincip för varje program.</span><span class="sxs-lookup"><span data-stu-id="360d3-223">**A:** With Azure AD conditional access, you can assign a unique access policy for each application.</span></span> <span data-ttu-id="360d3-224">Du kan kräva multifaktorautentisering alltid i en princip eller när användarna inte är anslutna toohello lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="360d3-224">In your policy, you can require multi-factor authentication always, or when users are not connected toohello local network.</span></span>  

<span data-ttu-id="360d3-225">Mer information finns i [säkerhetsåtkomst tooOffice 365 och andra appar anslutna tooAzure Active Directory](active-directory-conditional-access.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-225">For more information, see [Securing access tooOffice 365 and other apps connected tooAzure Active Directory](active-directory-conditional-access.md).</span></span>

- - -
<span data-ttu-id="360d3-226">**F: Vad är automatisk användaretablering för SaaS-appar?**</span><span class="sxs-lookup"><span data-stu-id="360d3-226">**Q: What is automated user provisioning for SaaS apps?**</span></span>

<span data-ttu-id="360d3-227">**S:** använda Azure AD tooautomate hello skapande, underhållet och borttagningen av användaridentiteter i många populära cloud SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="360d3-227">**A:** Use Azure AD tooautomate hello creation, maintenance, and removal of user identities in many popular cloud SaaS apps.</span></span>

<span data-ttu-id="360d3-228">Mer information finns i [automatisera användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="360d3-228">For more information, see [Automate user provisioning and deprovisioning tooSaaS applications with Azure Active Directory](active-directory-saas-app-provisioning.md).</span></span>

- - -
<span data-ttu-id="360d3-229">**F: Kan jag skapa en säker LDAP-anslutning med Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="360d3-229">**Q:  Can I set up a secure LDAP connection with Azure AD?**</span></span>

<span data-ttu-id="360d3-230">**S:** Nej.</span><span class="sxs-lookup"><span data-stu-id="360d3-230">**A:**  No.</span></span> <span data-ttu-id="360d3-231">Azure AD stöder inte hello LDAP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="360d3-231">Azure AD does not support hello LDAP protocol.</span></span>
