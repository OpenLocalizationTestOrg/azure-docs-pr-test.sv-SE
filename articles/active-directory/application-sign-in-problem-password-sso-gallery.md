---
title: "Problem med att logga att ett program för Azure AD-galleriet som konfigurerats för federerad enkel inloggning | Microsoft Docs"
description: "Felsökning av problem med Azure AD-galleriet program konfigurerade för lösenord för enkel inloggning"
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
ms.openlocfilehash: f8521d1386bba8004e84fe8862a5f59a65ea19ad
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="06084-103">Problem med att logga att ett program för Azure AD-galleriet som konfigurerats för federerad enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="06084-103">Problems signing in to an Azure AD Gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="06084-104">Åtkomstpanelen är en webbaserad portal som gör att en användare som har ett arbets- eller skolkonto konto i Azure Active Directory (Azure AD) för att visa och starta molnbaserade program som Azure AD-administratör har gett dem åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="06084-104">The Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) to view and launch cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="06084-105">En användare med Azure AD-versioner kan också använda självbetjäning och funktioner för hantering av appen via åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="06084-105">A user who has Azure AD editions can also use self-service group and app management capabilities through the Access Panel.</span></span> <span data-ttu-id="06084-106">Åtkomstpanelen är separat från Azure portal och kräver inte att användare ska ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="06084-106">The Access Panel is separate from the Azure portal and does not require users to have an Azure subscription.</span></span>

<span data-ttu-id="06084-107">Om du vill använda lösenordsbaserade enkel inloggning (SSO) på åtkomstpanelen måste åtkomstpanelen tillägget installeras i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="06084-107">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="06084-108">Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="06084-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="06084-109">Möte Webbläsarkrav för åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="06084-109">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="06084-110">Åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS.</span><span class="sxs-lookup"><span data-stu-id="06084-110">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="06084-111">Om du vill använda lösenordsbaserade enkel inloggning (SSO) på åtkomstpanelen måste åtkomstpanelen tillägget installeras i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="06084-111">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="06084-112">Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="06084-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="06084-113">Användarens webbläsare kan vara för lösenordsbaserad enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="06084-113">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="06084-114">Internet Explorer 8, 9, 10, 11 - på Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="06084-114">Internet Explorer 8, 9, 10, 11 - on Windows 7 or later</span></span>

-   <span data-ttu-id="06084-115">Chrome - på Windows 7 eller senare, och i MacOS X eller senare</span><span class="sxs-lookup"><span data-stu-id="06084-115">Chrome - on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="06084-116">Firefox 26.0 eller senare - på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare</span><span class="sxs-lookup"><span data-stu-id="06084-116">Firefox 26.0 or later - on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="06084-117">Tillägget lösenordsbaserade SSO blir tillgängliga för kvalificerade i Windows 10 när webbläsartillägg blir stöd för kant.</span><span class="sxs-lookup"><span data-stu-id="06084-117">The password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="06084-118">Så här installerar du Access panelen webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="06084-118">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="06084-119">Följ stegen nedan om du vill installera webbläsartillägget för åtkomst panelen:</span><span class="sxs-lookup"><span data-stu-id="06084-119">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="06084-120">Öppna den [åtkomstpanelen](https://myapps.microsoft.com) i en webbläsare som stöds och logga in som en **användaren** i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="06084-120">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="06084-121">Klicka på en **lösenord SSO-program** på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="06084-121">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="06084-122">Fråga om att installera programvara, Välj **installera nu**.</span><span class="sxs-lookup"><span data-stu-id="06084-122">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="06084-123">Baserat på din webbläsare du dirigeras till länken.</span><span class="sxs-lookup"><span data-stu-id="06084-123">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="06084-124">**Lägg till** tillägg till webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="06084-124">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="06084-125">Om din webbläsare, Välj antingen **aktivera** eller **Tillåt** tillägget.</span><span class="sxs-lookup"><span data-stu-id="06084-125">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="06084-126">När den har installerats, **starta om** webbläsarsessionen.</span><span class="sxs-lookup"><span data-stu-id="06084-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="06084-127">Logga in till åtkomstpanelen och se om kan du **starta** lösenord SSO-program</span><span class="sxs-lookup"><span data-stu-id="06084-127">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="06084-128">Du kan också ladda ned tillägget för Chrome och Firefox från direkt med länkarna nedan:</span><span class="sxs-lookup"><span data-stu-id="06084-128">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="06084-129">Tillägget för Chrome åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="06084-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="06084-130">Tillägget för Firefox åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="06084-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="06084-131">Konfigurera en grupprincip för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="06084-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="06084-132">Du kan konfigurera en grupprincip som gör det möjligt att fjärrinstallera åtkomstpanelen-tillägg för Internet Explorer på användarnas datorer.</span><span class="sxs-lookup"><span data-stu-id="06084-132">You can setup a group policy that allow you to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="06084-133">Kraven är:</span><span class="sxs-lookup"><span data-stu-id="06084-133">The prerequisites include:</span></span>

-   <span data-ttu-id="06084-134">Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer till domänen.</span><span class="sxs-lookup"><span data-stu-id="06084-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>

-   <span data-ttu-id="06084-135">Du måste ha behörigheten ”Redigera inställningar” så här redigerar du den grupprincipobjektet (GPO).</span><span class="sxs-lookup"><span data-stu-id="06084-135">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="06084-136">Som standard medlemmar i följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="06084-136">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="06084-137">[Läs mer](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="06084-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="06084-138">Följ guiden [hur du distribuerar Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) steg för steg instruktioner om hur du konfigurerar grupprincipen och distribuera till användare.</span><span class="sxs-lookup"><span data-stu-id="06084-138">Follow the tutorial [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how to configure the group policy and deploy it to users.</span></span>

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a><span data-ttu-id="06084-139">Felsöka åtkomstpanelen i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="06084-139">Troubleshoot the Access Panel in Internet Explorer</span></span>

<span data-ttu-id="06084-140">Följ den [felsöka Access panelen-tillägg för Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide för åtkomst till en diagnostik och steg-för-steg-instruktioner om hur du konfigurerar tillägget för Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="06084-140">Follow the [Troubleshoot the Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring the extension for IE.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="06084-141">Så här konfigurerar du lösenord för enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="06084-141">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="06084-142">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="06084-142">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="06084-143">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="06084-143">Add an application from the Azure AD gallery</span></span>](#_Add_an_application)

-   [<span data-ttu-id="06084-144">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="06084-144">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="06084-145">Tilldela användare till programmet</span><span class="sxs-lookup"><span data-stu-id="06084-145">Assign users to the application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="06084-146">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="06084-146">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="06084-147">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="06084-147">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="06084-148">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="06084-148">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="06084-149">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-149">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="06084-150">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="06084-150">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="06084-151">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-151">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="06084-152">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="06084-152">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="06084-153">I den **anger du ett namn** textruta från den **Lägg till från galleriet** avsnittet, skriver du namnet på programmet.</span><span class="sxs-lookup"><span data-stu-id="06084-153">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="06084-154">Välj det program som du vill konfigurera för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="06084-154">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="06084-155">Innan du lägger till programmet, kan du ändra dess namn från den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="06084-155">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="06084-156">Klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="06084-156">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="06084-157">Efter en kort period kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="06084-157">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="06084-158">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="06084-158">Configure the application for password single sign-on</span></span>

<span data-ttu-id="06084-159">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="06084-159">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="06084-160">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="06084-160">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="06084-161">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-161">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="06084-162">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="06084-162">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="06084-163">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-163">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="06084-164">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="06084-164">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="06084-165">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="06084-165">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="06084-166">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="06084-166">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="06084-167">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-167">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="06084-168">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="06084-168">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="06084-169">[Tilldela användare till programmet](#_How_to_assign).</span><span class="sxs-lookup"><span data-stu-id="06084-169">[Assign users to the application](#_How_to_assign).</span></span>

10. <span data-ttu-id="06084-170">Dessutom kan du också ange autentiseringsuppgifter för användarens räkning genom att markera rader användare och klicka på **referenser uppdatering** och ange användarnamn och lösenord för användarna.</span><span class="sxs-lookup"><span data-stu-id="06084-170">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="06084-171">Annars uppmanas användarna att ange autentiseringsuppgifterna sig vid start.</span><span class="sxs-lookup"><span data-stu-id="06084-171">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

### <a name="assign-users-to-the-application"></a><span data-ttu-id="06084-172">Tilldela användare till programmet</span><span class="sxs-lookup"><span data-stu-id="06084-172">Assign users to the application</span></span>

<span data-ttu-id="06084-173">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="06084-173">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="06084-174">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="06084-174">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="06084-175">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-175">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="06084-176">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="06084-176">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="06084-177">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-177">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="06084-178">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="06084-178">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="06084-179">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="06084-179">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="06084-180">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="06084-180">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="06084-181">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="06084-181">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="06084-182">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="06084-182">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="06084-183">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="06084-183">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="06084-184">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="06084-184">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="06084-185">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="06084-185">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="06084-186">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="06084-186">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="06084-187">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="06084-187">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="06084-188">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="06084-188">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="06084-189">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="06084-189">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="06084-190">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="06084-190">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="06084-191">Användare som du har valt att kunna starta programmen på åtkomstpanelen efter en kort period.</span><span class="sxs-lookup"><span data-stu-id="06084-191">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshoot-steps-dont-resolve-the-issue"></a><span data-ttu-id="06084-192">Om felsökning av dessa löser åtgärder inte problemet</span><span class="sxs-lookup"><span data-stu-id="06084-192">If these troubleshoot steps don't resolve the issue</span></span> 
<span data-ttu-id="06084-193">Öppna ett supportärende med följande information om de är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="06084-193">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="06084-194">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="06084-194">Correlation error ID</span></span>

-   <span data-ttu-id="06084-195">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="06084-195">UPN (user email address)</span></span>

-   <span data-ttu-id="06084-196">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="06084-196">TenantID</span></span>

-   <span data-ttu-id="06084-197">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="06084-197">Browser type</span></span>

-   <span data-ttu-id="06084-198">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="06084-198">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="06084-199">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="06084-199">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="06084-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06084-200">Next steps</span></span>
[<span data-ttu-id="06084-201">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="06084-201">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
