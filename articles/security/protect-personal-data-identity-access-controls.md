---
title: aaaProtect personliga data med Azure identitets- och kontroller | Microsoft Docs
description: "Med hjälp av Azure identitets- och kontroller toohelp skyddar du dina personliga data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3132c2af25f86662668e5b555eab1d81de7f2e6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a><span data-ttu-id="8713c-103">Azure Active Directory och Multi-Factor Authentication: skydda personuppgifter med identitets- och kontroller</span><span class="sxs-lookup"><span data-stu-id="8713c-103">Azure Active Directory and Multi-Factor Authentication: Protect personal data with identity and access controls</span></span>

<span data-ttu-id="8713c-104">Den här artikeln innehåller information och procedurer som du kan använda tooprotect personliga data med hjälp av Azure Active Directory och Multi-Factor authentication-säkerhetsfunktioner och tjänster.</span><span class="sxs-lookup"><span data-stu-id="8713c-104">This article provides information and procedures you can use tooprotect personal data using Azure Active Directory and Multi-factor authentication security features and services.</span></span>

## <a name="scenario"></a><span data-ttu-id="8713c-105">Scenario</span><span class="sxs-lookup"><span data-stu-id="8713c-105">Scenario</span></span>

<span data-ttu-id="8713c-106">Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna.</span><span class="sxs-lookup"><span data-stu-id="8713c-106">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="8713c-107">toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien Tyskland, Danmark och hello Storbritannien</span><span class="sxs-lookup"><span data-stu-id="8713c-107">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span> 

<span data-ttu-id="8713c-108">hello företag använder Microsoft Azure toostore företagsdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="8713c-108">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="8713c-109">Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas.</span><span class="sxs-lookup"><span data-stu-id="8713c-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="8713c-110">Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser.</span><span class="sxs-lookup"><span data-stu-id="8713c-110">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="8713c-111">hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.</span><span class="sxs-lookup"><span data-stu-id="8713c-111">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="8713c-112">Företagets anställda åtkomst hello nätverket från hello företagets fjärranslutna kontor och resa agenter finns runt hello world har åtkomst till företagsresurser för toosome.</span><span class="sxs-lookup"><span data-stu-id="8713c-112">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="8713c-113">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="8713c-113">Problem statement</span></span>

<span data-ttu-id="8713c-114">hello företag måste skydda hello sekretess kundernas och anställdas personliga data från angripare söker toouse komprometteras identiteter toogain åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8713c-114">hello company must protect hello privacy of customers’ and employees’ personal data from attackers seeking toouse compromised identities toogain access.</span></span> <span data-ttu-id="8713c-115">De måste kontrollera även att åtkomst toopersonal data genom att behöriga användare är begränsad till endast de som behöver den toodo sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="8713c-115">They also must ensure that access toopersonal data by legitimate users is restricted to only those who need it toodo their jobs.</span></span>

## <a name="company-goal"></a><span data-ttu-id="8713c-116">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="8713c-116">Company goal</span></span>

<span data-ttu-id="8713c-117">hello företagets mål är tooensure som toopersonal dataåtkomst strikt kontrollerad.</span><span class="sxs-lookup"><span data-stu-id="8713c-117">hello company’s goal is tooensure that access toopersonal data is strictly controlled.</span></span> <span data-ttu-id="8713c-118">Det är viktigt att identiteten för användare med åtkomst till toopersonal data skyddas av stark autentisering.</span><span class="sxs-lookup"><span data-stu-id="8713c-118">It is essential that identities of users with access toopersonal data be protected by strong authentication.</span></span> <span data-ttu-id="8713c-119">En princip av [minsta privilegium] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) måste vara tvingande så att behöriga användare har bara hello åtkomstnivå de behöver, och inga fler.</span><span class="sxs-lookup"><span data-stu-id="8713c-119">A policy of [least privilege] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) must be enforced so that legitimate users have only hello level of  access they need, and no more.</span></span>

## <a name="solutions"></a><span data-ttu-id="8713c-120">Lösningar</span><span class="sxs-lookup"><span data-stu-id="8713c-120">Solutions</span></span>

<span data-ttu-id="8713c-121">Microsoft Azure tillhandahåller identitets- och hanteringsverktyg toohelp företag kontrollera vem som har åtkomst till tooresources som innehåller personuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8713c-121">Microsoft Azure provides identity and access management tools toohelp companies control who has access tooresources that contain personal data.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="8713c-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8713c-122">Azure Active Directory</span></span>

<span data-ttu-id="8713c-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) hanterar identiteter och styr åtkomsten tooAzure samt andra lokalt och andra molnresurser, data och program.</span><span class="sxs-lookup"><span data-stu-id="8713c-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) manages identities and controls access tooAzure as well as other on-premises and other cloud resources, data, and applications.</span></span> <span data-ttu-id="8713c-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) hjälper Azure administratörer toominimize hello antalet personer som har åtkomst toocertain information, till exempel personuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8713c-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) helps Azure administrators toominimize hello number of people who have access toocertain information such as personal data.</span></span> <span data-ttu-id="8713c-125">Det möjliggör toodiscover, begränsa och övervaka Privilegierade identiteter och åtkomst tooresources och tooassign tillfälliga in JIT-administratörsrättigheter tooeligible användarna.</span><span class="sxs-lookup"><span data-stu-id="8713c-125">It enables them toodiscover, restrict, and monitor privileged identities and their access tooresources, and tooassign temporary, Just-In-Time (JIT) administrative rights tooeligible users.</span></span> <span data-ttu-id="8713c-126">Det ger också inblick i de som har administratörsbehörighet för AAD.</span><span class="sxs-lookup"><span data-stu-id="8713c-126">It also provides insight into those who have AAD administrative privileges.</span></span>

<span data-ttu-id="8713c-127">hello aktiviteter med hjälp av AAD PIM omfattar:</span><span class="sxs-lookup"><span data-stu-id="8713c-127">hello activities involved in using AAD PIM include:</span></span>

- <span data-ttu-id="8713c-128">Aktivera Privileged Identity Management för din katalog</span><span class="sxs-lookup"><span data-stu-id="8713c-128">Enabling Privileged Identity Management for your directory</span></span>

- <span data-ttu-id="8713c-129">Med Privileged Identity Management admin instrumentpanelen toosee viktig information på ett ögonblick</span><span class="sxs-lookup"><span data-stu-id="8713c-129">Using Privileged Identity Management admin dashboard toosee important information at a glance</span></span>

- <span data-ttu-id="8713c-130">Hantera hello Privilegierade identiteter (administratörer) genom att lägga till eller ta bort permanent eller är tillämpliga tooeach administratörsrollen</span><span class="sxs-lookup"><span data-stu-id="8713c-130">Managing hello privileged identities (administrators) by adding or removing permanent or eligible administrators tooeach role</span></span>

- <span data-ttu-id="8713c-131">Konfigurera hello produktaktivering rollinställningar</span><span class="sxs-lookup"><span data-stu-id="8713c-131">Configuring hello role activation settings</span></span>

- <span data-ttu-id="8713c-132">Aktivera roller</span><span class="sxs-lookup"><span data-stu-id="8713c-132">Activating roles</span></span>

- <span data-ttu-id="8713c-133">Granska rollen aktivitet</span><span class="sxs-lookup"><span data-stu-id="8713c-133">Reviewing role activity</span></span>

#### <a name="how-do-i-enable-aad-pim"></a><span data-ttu-id="8713c-134">Hur aktiverar AAD PIM?</span><span class="sxs-lookup"><span data-stu-id="8713c-134">How do I enable AAD PIM?</span></span>

<span data-ttu-id="8713c-135">toostart med hjälp av PIM för din katalog hello följande:</span><span class="sxs-lookup"><span data-stu-id="8713c-135">toostart using PIM for your directory, do hello following:</span></span>

1. <span data-ttu-id="8713c-136">Logga in toohello Azure-portalen som global administratör för din katalog.</span><span class="sxs-lookup"><span data-stu-id="8713c-136">Sign in toohello Azure portal as a global administrator of your directory.</span></span>

2. <span data-ttu-id="8713c-137">Om din organisation har mer än en katalog, väljer du ditt användarnamn i hello övre högra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8713c-137">If your organization has more than one directory, select your username in hello upper right-hand corner of hello Azure portal.</span></span> <span data-ttu-id="8713c-138">Välj hello katalog där du ska använda Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="8713c-138">Select hello directory where you will use Azure AD Privileged Identity Management.</span></span>

3. <span data-ttu-id="8713c-139">Välj **fler tjänster** och använda hello **Filter** textruta toosearch för Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="8713c-139">Select **More services** and use hello **Filter** textbox toosearch for Azure AD Privileged Identity Management.</span></span>

4. <span data-ttu-id="8713c-140">Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8713c-140">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="8713c-141">hello programmet Privileged Identity Management öppnas.</span><span class="sxs-lookup"><span data-stu-id="8713c-141">hello Privileged Identity Management application opens.</span></span>

<span data-ttu-id="8713c-142">När Azure AD Privileged Identity Management har konfigurerats, finns hello navigering bladet varje gång du öppnar programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8713c-142">Once Azure AD Privileged Identity Management is set up, you see hello navigation blade whenever you open hello application.</span></span>

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

<span data-ttu-id="8713c-143">Mer information och instruktioner för att komma igång med AAD PIM finns [starta med hjälp av Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span><span class="sxs-lookup"><span data-stu-id="8713c-143">For more information and instructions on getting started with AAD PIM, see [Start Using Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span></span>

### <a name="azure-role-based-access-control"></a><span data-ttu-id="8713c-144">Rollbaserad åtkomstkontroll i Azure</span><span class="sxs-lookup"><span data-stu-id="8713c-144">Azure Role-based Access Control</span></span>

<span data-ttu-id="8713c-145">[Azure rollbaserad åtkomstkontroll](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) hjälper Azure administratörer att hantera åtkomst tooAzure resurser genom att aktivera hello beviljas åtkomst baserat på hello användarens tilldelade rollen.</span><span class="sxs-lookup"><span data-stu-id="8713c-145">[Azure Role-Based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) helps Azure administrators manage access tooAzure resources by enabling hello granting of access based on hello user’s assigned role.</span></span> <span data-ttu-id="8713c-146">Du kan särskilja uppgifter i ett team och ge endast hello mängd åtkomst toousers, grupper och program som de behöver tooperform sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="8713c-146">You can segregate duties within a team and grant only hello amount of access toousers, groups and applications that they need tooperform their jobs.</span></span>

<span data-ttu-id="8713c-147">Rollbaserad åtkomst kan beviljas toousers med hello Azure-portalen, Azure kommandoradsverktyg eller Azure Management-API: er.</span><span class="sxs-lookup"><span data-stu-id="8713c-147">Role-based access can be granted toousers using hello Azure portal, Azure Command-Line tools or Azure Management APIs.</span></span>

<span data-ttu-id="8713c-148">Läs mer om grunderna i Azure RBAC [Kom igång med rollbaserad åtkomstkontroll i hello Azure-portalen.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span><span class="sxs-lookup"><span data-stu-id="8713c-148">For more information about Azure RBAC basics, see [Get started with Role-Based Access Control in hello Azure Portal.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span></span>

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a><span data-ttu-id="8713c-149">Hur hanterar Azure RBAC med PowerShell?</span><span class="sxs-lookup"><span data-stu-id="8713c-149">How do I manage Azure RBAC with PowerShell?</span></span>

<span data-ttu-id="8713c-150">Du kan använda PowerShell-cmdlets toomanage Azure RBAC, inklusive följande hanteringsuppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="8713c-150">You can use PowerShell cmdlets toomanage Azure RBAC, including hello following management tasks:</span></span>

- <span data-ttu-id="8713c-151">Lista roller</span><span class="sxs-lookup"><span data-stu-id="8713c-151">List roles</span></span>

- <span data-ttu-id="8713c-152">Se vem som har åtkomst</span><span class="sxs-lookup"><span data-stu-id="8713c-152">See who has access</span></span>

- <span data-ttu-id="8713c-153">Bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="8713c-153">Grant access</span></span>

- <span data-ttu-id="8713c-154">Ta bort åtkomst</span><span class="sxs-lookup"><span data-stu-id="8713c-154">Remove access</span></span>

- <span data-ttu-id="8713c-155">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="8713c-155">Create a custom role</span></span>

- <span data-ttu-id="8713c-156">Hämta åtgärder för en Resursprovider</span><span class="sxs-lookup"><span data-stu-id="8713c-156">Get Actions for a Resource Provider</span></span>

- <span data-ttu-id="8713c-157">Ändra en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="8713c-157">Modify a custom role</span></span>

- <span data-ttu-id="8713c-158">Ta bort en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="8713c-158">Delete a custom role</span></span>

- <span data-ttu-id="8713c-159">Lista över anpassade roller</span><span class="sxs-lookup"><span data-stu-id="8713c-159">List custom roles</span></span>

<span data-ttu-id="8713c-160">Anvisningar för hur toomanage Azure RBAC med PowerShell, se [Hantera rollbaserad åtkomst med Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span><span class="sxs-lookup"><span data-stu-id="8713c-160">For instructions on how toomanage Azure RBAC with PowerShell, see [Manage Role-based Access with Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span></span>

### <a name="azure-multi-factor-authentication"></a><span data-ttu-id="8713c-161">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="8713c-161">Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="8713c-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) är en lösning för verifiering av två steg som hjälper dig att skydda åtkomst toodata och program, samtidigt som du uppfyller efterfrågan från användarna för en process för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8713c-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) is a two-step verification solution that helps safeguard access toodata and applications, while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="8713c-163">Den ger stark autentisering via en mängd verifieringsmetoder, inklusive telefonsamtal, textmeddelande eller verifiering av mobilappar.</span><span class="sxs-lookup"><span data-stu-id="8713c-163">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

<span data-ttu-id="8713c-164">toodeploy MFA i hello Azure-molnet måste toofirst aktivera den och sedan aktivera tvåstegsverifiering för användare.</span><span class="sxs-lookup"><span data-stu-id="8713c-164">toodeploy MFA in hello Azure cloud, you need toofirst enable it and then turn on two-step verification for users.</span></span>

#### <a name="how-do-i-enable-azure-toouse-mfa"></a><span data-ttu-id="8713c-165">Hur aktiverar toouse Azure MFA?</span><span class="sxs-lookup"><span data-stu-id="8713c-165">How do I enable Azure toouse MFA?</span></span>

<span data-ttu-id="8713c-166">Om användarna har licenser som innehåller Azure Multi-Factor Authentication, finns det inget att du behöver toodo tooturn på Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="8713c-166">If your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need toodo tooturn on Azure MFA.</span></span> <span data-ttu-id="8713c-167">Om inte, du behöver toocreate en Multi-Factor Authentication-leverantör i din katalog.</span><span class="sxs-lookup"><span data-stu-id="8713c-167">If not, you need toocreate a Multi-Factor Auth provider in your directory.</span></span> <span data-ttu-id="8713c-168">toodo, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8713c-168">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="8713c-169">Välj **Active Directory** i hello klassiska Azure-portalen (logga in som administratör).</span><span class="sxs-lookup"><span data-stu-id="8713c-169">Select **Active Directory** in hello Azure classic portal (logged on as an administrator).</span></span>

2. <span data-ttu-id="8713c-170">Välj **Multi-Factor Authentication-Providers.**</span><span class="sxs-lookup"><span data-stu-id="8713c-170">Select **Multi-Factor Authentication Providers.**</span></span>

3. <span data-ttu-id="8713c-171">Välj **ny** och sedan under **Apptjänster,** Välj **leverantör av Multifaktorautent.**</span><span class="sxs-lookup"><span data-stu-id="8713c-171">Select **New,** and then under **App Services,** select **Multi-Factor Auth Provider.**</span></span>

4. <span data-ttu-id="8713c-172">Välj **Snabbregistrering.**</span><span class="sxs-lookup"><span data-stu-id="8713c-172">Select **Quick Create.**</span></span>

5. <span data-ttu-id="8713c-173">Fyll i fältet för hello namn och välj en användningsmodell (per autentisering eller per aktiverad användare).</span><span class="sxs-lookup"><span data-stu-id="8713c-173">Fill in hello name field and select a usage model (per authentication or per enabled user).</span></span>

6. <span data-ttu-id="8713c-174">Ange en katalog som hello MFA-leverantören är kopplad.</span><span class="sxs-lookup"><span data-stu-id="8713c-174">Designate a directory with which hello MFA Provider is associated.</span></span>

7. <span data-ttu-id="8713c-175">Klicka på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="8713c-175">Click hello **Create** button.</span></span>

![](media/protect-personal-data-identity-access-controls/quick-create.png)

<span data-ttu-id="8713c-176">Mer information om hur toomanage din leverantör av Multifaktorautent, se [komma igång med en Azure leverantör av Multifaktorautent.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span><span class="sxs-lookup"><span data-stu-id="8713c-176">For more instructions on how toomanage your Multi-Factor Auth Provider, see [Getting Started with an Azure Multi-Factor Auth Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span></span>

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a><span data-ttu-id="8713c-177">Hur aktiverar tvåstegsverifiering för användarna?</span><span class="sxs-lookup"><span data-stu-id="8713c-177">How do I turn on two-step verification for users?</span></span>

<span data-ttu-id="8713c-178">Du kan tillämpa tvåstegsverifiering för alla inloggningar eller skapa tvåstegsverifiering för villkorlig åtkomst principer toorequire endast när särskilda villkor gäller.</span><span class="sxs-lookup"><span data-stu-id="8713c-178">You can enforce two-step verification for all sign-ins, or you can create conditional access policies toorequire two-step verification only when specific conditions apply.</span></span>

<span data-ttu-id="8713c-179">Om du aktiverar Azure MFA genom att ändra användartillstånd är hello traditionella metoden för att kräva tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="8713c-179">Enabling Azure MFA by changing user states is hello traditional approach for requiring two-step verification.</span></span> <span data-ttu-id="8713c-180">Alla hello-användare som du aktiverar har hello samma krav tooperform tvåstegsverifiering varje gång de loggar in.</span><span class="sxs-lookup"><span data-stu-id="8713c-180">All hello users that you enable will have hello same requirement tooperform two-step verification every time they sign in.</span></span> <span data-ttu-id="8713c-181">Aktivera en användare åsidosätter alla principer för villkorlig åtkomst som kan påverka den användaren.</span><span class="sxs-lookup"><span data-stu-id="8713c-181">Enabling a user overrides any conditional access policies that may affect that user.</span></span>

<span data-ttu-id="8713c-182">Om du aktiverar Azure MFA med en princip för villkorlig åtkomst är ett mer flexibelt sätt för att kräva tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="8713c-182">Enabling Azure MFA with a conditional access policy is a more flexible approach for requiring two-step verification.</span></span> <span data-ttu-id="8713c-183">Du kan skapa principer för villkorlig åtkomst som gäller toogroups samt enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="8713c-183">You can create conditional access policies that apply toogroups as well as individual users.</span></span> <span data-ttu-id="8713c-184">Hög risk grupper kan få fler begränsningar än låg risk grupper eller tvåstegsverifiering kan krävs endast för hög risk molnappar och hoppades över för de låg risk.</span><span class="sxs-lookup"><span data-stu-id="8713c-184">High-risk groups can be given more restrictions than low-risk groups, or two-step verification can be required only for high-risk cloud apps and skipped for low-risk ones.</span></span> <span data-ttu-id="8713c-185">Villkorlig åtkomst är dock en betald funktion i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8713c-185">However, conditional access is a paid feature of Azure Active Directory.</span></span>

<span data-ttu-id="8713c-186">tooenable MFA genom att ändra användarens tillstånd, hello följande:</span><span class="sxs-lookup"><span data-stu-id="8713c-186">tooenable MFA by changing user state, do hello following:</span></span>

1. <span data-ttu-id="8713c-187">Logga in toohello Azure portal som administratör.</span><span class="sxs-lookup"><span data-stu-id="8713c-187">Sign in toohello Azure portal as an administrator.</span></span>
2. <span data-ttu-id="8713c-188">Gå för**Azure Active Directory \> användare och grupper \> alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8713c-188">Go too**Azure Active Directory \> Users and groups \> All users**.</span></span>
3. <span data-ttu-id="8713c-189">Välj **Multifaktorautentisering**.</span><span class="sxs-lookup"><span data-stu-id="8713c-189">Select **Multi-Factor Authentication**.</span></span>
4. <span data-ttu-id="8713c-190">Hitta hello användare som du vill tooenable för Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="8713c-190">Find hello user that you want tooenable for Azure MFA.</span></span> <span data-ttu-id="8713c-191">Du kan behöva toochange hello visa hello överst.</span><span class="sxs-lookup"><span data-stu-id="8713c-191">You may need toochange hello view at hello top.</span></span>
5. <span data-ttu-id="8713c-192">Kontrollera hello rutan nästa toohello användarens namn.</span><span class="sxs-lookup"><span data-stu-id="8713c-192">Check hello box next toohello user’s name.</span></span>
6. <span data-ttu-id="8713c-193">Välj på höger under snabbsteg hello, **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="8713c-193">On hello right, under quick steps, choose **Enable**.</span></span>

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. <span data-ttu-id="8713c-194">Bekräfta dina val i hello popup-fönster som öppnas.</span><span class="sxs-lookup"><span data-stu-id="8713c-194">Confirm your selection in hello pop-up window that opens.</span></span>  <span data-ttu-id="8713c-195">Användare som MFA har aktiverats måste tooregister hello nästa gång de loggar in.</span><span class="sxs-lookup"><span data-stu-id="8713c-195">Users for whom MFA has been enabled will be asked tooregister hello next time they sign in.</span></span>

<span data-ttu-id="8713c-196">tooenable Azure MFA med en princip för villkorlig åtkomst hello följande:</span><span class="sxs-lookup"><span data-stu-id="8713c-196">tooenable Azure MFA with a conditional access policy, do hello following:</span></span>

1. <span data-ttu-id="8713c-197">Logga in toohello Azure portal som administratör.</span><span class="sxs-lookup"><span data-stu-id="8713c-197">Sign in toohello Azure portal as an administrator.</span></span>

2. <span data-ttu-id="8713c-198">Gå för**Azure Active Directory \> villkorlig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="8713c-198">Go too**Azure Active Directory \> Conditional access**.</span></span>

3. <span data-ttu-id="8713c-199">Välj **ny princip**.</span><span class="sxs-lookup"><span data-stu-id="8713c-199">Select **New policy**.</span></span>

4. <span data-ttu-id="8713c-200">Under **tilldelningar**väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8713c-200">Under **Assignments**, select **Users and groups**.</span></span> <span data-ttu-id="8713c-201">Använd hello **inkludera** och **undanta** flikarna toospecify vilka användare och grupper som ska hanteras av hello princip.</span><span class="sxs-lookup"><span data-stu-id="8713c-201">Use hello **Include** and     **Exclude** tabs toospecify which users and groups will be managed by hello policy.</span></span>

5. <span data-ttu-id="8713c-202">Under **tilldelningar**väljer **Molnappar.**</span><span class="sxs-lookup"><span data-stu-id="8713c-202">Under **Assignments**, select **Cloud apps.**</span></span> <span data-ttu-id="8713c-203">Välj för**omfattar alla molnappar**.</span><span class="sxs-lookup"><span data-stu-id="8713c-203">Choose too**include All cloud apps**.</span></span>
6.  <span data-ttu-id="8713c-204">Under **åtkomstkontroller**väljer **bevilja**.</span><span class="sxs-lookup"><span data-stu-id="8713c-204">Under **Access controls**, select **Grant**.</span></span> <span data-ttu-id="8713c-205">Välj **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="8713c-205">Choose **Require multi-factor authentication**.</span></span>
7.  <span data-ttu-id="8713c-206">Aktivera **aktiverar principen** för**på** och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="8713c-206">Turn **Enable policy** too**On** and then select **Save**.</span></span>

<span data-ttu-id="8713c-207">Mer information om hur tooconfigure Azure MFA inställningar tooset in bedrägerivarningar, skapar en engångsförbikoppling kan använda anpassade röstmeddelanden, konfigurera cachelagring, ange tillförlitliga IP-adresser, skapa applösenord, aktivera komma ihåg MFA för enheter som användare litar på och välj verifieringsmetoderna, se [konfigurera inställningarna för Azure Multi-Factor Authentication.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span><span class="sxs-lookup"><span data-stu-id="8713c-207">For information on how tooconfigure Azure MFA settings tooset up fraud alerts, create a one-time bypass, use custom voice messages, configure caching, specify trusted IPs, create app passwords, enable remembering MFA for devices that users trust, and select verification methods, see [Configure Azure Multi-Factor Authentication Settings.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8713c-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8713c-208">Next steps</span></span>

- [<span data-ttu-id="8713c-209">Skydda privilegierad åtkomst i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8713c-209">Securing privileged access in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [<span data-ttu-id="8713c-210">Vanliga frågor om Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="8713c-210">Frequently asked questions about Azure Multi-Factor Authentication</span></span>](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [<span data-ttu-id="8713c-211">Rollbaserad åtkomstkontroll felsökning</span><span class="sxs-lookup"><span data-stu-id="8713c-211">Role-based Access Control troubleshooting</span></span>](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [<span data-ttu-id="8713c-212">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="8713c-212">Azure Active Directory Identity Protection</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
