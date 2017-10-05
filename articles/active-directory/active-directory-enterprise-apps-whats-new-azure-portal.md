---
title: "Vad är nytt i Enterprise programhantering i Azure Active Directory | Microsoft Docs"
description: "Lär dig vad är nytt i Enterprise programhantering i Azure Active Directory."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 0c32a6719292aa903aa32dfdc4a31114e7a28346
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a><span data-ttu-id="3891b-103">Vad är nytt i Enterprise programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3891b-103">What's new in Enterprise Application management in Azure Active Directory</span></span> 

<span data-ttu-id="3891b-104">Azure Active Directory (AD Azure) har förbättrats enterprise hantera program, med nya funktioner och möjligheter att hantera appar enklare och effektivare.</span><span class="sxs-lookup"><span data-stu-id="3891b-104">Azure Active Directory (Azure AD) has enhanced enterprise application management tools, with new features and capabilities to make managing apps simpler and efficient.</span></span>

<span data-ttu-id="3891b-105">Här följer några av förbättringarna för Azure AD i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3891b-105">Following are some of the enhancements for Azure AD in the [Azure portal](https://portal.azure.com).</span></span>

- <span data-ttu-id="3891b-106">En förbättrad programgalleriet användarupplevelse med en förenklad tillämpning skapa modell och stöd för alla programtyper som du ofta.</span><span class="sxs-lookup"><span data-stu-id="3891b-106">An improved application gallery experience, with a simplified application creation model and support for all the application types that you’re used to.</span></span> 
- <span data-ttu-id="3891b-107">En helt ny Snabbstart-upplevelse som kan hjälpa dig att komma igång med en pilot för programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-107">A brand-new quick start experience that can help you get going with a pilot of your application.</span></span> 
- <span data-ttu-id="3891b-108">Konfigurera självbetjäning principer med bara några klickningar.</span><span class="sxs-lookup"><span data-stu-id="3891b-108">Configure self-service policies with just a few clicks.</span></span> 
- <span data-ttu-id="3891b-109">Förbättringar av application proxy enkel inloggning konfiguration och sätta egna program-upplevelser, så att du kan få mer gjort än innan.</span><span class="sxs-lookup"><span data-stu-id="3891b-109">Improvements to application proxy, single sign-on configuration, and bring your own application experiences, allowing you to get more done than before.</span></span>

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a><span data-ttu-id="3891b-110">Förbättringar av Azure Active Directory-Programgalleriet</span><span class="sxs-lookup"><span data-stu-id="3891b-110">Improvements to the Azure Active Directory Application Gallery</span></span>

<span data-ttu-id="3891b-111">Lägg till program, oavsett om de är från den [programgalleriet](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), anpassade program som du utöka till molnet eller nya program som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="3891b-111">Add your favorite applications whether they are from the [application gallery](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), custom applications you’re extending to the cloud, or new applications you’re developing.</span></span>  <span data-ttu-id="3891b-112">Du kan komma igång med den nya upplevelsen genom att klicka på **Lägg till** på den **företagsprogram** översikt eller **alla program** blad.</span><span class="sxs-lookup"><span data-stu-id="3891b-112">You can get started with this new experience by clicking **Add** on the **Enterprise Applications** overview or **All applications** blades.</span></span>
 
  ![Lägga till ett program](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

<span data-ttu-id="3891b-114">En gång i galleriet ser du våra aktuella program som stöder användaretablering visas främre och center.</span><span class="sxs-lookup"><span data-stu-id="3891b-114">Once in the gallery, you’ll see all our featured applications which support user provisioning displayed front-and-center.</span></span>  <span data-ttu-id="3891b-115">Du kan bläddra i alla typer av olika kategorier till de program som du är intresserad mer detaljerat eller du kan använda sökupplevelsen för att snabbt hitta de program som du vill integrera.</span><span class="sxs-lookup"><span data-stu-id="3891b-115">You can browse all sorts of different categories to drill into the applications you care about, or you can use the search experience to rapidly find the applications you want to integrate.</span></span>

  ![Programgalleriet](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a><span data-ttu-id="3891b-117">Lägga till anpassade program från en plats</span><span class="sxs-lookup"><span data-stu-id="3891b-117">Add custom applications from one place</span></span>

<span data-ttu-id="3891b-118">Förutom att lägga till redan integrerade program från galleriet är alla anpassade programkonfigurationen inträffar som du har använt till i den klassiska hanteringsportalen nu möjligt i den nya portalen.</span><span class="sxs-lookup"><span data-stu-id="3891b-118">In addition to adding pre-integrated applications from the gallery, all the custom application configuration experiences that you were used to in the classic management portal are now possible in the new portal.</span></span> <span data-ttu-id="3891b-119">Om du vill utvidga ett program från lokalt med programproxyn integrera ditt eget lösenord eller en extern SSO-program, eller skapar en helt ny program använda registret för programmet, kan du till den från den här en enda plats.</span><span class="sxs-lookup"><span data-stu-id="3891b-119">Whether you are extending an application from on-premises using the application proxy, integrating your own password or federated SSO application, or creating a brand-new application using the application registry, you can get to it all from this one single place.</span></span>

  ![Lägga till egna program](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
<span data-ttu-id="3891b-121">**Börja lägga till egna program**:</span><span class="sxs-lookup"><span data-stu-id="3891b-121">**To get started adding your own application**:</span></span>

1. <span data-ttu-id="3891b-122">Klicka på den **lägga till en egen länk** överst i galleriet för programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-122">Click the **add your own link** at the top of the application gallery.</span></span> 
2. <span data-ttu-id="3891b-123">Du ser två alternativ framför du: **distribuera ett befintligt program** eller **utveckla ett nytt program**.</span><span class="sxs-lookup"><span data-stu-id="3891b-123">You’ll see two options in front of you: **deploy an existing application** or **develop a new application**.</span></span> <span data-ttu-id="3891b-124">Läs vidare för att lära dig skillnaden mellan de två alternativen och hur de används.</span><span class="sxs-lookup"><span data-stu-id="3891b-124">Read on to learn the difference between the two options and how to use them.</span></span>

### <a name="deploying-existing-applications"></a><span data-ttu-id="3891b-125">Distribuera befintliga program</span><span class="sxs-lookup"><span data-stu-id="3891b-125">Deploying existing applications</span></span>

1. <span data-ttu-id="3891b-126">Om du har ett program som redan kör och bara vill integrera den med Azure AD för enkel inloggning på eller etablering, Välj den **distribuera ett befintligt program** alternativet.</span><span class="sxs-lookup"><span data-stu-id="3891b-126">If you’ve got an application running already and just want to integrate it into Azure AD for single-sign on or provisioning, choose the **Deploy an existing application** option.</span></span> <span data-ttu-id="3891b-127">Välj ett namn för programmet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3891b-127">Pick a name for your application, click **Add**.</span></span>
2. <span data-ttu-id="3891b-128">Klart!</span><span class="sxs-lookup"><span data-stu-id="3891b-128">That's it!</span></span> <span data-ttu-id="3891b-129">I stället för att behöva känna till hur detaljerna om ditt program direkt, kan du nu ställa in hur programmet fungerar genom att navigera genom den vänstra menyn och konfigurera programmet så att dina önskemål när som helst.</span><span class="sxs-lookup"><span data-stu-id="3891b-129">Instead of needing to know all the details about your application up front, you can now set up how your new application works by navigating through the left menu and configuring the application to your liking at any time.</span></span>

  ![Lägga till ett befintligt program med ett enda klick](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a><span data-ttu-id="3891b-131">Utveckla nya program</span><span class="sxs-lookup"><span data-stu-id="3891b-131">Developing new applications</span></span>

1. <span data-ttu-id="3891b-132">Om du utvecklar ett nytt program är det enkelt för dig att få till registret för programmet från galleriet:</span><span class="sxs-lookup"><span data-stu-id="3891b-132">If you’re developing a new application, there's an easy way for you to get to the Application Registry right from the gallery:</span></span>
2. <span data-ttu-id="3891b-133">Klicka på den **lägga till egna** alternativet från galleriet program, Välj den **utveckla ett befintligt program** val och du ser en snabb länk till Lägg till programupplevelse.</span><span class="sxs-lookup"><span data-stu-id="3891b-133">Click on the **add your own** option from the Application Gallery, select the **develop an existing application** choice, and you’ll see a quick link right to the application add experience.</span></span>

  ![Lägga till en nyligen utvecklat program i ett par klick](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
><span data-ttu-id="3891b-135">När du lägger till ett program använda registret för programmet, visas den visas i listan över företagsprogram där du kommer att kunna konfigurera enkel inloggning och hantera principer för åtkomst för det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-135">Once you add an application using the Application Registry, you’ll see it show up in the list of Enterprise Applications where you’ll be able to configure single sign-on and manage access policies for your new application.</span></span>

  ![Hantera åtkomst till det nya programmet under företagsprogram](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a><span data-ttu-id="3891b-137">Snabbstartsguide: komma igång med ditt nya program direkt</span><span class="sxs-lookup"><span data-stu-id="3891b-137">Quick start: Get going with your new application right away</span></span> 

<span data-ttu-id="3891b-138">När du har lagt till ett program, om det vara förintegrerade eller egna app har vi skapat en skräddarsydd upplevelse Snabbkurs att få med dig snabbt grundat i den nya upplevelsen för program.</span><span class="sxs-lookup"><span data-stu-id="3891b-138">After you’ve added an application, whether it be pre-integrated or your own app, we’ve created a tailored quick-start experience to get you grounded in the new applications experience quickly.</span></span> <span data-ttu-id="3891b-139">Om du följer varje alternativ systematiskt vi vägleder dig genom Användargränssnittet och hur du kan komma igång med en pilot för det nya programmet så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="3891b-139">If you follow each option systematically, we’ll walk you through the UI and show you how to get going with a pilot of your new application as quickly as possible.</span></span> 
 
  ![De nya program snabb start upplevelse](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 <span data-ttu-id="3891b-141">Du kan besöka den nya Snabbstart upplevelsen när som helst och för alla program genom att klicka på **Snabbstart** från den vänstra navigeringsmenyn i programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-141">You can visit this new quick start experience at any time, and for any application, by clicking on **Quick start** from the application left navigation menu.</span></span>


## <a name="updated-application-proxy-configuration"></a><span data-ttu-id="3891b-142">Uppdaterade program proxykonfiguration</span><span class="sxs-lookup"><span data-stu-id="3891b-142">Updated application proxy configuration</span></span>
<span data-ttu-id="3891b-143">Nu ska vi Säg något av de nya program som du har lagt till körs i din lokala miljö och du vill integrera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3891b-143">Now, let’s say one of the new applications you added is running in your on-premises environment and you want to integrate it with Azure AD.</span></span>  <span data-ttu-id="3891b-144">En kall Nyheter på den nya konfigurationen upplevelsen i den nya Azure AD-portalen är att dela upp programmets inloggning-läge från dess proxykonfiguration för programmet, du kan nu enkelt exponera lösenord SSO eller federerade program som körs i företagsnätverket direkt till molnet, utan att behöva skapa flera instanser av programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-144">One of the cool new things about the new application configuration experience in the new Azure AD portal is that by splitting the application’s sign-on mode from its application proxy configuration, you can now easily expose password SSO or federated applications that are running in your corporate network right to the cloud, without having to create multiple instances of the application.</span></span>

<span data-ttu-id="3891b-145">Utöver detta, kan du nu också konfigurera något nytt program du har lagt till för användning med Azure AD Application Proxy direkt från den nya portalen, inklusive program som stöder intern upplevelser för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3891b-145">In addition to this, you can now also configure any of the new applications you’ve added for use with the Azure AD Application Proxy right from the new portal, including applications which support native Windows authentication experiences.</span></span>

  ![Konfigurera ett program att använda integrerad Windows-autentisering inloggning alternativet](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

<span data-ttu-id="3891b-147">Du kommer igång med att konfigurera en intern Windows-autentisering program med Application Proxy:</span><span class="sxs-lookup"><span data-stu-id="3891b-147">To get started configuring a native Windows authentication application with the Application Proxy:</span></span>
1. <span data-ttu-id="3891b-148">Klicka på enkel inloggning navigeringsobjektet och välj **integrerad Windows-autentisering** i bladet inställningar inloggning och konfigurera inställningarna enligt dina önskemål.</span><span class="sxs-lookup"><span data-stu-id="3891b-148">Click on the Single sign-on navigation item and choose **Integrated Windows Authentication** from the sign-on settings blade and configure the settings to your liking.</span></span>
2. <span data-ttu-id="3891b-149">Utöver att stödja dessa nya autentiseringslägen, kan du även överföra certifikat från anpassade domäner att stödja program som körs på säkra slutpunkter inom din organisation.</span><span class="sxs-lookup"><span data-stu-id="3891b-149">On top of supporting these new authentication modes, you can now also upload certificates from custom domains to support applications running on secure endpoints within your organization.</span></span>  
 
   ![Överföra ett certifikat som ska användas med Application Proxy](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. <span data-ttu-id="3891b-151">Om du vill överföra ett nytt certifikat för din favorit lokalt program klickar du på den **programproxy** från den vänstra navigeringsmenyn, klickar du på den **certifikat** selector och ladda upp en certifikatfil som vi kan använda för att kryptera förfrågningar från våra molnslutpunkt för ditt program.</span><span class="sxs-lookup"><span data-stu-id="3891b-151">To upload a new certificate for your favorite on-premises application, click on the **Application proxy** option from the left navigation menu, click the **Certificate** selector, and upload a certificate file we can use to encrypt requests from our cloud endpoint to your application.</span></span>

## <a name="advanced-federated-single-sign-on-configuration"></a><span data-ttu-id="3891b-152">Avancerad federerad enkel inloggning konfiguration</span><span class="sxs-lookup"><span data-stu-id="3891b-152">Advanced federated single sign-on configuration</span></span>

<span data-ttu-id="3891b-153">För de som du använder idag federerade program, finns det många nya funktioner i bladet SAML-baserad inloggning konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3891b-153">For those of you using federated applications today, there are many new features in the SAML-based sign-on configuration blade.</span></span> <span data-ttu-id="3891b-154">Börja med, kan nu du helt anpassa, lägga till, ta bort och mappa befintliga användarattribut som anspråk i SAML-token.</span><span class="sxs-lookup"><span data-stu-id="3891b-154">To start with, now you can fully customize, add, remove, and map the existing user attributes issued as claims in SAML tokens.</span></span>
 
  ![Anpassa attribut för SAML-token användaren skickades till externa program](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


<span data-ttu-id="3891b-156">Kontrollera att ut den nya federerad enkel inloggning konfiguration:</span><span class="sxs-lookup"><span data-stu-id="3891b-156">To check that out the new federated SSO configuration:</span></span>
1. <span data-ttu-id="3891b-157">Öppna en federerade program **enkel inloggning** bladet från den vänstra navigeringsmenyn och kontrollera att den '*SAML-baserade inloggning** läge är markerad.</span><span class="sxs-lookup"><span data-stu-id="3891b-157">Open a federated application’s **single sign-on** blade from the left navigation menu and make sure the ‘*SAML-based Sign-on** mode is selected.</span></span> 
2. <span data-ttu-id="3891b-158">En gång, aktivera kryssrutan under den **användarattribut** rubrik att ändra alla attribut som ingår i en SAML-token som skickades till programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-158">Once there, enable the checkbox under the **User Attributes** heading to modify all of the attributes included in the SAML token passed to that application.</span></span>

<span data-ttu-id="3891b-159">Du kan också skapa, förnyelse, och hantera certifikat för federerad enkel inloggning, samt redigera som hämtar ett meddelande när certifikatet upphör snart att gälla.</span><span class="sxs-lookup"><span data-stu-id="3891b-159">You can also create, rollover, and manage certificates used for federated single sign-on, as well as edit who gets notified when your certificate is about to expire.</span></span> <span data-ttu-id="3891b-160">Du ser dessa nya alternativ under den **certifikat** rubrik i samma enkel inloggning bladet.</span><span class="sxs-lookup"><span data-stu-id="3891b-160">You’ll see these new options under the **Certificates** heading on the same Single sign-on blade.</span></span>
 
  ![Skapa ett nytt certifikat, anpassa e-postmeddelanden upphör att gälla och alternativ för Certifikatsignering](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a><span data-ttu-id="3891b-162">Relay tillstånd paramenter</span><span class="sxs-lookup"><span data-stu-id="3891b-162">Relay State paramenter</span></span>
<span data-ttu-id="3891b-163">Slutligen har vi också utökade uppsättning URL för SAML-parametrar som vi stöder för att inkludera den **Relay tillstånd parametern**, vilket är den sida som användarna hamnar på inuti en externa program när inloggningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3891b-163">Finally, we’ve also extended the set of SAML URL parameters we support to include the **Relay State parameter**, which is the page your users will land on inside of a federated application once the sign-in is completed.</span></span> <span data-ttu-id="3891b-164">Detta är mycket användbar inställningen för att konfigurera om du vill skicka dina användare till en specifik plats i programmet för att få dem går snabbt.</span><span class="sxs-lookup"><span data-stu-id="3891b-164">This is very useful setting to configure if you want to send your users to a specific place within the application to get them going quickly.</span></span>

  ![Parametern SAML Relay tillstånd](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
<span data-ttu-id="3891b-166">**Ange parametern state relay**:</span><span class="sxs-lookup"><span data-stu-id="3891b-166">**To set the relay state parameter**:</span></span>

1. <span data-ttu-id="3891b-167">Aktivera den **visa avancerade inställningar för URL: en** kryssrutan under den **domän och URL: er** rubriken på den enkel inloggning på bladet för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3891b-167">Enable the **Show advanced URL settings** check-box under the **Domain and URLs** heading on the single-sign on configuration blade.</span></span> 
2. <span data-ttu-id="3891b-168">När du gör det visas en uppsättning nya URL: en inkommande rutor visas som gör att du kan ange den här och andra, SAML-URL-inställningar.</span><span class="sxs-lookup"><span data-stu-id="3891b-168">Once you do this, you’ll see a set of new URL input boxes appear which will allow you to set this, and other, SAML URL settings.</span></span>

## <a name="bring-your-own-password-sso-applications"></a><span data-ttu-id="3891b-169">Ta med ditt eget lösenord SSO-program</span><span class="sxs-lookup"><span data-stu-id="3891b-169">Bring your own password SSO applications</span></span>

<span data-ttu-id="3891b-170">Vi vet att inte alla program har stöd för identitetsfederation direkt ur lådan.</span><span class="sxs-lookup"><span data-stu-id="3891b-170">We know that not every application supports federation right out of the box.</span></span> <span data-ttu-id="3891b-171">Till exempel kanske har en av de nya program som du har lagt till en anpassad inloggningsskärm som användarna använda ett användarnamn och lösenord för att logga in på.</span><span class="sxs-lookup"><span data-stu-id="3891b-171">For example, maybe one of the new applications you added has a custom login screen that your users use a username and password to sign in to.</span></span> <span data-ttu-id="3891b-172">Du kan fortfarande integrera dessa typer av program med Azure AD med hjälp av vår **sätta egna program** funktionen som är nu tillgänglig för dig att konfigurera i den nya portalen.</span><span class="sxs-lookup"><span data-stu-id="3891b-172">You can still integrate these types of applications with Azure AD using our **Bring your own applications** feature, which is now available for you to configure in the new portal.</span></span>
 
  ![Integrera anpassade lösenord vaulting program med Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

<span data-ttu-id="3891b-174">**Checka ut funktionen Ta med egna program**:</span><span class="sxs-lookup"><span data-stu-id="3891b-174">**To check out the 'Bring your own applications' feature**:</span></span>

1. <span data-ttu-id="3891b-175">När du ställer in enkel inloggning läge för ett nytt anpassat program som du har lagt till **lösenordsbaserade inloggning**, ange URL: en där programmet återgivningar dess inloggningssidan och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3891b-175">After you set the single sign-on mode for a new custom application that you’ve added to **Password-based Sign-on**, enter the URL where the application renders its login screen and click **Save**.</span></span>  
2. <span data-ttu-id="3891b-176">När du gör det kan vi ska automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och du kan använda Azure AD för att säkert överföra lösenord för det aktuella programmet med hjälp av webbläsartillägget för åtkomst-panelen.</span><span class="sxs-lookup"><span data-stu-id="3891b-176">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span>

## <a name="configure-self-service-application-access"></a><span data-ttu-id="3891b-177">Konfigurera självbetjäning programåtkomst</span><span class="sxs-lookup"><span data-stu-id="3891b-177">Configure self-service application access</span></span>

<span data-ttu-id="3891b-178">När du har lagt till många nya program, kanske vill du tillåta användarna att bläddra och lägga till dessa program till sina egna åtkomst paneler, utan att behöva bry dig du som administratör.</span><span class="sxs-lookup"><span data-stu-id="3891b-178">After you’ve added lots of new applications, maybe you want to allow your users to browse and add those applications to their own access panels, without needing to bother you as an administrator.</span></span> <span data-ttu-id="3891b-179">Med den senaste versionen kan du nu konfigurera och hantera självbetjäning programåtkomst direkt från den nya portalen.</span><span class="sxs-lookup"><span data-stu-id="3891b-179">Now, with this latest release, you can configure and manage self-service application access right from the new portal.</span></span>

  ![Konfigurera självbetjäning programåtkomst lösenord SSO-program](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
<span data-ttu-id="3891b-181">**Konfigurera och hantera självbetjäning programåtkomst**:</span><span class="sxs-lookup"><span data-stu-id="3891b-181">**To configure and manage self-service application access**:</span></span>

1. <span data-ttu-id="3891b-182">Om du vill komma igång, kan du välja den **självbetjäning** alternativet från programmet har lämnat navigeringsmenyn och ange den **Tillåt användare att begära åtkomst till det här programmet?** att '**Ja**'.</span><span class="sxs-lookup"><span data-stu-id="3891b-182">To get started, you can select the **Self-service** option from the application’s left navigation menu and set the **Allow users to request access to this application?** option to ‘**Yes**’.</span></span> 
2. <span data-ttu-id="3891b-183">Detta gör att du konfigurerar som har tillåtelse att godkänna åtkomst till det här programmet och vilken grupp Självbetjäningsanvändare läggs.</span><span class="sxs-lookup"><span data-stu-id="3891b-183">This will enable you to configure who is allowed to approve access to this application, and which group self-service users will be added.</span></span> <span data-ttu-id="3891b-184">Dessutom, om programmet har konfigurerats för lösenord enkel inloggning på, får du också se ett annat alternativ som gör att du kan låta de godkännarna för att hantera de lösenord som tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="3891b-184">In addition, if the application is configured for password single-sign on, you’ll also see another option which lets you optionally allow those approvers to manage the passwords assigned to the application.</span></span>

##<a name="feedback"></a><span data-ttu-id="3891b-185">Feedback</span><span class="sxs-lookup"><span data-stu-id="3891b-185">Feedback</span></span>

<span data-ttu-id="3891b-186">Vi hoppas att du vill med hjälp av den förbättrade Azure AD-upplevelse.</span><span class="sxs-lookup"><span data-stu-id="3891b-186">We hope you like using the improved Azure AD experience.</span></span> <span data-ttu-id="3891b-187">Skriv ned feedback kommer!</span><span class="sxs-lookup"><span data-stu-id="3891b-187">Please keep the feedback coming!</span></span> <span data-ttu-id="3891b-188">Publicera din feedback och förslag på förbättringar i den **administrationsportalen** avsnitt i vår [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span><span class="sxs-lookup"><span data-stu-id="3891b-188">Post your feedback and ideas for improvement in the **Admin Portal** section of our [feedback forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span></span>  <span data-ttu-id="3891b-189">Vi är glada om hur du skapar nya nya produkter varje dag, och använder din information i form och definiera vad vi bygga härnäst.</span><span class="sxs-lookup"><span data-stu-id="3891b-189">We’re excited about building cool new stuff every day, and use your guidance to shape and define what we build next.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3891b-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3891b-190">Next steps</span></span>

<span data-ttu-id="3891b-191">Mer information finns i [hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md).</span><span class="sxs-lookup"><span data-stu-id="3891b-191">For more details, see [Managing Applications with Azure Active Directory](active-directory-enable-sso-scenario.md).</span></span>



