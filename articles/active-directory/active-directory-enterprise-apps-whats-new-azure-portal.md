---
title: "Vad är nytt i Enterprise programhantering i Azure Active Directory aaa | Microsoft Docs"
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
ms.openlocfilehash: 7f4b7b11b256f1e910e557f45f3709d762416f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a><span data-ttu-id="7f501-103">Vad är nytt i Enterprise programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7f501-103">What's new in Enterprise Application management in Azure Active Directory</span></span> 

<span data-ttu-id="7f501-104">Azure Active Directory (AD Azure) har förbättrats hanteringsverktyg för enterprise-programmet, med nya funktioner och möjligheter toomake hantera appar enklare och effektivare.</span><span class="sxs-lookup"><span data-stu-id="7f501-104">Azure Active Directory (Azure AD) has enhanced enterprise application management tools, with new features and capabilities toomake managing apps simpler and efficient.</span></span>

<span data-ttu-id="7f501-105">Här följer några av hello förbättringar för Azure AD i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f501-105">Following are some of hello enhancements for Azure AD in hello [Azure portal](https://portal.azure.com).</span></span>

- <span data-ttu-id="7f501-106">En förbättrad programgalleriet användarupplevelse med en förenklad tillämpning skapa modell och stöd för alla hello programtyper som du ofta.</span><span class="sxs-lookup"><span data-stu-id="7f501-106">An improved application gallery experience, with a simplified application creation model and support for all hello application types that you’re used to.</span></span> 
- <span data-ttu-id="7f501-107">En helt ny Snabbstart-upplevelse som kan hjälpa dig att komma igång med en pilot för programmet.</span><span class="sxs-lookup"><span data-stu-id="7f501-107">A brand-new quick start experience that can help you get going with a pilot of your application.</span></span> 
- <span data-ttu-id="7f501-108">Konfigurera självbetjäning principer med bara några klickningar.</span><span class="sxs-lookup"><span data-stu-id="7f501-108">Configure self-service policies with just a few clicks.</span></span> 
- <span data-ttu-id="7f501-109">Förbättringar tooapplication proxy, enkel inloggning konfiguration och sätta egna program-upplevelser, så att du tooget göra mer än tidigare.</span><span class="sxs-lookup"><span data-stu-id="7f501-109">Improvements tooapplication proxy, single sign-on configuration, and bring your own application experiences, allowing you tooget more done than before.</span></span>

## <a name="improvements-toohello-azure-active-directory-application-gallery"></a><span data-ttu-id="7f501-110">Förbättringar av toohello Azure Active Directory-Programgalleriet</span><span class="sxs-lookup"><span data-stu-id="7f501-110">Improvements toohello Azure Active Directory Application Gallery</span></span>

<span data-ttu-id="7f501-111">Lägg till program, oavsett om de är från hello [programgalleriet](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), anpassade program du utvidgar toohello moln eller nya program som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="7f501-111">Add your favorite applications whether they are from hello [application gallery](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), custom applications you’re extending toohello cloud, or new applications you’re developing.</span></span>  <span data-ttu-id="7f501-112">Du kan komma igång med den nya upplevelsen genom att klicka på **Lägg till** på hello **företagsprogram** översikt eller **alla program** blad.</span><span class="sxs-lookup"><span data-stu-id="7f501-112">You can get started with this new experience by clicking **Add** on hello **Enterprise Applications** overview or **All applications** blades.</span></span>
 
  ![Lägga till ett program](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

<span data-ttu-id="7f501-114">En gång i hello gallery ser du våra aktuella program som stöder användaretablering visas främre och center.</span><span class="sxs-lookup"><span data-stu-id="7f501-114">Once in hello gallery, you’ll see all our featured applications which support user provisioning displayed front-and-center.</span></span>  <span data-ttu-id="7f501-115">Du kan bläddra alla typer av olika kategorier toodrill i hello-program som du är intresserad eller du kan använda hello upplevelse toorapidly hitta hello sökprogram du vill toointegrate.</span><span class="sxs-lookup"><span data-stu-id="7f501-115">You can browse all sorts of different categories toodrill into hello applications you care about, or you can use hello search experience toorapidly find hello applications you want toointegrate.</span></span>

  ![hello-programgalleriet](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a><span data-ttu-id="7f501-117">Lägga till anpassade program från en plats</span><span class="sxs-lookup"><span data-stu-id="7f501-117">Add custom applications from one place</span></span>

<span data-ttu-id="7f501-118">Dessutom tooadding redan integrerade program från hello-galleriet och anpassade program konfiguration av alla hello-upplevelser som du har använt tooin hello klassiska hanteringsportalen är nu möjligt i hello nya portalen.</span><span class="sxs-lookup"><span data-stu-id="7f501-118">In addition tooadding pre-integrated applications from hello gallery, all hello custom application configuration experiences that you were used tooin hello classic management portal are now possible in hello new portal.</span></span> <span data-ttu-id="7f501-119">Om du vill utvidga ett program från lokala använder hello programproxy, integrera ditt eget lösenord eller en extern SSO-program, eller skapar en helt ny program med hjälp av hello registret för programmet, du kan hämta tooit från den här ett enda Placera.</span><span class="sxs-lookup"><span data-stu-id="7f501-119">Whether you are extending an application from on-premises using hello application proxy, integrating your own password or federated SSO application, or creating a brand-new application using hello application registry, you can get tooit all from this one single place.</span></span>

  ![Lägga till egna program](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
<span data-ttu-id="7f501-121">**tooget börjar lägga till ditt eget program**:</span><span class="sxs-lookup"><span data-stu-id="7f501-121">**tooget started adding your own application**:</span></span>

1. <span data-ttu-id="7f501-122">Klicka på hello **lägga till en egen länk** hello överst i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="7f501-122">Click hello **add your own link** at hello top of hello application gallery.</span></span> 
2. <span data-ttu-id="7f501-123">Du ser två alternativ framför du: **distribuera ett befintligt program** eller **utveckla ett nytt program**.</span><span class="sxs-lookup"><span data-stu-id="7f501-123">You’ll see two options in front of you: **deploy an existing application** or **develop a new application**.</span></span> <span data-ttu-id="7f501-124">Läs vidare toolearn hello skillnaden mellan hello två alternativ och hur toouse dem.</span><span class="sxs-lookup"><span data-stu-id="7f501-124">Read on toolearn hello difference between hello two options and how toouse them.</span></span>

### <a name="deploying-existing-applications"></a><span data-ttu-id="7f501-125">Distribuera befintliga program</span><span class="sxs-lookup"><span data-stu-id="7f501-125">Deploying existing applications</span></span>

1. <span data-ttu-id="7f501-126">Om du har ett program som redan kör och bara vill toointegrate till Azure AD för enkel inloggning på eller etablering, Välj hello **distribuera ett befintligt program** alternativet.</span><span class="sxs-lookup"><span data-stu-id="7f501-126">If you’ve got an application running already and just want toointegrate it into Azure AD for single-sign on or provisioning, choose hello **Deploy an existing application** option.</span></span> <span data-ttu-id="7f501-127">Välj ett namn för programmet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7f501-127">Pick a name for your application, click **Add**.</span></span>
2. <span data-ttu-id="7f501-128">Klart!</span><span class="sxs-lookup"><span data-stu-id="7f501-128">That's it!</span></span> <span data-ttu-id="7f501-129">I stället för att behöva tooknow alla hello information om programmet direkt, kan du nu ställa in hur programmet fungerar genom att navigera genom hello vänstra menyn och konfigurera hello programmet tooyour önskemål när som helst.</span><span class="sxs-lookup"><span data-stu-id="7f501-129">Instead of needing tooknow all hello details about your application up front, you can now set up how your new application works by navigating through hello left menu and configuring hello application tooyour liking at any time.</span></span>

  ![Lägga till ett befintligt program med ett enda klick](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a><span data-ttu-id="7f501-131">Utveckla nya program</span><span class="sxs-lookup"><span data-stu-id="7f501-131">Developing new applications</span></span>

1. <span data-ttu-id="7f501-132">Om du utvecklar ett nytt program kan du enkelt för du tooget toohello programmet registret direkt från galleriet hello:</span><span class="sxs-lookup"><span data-stu-id="7f501-132">If you’re developing a new application, there's an easy way for you tooget toohello Application Registry right from hello gallery:</span></span>
2. <span data-ttu-id="7f501-133">Klicka på hello **lägga till egna** alternativet från hello Application Gallery väljer hello **utveckla ett befintligt program** val och se en snabb länk rätt toohello program lägga till upplevelse.</span><span class="sxs-lookup"><span data-stu-id="7f501-133">Click on hello **add your own** option from hello Application Gallery, select hello **develop an existing application** choice, and you’ll see a quick link right toohello application add experience.</span></span>

  ![Lägga till en nyligen utvecklat program i ett par klick](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
><span data-ttu-id="7f501-135">När du lägger till ett program med hello registret för programmet, visas den visa upp i hello lista över företagsprogram där du kommer att kunna tooconfigure enkel inloggning och hantera principer för åtkomst för det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="7f501-135">Once you add an application using hello Application Registry, you’ll see it show up in hello list of Enterprise Applications where you’ll be able tooconfigure single sign-on and manage access policies for your new application.</span></span>

  ![Hantera åtkomst tooyour nytt program under företagsprogram](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a><span data-ttu-id="7f501-137">Snabbstartsguide: komma igång med ditt nya program direkt</span><span class="sxs-lookup"><span data-stu-id="7f501-137">Quick start: Get going with your new application right away</span></span> 

<span data-ttu-id="7f501-138">När du har lagt till ett program, om det vara förintegrerade eller egna app har vi skapat en skräddarsydd upplevelse för Snabbkurs tooget du snabbt grundat i hello nya program upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="7f501-138">After you’ve added an application, whether it be pre-integrated or your own app, we’ve created a tailored quick-start experience tooget you grounded in hello new applications experience quickly.</span></span> <span data-ttu-id="7f501-139">Om du följer varje alternativ systematiskt vi dig igenom hello UI och visar hur tooget igång med en pilot för det nya programmet så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="7f501-139">If you follow each option systematically, we’ll walk you through hello UI and show you how tooget going with a pilot of your new application as quickly as possible.</span></span> 
 
  ![hello nya program snabb start upplevelse](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 <span data-ttu-id="7f501-141">Du kan besöka den nya Snabbstart upplevelsen när som helst och för alla program genom att klicka på **Snabbstart** från hello programmenyn vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="7f501-141">You can visit this new quick start experience at any time, and for any application, by clicking on **Quick start** from hello application left navigation menu.</span></span>


## <a name="updated-application-proxy-configuration"></a><span data-ttu-id="7f501-142">Uppdaterade program proxykonfiguration</span><span class="sxs-lookup"><span data-stu-id="7f501-142">Updated application proxy configuration</span></span>
<span data-ttu-id="7f501-143">Nu ska vi Säg en hello nya program som du har lagt till körs i din lokala miljö och du vill toointegrate den med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f501-143">Now, let’s say one of hello new applications you added is running in your on-premises environment and you want toointegrate it with Azure AD.</span></span>  <span data-ttu-id="7f501-144">Ett av hello kall Nyheter på hello ny konfiguration programupplevelse hello nya Azure AD-portalen är att dela upp hello programmet inloggning-läge från dess proxykonfiguration för programmet, du kan nu enkelt exponera lösenord SSO eller federerad program som körs i företagsnätverket högra toohello moln, utan att behöva toocreate flera instanser av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="7f501-144">One of hello cool new things about hello new application configuration experience in hello new Azure AD portal is that by splitting hello application’s sign-on mode from its application proxy configuration, you can now easily expose password SSO or federated applications that are running in your corporate network right toohello cloud, without having toocreate multiple instances of hello application.</span></span>

<span data-ttu-id="7f501-145">Dessutom toothis, du kan också konfigurera något hello nya program som du har lagt till för användning med hello Azure AD Application Proxy direkt från hello nya portalen, inklusive program som stöder intern Windows-autentisering inträffar.</span><span class="sxs-lookup"><span data-stu-id="7f501-145">In addition toothis, you can now also configure any of hello new applications you’ve added for use with hello Azure AD Application Proxy right from hello new portal, including applications which support native Windows authentication experiences.</span></span>

  ![Konfigurera ett program toouse hello integrerad Windows-autentisering inloggning alternativet](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

<span data-ttu-id="7f501-147">tooget igång med att konfigurera en intern Windows-program för autentisering med hello Application Proxy:</span><span class="sxs-lookup"><span data-stu-id="7f501-147">tooget started configuring a native Windows authentication application with hello Application Proxy:</span></span>
1. <span data-ttu-id="7f501-148">Klicka på hello enkel inloggning navigeringsobjektet och välj **integrerad Windows-autentisering** från hello inloggning inställningsbladet och konfigurera hello inställningar tooyour önskemål.</span><span class="sxs-lookup"><span data-stu-id="7f501-148">Click on hello Single sign-on navigation item and choose **Integrated Windows Authentication** from hello sign-on settings blade and configure hello settings tooyour liking.</span></span>
2. <span data-ttu-id="7f501-149">Utöver att stödja dessa nya autentiseringslägen, kan du även överföra certifikat från anpassade domäner toosupport program som körs på säkra slutpunkter inom din organisation.</span><span class="sxs-lookup"><span data-stu-id="7f501-149">On top of supporting these new authentication modes, you can now also upload certificates from custom domains toosupport applications running on secure endpoints within your organization.</span></span>  
 
   ![Överföra ett certifikat toobe används med hello Application Proxy](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. <span data-ttu-id="7f501-151">tooupload ett nytt certifikat för din favorit lokalt program klickar du på hello **programproxy** hello vänstra navigeringsfönstret-menyn, klickar på hello **certifikat** selector och ladda upp en certifikatfilen som vi kan använda tooencrypt begäranden från våra tooyour slutpunktsprogrammet moln.</span><span class="sxs-lookup"><span data-stu-id="7f501-151">tooupload a new certificate for your favorite on-premises application, click on hello **Application proxy** option from hello left navigation menu, click hello **Certificate** selector, and upload a certificate file we can use tooencrypt requests from our cloud endpoint tooyour application.</span></span>

## <a name="advanced-federated-single-sign-on-configuration"></a><span data-ttu-id="7f501-152">Avancerad federerad enkel inloggning konfiguration</span><span class="sxs-lookup"><span data-stu-id="7f501-152">Advanced federated single sign-on configuration</span></span>

<span data-ttu-id="7f501-153">För de som du använder idag federerade program, finns det många nya funktioner i hello SAML-baserad inloggning konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="7f501-153">For those of you using federated applications today, there are many new features in hello SAML-based sign-on configuration blade.</span></span> <span data-ttu-id="7f501-154">toostart med, nu du kan anpassa, lägga till, ta bort och mappa hello befintliga användare-attribut som anspråk i SAML-token.</span><span class="sxs-lookup"><span data-stu-id="7f501-154">toostart with, now you can fully customize, add, remove, and map hello existing user attributes issued as claims in SAML tokens.</span></span>
 
  ![Anpassa hello SAML-token användarattribut skickades tooa federerade program](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


<span data-ttu-id="7f501-156">toocheck som ut hello nya federerad enkel inloggning konfiguration:</span><span class="sxs-lookup"><span data-stu-id="7f501-156">toocheck that out hello new federated SSO configuration:</span></span>
1. <span data-ttu-id="7f501-157">Öppna en federerade program **enkel inloggning** bladet från hello vänstra navigeringsfönstret-menyn och se till att hello '*SAML-baserade inloggning** läge är markerad.</span><span class="sxs-lookup"><span data-stu-id="7f501-157">Open a federated application’s **single sign-on** blade from hello left navigation menu and make sure hello ‘*SAML-based Sign-on** mode is selected.</span></span> 
2. <span data-ttu-id="7f501-158">En gång, aktivera hello kryssrutan under hello **användarattribut** rubrik toomodify alla hello attribut ingår i hello SAML-token skickades toothat program.</span><span class="sxs-lookup"><span data-stu-id="7f501-158">Once there, enable hello checkbox under hello **User Attributes** heading toomodify all of hello attributes included in hello SAML token passed toothat application.</span></span>

<span data-ttu-id="7f501-159">Du kan också skapa, förnyelse, och hantera certifikat för federerad enkel inloggning, samt redigera som hämtar ett meddelande när ditt certifikat är om tooexpire.</span><span class="sxs-lookup"><span data-stu-id="7f501-159">You can also create, rollover, and manage certificates used for federated single sign-on, as well as edit who gets notified when your certificate is about tooexpire.</span></span> <span data-ttu-id="7f501-160">Du ser dessa nya alternativ under hello **certifikat** rubriken hello samma bladet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7f501-160">You’ll see these new options under hello **Certificates** heading on hello same Single sign-on blade.</span></span>
 
  ![Skapa ett nytt certifikat, anpassa e-postmeddelanden upphör att gälla och alternativ för Certifikatsignering](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a><span data-ttu-id="7f501-162">Relay tillstånd paramenter</span><span class="sxs-lookup"><span data-stu-id="7f501-162">Relay State paramenter</span></span>
<span data-ttu-id="7f501-163">Slutligen har vi också utökade hello uppsättning URL för SAML-parametrar vi stöder tooinclude hello **Relay tillstånd parametern**, vilket är hello sidan användarna hamnar på inuti en externa program när hello inloggningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7f501-163">Finally, we’ve also extended hello set of SAML URL parameters we support tooinclude hello **Relay State parameter**, which is hello page your users will land on inside of a federated application once hello sign-in is completed.</span></span> <span data-ttu-id="7f501-164">Detta är mycket användbart inställningen tooconfigure om du vill toosend ditt användare tooa specifika placeras inom hello programmet tooget går snabbt.</span><span class="sxs-lookup"><span data-stu-id="7f501-164">This is very useful setting tooconfigure if you want toosend your users tooa specific place within hello application tooget them going quickly.</span></span>

  ![Hello SAML Relay tillstånd för parametern](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
<span data-ttu-id="7f501-166">**Parametern för tooset hello relay state**:</span><span class="sxs-lookup"><span data-stu-id="7f501-166">**tooset hello relay state parameter**:</span></span>

1. <span data-ttu-id="7f501-167">Aktivera hello **visa avancerade inställningar för URL: en** kryssrutan under hello **domän och URL: er** rubriken på hello enkel inloggning på bladet för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7f501-167">Enable hello **Show advanced URL settings** check-box under hello **Domain and URLs** heading on hello single-sign on configuration blade.</span></span> 
2. <span data-ttu-id="7f501-168">När du gör det visas en uppsättning nya URL: en inkommande rutor visas som gör att du tooset denna och andra inställningar för SAML-URL.</span><span class="sxs-lookup"><span data-stu-id="7f501-168">Once you do this, you’ll see a set of new URL input boxes appear which will allow you tooset this, and other, SAML URL settings.</span></span>

## <a name="bring-your-own-password-sso-applications"></a><span data-ttu-id="7f501-169">Ta med ditt eget lösenord SSO-program</span><span class="sxs-lookup"><span data-stu-id="7f501-169">Bring your own password SSO applications</span></span>

<span data-ttu-id="7f501-170">Vi vet att inte alla program stöder federation höger out of hello box.</span><span class="sxs-lookup"><span data-stu-id="7f501-170">We know that not every application supports federation right out of hello box.</span></span> <span data-ttu-id="7f501-171">Till exempel kanske har en hello nya program som du har lagt till en anpassad inloggningsskärm som användarna använda ett användarnamn och lösenord toosign i till.</span><span class="sxs-lookup"><span data-stu-id="7f501-171">For example, maybe one of hello new applications you added has a custom login screen that your users use a username and password toosign in to.</span></span> <span data-ttu-id="7f501-172">Du kan fortfarande integrera dessa typer av program med Azure AD med hjälp av vår **sätta egna program** funktionen som är nu tillgänglig för du tooconfigure i hello nya portalen.</span><span class="sxs-lookup"><span data-stu-id="7f501-172">You can still integrate these types of applications with Azure AD using our **Bring your own applications** feature, which is now available for you tooconfigure in hello new portal.</span></span>
 
  ![Integrera anpassade lösenord vaulting program med Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

<span data-ttu-id="7f501-174">**toocheck ut hello ta med egna program funktionen**:</span><span class="sxs-lookup"><span data-stu-id="7f501-174">**toocheck out hello 'Bring your own applications' feature**:</span></span>

1. <span data-ttu-id="7f501-175">När du ställer in hello enkel inloggning läge för ett nytt anpassat program som du har lagt till för**lösenordsbaserade inloggning**, ange hello URL där programmet hello återgivningar dess inloggningssidan och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7f501-175">After you set hello single sign-on mode for a new custom application that you’ve added too**Password-based Sign-on**, enter hello URL where hello application renders its login screen and click **Save**.</span></span>  
2. <span data-ttu-id="7f501-176">När du gör det, kommer vi automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och kan du överföra toouse Azure AD toosecurely lösenord toothat program med hjälp av hello åtkomst panelen webbläsartillägg.</span><span class="sxs-lookup"><span data-stu-id="7f501-176">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span>

## <a name="configure-self-service-application-access"></a><span data-ttu-id="7f501-177">Konfigurera självbetjäning programåtkomst</span><span class="sxs-lookup"><span data-stu-id="7f501-177">Configure self-service application access</span></span>

<span data-ttu-id="7f501-178">När du har lagt till många nya program, kanske du vill tooallow toobrowse din användare och lägga till dessa program tootheir egna åtkomst paneler, utan att behöva toobother du som administratör.</span><span class="sxs-lookup"><span data-stu-id="7f501-178">After you’ve added lots of new applications, maybe you want tooallow your users toobrowse and add those applications tootheir own access panels, without needing toobother you as an administrator.</span></span> <span data-ttu-id="7f501-179">Med den senaste versionen kan du nu konfigurera och hantera självbetjäning programåtkomst direkt från hello nya portalen.</span><span class="sxs-lookup"><span data-stu-id="7f501-179">Now, with this latest release, you can configure and manage self-service application access right from hello new portal.</span></span>

  ![Konfigurera självbetjäning programåtkomst lösenord SSO-program](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
<span data-ttu-id="7f501-181">**tooconfigure och hantera självbetjäning programåtkomst**:</span><span class="sxs-lookup"><span data-stu-id="7f501-181">**tooconfigure and manage self-service application access**:</span></span>

1. <span data-ttu-id="7f501-182">tooget igång, kan du välja hello **självbetjäning** alternativet från hello program kvar navigeringsmenyn och ange hello **toorequest åtkomst toothis program för användarna?** för alternativet ' **Ja**'.</span><span class="sxs-lookup"><span data-stu-id="7f501-182">tooget started, you can select hello **Self-service** option from hello application’s left navigation menu and set hello **Allow users toorequest access toothis application?** option too‘**Yes**’.</span></span> 
2. <span data-ttu-id="7f501-183">Detta gör att du tooconfigure vem som får tooapprove åtkomst toothis program och vilken grupp Självbetjäningsanvändare läggs.</span><span class="sxs-lookup"><span data-stu-id="7f501-183">This will enable you tooconfigure who is allowed tooapprove access toothis application, and which group self-service users will be added.</span></span> <span data-ttu-id="7f501-184">Dessutom om hello programmet har konfigurerats för lösenord enkel inloggning på kan visas även ett annat alternativ som gör du om du vill att dessa godkännare toomanage hello lösenord tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="7f501-184">In addition, if hello application is configured for password single-sign on, you’ll also see another option which lets you optionally allow those approvers toomanage hello passwords assigned toohello application.</span></span>

##<a name="feedback"></a><span data-ttu-id="7f501-185">Feedback</span><span class="sxs-lookup"><span data-stu-id="7f501-185">Feedback</span></span>

<span data-ttu-id="7f501-186">Vi hoppas att du vill med hjälp av hello förbättrad upplevelse för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f501-186">We hope you like using hello improved Azure AD experience.</span></span> <span data-ttu-id="7f501-187">Skriv ned hello feedback kommer!</span><span class="sxs-lookup"><span data-stu-id="7f501-187">Please keep hello feedback coming!</span></span> <span data-ttu-id="7f501-188">Publicera din feedback och förslag på förbättringar i hello **administrationsportalen** avsnitt i vår [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span><span class="sxs-lookup"><span data-stu-id="7f501-188">Post your feedback and ideas for improvement in hello **Admin Portal** section of our [feedback forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span></span>  <span data-ttu-id="7f501-189">Vi är glada om hur du skapar nya nya produkter varje dag, och använda vägledning-tooshape och definiera vad vi bygga härnäst.</span><span class="sxs-lookup"><span data-stu-id="7f501-189">We’re excited about building cool new stuff every day, and use your guidance tooshape and define what we build next.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f501-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f501-190">Next steps</span></span>

<span data-ttu-id="7f501-191">Mer information finns i [hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md).</span><span class="sxs-lookup"><span data-stu-id="7f501-191">For more details, see [Managing Applications with Azure Active Directory](active-directory-enable-sso-scenario.md).</span></span>



