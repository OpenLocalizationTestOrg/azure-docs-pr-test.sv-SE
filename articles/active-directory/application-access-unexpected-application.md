---
title: "Oväntat programmet i listan med mina program | Microsoft Docs"
description: "Hur du ser alla program i din klient och förstå hur program visas i listan alla program under företagsprogram"
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
ms.openlocfilehash: 0f8136cb066fa6e3e4a8dd06e34b9f866e3916f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-application-in-my-applications-list"></a><span data-ttu-id="fd845-103">Oväntat programmet i listan med mina program</span><span class="sxs-lookup"><span data-stu-id="fd845-103">Unexpected application in my applications list</span></span>

<span data-ttu-id="fd845-104">Den här artikeln hjälper dig att förstå hur program visas i din **alla program** listan **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fd845-104">This article help you to understand how applications appear in your **All Applications** list under **Enterprise Applications**.</span></span> 

## <a name="how-to-see-all-applications-in-your-tenant"></a><span data-ttu-id="fd845-105">Hur man ser alla program i din klient</span><span class="sxs-lookup"><span data-stu-id="fd845-105">How to see all applications in your tenant</span></span>

<span data-ttu-id="fd845-106">Om du vill se alla program i din klient, måste du använda den **Filter** kontroll för att kunna visa **alla program** under den **alla program** lista.</span><span class="sxs-lookup"><span data-stu-id="fd845-106">To see all the applications in your tenant, you need to use the **Filter** control to show **All Applications** under the **All Applications** list.</span></span> <span data-ttu-id="fd845-107">Följ stegen nedan om du vill göra detta:</span><span class="sxs-lookup"><span data-stu-id="fd845-107">To do this, follow the steps below:</span></span>

1.  <span data-ttu-id="fd845-108">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="fd845-108">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="fd845-109">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fd845-109">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fd845-110">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="fd845-110">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fd845-111">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fd845-111">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fd845-112">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="fd845-112">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="fd845-113">Klicka på användningen i **Filter** kontrollen längst upp i den **listan med alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd845-113">click the use the **Filter** control at the top of the **All Applications List**.</span></span>

7.  <span data-ttu-id="fd845-114">På den **Filter** bladet, ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="fd845-114">On the **Filter** blade, set the **Show** option to **All Applications.**</span></span>

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a><span data-ttu-id="fd845-115">Varför visas ett visst program i programlistan alla?</span><span class="sxs-lookup"><span data-stu-id="fd845-115">Why does a specific application appear in my all applications list?</span></span>

<span data-ttu-id="fd845-116">När filtrerats till **alla program**, **alla program** **listan** visas alla objekt för tjänstens huvudnamn i din klient.</span><span class="sxs-lookup"><span data-stu-id="fd845-116">When filtered to **All Applications**, the **All Applications** **List** shows every Service Principal object in your tenant.</span></span> <span data-ttu-id="fd845-117">Tjänstens huvudnamn objekt som kan visas i listan på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="fd845-117">Service Principal objects can appear in this list in a various ways:</span></span>

1.  <span data-ttu-id="fd845-118">När du lägger till ett program från programgalleriet, inklusive:</span><span class="sxs-lookup"><span data-stu-id="fd845-118">When you add any application from the application gallery, including:</span></span>

   1. <span data-ttu-id="fd845-119">**Azure AD-galleriet program** – ett program som har varit förintegrerade för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd845-119">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

   2. <span data-ttu-id="fd845-120">**Application Proxy program** – ett program som körs i din lokala miljö som du vill ge säker enkel inloggning till externt.</span><span class="sxs-lookup"><span data-stu-id="fd845-120">**Application Proxy Applications** – An application running in your on-premises environment that you want to provide secure single-sign on to externally.</span></span>

   3. <span data-ttu-id="fd845-121">**Anpassad utvecklat program** – ett program som din organisation vill utveckla på den Azure AD plattform, men som kanske inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="fd845-121">**Custom-developed Applications** – An application that your organization wishes to develop on the Azure AD Application Development Platform, but that may not exist yet.</span></span>

   4. <span data-ttu-id="fd845-122">**Icke-galleriet program** – ta med egna program!</span><span class="sxs-lookup"><span data-stu-id="fd845-122">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="fd845-123">En länk som du vill eller program som visas med ett användarnamn och lösenord fält har stöd för SAML- eller OpenID Connect protokoll eller stöder SCIM som du vill integrera för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd845-123">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish to integrate for single sign-on with Azure AD.</span></span>

2.  <span data-ttu-id="fd845-124">När du registrerar dig för eller loggar in på en 3<sup>rd</sup> partsprogram integrerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fd845-124">When signing up for, or signing in to, a 3<sup>rd</sup> party application integrated with Azure Active Directory.</span></span> <span data-ttu-id="fd845-125">Ett exempel på detta är [Smartsheet](https://app.smartsheet.com/b/home) eller [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd845-125">One example of this is [Smartsheet](https://app.smartsheet.com/b/home) or [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span></span>

3.  <span data-ttu-id="fd845-126">När registreringen eller att lägga till en licens till en användare eller grupp i ett partsprogram för första, som [Microsoft Office 365](http://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="fd845-126">When signing up for, or adding a license to a user or a group to a first party application, like [Microsoft Office 365](http://products.office.com/).</span></span>

4.  <span data-ttu-id="fd845-127">När du lägger till en ny programregistrering genom att skapa en anpassad utvecklat program med hjälp av den [programmet registret](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span><span class="sxs-lookup"><span data-stu-id="fd845-127">When you add a new application registration by creating a custom-developed application using the [Application Registry](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span></span>

5.  <span data-ttu-id="fd845-128">När du lägger till en ny programregistrering genom att skapa en anpassad utvecklat program med hjälp av den [V2.0 Programregistreringsportalen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span><span class="sxs-lookup"><span data-stu-id="fd845-128">When you add a new application registration by creating a custom-developed application using the [V2.0 Application Registration Portal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span></span>

6.  <span data-ttu-id="fd845-129">När du lägger till ett program du utvecklar med Visual Studio [ASP.net autentiseringsmetoder](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) eller [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd845-129">When you add an application you’re developing using Visual Studio’s [ASP.net Authentication Methods](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) or [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span></span>

7.  <span data-ttu-id="fd845-130">När du skapar en huvudobjekt med hjälp av [Azure AD PowerShell-modulen](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="fd845-130">When you create a service principal object using the [Azure AD PowerShell Module](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

8.  <span data-ttu-id="fd845-131">När du [samtycker till att ett program](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) som administratör för att kunna använda data i din klient.</span><span class="sxs-lookup"><span data-stu-id="fd845-131">When you [consent to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) as an administrator to use data in your tenant.</span></span>

9.  <span data-ttu-id="fd845-132">När en [användaren godkänner till ett program](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) använda data i din klient.</span><span class="sxs-lookup"><span data-stu-id="fd845-132">When a [user consents to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) to use data in your tenant.</span></span>

10. <span data-ttu-id="fd845-133">När du aktiverar vissa tjänster som lagrar data i din klient.</span><span class="sxs-lookup"><span data-stu-id="fd845-133">When you enable certain services that store data in your tenant.</span></span> <span data-ttu-id="fd845-134">Ett exempel på detta är återställning av lösenord, som modellerats som ett huvudnamn för tjänsten att lagra lösenordet Återställ principen på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="fd845-134">One example of this is Password Reset, which is modeled as a service principal to store your password reset policy securely.</span></span>

<span data-ttu-id="fd845-135">Om du vill ha mer information om hur appar har lagts till i din katalog läsa [hur och varför program läggs till Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span><span class="sxs-lookup"><span data-stu-id="fd845-135">To get more details on how apps are added to your directory, read [How and why applications are added to Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span></span>

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a><span data-ttu-id="fd845-136">Jag vill ta bort en specifik användare eller grupp tilldelning till ett program</span><span class="sxs-lookup"><span data-stu-id="fd845-136">I want to remove a specific user’s or group’s assignment to an application</span></span>

<span data-ttu-id="fd845-137">Ta bort en användare eller grupptilldelning till ett program genom att följa instruktionerna i den [ta bort en användare eller grupp från en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.</span><span class="sxs-lookup"><span data-stu-id="fd845-137">To remove a user or group assignment to an application, follow the steps listed in the [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a><span data-ttu-id="fd845-138">Jag vill inaktivera all åtkomst till ett program för varje användare</span><span class="sxs-lookup"><span data-stu-id="fd845-138">I want to disable all access to an application for every user</span></span>

<span data-ttu-id="fd845-139">Om du vill inaktivera alla användarinloggningar till ett program följer du stegen i den [inaktivera användarinloggningar för en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.</span><span class="sxs-lookup"><span data-stu-id="fd845-139">To disable all user sign-ins to an application, follow the steps listed in the [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-to-delete-an-application-entirely"></a><span data-ttu-id="fd845-140">Jag vill ta bort ett program helt</span><span class="sxs-lookup"><span data-stu-id="fd845-140">I want to delete an application entirely</span></span>

<span data-ttu-id="fd845-141">Att **ta bort ett program**, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="fd845-141">To **delete an application**, follow the instructions below:</span></span>

1.  <span data-ttu-id="fd845-142">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="fd845-142">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="fd845-143">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fd845-143">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fd845-144">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="fd845-144">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fd845-145">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fd845-145">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fd845-146">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="fd845-146">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="fd845-147">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="fd845-147">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="fd845-148">Välj det program som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="fd845-148">Select the application you want to delete.</span></span>

7.  <span data-ttu-id="fd845-149">När programmet läses in klickar du på **ta bort** ikon från översta programmet **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="fd845-149">Once the application loads, click **Delete** icon from the top application’s **Overview** blade.</span></span>

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a><span data-ttu-id="fd845-150">Jag vill inaktivera alla framtida användarens medgivande åtgärder för alla program</span><span class="sxs-lookup"><span data-stu-id="fd845-150">I want to disable all future user consent operations to any application</span></span>

<span data-ttu-id="fd845-151">Inaktiverar användargodkännande för hela katalogen att förhindra att slutanvändare principer för alla program.</span><span class="sxs-lookup"><span data-stu-id="fd845-151">Disabling user consent for your entire directory prevent end users from consenting to any application.</span></span> <span data-ttu-id="fd845-152">Administratörer kan fortfarande medgivande på användarens behalves.</span><span class="sxs-lookup"><span data-stu-id="fd845-152">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="fd845-153">Lär dig mer om programmet medgivande och varför du kanske eller kanske inte vill göra detta, Läs [förstå användare och admin medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="fd845-153">To learn more about application consent, and why you may or may not wish to do this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="fd845-154">Att **inaktivera alla framtida användarens medgivande åtgärder i hela katalogen**, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="fd845-154">To **disable all future user consent operations in your entire directory**, follow the instructions below:</span></span>

1.  <span data-ttu-id="fd845-155">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="fd845-155">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="fd845-156">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fd845-156">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fd845-157">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="fd845-157">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fd845-158">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fd845-158">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="fd845-159">Klicka på **användarinställningar**.</span><span class="sxs-lookup"><span data-stu-id="fd845-159">click **User settings**.</span></span>

6.  <span data-ttu-id="fd845-160">Inaktivera alla framtida användarens medgivande åtgärder genom att ange den **användare kan Tillåt appar att komma åt sina data** växla till **nr** och klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd845-160">Disable all future user consent operations by setting the **Users can allow apps to access their data** toggle to **No** and click the **Save** button.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd845-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd845-161">Next steps</span></span>
[<span data-ttu-id="fd845-162">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd845-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
