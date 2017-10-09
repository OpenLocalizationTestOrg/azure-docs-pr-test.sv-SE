---
title: aaaUnexpected programmet i listan med mina program | Microsoft Docs
description: "Hur toosee alla program i din klient och förstå hur program visas i listan alla program under företagsprogram"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2f974046b655bc36a05f002c56511a8a988cd01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-application-in-my-applications-list"></a><span data-ttu-id="38f13-103">Oväntat programmet i listan med mina program</span><span class="sxs-lookup"><span data-stu-id="38f13-103">Unexpected application in my applications list</span></span>

<span data-ttu-id="38f13-104">Den här artikeln hjälper dig toounderstand hur program visas i din **alla program** listan **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="38f13-104">This article help you toounderstand how applications appear in your **All Applications** list under **Enterprise Applications**.</span></span> 

## <a name="how-toosee-all-applications-in-your-tenant"></a><span data-ttu-id="38f13-105">Hur toosee alla program i din klient</span><span class="sxs-lookup"><span data-stu-id="38f13-105">How toosee all applications in your tenant</span></span>

<span data-ttu-id="38f13-106">toosee alla hello program i din klient, måste du toouse hello **Filter** styra tooshow **alla program** under hello **alla program** lista.</span><span class="sxs-lookup"><span data-stu-id="38f13-106">toosee all hello applications in your tenant, you need toouse hello **Filter** control tooshow **All Applications** under hello **All Applications** list.</span></span> <span data-ttu-id="38f13-107">toodo detta, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="38f13-107">toodo this, follow hello steps below:</span></span>

1.  <span data-ttu-id="38f13-108">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="38f13-108">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="38f13-109">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="38f13-109">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="38f13-110">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="38f13-110">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="38f13-111">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="38f13-111">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="38f13-112">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="38f13-112">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="38f13-113">Klicka på hello Använd hello **Filter** kontroll hello överst i hello **listan med alla program**.</span><span class="sxs-lookup"><span data-stu-id="38f13-113">click hello use hello **Filter** control at hello top of hello **All Applications List**.</span></span>

7.  <span data-ttu-id="38f13-114">På hello **Filter** bladet, ange hello **visa** alternativet för**alla program.**</span><span class="sxs-lookup"><span data-stu-id="38f13-114">On hello **Filter** blade, set hello **Show** option too**All Applications.**</span></span>

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a><span data-ttu-id="38f13-115">Varför visas ett visst program i programlistan alla?</span><span class="sxs-lookup"><span data-stu-id="38f13-115">Why does a specific application appear in my all applications list?</span></span>

<span data-ttu-id="38f13-116">När filtreras för**alla program**, hello **alla program** **listan** visas alla objekt för tjänstens huvudnamn i din klient.</span><span class="sxs-lookup"><span data-stu-id="38f13-116">When filtered too**All Applications**, hello **All Applications** **List** shows every Service Principal object in your tenant.</span></span> <span data-ttu-id="38f13-117">Tjänstens huvudnamn objekt som kan visas i listan på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="38f13-117">Service Principal objects can appear in this list in a various ways:</span></span>

1.  <span data-ttu-id="38f13-118">När du lägger till ett program från hello programgalleriet, inklusive:</span><span class="sxs-lookup"><span data-stu-id="38f13-118">When you add any application from hello application gallery, including:</span></span>

   1. <span data-ttu-id="38f13-119">**Azure AD-galleriet program** – ett program som har varit förintegrerade för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38f13-119">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

   2. <span data-ttu-id="38f13-120">**Application Proxy program** – ett program som körs i din lokala miljö som du vill tooprovide säker enkel inloggning på tooexternally.</span><span class="sxs-lookup"><span data-stu-id="38f13-120">**Application Proxy Applications** – An application running in your on-premises environment that you want tooprovide secure single-sign on tooexternally.</span></span>

   3. <span data-ttu-id="38f13-121">**Anpassad utvecklat program** – ett program som din organisation vill toodevelop på hello Azure AD plattform, men som kanske inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="38f13-121">**Custom-developed Applications** – An application that your organization wishes toodevelop on hello Azure AD Application Development Platform, but that may not exist yet.</span></span>

   4. <span data-ttu-id="38f13-122">**Icke-galleriet program** – ta med egna program!</span><span class="sxs-lookup"><span data-stu-id="38f13-122">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="38f13-123">En länk som du vill eller program som visas med ett användarnamn och lösenord fält har stöd för SAML- eller OpenID Connect protokoll eller stöder SCIM som du önskar toointegrate för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38f13-123">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish toointegrate for single sign-on with Azure AD.</span></span>

2.  <span data-ttu-id="38f13-124">När du registrerar dig för eller loggar in på en 3<sup>rd</sup> partsprogram integrerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="38f13-124">When signing up for, or signing in to, a 3<sup>rd</sup> party application integrated with Azure Active Directory.</span></span> <span data-ttu-id="38f13-125">Ett exempel på detta är [Smartsheet](https://app.smartsheet.com/b/home) eller [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span><span class="sxs-lookup"><span data-stu-id="38f13-125">One example of this is [Smartsheet](https://app.smartsheet.com/b/home) or [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span></span>

3.  <span data-ttu-id="38f13-126">När du registrerar dig för eller lägga till en licens tooa användare eller grupp tooa första partsprogram, t.ex. [Microsoft Office 365](http://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="38f13-126">When signing up for, or adding a license tooa user or a group tooa first party application, like [Microsoft Office 365](http://products.office.com/).</span></span>

4.  <span data-ttu-id="38f13-127">När du lägger till en ny programregistrering genom att skapa en anpassad utvecklade program med hjälp av hello [programmet registret](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span><span class="sxs-lookup"><span data-stu-id="38f13-127">When you add a new application registration by creating a custom-developed application using hello [Application Registry](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span></span>

5.  <span data-ttu-id="38f13-128">När du lägger till en ny programregistrering genom att skapa en anpassad utvecklade program med hjälp av hello [V2.0 Programregistreringsportalen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span><span class="sxs-lookup"><span data-stu-id="38f13-128">When you add a new application registration by creating a custom-developed application using hello [V2.0 Application Registration Portal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span></span>

6.  <span data-ttu-id="38f13-129">När du lägger till ett program du utvecklar med Visual Studio [ASP.net autentiseringsmetoder](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) eller [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span><span class="sxs-lookup"><span data-stu-id="38f13-129">When you add an application you’re developing using Visual Studio’s [ASP.net Authentication Methods](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) or [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span></span>

7.  <span data-ttu-id="38f13-130">När du skapar ett service principal objekt med hello [Azure AD PowerShell-modulen](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="38f13-130">When you create a service principal object using hello [Azure AD PowerShell Module](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

8.  <span data-ttu-id="38f13-131">När du [medgivande tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) som en administratör toouse data i din klient.</span><span class="sxs-lookup"><span data-stu-id="38f13-131">When you [consent tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) as an administrator toouse data in your tenant.</span></span>

9.  <span data-ttu-id="38f13-132">När en [användaren godkänner tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data i din klient.</span><span class="sxs-lookup"><span data-stu-id="38f13-132">When a [user consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data in your tenant.</span></span>

10. <span data-ttu-id="38f13-133">När du aktiverar vissa tjänster som lagrar data i din klient.</span><span class="sxs-lookup"><span data-stu-id="38f13-133">When you enable certain services that store data in your tenant.</span></span> <span data-ttu-id="38f13-134">Ett exempel på detta är återställning av lösenord, som modellerats som en service principal toostore din princip för lösenordsåterställning på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="38f13-134">One example of this is Password Reset, which is modeled as a service principal toostore your password reset policy securely.</span></span>

<span data-ttu-id="38f13-135">tooget mer information om hur appar läggs tooyour directory läsa [hur och varför program läggs tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span><span class="sxs-lookup"><span data-stu-id="38f13-135">tooget more details on how apps are added tooyour directory, read [How and why applications are added tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span></span>

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a><span data-ttu-id="38f13-136">Jag vill tooremove en specifik användare eller grupp tilldelning tooan program</span><span class="sxs-lookup"><span data-stu-id="38f13-136">I want tooremove a specific user’s or group’s assignment tooan application</span></span>

<span data-ttu-id="38f13-137">tooremove en användare eller grupp tilldelning tooan program, följ hello steg i hello [ta bort en användare eller grupp från en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.</span><span class="sxs-lookup"><span data-stu-id="38f13-137">tooremove a user or group assignment tooan application, follow hello steps listed in hello [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

## <a name="i-want-toodisable-all-access-tooan-application-for-every-user"></a><span data-ttu-id="38f13-138">Jag vill toodisable alla åtkomst tooan program för varje användare</span><span class="sxs-lookup"><span data-stu-id="38f13-138">I want toodisable all access tooan application for every user</span></span>

<span data-ttu-id="38f13-139">toodisable alla inloggningar tooan användarprogram, följ hello stegen i hello [inaktivera användarinloggningar för en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.</span><span class="sxs-lookup"><span data-stu-id="38f13-139">toodisable all user sign-ins tooan application, follow hello steps listed in hello [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-toodelete-an-application-entirely"></a><span data-ttu-id="38f13-140">Jag vill toodelete ett program helt</span><span class="sxs-lookup"><span data-stu-id="38f13-140">I want toodelete an application entirely</span></span>

<span data-ttu-id="38f13-141">för**ta bort ett program**, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="38f13-141">too**delete an application**, follow hello instructions below:</span></span>

1.  <span data-ttu-id="38f13-142">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="38f13-142">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="38f13-143">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="38f13-143">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="38f13-144">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="38f13-144">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="38f13-145">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="38f13-145">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="38f13-146">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="38f13-146">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="38f13-147">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="38f13-147">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="38f13-148">Välj hello-program som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="38f13-148">Select hello application you want toodelete.</span></span>

7.  <span data-ttu-id="38f13-149">När programmet hello läses in klickar du på **ta bort** ikon från hello översta program **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="38f13-149">Once hello application loads, click **Delete** icon from hello top application’s **Overview** blade.</span></span>

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a><span data-ttu-id="38f13-150">Jag vill toodisable alla framtida medgivande operations tooany-användarprogram</span><span class="sxs-lookup"><span data-stu-id="38f13-150">I want toodisable all future user consent operations tooany application</span></span>

<span data-ttu-id="38f13-151">Inaktiverar användargodkännande för hela katalogen förhindra användare från principer tooany program.</span><span class="sxs-lookup"><span data-stu-id="38f13-151">Disabling user consent for your entire directory prevent end users from consenting tooany application.</span></span> <span data-ttu-id="38f13-152">Administratörer kan fortfarande medgivande på användarens behalves.</span><span class="sxs-lookup"><span data-stu-id="38f13-152">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="38f13-153">toolearn mer om programmet samtycke och varför du kanske eller kanske inte vill toodo detta, Läs [förstå användare och admin medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="38f13-153">toolearn more about application consent, and why you may or may not wish toodo this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="38f13-154">för**inaktivera alla framtida användarens medgivande åtgärder i hela katalogen**, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="38f13-154">too**disable all future user consent operations in your entire directory**, follow hello instructions below:</span></span>

1.  <span data-ttu-id="38f13-155">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="38f13-155">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="38f13-156">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="38f13-156">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="38f13-157">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="38f13-157">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="38f13-158">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="38f13-158">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="38f13-159">Klicka på **användarinställningar**.</span><span class="sxs-lookup"><span data-stu-id="38f13-159">click **User settings**.</span></span>

6.  <span data-ttu-id="38f13-160">Inaktivera alla framtida användarens medgivande åtgärder genom att ange hello **användare kan låta appar tooaccess sina data** växla för**nr** och klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="38f13-160">Disable all future user consent operations by setting hello **Users can allow apps tooaccess their data** toggle too**No** and click hello **Save** button.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38f13-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="38f13-161">Next steps</span></span>
[<span data-ttu-id="38f13-162">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="38f13-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
