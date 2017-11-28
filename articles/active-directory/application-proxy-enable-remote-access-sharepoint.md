---
title: "aaaEnable fjärråtkomst tooSharePoint med Azure AD Application Proxy | Microsoft Docs"
description: Beskriver hello grunderna om hur toointegrate ett lokalt SharePoint server med Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6ab413462fcaf387e150449df9c97505c4108bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-access-toosharepoint-with-azure-ad-application-proxy"></a><span data-ttu-id="3ce02-103">Aktivera fjärråtkomst tooSharePoint med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="3ce02-103">Enable remote access tooSharePoint with Azure AD Application Proxy</span></span>

<span data-ttu-id="3ce02-104">Den här artikeln beskrivs hur toointegrate ett lokalt SharePoint server med Azure Active Directory (Azure AD) Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="3ce02-104">This article discusses how toointegrate an on-premises SharePoint server with Azure Active Directory (Azure AD) Application Proxy.</span></span>

<span data-ttu-id="3ce02-105">tooenable fjärråtkomst tooSharePoint med Azure AD Application Proxy Följ hello avsnitt i den här artikeln steg för steg.</span><span class="sxs-lookup"><span data-stu-id="3ce02-105">tooenable remote access tooSharePoint with Azure AD Application Proxy, follow hello sections in this article step by step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ce02-106">Krav</span><span class="sxs-lookup"><span data-stu-id="3ce02-106">Prerequisites</span></span>

<span data-ttu-id="3ce02-107">Den här artikeln förutsätter att du redan har SharePoint 2013 eller senare i din miljö.</span><span class="sxs-lookup"><span data-stu-id="3ce02-107">This article assumes that you already have SharePoint 2013 or newer in your environment.</span></span> <span data-ttu-id="3ce02-108">Tänk dessutom hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="3ce02-108">In addition, consider hello following prerequisites:</span></span>

* <span data-ttu-id="3ce02-109">SharePoint innehåller inbyggt stöd för Kerberos.</span><span class="sxs-lookup"><span data-stu-id="3ce02-109">SharePoint includes native Kerberos support.</span></span> <span data-ttu-id="3ce02-110">Användare som ansluter till interna webbplatser via fjärranslutning via Azure AD Application Proxy kan därför anta toohave en enkel inloggning (SSO) uppstår.</span><span class="sxs-lookup"><span data-stu-id="3ce02-110">Therefore, users who are accessing internal sites remotely through Azure AD Application Proxy can assume toohave a single sign-on (SSO) experience.</span></span>

* <span data-ttu-id="3ce02-111">Du måste toomake några ändringar tooyour SharePoint konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="3ce02-111">You need toomake a few configuration changes tooyour SharePoint server.</span></span> <span data-ttu-id="3ce02-112">Vi rekommenderar att du använder en fristående miljö.</span><span class="sxs-lookup"><span data-stu-id="3ce02-112">We recommend using a staging environment.</span></span> <span data-ttu-id="3ce02-113">På så sätt kan du kan göra uppdateringar tooyour mellanlagring server först och underlätta en tester cykel innan du fortsätter till produktionen.</span><span class="sxs-lookup"><span data-stu-id="3ce02-113">This way, you can make updates tooyour staging server first, and then facilitate a testing cycle before going into production.</span></span>

* <span data-ttu-id="3ce02-114">Vi förutsätter att du har ställt in SSL för SharePoint, eftersom vi kräver SSL på hello publicerade URL.</span><span class="sxs-lookup"><span data-stu-id="3ce02-114">We assume that you have already set up SSL for SharePoint, because we require SSL on hello published URL.</span></span> <span data-ttu-id="3ce02-115">Du måste toohave SSL aktiverat på webbplatsen interna tooensure att länkar skickas/mappas korrekt.</span><span class="sxs-lookup"><span data-stu-id="3ce02-115">You need toohave SSL enabled on your internal site, tooensure that links are sent/mapped correctly.</span></span> <span data-ttu-id="3ce02-116">Om du inte har konfigurerat SSL, se [Konfigurera SSL för SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="3ce02-116">If you haven't configured SSL, see [Configure SSL for SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) for instructions.</span></span> <span data-ttu-id="3ce02-117">Kontrollera också att hello connector förtroenden hello datorcertifikat som du skickar.</span><span class="sxs-lookup"><span data-stu-id="3ce02-117">Also, make sure that hello connector machine trusts hello certificate that you issue.</span></span> <span data-ttu-id="3ce02-118">(hello certifikat behöver inte toobe offentligt utfärdade.)</span><span class="sxs-lookup"><span data-stu-id="3ce02-118">(hello certificate does not need toobe publicly issued.)</span></span>

## <a name="step-1-set-up-single-sign-on-toosharepoint"></a><span data-ttu-id="3ce02-119">Steg 1: Konfigurera enkel inloggning tooSharePoint</span><span class="sxs-lookup"><span data-stu-id="3ce02-119">Step 1: Set up single sign-on tooSharePoint</span></span>

<span data-ttu-id="3ce02-120">Våra kunder vill hello bästa SSO-upplevelse för deras backend-program, SharePoint-servern i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="3ce02-120">Our customers want hello best SSO experience for their back-end applications, SharePoint server in this case.</span></span> <span data-ttu-id="3ce02-121">I det här vanliga scenariot i Azure AD hello användaren autentiseras bara en gång, eftersom de inte uppmanas att autentiseringen igen.</span><span class="sxs-lookup"><span data-stu-id="3ce02-121">In this common Azure AD scenario, hello user is authenticated only once, because they will not be prompted for authentication again.</span></span>

<span data-ttu-id="3ce02-122">Du kan uppnå SSO för lokala program som kräver eller använder Windows-autentisering med hjälp av hello Kerberos-autentiseringsprotokollet och en funktion som kallas Kerberos-begränsad delegering (KCD).</span><span class="sxs-lookup"><span data-stu-id="3ce02-122">For on-premises applications that require or use Windows authentication, you can achieve SSO by using hello Kerberos authentication protocol and a feature called Kerberos constrained delegation (KCD).</span></span> <span data-ttu-id="3ce02-123">KCD när konfigurerad, kan hello Application Proxy connector tooobtain windows biljett/token för en användare, även om hello användaren inte har loggat in tooWindows direkt.</span><span class="sxs-lookup"><span data-stu-id="3ce02-123">KCD, when configured, allows hello Application Proxy connector tooobtain a windows ticket/token for a user, even if hello user hasn’t signed in tooWindows directly.</span></span> <span data-ttu-id="3ce02-124">toolearn mer om KCD, se [Kerberos-begränsad delegering översikt](https://technet.microsoft.com/library/jj553400.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ce02-124">toolearn more about KCD, see [Kerberos Constrained Delegation Overview](https://technet.microsoft.com/library/jj553400.aspx).</span></span>

<span data-ttu-id="3ce02-125">tooset in KCD för en SharePoint-server, Använd hello procedurerna i hello följande sekventiella avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3ce02-125">tooset up KCD for a SharePoint server, use hello procedures in hello following sequential sections:</span></span>

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a><span data-ttu-id="3ce02-126">Kontrollera att SharePoint körs under ett tjänstkonto</span><span class="sxs-lookup"><span data-stu-id="3ce02-126">Ensure that SharePoint is running under a service account</span></span>

<span data-ttu-id="3ce02-127">Kontrollera först att SharePoint körs under en definierad tjänstkonto--inte lokalt system, lokal tjänst eller Nätverkstjänst.</span><span class="sxs-lookup"><span data-stu-id="3ce02-127">First, make sure that SharePoint is running under a defined service account--not local system, local service, or network service.</span></span> <span data-ttu-id="3ce02-128">Gör så att du kan koppla tjänstens huvudnamn (SPN) tooa giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="3ce02-128">Do this so that you can attach service principal names (SPNs) tooa valid account.</span></span> <span data-ttu-id="3ce02-129">SPN-namn är hur hello Kerberos-protokollet identifierar olika tjänster.</span><span class="sxs-lookup"><span data-stu-id="3ce02-129">SPNs are how hello Kerberos protocol identifies different services.</span></span> <span data-ttu-id="3ce02-130">Och du behöver hello konto senare tooconfigure hello KCD.</span><span class="sxs-lookup"><span data-stu-id="3ce02-130">And you will need hello account later tooconfigure hello KCD.</span></span>

<span data-ttu-id="3ce02-131">tooensure som dina platser körs under en definierad tjänstkonto utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3ce02-131">tooensure that your sites are running under a defined service account, perform hello following steps:</span></span>

1. <span data-ttu-id="3ce02-132">Öppna hello **Central Administration av SharePoint 2013** plats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-132">Open hello **SharePoint 2013 Central Administration** site.</span></span>
2. <span data-ttu-id="3ce02-133">Gå för**säkerhet** och välj **Konfigurera tjänstkonton**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-133">Go too**Security** and select **Configure service accounts**.</span></span>
3. <span data-ttu-id="3ce02-134">Välj **Web programpoolen - SharePoint - 80**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-134">Select **Web Application Pool - SharePoint - 80**.</span></span> <span data-ttu-id="3ce02-135">hello alternativ kan avvika något baserat på hello namnet på din webbserver-pool eller om hello web pool använder SSL som standard.</span><span class="sxs-lookup"><span data-stu-id="3ce02-135">hello options may be slightly different based on hello name of your web pool, or if hello web pool uses SSL by default.</span></span>

  ![Alternativ för att konfigurera ett tjänstkonto](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. <span data-ttu-id="3ce02-137">Om **Markera ett konto för den här komponenten** är **lokal tjänst** eller **nätverkstjänsten**, behöver du toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="3ce02-137">If **Select an account for this component** is **Local Service** or **Network Service**, you need toocreate an account.</span></span> <span data-ttu-id="3ce02-138">Om inte du är klar och kan flytta toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3ce02-138">If not, you're finished and can move toohello next section.</span></span>
5. <span data-ttu-id="3ce02-139">Välj **registrera nytt hanterat konto**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-139">Select **Register new managed account**.</span></span> <span data-ttu-id="3ce02-140">När ditt konto har skapats måste du ange **webbprogrampoolen** innan du kan använda hello-konto.</span><span class="sxs-lookup"><span data-stu-id="3ce02-140">After your account is created, you must set **Web Application Pool** before you can use hello account.</span></span>

> [!NOTE]
<span data-ttu-id="3ce02-141">Du behöver toohave en tidigare skapade Azure AD-kontot för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3ce02-141">You need toohave a previously created Azure AD account for hello service.</span></span> <span data-ttu-id="3ce02-142">Vi rekommenderar att du tillåter en automatisk ändring av lösenord.</span><span class="sxs-lookup"><span data-stu-id="3ce02-142">We suggest that you allow for an automatic password change.</span></span> <span data-ttu-id="3ce02-143">Mer information om hello fullständig uppsättning steg och felsökningsproblem finns [konfigurera automatisk ändring av lösenord i SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ce02-143">For more information about hello full set of steps and troubleshooting issues, see [Configure automatic password change in SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).</span></span>

### <a name="configure-sharepoint-for-kerberos"></a><span data-ttu-id="3ce02-144">Konfigurera SharePoint för Kerberos</span><span class="sxs-lookup"><span data-stu-id="3ce02-144">Configure SharePoint for Kerberos</span></span>

<span data-ttu-id="3ce02-145">Du använder KCD tooperform enkel inloggning toohello SharePoint server och det fungerar bara med Kerberos.</span><span class="sxs-lookup"><span data-stu-id="3ce02-145">You use KCD tooperform single sign-on toohello SharePoint server, and this works only with Kerberos.</span></span>

<span data-ttu-id="3ce02-146">tooconfigure för din SharePoint-webbplatsen för Kerberos-autentisering:</span><span class="sxs-lookup"><span data-stu-id="3ce02-146">tooconfigure your SharePoint site for Kerberos authentication:</span></span>

1. <span data-ttu-id="3ce02-147">Öppna hello **Central Administration av SharePoint 2013** plats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-147">Open hello **SharePoint 2013 Central Administration** site.</span></span>
2. <span data-ttu-id="3ce02-148">Gå för**programhantering**väljer **hantera webbprogram**, och välj din SharePoint-webbplats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-148">Go too**Application Management**, select **Manage web applications**, and select your SharePoint site.</span></span> <span data-ttu-id="3ce02-149">I det här exemplet är det **SharePoint - 80**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-149">In this example, it is **SharePoint - 80**.</span></span>

  ![Att välja hello SharePoint-webbplats](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)

3. <span data-ttu-id="3ce02-151">Klicka på **autentiseringsproviders** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="3ce02-151">Click **Authentication Providers** on hello toolbar.</span></span>
4. <span data-ttu-id="3ce02-152">I hello **autentiseringsproviders** klickar du på **standardzonen** tooview hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="3ce02-152">In hello **Authentication Providers** box, click **Default Zone** tooview hello settings.</span></span>
5. <span data-ttu-id="3ce02-153">I hello **Redigera autentisering** dialogrutan rutan, bläddra nedåt tills du ser **Anspråksautentiseringstyper** och kontrollera att både **aktivera Windows-autentisering** och  **Integrerad Windows-autentisering** har valts.</span><span class="sxs-lookup"><span data-stu-id="3ce02-153">In hello **Edit Authentication** dialog box, scroll down until you see **Claims Authentication Types** and ensure that both **Enable Windows Authentication** and **Integrated Windows Authentication** are selected.</span></span>
6. <span data-ttu-id="3ce02-154">I hello listrutan, se till att **förhandla (Kerberos)** är markerad.</span><span class="sxs-lookup"><span data-stu-id="3ce02-154">In hello drop-down box, make sure that **Negotiate (Kerberos)** is selected.</span></span>

  ![Dialogrutan Redigera autentisering](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. <span data-ttu-id="3ce02-156">Längst ned hello hello **Redigera autentisering** dialogrutan klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-156">At hello bottom of hello **Edit Authentication** dialog box, click **Save**.</span></span>

### <a name="set-a-service-principal-name-for-hello-sharepoint-service-account"></a><span data-ttu-id="3ce02-157">Ange tjänstens huvudnamn för hello SharePoint-tjänstkonto</span><span class="sxs-lookup"><span data-stu-id="3ce02-157">Set a service principal name for hello SharePoint service account</span></span>

<span data-ttu-id="3ce02-158">Innan du konfigurerar hello KCD måste tooidentify hello SharePoint-tjänsten körs som hello-tjänstkonto som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="3ce02-158">Before you configure hello KCD, you need tooidentify hello SharePoint service running as hello service account that you've configured.</span></span> <span data-ttu-id="3ce02-159">Det gör du genom att ange ett SPN-namn.</span><span class="sxs-lookup"><span data-stu-id="3ce02-159">You do this by setting an SPN.</span></span> <span data-ttu-id="3ce02-160">Mer information finns i [Service Principal Names](https://technet.microsoft.com/library/cc961723.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ce02-160">For more information, see [Service Principal Names](https://technet.microsoft.com/library/cc961723.aspx).</span></span>

<span data-ttu-id="3ce02-161">hello SPN-format är:</span><span class="sxs-lookup"><span data-stu-id="3ce02-161">hello SPN format is:</span></span>

```
<service class>/<host>:<port>
```

<span data-ttu-id="3ce02-162">Hello SPN-format:</span><span class="sxs-lookup"><span data-stu-id="3ce02-162">In hello SPN format:</span></span>

* <span data-ttu-id="3ce02-163">_tjänsten klassen_ är ett unikt namn för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3ce02-163">_service class_ is a unique name for hello service.</span></span> <span data-ttu-id="3ce02-164">För SharePoint, använder du **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-164">For SharePoint, you use **HTTP**.</span></span>

* <span data-ttu-id="3ce02-165">_värden_ är hello fullständigt kvalificerade domännamnet eller NetBIOS-namnet på hello värddatorn hello tjänsten körs på.</span><span class="sxs-lookup"><span data-stu-id="3ce02-165">_host_ is hello fully qualified domain or NetBIOS name of hello host that hello service is running on.</span></span> <span data-ttu-id="3ce02-166">Den här texten kanske måste toobe hello URL hello plats, beroende på hello version av IIS som du använder för en SharePoint-webbplats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-166">For a SharePoint site, this text might need toobe hello URL of hello site, depending on hello version of IIS that you're using.</span></span>

* <span data-ttu-id="3ce02-167">_port_ är valfritt.</span><span class="sxs-lookup"><span data-stu-id="3ce02-167">_port_ is optional.</span></span>

<span data-ttu-id="3ce02-168">Om hello hello SharePoint serverns FQDN är:</span><span class="sxs-lookup"><span data-stu-id="3ce02-168">If hello FQDN of hello SharePoint server is:</span></span>

```
sharepoint.demo.o365identity.us
```

<span data-ttu-id="3ce02-169">Sedan hello SPN är:</span><span class="sxs-lookup"><span data-stu-id="3ce02-169">Then hello SPN is:</span></span>

```
HTTP/ sharepoint.demo.o365identity.us demo
```

<span data-ttu-id="3ce02-170">Du kanske också tooset SPN för specifika webbplatser på servern.</span><span class="sxs-lookup"><span data-stu-id="3ce02-170">You might also need tooset SPNs for specific sites on your server.</span></span> <span data-ttu-id="3ce02-171">Mer information finns i [konfigurera Kerberos-autentisering](https://technet.microsoft.com/library/cc263449(v=office.12).aspx).</span><span class="sxs-lookup"><span data-stu-id="3ce02-171">For more information, see [Configure Kerberos authentication](https://technet.microsoft.com/library/cc263449(v=office.12).aspx).</span></span> <span data-ttu-id="3ce02-172">Betala uppmärksam toohello avsnittet ”Skapa tjänstens huvudnamn för ditt webbprogram med hjälp av Kerberos-autentisering”.</span><span class="sxs-lookup"><span data-stu-id="3ce02-172">Pay close attention toohello section "Create Service Principal Names for your Web applications using Kerberos authentication."</span></span>

<span data-ttu-id="3ce02-173">Hej enklast för tooset SPN toofollow hello SPN format som kanske redan finns i dina platser.</span><span class="sxs-lookup"><span data-stu-id="3ce02-173">hello easiest way for you tooset SPNs is toofollow hello SPN formats that may already be present for your sites.</span></span> <span data-ttu-id="3ce02-174">Kopiera dessa SPN tooregister mot hello-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="3ce02-174">Copy those SPNs tooregister against hello service account.</span></span> <span data-ttu-id="3ce02-175">toodo detta:</span><span class="sxs-lookup"><span data-stu-id="3ce02-175">toodo this:</span></span>

1. <span data-ttu-id="3ce02-176">Bläddra toohello plats med hello SPN från en annan dator.</span><span class="sxs-lookup"><span data-stu-id="3ce02-176">Browse toohello site with hello SPN from another machine.</span></span>
 <span data-ttu-id="3ce02-177">När du gör hello cachelagras relevanta uppsättning Kerberos-biljetter på hello datorn.</span><span class="sxs-lookup"><span data-stu-id="3ce02-177">When you do, hello relevant set of Kerberos tickets is cached on hello machine.</span></span> <span data-ttu-id="3ce02-178">Dessa biljetter innehåller hello SPN för hello målwebbplatsen som du har öppnat.</span><span class="sxs-lookup"><span data-stu-id="3ce02-178">These tickets contain hello SPN of hello target site that you browsed to.</span></span>

2. <span data-ttu-id="3ce02-179">Du kan dra hello SPN för den platsen med hjälp av verktyget [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html).</span><span class="sxs-lookup"><span data-stu-id="3ce02-179">You can pull hello SPN for that site by using a tool called [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html).</span></span> <span data-ttu-id="3ce02-180">I ett kommandofönster som körs i hello hello samma kontext som hello användare som öppnade hello plats i hello webbläsaren kan köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3ce02-180">In a command window that's running in hello same context as hello user who accessed hello site in hello browser, run hello following command:</span></span>
```
Klist
```
<span data-ttu-id="3ce02-181">Klist returnerar sedan hello uppsättning mål SPN-namn.</span><span class="sxs-lookup"><span data-stu-id="3ce02-181">Klist then returns hello set of target SPNs.</span></span> <span data-ttu-id="3ce02-182">I det här exemplet är hello markerade värdet hello SPN som behövs:</span><span class="sxs-lookup"><span data-stu-id="3ce02-182">In this example, hello highlighted value is hello SPN that's needed:</span></span>

  ![Exempel Klist resultat](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. <span data-ttu-id="3ce02-184">Nu när du har hello SPN måste toomake du att den är korrekt konfigurerade på hello-tjänstkonto som du skapat tidigare hello-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3ce02-184">Now that you have hello SPN, you need toomake sure that it's configured correctly on hello service account that you set up for hello web application earlier.</span></span> <span data-ttu-id="3ce02-185">Kör följande kommando från hello kommandotolk som administratör för hello domän hello:</span><span class="sxs-lookup"><span data-stu-id="3ce02-185">Run hello following command from hello command prompt as an administrator of hello domain:</span></span>

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 <span data-ttu-id="3ce02-186">Det här kommandot anger hello SPN för hello SharePoint service-kontot körs som _demo\sp_svc_.</span><span class="sxs-lookup"><span data-stu-id="3ce02-186">This command sets hello SPN for hello SharePoint service account running as _demo\sp_svc_.</span></span>

 <span data-ttu-id="3ce02-187">Ersätt _http/sharepoint.demo.o365identity.us_ med hello SPN för servern och _demo\sp_svc_ med hello-tjänstkontot i din miljö.</span><span class="sxs-lookup"><span data-stu-id="3ce02-187">Replace _http/sharepoint.demo.o365identity.us_ with hello SPN for your server and _demo\sp_svc_ with hello service account in your environment.</span></span> <span data-ttu-id="3ce02-188">hello Setspn kommando söker efter hello SPN innan den lägger till den.</span><span class="sxs-lookup"><span data-stu-id="3ce02-188">hello Setspn command searches for hello SPN before it adds it.</span></span> <span data-ttu-id="3ce02-189">I det här fallet kan du se en **dubbla SPN-värdet** fel.</span><span class="sxs-lookup"><span data-stu-id="3ce02-189">In this case, you might see a **Duplicate SPN Value** error.</span></span> <span data-ttu-id="3ce02-190">Om du ser det här felet, se till att hello-värdet är associerad med hello-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="3ce02-190">If you see this error, make sure that hello value is associated with hello service account.</span></span>

<span data-ttu-id="3ce02-191">Du kan verifiera att SPN har lagts till genom att köra hello Setspn kommando med hello -l alternativet hello.</span><span class="sxs-lookup"><span data-stu-id="3ce02-191">You can verify that hello SPN was added by running hello Setspn command with hello -l option.</span></span> <span data-ttu-id="3ce02-192">toolearn mer om det här kommandot finns [Setspn](https://technet.microsoft.com/library/cc731241.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ce02-192">toolearn more about this command, see [Setspn](https://technet.microsoft.com/library/cc731241.aspx).</span></span>

### <a name="ensure-that-hello-connector-is-set-as-a-trusted-delegate-toosharepoint"></a><span data-ttu-id="3ce02-193">Se till att kopplingen hello har angetts som en betrodd delegera tooSharePoint</span><span class="sxs-lookup"><span data-stu-id="3ce02-193">Ensure that hello connector is set as a trusted delegate tooSharePoint</span></span>

<span data-ttu-id="3ce02-194">Konfigurera hello KCD så att hello Azure AD Application Proxy-tjänsten kan delegera användaren identiteter toohello SharePoint-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3ce02-194">Configure hello KCD so that hello Azure AD Application Proxy service can delegate user identities toohello SharePoint service.</span></span> <span data-ttu-id="3ce02-195">Du gör detta genom att aktivera hello Application Proxy connector tooretrieve Kerberos-biljetter för de användare som har verifierats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3ce02-195">You do this by enabling hello Application Proxy connector tooretrieve Kerberos tickets for your users who have been authenticated in Azure AD.</span></span> <span data-ttu-id="3ce02-196">Servern skickar sedan hello kontexten toohello målprogrammet eller SharePoint i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="3ce02-196">Then that server passes hello context toohello target application, or SharePoint in this case.</span></span>

<span data-ttu-id="3ce02-197">tooconfigure hello KCD, upprepa hello följande steg för varje koppling dator:</span><span class="sxs-lookup"><span data-stu-id="3ce02-197">tooconfigure hello KCD, repeat hello following steps for each connector machine:</span></span>

1. <span data-ttu-id="3ce02-198">Logga in som en domän administratören tooa DC och öppna sedan **Active Directory-användare och datorer**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-198">Log in as a domain administrator tooa DC, and then open **Active Directory Users and Computers**.</span></span>
2. <span data-ttu-id="3ce02-199">Hitta hello-dator som hello connector körs på.</span><span class="sxs-lookup"><span data-stu-id="3ce02-199">Find hello computer that hello connector is running on.</span></span> <span data-ttu-id="3ce02-200">I det här exemplet har den hello samma SharePoint-servern.</span><span class="sxs-lookup"><span data-stu-id="3ce02-200">In this example, it's hello same SharePoint server.</span></span>
3. <span data-ttu-id="3ce02-201">Dubbelklicka på hello datorn och klicka sedan på hello **delegering** fliken.</span><span class="sxs-lookup"><span data-stu-id="3ce02-201">Double-click hello computer, and then click hello **Delegation** tab.</span></span>
4. <span data-ttu-id="3ce02-202">Se till att hello delegeringsinställningar är inställda för**förtroende för den här datorn för delegering toohello angetts services endast**, och välj sedan **Använd valfritt autentiseringsprotokoll**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-202">Ensure that hello delegation settings are set too**Trust this computer for delegation toohello specified services only**, and then select **Use any authentication protocol**.</span></span>

  ![Inställningarna för standarddelegering](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)

5. <span data-ttu-id="3ce02-204">Klicka på hello **Lägg till** , klicka på **användare eller datorer**, och leta upp hello-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="3ce02-204">Click hello **Add** button, click **Users or Computers**, and locate hello service account.</span></span>

  ![Att lägga till hello SPN för hello-tjänstkontot](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

6. <span data-ttu-id="3ce02-206">I hello lista över SPN-namn, väljer du hello som du skapade tidigare för hello-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="3ce02-206">In hello list of SPNs, select hello one that you created earlier for hello service account.</span></span>
7. <span data-ttu-id="3ce02-207">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-207">Click **OK**.</span></span> <span data-ttu-id="3ce02-208">Klicka på **OK** igen toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="3ce02-208">Click **OK** again toosave hello changes.</span></span>

## <a name="step-2-enable-remote-access-toosharepoint"></a><span data-ttu-id="3ce02-209">Steg 2: Aktivera fjärråtkomst tooSharePoint</span><span class="sxs-lookup"><span data-stu-id="3ce02-209">Step 2: Enable remote access tooSharePoint</span></span>

<span data-ttu-id="3ce02-210">Nu när du har aktiverat SharePoint för Kerberos och konfigurerat KCD, är du redo tooset in tooSharePoint för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3ce02-210">Now that you’ve enabled SharePoint for Kerberos and configured KCD, you're ready tooset up single sign-on tooSharePoint.</span></span> <span data-ttu-id="3ce02-211">Du kan sedan publicera hello SharePoint-servergruppen för fjärråtkomst via Azure AD Application Proxy från hello-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3ce02-211">Then from hello connector, you can publish hello SharePoint farm for remote access through Azure AD Application Proxy.</span></span>

<span data-ttu-id="3ce02-212">tooperform hello följande steg, behöver du toobe medlem i hello rollen Global administratör i organisationens Azure Active Directory-konto.</span><span class="sxs-lookup"><span data-stu-id="3ce02-212">tooperform hello following steps, you need toobe a member of hello Global Administrator Role in your organization's Azure Active Directory account.</span></span>

1. <span data-ttu-id="3ce02-213">Logga in toohello [Azure-portalen](https://manage.windowsazure.com) och hitta din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="3ce02-213">Sign in toohello [Azure portal](https://manage.windowsazure.com) and find your Azure AD tenant.</span></span>
2. <span data-ttu-id="3ce02-214">Klicka på **program**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-214">Click **Applications**, and then click **Add**.</span></span>
3. <span data-ttu-id="3ce02-215">Välj **Publicera ett program som ska vara tillgängligt utanför ditt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-215">Select **Publish an application that will be accessible from outside your network**.</span></span> <span data-ttu-id="3ce02-216">Om du inte ser det här alternativet, se till att du har Azure AD Basic eller Premium som angetts i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="3ce02-216">If you don’t see this option, make sure that you have Azure AD Basic or Premium set up in hello tenant.</span></span>
4. <span data-ttu-id="3ce02-217">Slutför hello alternativ enligt följande:</span><span class="sxs-lookup"><span data-stu-id="3ce02-217">Complete each of hello options as follows:</span></span>
 * <span data-ttu-id="3ce02-218">**Namnet**: Använd ett värde som du vill till exempel **SharePoint**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-218">**Name**: Use any value that you want--for example, **SharePoint**.</span></span>
 * <span data-ttu-id="3ce02-219">**Intern URL**: Detta är hello URL: en för SharePoint-webbplatsen för hello internt, såsom **https://SharePoint/**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-219">**Internal URL**: This is hello URL of hello SharePoint site internally, such as **https://SharePoint/**.</span></span> <span data-ttu-id="3ce02-220">I det här exemplet, se till att toouse **https**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-220">In this example, make sure toouse **https**.</span></span>
 * <span data-ttu-id="3ce02-221">**Förautentiseringsmetod**: Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-221">**Preauthentication Method**: Select **Azure Active Directory**.</span></span>

  ![Alternativ för att lägga till ett program](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. <span data-ttu-id="3ce02-223">När hello appen publiceras, klickar du på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="3ce02-223">After hello app is published, click hello **Configure** tab.</span></span>
6. <span data-ttu-id="3ce02-224">Bläddra nedåt toohello alternativet **översätta URL: en i sidhuvuden**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-224">Scroll down toohello option **Translate URL in Headers**.</span></span> <span data-ttu-id="3ce02-225">hello standardvärdet är **Ja**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-225">hello default value is **YES**.</span></span> <span data-ttu-id="3ce02-226">Ändra också**nr**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-226">Change it too**NO**.</span></span>

 <span data-ttu-id="3ce02-227">SharePoint använder hello _värdadressen_ värdet toolook upp hello plats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-227">SharePoint uses hello _Host Header_ value toolook up hello site.</span></span> <span data-ttu-id="3ce02-228">Den genererar även länkar baserat på det här värdet.</span><span class="sxs-lookup"><span data-stu-id="3ce02-228">It also generates links based on this value.</span></span> <span data-ttu-id="3ce02-229">hello net effekten är toomake till att alla länkar som SharePoint genererar är en publicerade URL som är korrekt inställd toouse hello externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="3ce02-229">hello net effect is toomake sure that any link that SharePoint generates is a published URL that is correctly set toouse hello external URL.</span></span> <span data-ttu-id="3ce02-230">Om hello värdet för**Ja** också aktiverar hello tooforward hello begäran toohello backend-anslutningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3ce02-230">Setting hello value too**YES** also enables hello connector tooforward hello request toohello back-end application.</span></span> <span data-ttu-id="3ce02-231">Men om hello värdet för**nr** innebär att hello anslutningen kommer inte att skicka hello interna värdnamn.</span><span class="sxs-lookup"><span data-stu-id="3ce02-231">However, setting hello value too**NO** means that hello connector will not send hello internal host name.</span></span> <span data-ttu-id="3ce02-232">I stället skickar hello connector hello värdadressen som hello publicerade URL toohello serverprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3ce02-232">Instead, hello connector sends hello host header as hello published URL toohello back-end application.</span></span>

 <span data-ttu-id="3ce02-233">Dessutom tooensure att SharePoint accepterar den här URL: en måste du toocomplete en mer konfiguration på hello SharePoint-server.</span><span class="sxs-lookup"><span data-stu-id="3ce02-233">Also, tooensure that SharePoint accepts this URL, you need toocomplete one more configuration on hello SharePoint server.</span></span> <span data-ttu-id="3ce02-234">Gör du det i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="3ce02-234">You'll do that in hello next section.</span></span>

7. <span data-ttu-id="3ce02-235">Ändra **inre autentiseringsmetod** för**integrerad Windows-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-235">Change **Internal Authentication Method** too**Integrated Windows Authentication**.</span></span> <span data-ttu-id="3ce02-236">Om Azure AD-klienten använder en UPN i hello moln som skiljer sig från hello UPN lokalt, kan du komma ihåg tooupdate **delegerad inloggningen identitet** samt.</span><span class="sxs-lookup"><span data-stu-id="3ce02-236">If your Azure AD tenant uses a UPN in hello cloud that's different from hello UPN on-premises, remember tooupdate **Delegated Login Identity** as well.</span></span>
8. <span data-ttu-id="3ce02-237">Ange **interna program SPN** toohello värdet som du angett tidigare.</span><span class="sxs-lookup"><span data-stu-id="3ce02-237">Set **Internal Application SPN** toohello value that you set earlier.</span></span> <span data-ttu-id="3ce02-238">Till exempel använda **http/sharepoint.demo.o365identity.us**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-238">For example, use **http/sharepoint.demo.o365identity.us**.</span></span>
9. <span data-ttu-id="3ce02-239">Tilldela hello programmet tooyour target användare.</span><span class="sxs-lookup"><span data-stu-id="3ce02-239">Assign hello application tooyour target users.</span></span>

<span data-ttu-id="3ce02-240">Ditt program bör se ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3ce02-240">Your application should look similar toohello following example:</span></span>

  ![Färdiga programmet](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

## <a name="step-3-ensure-that-sharepoint-knows-about-hello-external-url"></a><span data-ttu-id="3ce02-242">Steg 3: Kontrollera att SharePoint känner hello extern URL</span><span class="sxs-lookup"><span data-stu-id="3ce02-242">Step 3: Ensure that SharePoint knows about hello external URL</span></span>

<span data-ttu-id="3ce02-243">Din senaste steg tooensure som SharePoint hittar hello plats baserat på hello externa URL: en, så att den blir länkar baserat på den externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="3ce02-243">Your last step tooensure that SharePoint can find hello site based on hello external URL, so that it will render links based on that external URL.</span></span> <span data-ttu-id="3ce02-244">Det gör du genom att konfigurera alternativa åtkomstmappningar för hello SharePoint-webbplats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-244">You do this by configuring alternate access mappings for hello SharePoint site.</span></span>

1. <span data-ttu-id="3ce02-245">Öppna hello **Central Administration av SharePoint 2013** plats.</span><span class="sxs-lookup"><span data-stu-id="3ce02-245">Open hello **SharePoint 2013 Central Administration** site.</span></span>
2. <span data-ttu-id="3ce02-246">Under **systeminställningar**väljer **konfigurera alternativa åtkomstmappningar**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-246">Under **System Settings**, select **Configure Alternate Access Mappings**.</span></span>

 <span data-ttu-id="3ce02-247">Då öppnas hello **alternativa åtkomstmappningar** rutan.</span><span class="sxs-lookup"><span data-stu-id="3ce02-247">This opens hello **Alternate Access Mappings** box.</span></span>

  ![Alternativa åtkomstmappningar rutan](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. <span data-ttu-id="3ce02-249">I hello listrutan bredvid **alternativa åtkomst mappning samling**väljer **ändra alternativa åtkomst mappning samlingen**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-249">In hello drop-down list beside **Alternate Access Mapping Collection**, select **Change Alternate Access Mapping Collection**.</span></span>
4. <span data-ttu-id="3ce02-250">Välj plats – till exempel **SharePoint - 80**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-250">Select your site--for example, **SharePoint - 80**.</span></span>

  ![Du väljer en plats](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. <span data-ttu-id="3ce02-252">Du kan välja tooadd hello publicerade URL: en som en intern URL eller en offentlig URL.</span><span class="sxs-lookup"><span data-stu-id="3ce02-252">You can choose tooadd hello published URL as either an internal URL or a public URL.</span></span> <span data-ttu-id="3ce02-253">Det här exemplet använder en offentlig URL som hello extranät.</span><span class="sxs-lookup"><span data-stu-id="3ce02-253">This example uses a public URL as hello extranet.</span></span>
6. <span data-ttu-id="3ce02-254">Klicka på **redigera offentliga URL: er** i hello **extranät** sökväg, och ange sedan hello sökvägen för hello publicerat program som hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="3ce02-254">Click **Edit Public URLs** in hello **Extranet** path, and then enter hello path for hello published application, as in hello previous step.</span></span> <span data-ttu-id="3ce02-255">Ange till exempel **https://sharepoint-iddemo.msappproxy.net**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-255">For example, enter **https://sharepoint-iddemo.msappproxy.net**.</span></span>

  ![Att ange hello sökväg](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. <span data-ttu-id="3ce02-257">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3ce02-257">Click **Save**.</span></span>

 <span data-ttu-id="3ce02-258">Du kan nu komma åt hello SharePoint-webbplatsen externt via Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="3ce02-258">You can now access hello SharePoint site externally via Azure AD Application Proxy.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ce02-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ce02-259">Next steps</span></span>

- [<span data-ttu-id="3ce02-260">Hur säkra tooprovide fjärråtkomst tooon lokala program</span><span class="sxs-lookup"><span data-stu-id="3ce02-260">How tooprovide secure remote access tooon-premises applications</span></span>](active-directory-application-proxy-get-started.md)
- [<span data-ttu-id="3ce02-261">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="3ce02-261">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
- [<span data-ttu-id="3ce02-262">Publicering av SharePoint 2016 och Office Online Server med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="3ce02-262">Publishing SharePoint 2016 and Office Online Server with Azure AD Application Proxy</span></span>](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)
