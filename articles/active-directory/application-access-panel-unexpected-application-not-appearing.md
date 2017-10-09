---
title: "aaaAn som tilldelats programmet visas inte på hello åtkomstpanelen | Microsoft Docs"
description: "Felsöka anledningen till ett program inte visas i hello åtkomstpanelen"
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
ms.reviwer: japere
ms.openlocfilehash: 089883f406267df4552c7fc991883f335ad49fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-assigned-application-is-not-appearing-on-hello-access-panel"></a><span data-ttu-id="88f07-103">Ett tilldelat program visas inte på hello åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="88f07-103">An assigned application is not appearing on hello access panel</span></span>

<span data-ttu-id="88f07-104">Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till.</span><span class="sxs-lookup"><span data-stu-id="88f07-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="88f07-105">Dessa program är konfigurerade för hello användare i hello Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="88f07-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="88f07-106">hello-programmet måste konfigureras korrekt och tilldelade toohello användare eller en grupp hello användare är medlem i toosee hello programmet hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="88f07-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="88f07-107">hello typ av appar som visas kanske en användare finns i hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="88f07-107">hello type of apps a user may be seeing fall in hello following categories:</span></span>

-   <span data-ttu-id="88f07-108">Office 365-program</span><span class="sxs-lookup"><span data-stu-id="88f07-108">Office 365 Applications</span></span>

-   <span data-ttu-id="88f07-109">Microsoft och tredje parts program som har konfigurerats med federationsbaserat enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88f07-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="88f07-110">Lösenordsbaserade SSO-program</span><span class="sxs-lookup"><span data-stu-id="88f07-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="88f07-111">Program med befintliga lösningar för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88f07-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="88f07-112">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="88f07-112">General issues toocheck first</span></span>

-   <span data-ttu-id="88f07-113">Om ett program har precis lagts tooa användare, försök toosign in och ut i hello användarens åtkomstpanelen efter några minuter toosee om hello programmet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="88f07-113">If an application was just added tooa user, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello application is added.</span></span>

-   <span data-ttu-id="88f07-114">Om en licens precis har tagits bort från en användare eller grupp hello användaren är medlem i det här kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp för ändringar toobe görs.</span><span class="sxs-lookup"><span data-stu-id="88f07-114">If a license was just removed from a user or group hello user is a member of this may take a long time, depending on hello size and complexity of hello group for changes toobe made.</span></span> <span data-ttu-id="88f07-115">Tillåt extra tid innan du loggar in på hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="88f07-115">Allow for extra time before signing into hello Access Panel.</span></span>

## <a name="problems-related-tooapplication-configuration"></a><span data-ttu-id="88f07-116">Problem relaterade tooapplication konfiguration</span><span class="sxs-lookup"><span data-stu-id="88f07-116">Problems related tooapplication configuration</span></span>

<span data-ttu-id="88f07-117">Ett program kan inte visas i en användares åtkomstpanelen eftersom den inte har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-117">An application may not be appearing in a user’s Access Panel because it is not configured properly.</span></span> <span data-ttu-id="88f07-118">Här följer några exempel på hur du kan felsöka problem relaterade tooapplication konfiguration:</span><span class="sxs-lookup"><span data-stu-id="88f07-118">Below are some ways you can troubleshoot issues related tooapplication configuration:</span></span>

-   [<span data-ttu-id="88f07-119">Hur tooconfigure federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-119">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [<span data-ttu-id="88f07-120">Hur tooconfigure federerad enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-120">How tooconfigure federated single sign-on for a non-gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="88f07-121">Hur tooconfigure ett lösenord med enkel inloggning program för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-121">How tooconfigure a password single sign-on application for an Azure AD gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="88f07-122">Hur tooconfigure ett lösenord med enkel inloggning program för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-122">How tooconfigure a password single sign-on application for a non-gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="88f07-123">Hur tooconfigure federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-123">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="88f07-124">Alla program i hello Azure AD-galleriet aktiverad med Enterprise Single Sign-On-funktionen har en stegvis självstudiekurs som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="88f07-124">All applications in hello Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="88f07-125">Du kan komma åt hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.</span><span class="sxs-lookup"><span data-stu-id="88f07-125">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="88f07-126">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="88f07-126">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="88f07-127">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-127">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="88f07-128">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="88f07-128">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="88f07-129">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="88f07-129">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="88f07-130">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="88f07-130">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="88f07-131">Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="88f07-131">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="88f07-132">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-132">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="88f07-133">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-133">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-134">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="88f07-134">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="88f07-135">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-135">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-136">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-136">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-137">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-137">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-138">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-138">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="88f07-139">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-139">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="88f07-140">Välj hello-program som du vill använda tooconfigure för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-140">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="88f07-141">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="88f07-141">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="88f07-142">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-142">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="88f07-143">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-143">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="88f07-144">Konfigurera enkel inloggning för ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-144">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="88f07-145">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-145">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-146">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-146">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-147">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-147">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-148">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-148">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-149">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-149">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-150">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-150">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="88f07-151">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-151">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-152">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-152">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="88f07-153">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-153">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-154">Välj **SAML-baserade inloggning** från hello **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-154">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="88f07-155">Ange hello krävs värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="88f07-155">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="88f07-156">Du bör få värdena från hello programvaruleverantören.</span><span class="sxs-lookup"><span data-stu-id="88f07-156">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="88f07-157">tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="88f07-157">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="88f07-158">Hello identifierare är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="88f07-158">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="88f07-159">tooconfigure hello program som IdP-initierad SSO hello Reply-URL som det är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="88f07-159">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="88f07-160">Hello identifierare är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="88f07-160">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="88f07-161">**Valfritt:** klickar du på **visa avancerade inställningar för URL: en** om du vill toosee hello icke obligatoriska värden.</span><span class="sxs-lookup"><span data-stu-id="88f07-161">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="88f07-162">I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-162">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="88f07-163">**Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="88f07-163">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="88f07-164">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="88f07-164">tooadd an attribute:</span></span>

   1. <span data-ttu-id="88f07-165">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="88f07-165">click **Add attribute**.</span></span> <span data-ttu-id="88f07-166">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-166">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="88f07-167">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="88f07-167">click **Save.**</span></span> <span data-ttu-id="88f07-168">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="88f07-168">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="88f07-169">Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-169">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="88f07-170">Du har dessutom hello metadata URL: er och certifikat som krävs för toosetup SSO med hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-170">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="88f07-171">Klicka på **spara** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="88f07-171">click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="88f07-172">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-172">Assign users toohello application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="88f07-173">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="88f07-173">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="88f07-174">tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="88f07-174">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-175">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-175">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-176">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-176">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-177">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-177">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-178">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-178">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-179">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-179">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="88f07-180">Om du inte ser hello-program som du vill tooshow här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-180">If you do not see hello application you want tooshow up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-181">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-181">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="88f07-182">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-182">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-183">Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-183">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="88f07-184">hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="88f07-184">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="88f07-185">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="88f07-185">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="88f07-186">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="88f07-186">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="88f07-187">tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="88f07-187">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="88f07-188">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="88f07-188">tooadd an attribute:</span></span>

   1. <span data-ttu-id="88f07-189">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="88f07-189">click **Add attribute**.</span></span> <span data-ttu-id="88f07-190">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-190">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="88f07-191">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="88f07-191">click **Save.**</span></span> <span data-ttu-id="88f07-192">Hello nytt attribut i hello tabellen visas.</span><span class="sxs-lookup"><span data-stu-id="88f07-192">You will see hello new attribute in hello table.</span></span>

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="88f07-193">Hämta metadata för hello Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="88f07-193">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="88f07-194">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-194">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-195">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-195">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-196">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-196">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-197">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-197">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-198">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-198">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-199">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-199">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="88f07-200">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-200">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-201">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-201">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="88f07-202">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-202">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-203">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="88f07-203">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="88f07-204">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="88f07-204">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="88f07-205">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="88f07-205">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="88f07-206">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="88f07-206">hello metadata can only be retrieved as a XML file.</span></span>

### <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="88f07-207">Hur tooconfigure federerad enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-207">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="88f07-208">tooconfigure ett icke-galleriet program behöver du toohave Azure AD premium och hello programmet stöder SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="88f07-208">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="88f07-209">Mer information om Azure AD-versioner finns [priser för Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="88f07-209">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="88f07-210">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="88f07-210">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="88f07-211">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="88f07-211">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="88f07-212">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="88f07-212">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="88f07-213">Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="88f07-213">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

#### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="88f07-214">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="88f07-214">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="88f07-215">tooconfigure enkel inloggning för ett program som inte är i hello Azure AD-galleriet gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-215">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-216">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-216">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-217">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-217">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-218">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-218">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-219">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-219">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-220">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-220">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="88f07-221">Klicka på **icke-galleriet programmet** i hello **lägga till egna app** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="88f07-221">click **Non-gallery application** in hello **Add your own app** section.</span></span>

7.  <span data-ttu-id="88f07-222">Ange hello namnet på programmet hello i hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="88f07-222">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="88f07-223">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-223">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="88f07-224">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-224">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="88f07-225">Välj **SAML-baserade inloggning** i hello **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-225">Select **SAML-based Sign-on** in hello **Mode** dropdown.</span></span>

11. <span data-ttu-id="88f07-226">Ange hello krävs värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="88f07-226">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="88f07-227">Du bör få värdena från hello programvaruleverantören.</span><span class="sxs-lookup"><span data-stu-id="88f07-227">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="88f07-228">tooconfigure hello program som IdP-initierad SSO ange hello Reply URL och hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="88f07-228">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

   2.  <span data-ttu-id="88f07-229">**Valfritt:** tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="88f07-229">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="88f07-230">I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-230">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="88f07-231">**Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="88f07-231">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="88f07-232">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="88f07-232">tooadd an attribute:</span></span>

   1. <span data-ttu-id="88f07-233">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="88f07-233">click **Add attribute**.</span></span> <span data-ttu-id="88f07-234">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-234">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="88f07-235">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="88f07-235">Click **Save.**</span></span> <span data-ttu-id="88f07-236">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="88f07-236">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="88f07-237">Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-237">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="88f07-238">Du har även Azure AD-URL: er och certifikat som krävs för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="88f07-238">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="88f07-239">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="88f07-239">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="88f07-240">tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="88f07-240">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-241">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-241">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-242">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-242">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-243">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-243">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-244">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-244">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-245">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-245">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="88f07-246">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-246">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-247">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-247">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="88f07-248">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-248">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-249">Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-249">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="88f07-250">hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="88f07-250">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="88f07-251">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="88f07-251">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="88f07-252">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="88f07-252">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="88f07-253">tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="88f07-253">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="88f07-254">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="88f07-254">tooadd an attribute:</span></span>

   1. <span data-ttu-id="88f07-255">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="88f07-255">click **Add attribute**.</span></span> <span data-ttu-id="88f07-256">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-256">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="88f07-257">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="88f07-257">Click **Save.**</span></span> <span data-ttu-id="88f07-258">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="88f07-258">You see hello new attribute in hello table.</span></span>

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="88f07-259">Hämta metadata för hello Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="88f07-259">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="88f07-260">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-260">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-261">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-261">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-262">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-262">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-263">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-263">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-264">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-264">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-265">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-265">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="88f07-266">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-266">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-267">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-267">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="88f07-268">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-268">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-269">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="88f07-269">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="88f07-270">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="88f07-270">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="88f07-271">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="88f07-271">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="88f07-272">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="88f07-272">hello metadata can only be retrieved as a XML file.</span></span>

### <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="88f07-273">Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-273">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="88f07-274">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="88f07-274">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="88f07-275">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-275">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="88f07-276">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88f07-276">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="88f07-277">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-277">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="88f07-278">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-278">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-279">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="88f07-279">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="88f07-280">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-280">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-281">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-281">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-282">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-282">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-283">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-283">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="88f07-284">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-284">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="88f07-285">Välj hello-program som du vill använda tooconfigure för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-285">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="88f07-286">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="88f07-286">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="88f07-287">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-287">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="88f07-288">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-288">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="88f07-289">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88f07-289">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="88f07-290">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-290">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-291">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-291">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-292">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-292">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-293">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-293">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-294">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-294">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-295">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-295">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="88f07-296">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-296">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-297">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-297">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="88f07-298">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-298">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-299">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="88f07-299">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="88f07-300">[Tilldela användare toohello program](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="88f07-300">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="88f07-301">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="88f07-301">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="88f07-302">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="88f07-302">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="88f07-303">Hur tooconfigure lösenord enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="88f07-303">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="88f07-304">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="88f07-304">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="88f07-305">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="88f07-305">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="88f07-306">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88f07-306">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a><span data-ttu-id="88f07-307">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="88f07-307">Add a non-gallery application</span></span>

<span data-ttu-id="88f07-308">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-308">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-309">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**.</span><span class="sxs-lookup"><span data-stu-id="88f07-309">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="88f07-310">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-310">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-311">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-311">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-312">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-312">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-313">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-313">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="88f07-314">Klicka på **icke-galleriet program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-314">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="88f07-315">Ange hello namnet på ditt program i hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="88f07-315">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="88f07-316">Välj **lägga till.**</span><span class="sxs-lookup"><span data-stu-id="88f07-316">Select **Add.**</span></span>

<span data-ttu-id="88f07-317">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-317">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="88f07-318">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88f07-318">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="88f07-319">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-319">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-320">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-320">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="88f07-321">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-321">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-322">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-322">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-323">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-323">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-324">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-324">click **All Applications** tooview a list of all your applications.</span></span>

    1.  <span data-ttu-id="88f07-325">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-325">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-326">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88f07-326">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="88f07-327">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-327">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-328">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="88f07-328">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="88f07-329">Ange hello **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="88f07-329">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="88f07-330">Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till.</span><span class="sxs-lookup"><span data-stu-id="88f07-330">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="88f07-331">Kontrollera hello inloggning fält är synliga på hello-URL.</span><span class="sxs-lookup"><span data-stu-id="88f07-331">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="88f07-332">[Tilldela användare toohello program](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="88f07-332">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

11. <span data-ttu-id="88f07-333">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="88f07-333">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="88f07-334">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="88f07-334">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="problems-related-tooassigning-applications-toousers"></a><span data-ttu-id="88f07-335">Problem relaterade tooassigning program toousers</span><span class="sxs-lookup"><span data-stu-id="88f07-335">Problems related tooassigning applications toousers</span></span>

<span data-ttu-id="88f07-336">En användare kan inte visas ett program på deras åtkomstpanelen eftersom de inte har tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-336">A user may not be seeing an application on their Access Panel because they are not assigned toohello application.</span></span> <span data-ttu-id="88f07-337">Nedan visas några sätt toocheck:</span><span class="sxs-lookup"><span data-stu-id="88f07-337">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="88f07-338">Kontrollera om en användare har tilldelats toohello program</span><span class="sxs-lookup"><span data-stu-id="88f07-338">Check if a user is assigned toohello application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="88f07-339">Hur tooassign en tooan användarprogram direkt</span><span class="sxs-lookup"><span data-stu-id="88f07-339">How tooassign a user tooan application directly</span></span>](#how-to-assign-a-user-to-an-application-directly)

-   [<span data-ttu-id="88f07-340">Kontrollera om en användare har tilldelats tooa licens relaterade toohello program</span><span class="sxs-lookup"><span data-stu-id="88f07-340">Check if a user is assigned tooa license related toohello application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [<span data-ttu-id="88f07-341">Hur tooassign en licens tooa användare</span><span class="sxs-lookup"><span data-stu-id="88f07-341">How tooassign a license tooa user</span></span>](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-toohello-application"></a><span data-ttu-id="88f07-342">Kontrollera om en användare har tilldelats toohello program</span><span class="sxs-lookup"><span data-stu-id="88f07-342">Check if a user is assigned toohello application</span></span>

<span data-ttu-id="88f07-343">toocheck om en användare har tilldelats toohello programmet gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-343">toocheck if a user is assigned toohello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-344">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-344">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="88f07-345">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-345">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-346">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-346">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-347">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-347">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-348">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-348">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="88f07-349">**Sök** för hello namnet på hello-programmet i fråga.</span><span class="sxs-lookup"><span data-stu-id="88f07-349">**Search** for hello name of hello application in question.</span></span>

7.  <span data-ttu-id="88f07-350">Klicka på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="88f07-350">click **Users and groups**.</span></span>

8.  <span data-ttu-id="88f07-351">Kontrollera toosee om dina användare har tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-351">Check toosee if your user is assigned toohello application.</span></span>

   * <span data-ttu-id="88f07-352">Om den inte följer hello anvisningarna i ”hur tooassign en tooan användarprogram direkt” toodo så.</span><span class="sxs-lookup"><span data-stu-id="88f07-352">If not follow hello steps in “How tooassign a user tooan application directly” toodo so.</span></span>

### <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="88f07-353">Hur tooassign en tooan användarprogram direkt</span><span class="sxs-lookup"><span data-stu-id="88f07-353">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="88f07-354">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-354">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-355">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör**.</span><span class="sxs-lookup"><span data-stu-id="88f07-355">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="88f07-356">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-356">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-357">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-357">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-358">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-358">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-359">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-359">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="88f07-360">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-360">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-361">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="88f07-361">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="88f07-362">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-362">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-363">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-363">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="88f07-364">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-364">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="88f07-365">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-365">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="88f07-366">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="88f07-366">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="88f07-367">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="88f07-367">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="88f07-368">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="88f07-368">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="88f07-369">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-369">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="88f07-370">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="88f07-370">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="88f07-371">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="88f07-371">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="88f07-372">Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="88f07-372">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a><span data-ttu-id="88f07-373">Kontrollera om en användare är under en licens relaterade toohello program</span><span class="sxs-lookup"><span data-stu-id="88f07-373">Check if a user is under a license related toohello application</span></span>

<span data-ttu-id="88f07-374">toocheck en användare tilldelade licenser, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-374">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-375">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-375">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="88f07-376">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-376">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-377">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-377">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-378">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-378">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="88f07-379">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="88f07-379">click **All users**.</span></span>

6.  <span data-ttu-id="88f07-380">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="88f07-380">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="88f07-381">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="88f07-381">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

  * <span data-ttu-id="88f07-382">Hello användare som är tilldelade tooan Office-licens möjliggör detta första part Office program tooappear på hello användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="88f07-382">If hello user is assigned tooan Office license this enable First Party Office applications tooappear on hello user’s Access Panel.</span></span>

### <a name="how-tooassign-a-user-a-license"></a><span data-ttu-id="88f07-383">Hur tooassign en användare en licens</span><span class="sxs-lookup"><span data-stu-id="88f07-383">How tooassign a user a license</span></span> 

<span data-ttu-id="88f07-384">tooassign en licens tooa användare gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-384">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-385">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-385">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="88f07-386">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-386">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-387">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-387">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-388">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-388">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="88f07-389">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="88f07-389">click **All users**.</span></span>

6.  <span data-ttu-id="88f07-390">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="88f07-390">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="88f07-391">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="88f07-391">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="88f07-392">Klicka på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="88f07-392">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="88f07-393">Välj **en eller flera produkter** hello listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="88f07-393">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="88f07-394">**Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter.</span><span class="sxs-lookup"><span data-stu-id="88f07-394">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="88f07-395">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="88f07-395">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="88f07-396">Klicka på hello **tilldela** knappen tooassign dessa licenser toothis användare.</span><span class="sxs-lookup"><span data-stu-id="88f07-396">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="problems-related-tooassigning-applications-toogroups"></a><span data-ttu-id="88f07-397">Problem relaterade tooassigning program toogroups</span><span class="sxs-lookup"><span data-stu-id="88f07-397">Problems related tooassigning applications toogroups</span></span>

<span data-ttu-id="88f07-398">En användare visas kanske ett program på deras åtkomstpanelen eftersom de är en del av en grupp som har tilldelats hello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-398">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned hello application.</span></span> <span data-ttu-id="88f07-399">Nedan visas några sätt toocheck:</span><span class="sxs-lookup"><span data-stu-id="88f07-399">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="88f07-400">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="88f07-400">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="88f07-401">Hur tooassign ett program tooa gruppen direkt</span><span class="sxs-lookup"><span data-stu-id="88f07-401">How tooassign an application tooa group directly</span></span>](#how-to-assign-an-application-to-a-group-directly)

-   [<span data-ttu-id="88f07-402">Kontrollera om en användare är en del av gruppen tilldelats tooa licens</span><span class="sxs-lookup"><span data-stu-id="88f07-402">Check if a user is part of group assigned tooa license</span></span>](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [<span data-ttu-id="88f07-403">Hur tooassign en tooa licensgrupp</span><span class="sxs-lookup"><span data-stu-id="88f07-403">How tooassign a license tooa group</span></span>](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="88f07-404">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="88f07-404">Check a user’s group memberships</span></span>

<span data-ttu-id="88f07-405">toocheck en grupps medlemskap, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-405">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-406">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-406">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="88f07-407">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-407">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-408">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-408">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-409">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-409">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="88f07-410">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="88f07-410">click **All users**.</span></span>

6.  <span data-ttu-id="88f07-411">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="88f07-411">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="88f07-412">Klicka på **grupper**.</span><span class="sxs-lookup"><span data-stu-id="88f07-412">click **Groups**.</span></span>

8.  <span data-ttu-id="88f07-413">Kontrollera toosee om användaren är en del av en grupp som tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-413">Check toosee if your user is part of a Group assigned toohello application.</span></span>

  * <span data-ttu-id="88f07-414">Om du vill tooremove hello användare från hello grupp **klickar du på raden hello** i hello gruppen och väljer Ta bort.</span><span class="sxs-lookup"><span data-stu-id="88f07-414">If you want tooremove hello user from hello group, **click hello row** of hello group and select delete.</span></span>

### <a name="how-tooassign-an-application-tooa-group-directly"></a><span data-ttu-id="88f07-415">Hur tooassign ett program tooa gruppen direkt</span><span class="sxs-lookup"><span data-stu-id="88f07-415">How tooassign an application tooa group directly</span></span>

<span data-ttu-id="88f07-416">tooassign en eller flera grupper tooan programmet direkt, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-416">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-417">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör**.</span><span class="sxs-lookup"><span data-stu-id="88f07-417">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="88f07-418">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-418">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-419">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-419">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-420">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-420">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="88f07-421">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="88f07-421">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="88f07-422">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="88f07-422">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="88f07-423">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="88f07-423">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="88f07-424">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-424">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="88f07-425">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-425">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="88f07-426">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="88f07-426">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="88f07-427">Typen i hello **fullständig gruppnamn** av hello-grupp som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="88f07-427">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="88f07-428">Hovra över hello **grupp** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="88f07-428">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="88f07-429">Klicka på hello kryssrutan nästa toohello gruppens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="88f07-429">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="88f07-430">**Valfritt:** om du vill ha för**lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här gruppen toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="88f07-430">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="88f07-431">När du har valt grupper klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="88f07-431">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="88f07-432">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="88f07-432">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="88f07-433">Klicka på hello **tilldela** knappen tooassign hello programmet toohello valda grupper.</span><span class="sxs-lookup"><span data-stu-id="88f07-433">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="88f07-434">Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="88f07-434">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

### <a name="check-if-a-user-is-part-of-group-assigned-tooa-license"></a><span data-ttu-id="88f07-435">Kontrollera om en användare är en del av gruppen tilldelats tooa licens</span><span class="sxs-lookup"><span data-stu-id="88f07-435">Check if a user is part of group assigned tooa license</span></span>

1.  <span data-ttu-id="88f07-436">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-436">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="88f07-437">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-437">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-438">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-438">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-439">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-439">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="88f07-440">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="88f07-440">click **All users**.</span></span>

6.  <span data-ttu-id="88f07-441">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="88f07-441">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="88f07-442">Klicka på **grupper**.</span><span class="sxs-lookup"><span data-stu-id="88f07-442">click **Groups**.</span></span>

8.  <span data-ttu-id="88f07-443">Klicka på hello rad i en viss grupp.</span><span class="sxs-lookup"><span data-stu-id="88f07-443">click hello row of a specific group.</span></span>

9.  <span data-ttu-id="88f07-444">Klicka på **licenser** toosee vilken licenser hello-grupp har tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="88f07-444">click **Licenses** toosee which licenses hello group has assigned tooit.</span></span>

   * <span data-ttu-id="88f07-445">Om hello är tilldelade tooan Office-licens möjliggör detta tooappear vissa första part Office-program på hello användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="88f07-445">If hello group is assigned tooan Office license this may enable certain First Party Office applications tooappear on hello user’s Access Panel.</span></span>

### <a name="how-tooassign-a-license-tooa-group"></a><span data-ttu-id="88f07-446">Hur tooassign en tooa licensgrupp</span><span class="sxs-lookup"><span data-stu-id="88f07-446">How tooassign a license tooa group</span></span>

<span data-ttu-id="88f07-447">tooassign tooa licensgrupp, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="88f07-447">tooassign a license tooa group, follow hello steps below:</span></span>

1.  <span data-ttu-id="88f07-448">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="88f07-448">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="88f07-449">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-449">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="88f07-450">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="88f07-450">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="88f07-451">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="88f07-451">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="88f07-452">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="88f07-452">click **All groups**.</span></span>

6.  <span data-ttu-id="88f07-453">**Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="88f07-453">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="88f07-454">Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="88f07-454">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="88f07-455">Klicka på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="88f07-455">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="88f07-456">Välj **en eller flera produkter** hello listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="88f07-456">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="88f07-457">**Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter.</span><span class="sxs-lookup"><span data-stu-id="88f07-457">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="88f07-458">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="88f07-458">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="88f07-459">Klicka på hello **tilldela** knappen tooassign dessa licenser toothis grupp.</span><span class="sxs-lookup"><span data-stu-id="88f07-459">Click hello **Assign** button tooassign these licenses toothis group.</span></span> <span data-ttu-id="88f07-460">Det kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="88f07-460">This may take a long time, depending on hello size and complexity of hello group.</span></span>

>[!NOTE]
><span data-ttu-id="88f07-461">toodo detta snabbare, Överväg att tillfälligt tilldela en licens toohello användare direkt.</span><span class="sxs-lookup"><span data-stu-id="88f07-461">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> 
>
>

## <a name="next-steps"></a><span data-ttu-id="88f07-462">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="88f07-462">Next steps</span></span>
[<span data-ttu-id="88f07-463">Lägga till nya användare tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="88f07-463">Add new users tooAzure Active Directory</span></span>](active-directory-users-create-azure-portal.md)

