---
title: aaaWork med befintliga lokala proxyservrar och Azure AD | Microsoft Docs
description: Beskriver hur toowork med befintliga lokala proxyservrar.
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
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 7f8cec4f676f99bead5211bcbcf23056bd7f211f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a><span data-ttu-id="de3ed-103">Arbeta med befintliga lokala proxyservrar</span><span class="sxs-lookup"><span data-stu-id="de3ed-103">Work with existing on-premises proxy servers</span></span>

<span data-ttu-id="de3ed-104">Den här artikeln förklarar hur tooconfigure Azure Active Directory (Azure AD) Application Proxy kopplingar toowork med utgående proxy-servrar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-104">This article explains how tooconfigure Azure Active Directory (Azure AD) Application Proxy connectors toowork with outbound proxy servers.</span></span> <span data-ttu-id="de3ed-105">Det är avsett för kunder med nätverksmiljöer som har befintliga proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-105">It is intended for customers with network environments that have existing proxies.</span></span>

<span data-ttu-id="de3ed-106">Vi börjar med att titta på dessa huvudsakliga distributionsscenarier:</span><span class="sxs-lookup"><span data-stu-id="de3ed-106">We start by looking at these main deployment scenarios:</span></span>
* <span data-ttu-id="de3ed-107">Konfigurera kopplingar toobypass din lokala utgående proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-107">Configure connectors toobypass your on-premises outbound proxies.</span></span>
* <span data-ttu-id="de3ed-108">Konfigurera kopplingar toouse en utgående proxy tooaccess Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="de3ed-108">Configure connectors toouse an outbound proxy tooaccess Azure AD Application Proxy.</span></span>

<span data-ttu-id="de3ed-109">Mer information om hur kopplingar fungerar finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="de3ed-109">For more information about how connectors work, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

## <a name="configure-hello-outbound-proxy"></a><span data-ttu-id="de3ed-110">Konfigurera hello utgående proxy</span><span class="sxs-lookup"><span data-stu-id="de3ed-110">Configure hello outbound proxy</span></span>

<span data-ttu-id="de3ed-111">Om du har en utgående proxy i din miljö kan du använda ett konto med behörighet tooconfigure hello utgående proxy.</span><span class="sxs-lookup"><span data-stu-id="de3ed-111">If you have an outbound proxy in your environment, use an account with appropriate permissions tooconfigure hello outbound proxy.</span></span> <span data-ttu-id="de3ed-112">Eftersom hello installationsprogrammet körs i hello kontexten för hello användare som utför installationen hello, kan du kontrollera hello konfigurationen med hjälp av Microsoft Edge eller en annan webbläsare.</span><span class="sxs-lookup"><span data-stu-id="de3ed-112">Because hello installer runs in hello context of hello user who's doing hello installation, you can check hello configuration by using Microsoft Edge or another Internet browser.</span></span>

<span data-ttu-id="de3ed-113">tooconfigure hello proxyinställningarna i Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="de3ed-113">tooconfigure hello proxy settings in Microsoft Edge:</span></span>

1. <span data-ttu-id="de3ed-114">Gå för**inställningar** > **visa avancerade inställningar** > **öppna proxyinställningar** > **manuell Proxy-inställningar** .</span><span class="sxs-lookup"><span data-stu-id="de3ed-114">Go too**Settings** > **View Advanced Settings** > **Open Proxy Settings** > **Manual Proxy Setup**.</span></span>
2. <span data-ttu-id="de3ed-115">Ange **använder en proxyserver** för**på**väljer hello **inte använder hello proxyserver för lokala adresser (intranät)** kryssrutan och sedan ändra hello-adress och port tooreflect en lokal proxyserver.</span><span class="sxs-lookup"><span data-stu-id="de3ed-115">Set **Use a proxy server** too**On**, select hello **Don’t use hello proxy server for local (intranet) addresses** check box, and then change hello address and port tooreflect your local proxy server.</span></span>
3. <span data-ttu-id="de3ed-116">Fyll i hello nödvändiga proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-116">Fill in hello necessary proxy settings.</span></span>

   ![Dialogrutan för proxy-inställningar](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a><span data-ttu-id="de3ed-118">Kringgå utgående proxyservrar</span><span class="sxs-lookup"><span data-stu-id="de3ed-118">Bypass outbound proxies</span></span>

<span data-ttu-id="de3ed-119">Kopplingar har underliggande OS-komponenter som begär utgående.</span><span class="sxs-lookup"><span data-stu-id="de3ed-119">Connectors have underlying OS components that make outbound requests.</span></span> <span data-ttu-id="de3ed-120">De här komponenterna försöker automatiskt toolocate en proxyserver i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="de3ed-120">These components automatically attempt toolocate a proxy server on hello network.</span></span> <span data-ttu-id="de3ed-121">De kan använda Web Proxy Auto-Discovery (WPAD), om den är aktiverad i hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="de3ed-121">They use Web Proxy Auto-Discovery (WPAD), if it's enabled in hello environment.</span></span>

<span data-ttu-id="de3ed-122">hello OS komponenter försök toolocate en proxyserver genom att utföra en DNS-sökning för wpad.domainsuffix.</span><span class="sxs-lookup"><span data-stu-id="de3ed-122">hello OS components attempt toolocate a proxy server by carrying out a DNS lookup for wpad.domainsuffix.</span></span> <span data-ttu-id="de3ed-123">Om det löser i DNS, sedan en HTTP-begäran görs toohello IP-adress för wpad.dat.</span><span class="sxs-lookup"><span data-stu-id="de3ed-123">If this resolves in DNS, an HTTP request is then made toohello IP address for wpad.dat.</span></span> <span data-ttu-id="de3ed-124">Denna begäran blir hello proxykonfigurationsskript i din miljö.</span><span class="sxs-lookup"><span data-stu-id="de3ed-124">This request becomes hello proxy configuration script in your environment.</span></span> <span data-ttu-id="de3ed-125">hello-anslutningen använder det här skriptet tooselect en utgående proxyserver.</span><span class="sxs-lookup"><span data-stu-id="de3ed-125">hello connector uses this script tooselect an outbound proxy server.</span></span> <span data-ttu-id="de3ed-126">Dock kan connector trafik fortfarande inte går via, på grund av ytterligare konfiguration krävs på hello proxy.</span><span class="sxs-lookup"><span data-stu-id="de3ed-126">However, connector traffic might still not go through, because of additional configuration settings needed on hello proxy.</span></span>

<span data-ttu-id="de3ed-127">Du kan konfigurera hello connector toobypass din lokala proxy tooensure som används för direkt anslutning toohello Azure services.</span><span class="sxs-lookup"><span data-stu-id="de3ed-127">You can configure hello connector toobypass your on-premises proxy tooensure that it uses direct connectivity toohello Azure services.</span></span> <span data-ttu-id="de3ed-128">Vi rekommenderar den här metoden (om nätverksprincipen tillåter det), eftersom det innebär att du har en mindre configuration toomaintain.</span><span class="sxs-lookup"><span data-stu-id="de3ed-128">We recommend this approach (if your network policy allows for it), because it means that you have one less configuration toomaintain.</span></span>

<span data-ttu-id="de3ed-129">toodisable utgående proxy-användning för hello connector redigera hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config filen och Lägg till hello *system.net* avsnittet som visas i det här kodexemplet :</span><span class="sxs-lookup"><span data-stu-id="de3ed-129">toodisable outbound proxy usage for hello connector, edit hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add hello *system.net* section shown in this code sample:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
<span data-ttu-id="de3ed-130">tooensure hello Connector Updater tjänsten också kringgår hello proxy, se en liknande ändra toohello ApplicationProxyConnectorUpdaterService.exe.config-fil som finns i C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span><span class="sxs-lookup"><span data-stu-id="de3ed-130">tooensure that hello Connector Updater service also bypasses hello proxy, make a similar change toohello ApplicationProxyConnectorUpdaterService.exe.config file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span></span>

<span data-ttu-id="de3ed-131">Vara säker på att toomake kopior av hello ursprungliga filer om du behöver toorevert toohello standard .config-filer.</span><span class="sxs-lookup"><span data-stu-id="de3ed-131">Be sure toomake copies of hello original files, in case you need toorevert toohello default .config files.</span></span>

## <a name="use-hello-outbound-proxy-server"></a><span data-ttu-id="de3ed-132">Använd hello utgående proxyserver</span><span class="sxs-lookup"><span data-stu-id="de3ed-132">Use hello outbound proxy server</span></span>

<span data-ttu-id="de3ed-133">Vissa miljöer kräver att alla utgående trafik toogo via en utgående proxy utan undantag.</span><span class="sxs-lookup"><span data-stu-id="de3ed-133">Some environments require all outbound traffic toogo through an outbound proxy, without exception.</span></span> <span data-ttu-id="de3ed-134">Kringgå proxy för hello är därför inte ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="de3ed-134">As a result, bypassing hello proxy is not an option.</span></span>

<span data-ttu-id="de3ed-135">Du kan konfigurera hello connector trafik toogo via hello utgående proxy, som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="de3ed-135">You can configure hello connector traffic toogo through hello outbound proxy, as shown in hello following diagram:</span></span>

 ![Konfigurera anslutningen trafik toogo via en utgående proxy tooAzure AD Application Proxy](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

<span data-ttu-id="de3ed-137">Som ett resultat av med endast utgående trafik, det finns inget behov av tooconfigure inkommande åtkomst via dina brandväggar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-137">As a result of having only outbound traffic, there's no need tooconfigure inbound access through your firewalls.</span></span>

### <a name="step-1-configure-hello-connector-and-related-services-toogo-through-hello-outbound-proxy"></a><span data-ttu-id="de3ed-138">Steg 1: Konfigurera hello-anslutningen och relaterade tjänster toogo via hello utgående proxy</span><span class="sxs-lookup"><span data-stu-id="de3ed-138">Step 1: Configure hello connector and related services toogo through hello outbound proxy</span></span>

<span data-ttu-id="de3ed-139">Som omfattas tidigare, om WPAD är aktiverat i hello miljö och korrekt konfigurerad, hello connector automatiskt identifiera hello utgående proxy-servern och försök toouse den.</span><span class="sxs-lookup"><span data-stu-id="de3ed-139">As covered earlier, if WPAD is enabled in hello environment and configured appropriately, hello connector will automatically discover hello outbound proxy server and attempt toouse it.</span></span> <span data-ttu-id="de3ed-140">Du kan uttryckligen konfigurera hello connector toogo via en utgående proxy.</span><span class="sxs-lookup"><span data-stu-id="de3ed-140">However, you can explicitly configure hello connector toogo through an outbound proxy.</span></span>

<span data-ttu-id="de3ed-141">toodo så redigera hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config filen och Lägg till hello *system.net* avsnittet som visas i det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="de3ed-141">toodo so, edit hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add hello *system.net* section shown in this code sample.</span></span> <span data-ttu-id="de3ed-142">Ändra *proxyserver:8080* tooreflect din lokala Proxyservernamn eller IP-adress och hello porten den lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="de3ed-142">Change *proxyserver:8080* tooreflect your local proxy server name or IP address, and hello port that it's listening on.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

<span data-ttu-id="de3ed-143">Konfigurera hello Connector Updater toouse hello proxyservern genom att göra en liknande ändra toohello-fil som finns i C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span><span class="sxs-lookup"><span data-stu-id="de3ed-143">Next, configure hello Connector Updater service toouse hello proxy by making a similar change toohello file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span></span>

### <a name="step-2-configure-hello-proxy-tooallow-traffic-from-hello-connector-and-related-services-tooflow-through"></a><span data-ttu-id="de3ed-144">Steg 2: Konfigurera hello proxy tooallow trafik från hello koppling och relaterade tjänster tooflow via</span><span class="sxs-lookup"><span data-stu-id="de3ed-144">Step 2: Configure hello proxy tooallow traffic from hello connector and related services tooflow through</span></span>

<span data-ttu-id="de3ed-145">Det finns fyra aspekter tooconsider på hello utgående proxy:</span><span class="sxs-lookup"><span data-stu-id="de3ed-145">There are four aspects tooconsider at hello outbound proxy:</span></span>
* <span data-ttu-id="de3ed-146">Proxy utgående regler</span><span class="sxs-lookup"><span data-stu-id="de3ed-146">Proxy outbound rules</span></span>
* <span data-ttu-id="de3ed-147">Proxyautentisering</span><span class="sxs-lookup"><span data-stu-id="de3ed-147">Proxy authentication</span></span>
* <span data-ttu-id="de3ed-148">Proxy-portar</span><span class="sxs-lookup"><span data-stu-id="de3ed-148">Proxy ports</span></span>
* <span data-ttu-id="de3ed-149">SSL-kontroll</span><span class="sxs-lookup"><span data-stu-id="de3ed-149">SSL inspection</span></span>

#### <a name="proxy-outbound-rules"></a><span data-ttu-id="de3ed-150">Proxy utgående regler</span><span class="sxs-lookup"><span data-stu-id="de3ed-150">Proxy outbound rules</span></span>
<span data-ttu-id="de3ed-151">Tillåt åtkomst toohello följande slutpunkter för åtkomst till anslutningen:</span><span class="sxs-lookup"><span data-stu-id="de3ed-151">Allow access toohello following endpoints for connector service access:</span></span>

* <span data-ttu-id="de3ed-152">*. msappproxy.net</span><span class="sxs-lookup"><span data-stu-id="de3ed-152">*.msappproxy.net</span></span>
* <span data-ttu-id="de3ed-153">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="de3ed-153">*.servicebus.windows.net</span></span>

<span data-ttu-id="de3ed-154">Tillåt åtkomst toohello följande slutpunkter för inledande registrering:</span><span class="sxs-lookup"><span data-stu-id="de3ed-154">For initial registration, allow access toohello following endpoints:</span></span>

* <span data-ttu-id="de3ed-155">login.windows.net</span><span class="sxs-lookup"><span data-stu-id="de3ed-155">login.windows.net</span></span>
* <span data-ttu-id="de3ed-156">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="de3ed-156">login.microsoftonline.com</span></span>

<span data-ttu-id="de3ed-157">Om du inte tillåter anslutningar av FQDN och behöver toospecify IP-adressintervall i stället kan du använda följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="de3ed-157">If you can't allow connectivity by FQDN and need toospecify IP ranges instead, use these options:</span></span>

* <span data-ttu-id="de3ed-158">Tillåt utgående åtkomst för hello connector tooall mål.</span><span class="sxs-lookup"><span data-stu-id="de3ed-158">Allow hello connector outbound access tooall destinations.</span></span>
* <span data-ttu-id="de3ed-159">Tillåt utgående åtkomst för hello connector för[IP-adressintervall för Azure-datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="de3ed-159">Allow hello connector outbound access too[Azure datacenter IP ranges](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span></span> <span data-ttu-id="de3ed-160">hello utmaningen med hello lista med IP-adressintervall för Azure-datacenter är uppdateras varje vecka.</span><span class="sxs-lookup"><span data-stu-id="de3ed-160">hello challenge with using hello list of Azure datacenter IP ranges is that it's updated weekly.</span></span> <span data-ttu-id="de3ed-161">Du måste tooput en process i plats tooensure att reglerna för access uppdateras.</span><span class="sxs-lookup"><span data-stu-id="de3ed-161">You need tooput a process in place tooensure that your access rules are updated accordingly.</span></span>

#### <a name="proxy-authentication"></a><span data-ttu-id="de3ed-162">Proxyautentisering</span><span class="sxs-lookup"><span data-stu-id="de3ed-162">Proxy authentication</span></span>

<span data-ttu-id="de3ed-163">Proxy-autentisering stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="de3ed-163">Proxy authentication is not currently supported.</span></span> <span data-ttu-id="de3ed-164">Vår aktuell rekommendation är tooallow hello connector anonym åtkomst toohello Internet mål.</span><span class="sxs-lookup"><span data-stu-id="de3ed-164">Our current recommendation is tooallow hello connector anonymous access toohello Internet destinations.</span></span>

#### <a name="proxy-ports"></a><span data-ttu-id="de3ed-165">Proxy-portar</span><span class="sxs-lookup"><span data-stu-id="de3ed-165">Proxy ports</span></span>

<span data-ttu-id="de3ed-166">hello connector gör utgående SSL-baserad anslutningar med hjälp av metoden för hello CONNECT.</span><span class="sxs-lookup"><span data-stu-id="de3ed-166">hello connector makes outbound SSL-based connections by using hello CONNECT method.</span></span> <span data-ttu-id="de3ed-167">Den här metoden anger i princip en tunnel genom hello utgående proxy.</span><span class="sxs-lookup"><span data-stu-id="de3ed-167">This method essentially sets up a tunnel through hello outbound proxy.</span></span> <span data-ttu-id="de3ed-168">Konfigurera hello proxy server tooallow tunneling tooports 443 och 80.</span><span class="sxs-lookup"><span data-stu-id="de3ed-168">Configure hello proxy server tooallow tunneling tooports 443 and 80.</span></span>

>[!NOTE]
><span data-ttu-id="de3ed-169">När Service Bus körs via HTTPS, använder port 443.</span><span class="sxs-lookup"><span data-stu-id="de3ed-169">When Service Bus runs over HTTPS, it uses port 443.</span></span> <span data-ttu-id="de3ed-170">Dock försöker TCP-direktanslutningar Service Bus som standard och faller tillbaka tooHTTPS endast om direkt anslutning misslyckades.</span><span class="sxs-lookup"><span data-stu-id="de3ed-170">However, by default, Service Bus attempts direct TCP connections and falls back tooHTTPS only if direct connectivity fails.</span></span>

<span data-ttu-id="de3ed-171">tooensure som Hej Service Bus trafik som också skickas via hello utgående proxyserver, kontrollera hello kopplingen inte kan ansluta direkt toohello Azure-tjänster för portar 9350 och 9352 5671.</span><span class="sxs-lookup"><span data-stu-id="de3ed-171">tooensure that hello Service Bus traffic is also sent through hello outbound proxy server, ensure that hello connector cannot directly connect toohello Azure services for ports 9350, 9352, and 5671.</span></span>

#### <a name="ssl-inspection"></a><span data-ttu-id="de3ed-172">SSL-kontroll</span><span class="sxs-lookup"><span data-stu-id="de3ed-172">SSL inspection</span></span>
<span data-ttu-id="de3ed-173">Använd inte SSL-kontroll för hello connector trafik, eftersom den orsakar problem för hello connector-trafik.</span><span class="sxs-lookup"><span data-stu-id="de3ed-173">Do not use SSL inspection for hello connector traffic, because it causes problems for hello connector traffic.</span></span>

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a><span data-ttu-id="de3ed-174">Felsöka problem med anslutningen proxy och anslutningsproblem för tjänsten</span><span class="sxs-lookup"><span data-stu-id="de3ed-174">Troubleshoot connector proxy problems and service connectivity issues</span></span>
<span data-ttu-id="de3ed-175">Du bör nu se all trafik som passerar genom hello proxy.</span><span class="sxs-lookup"><span data-stu-id="de3ed-175">Now you should see all traffic flowing through hello proxy.</span></span> <span data-ttu-id="de3ed-176">Om du har problem bör hjälpa hello efter felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="de3ed-176">If you have problems, hello following troubleshooting information should help.</span></span>

<span data-ttu-id="de3ed-177">Hej bästa sätt tooidentify och felsöka-anslutning problem är tootake ett nätverk avbilda på hello kopplingstjänsten när hello kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="de3ed-177">hello best way tooidentify and troubleshoot connector connectivity issues is tootake a network capture on hello connector service while starting hello connector service.</span></span> <span data-ttu-id="de3ed-178">Detta kan vara problematiskt, vi ska titta på snabba tips om att samla in och filtrera nätverksspår.</span><span class="sxs-lookup"><span data-stu-id="de3ed-178">This can be a daunting task, so let’s look at quick tips on capturing and filtering network traces.</span></span>

<span data-ttu-id="de3ed-179">Du kan använda hello övervakning verktyg som helst.</span><span class="sxs-lookup"><span data-stu-id="de3ed-179">You can use hello monitoring tool of your choice.</span></span> <span data-ttu-id="de3ed-180">Vi använde Microsoft Network Monitor 3.4 hello enligt den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="de3ed-180">For hello purposes of this article, we used Microsoft Network Monitor 3.4.</span></span> <span data-ttu-id="de3ed-181">Du kan [ladda ned det från Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span><span class="sxs-lookup"><span data-stu-id="de3ed-181">You can [download it from Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span></span>

<span data-ttu-id="de3ed-182">hello exempel och filter som vi använder i följande avsnitt hello är särskilda tooNetwork övervakaren men hello principer kan vara tillämpade tooany verktyget.</span><span class="sxs-lookup"><span data-stu-id="de3ed-182">hello examples and filters that we use in hello following sections are specific tooNetwork Monitor, but hello principles can be applied tooany analysis tool.</span></span>

### <a name="take-a-capture-by-using-network-monitor"></a><span data-ttu-id="de3ed-183">Ta en avbildning med hjälp av Network Monitor</span><span class="sxs-lookup"><span data-stu-id="de3ed-183">Take a capture by using Network Monitor</span></span>

<span data-ttu-id="de3ed-184">toostart en avbildning:</span><span class="sxs-lookup"><span data-stu-id="de3ed-184">toostart a capture:</span></span>

1. <span data-ttu-id="de3ed-185">Öppna Network Monitor och klicka på **nya avbilda**.</span><span class="sxs-lookup"><span data-stu-id="de3ed-185">Open Network Monitor and click **New Capture**.</span></span>
2. <span data-ttu-id="de3ed-186">Klicka på hello **starta** knappen.</span><span class="sxs-lookup"><span data-stu-id="de3ed-186">Click hello **Start** button.</span></span>

   ![Fönstret i Network Monitor](./media/application-proxy-working-with-proxy-servers/network-capture.png)

<span data-ttu-id="de3ed-188">När du har slutfört en avbildning, klickar du på hello **stoppa** knappen tooend den.</span><span class="sxs-lookup"><span data-stu-id="de3ed-188">After you complete a capture, click hello **Stop** button tooend it.</span></span>

### <a name="take-a-capture-of-connector-traffic"></a><span data-ttu-id="de3ed-189">Ta en avbildning av connector-trafik</span><span class="sxs-lookup"><span data-stu-id="de3ed-189">Take a capture of connector traffic</span></span>

<span data-ttu-id="de3ed-190">Utför följande hello inledande felsökningsinformation:</span><span class="sxs-lookup"><span data-stu-id="de3ed-190">For initial troubleshooting, perform hello following steps:</span></span>

1. <span data-ttu-id="de3ed-191">Stoppa hello Azure AD Application Proxy Connector tjänsten från services.msc.</span><span class="sxs-lookup"><span data-stu-id="de3ed-191">From services.msc, stop hello Azure AD Application Proxy Connector service.</span></span>
2. <span data-ttu-id="de3ed-192">Starta hello nätverksbild.</span><span class="sxs-lookup"><span data-stu-id="de3ed-192">Start hello network capture.</span></span>
3. <span data-ttu-id="de3ed-193">Starta hello Azure AD Application Proxy Connector-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="de3ed-193">Start hello Azure AD Application Proxy Connector service.</span></span>
4. <span data-ttu-id="de3ed-194">Stoppa hello nätverksbild.</span><span class="sxs-lookup"><span data-stu-id="de3ed-194">Stop hello network capture.</span></span>

   ![Azure AD Application Proxy Connector-tjänsten i services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-hello-requests-from-hello-connector-toohello-proxy-server"></a><span data-ttu-id="de3ed-196">Titta på hello begäranden från hello connector toohello proxyserver</span><span class="sxs-lookup"><span data-stu-id="de3ed-196">Look at hello requests from hello connector toohello proxy server</span></span>

<span data-ttu-id="de3ed-197">Nu när du har en nätverksbild du är klar toofilter den.</span><span class="sxs-lookup"><span data-stu-id="de3ed-197">Now that you’ve got a network capture, you're ready toofilter it.</span></span> <span data-ttu-id="de3ed-198">hello viktiga toolooking på hello trace är att förstå hur toofilter hello avbilda.</span><span class="sxs-lookup"><span data-stu-id="de3ed-198">hello key toolooking at hello trace is understanding how toofilter hello capture.</span></span>

<span data-ttu-id="de3ed-199">Ett filter är (där 8080 är hello Proxyport service):</span><span class="sxs-lookup"><span data-stu-id="de3ed-199">One filter is as follows (where 8080 is hello proxy service port):</span></span>

<span data-ttu-id="de3ed-200">**(http. Begäran eller http. Svar) och tcp.port==8080**</span><span class="sxs-lookup"><span data-stu-id="de3ed-200">**(http.Request or http.Response) and tcp.port==8080**</span></span>

<span data-ttu-id="de3ed-201">Om du anger det här filtret i hello **visningsfilter** och välj **tillämpa**, filtrerar hello avbildas trafik utifrån hello filter.</span><span class="sxs-lookup"><span data-stu-id="de3ed-201">If you enter this filter in hello **Display Filter** window and select **Apply**, it filters hello captured traffic based on hello filter.</span></span>

<span data-ttu-id="de3ed-202">hello föregående filtret visas bara hello HTTP-begäranden och -svar från hello Proxyport.</span><span class="sxs-lookup"><span data-stu-id="de3ed-202">hello preceding filter shows just hello HTTP requests and responses to/from hello proxy port.</span></span> <span data-ttu-id="de3ed-203">För en koppling Start där hello connector är konfigurerade toouse en proxyserver, visas hello filter ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="de3ed-203">For a connector startup where hello connector is configured toouse a proxy server, hello filter would show something like this:</span></span>

 ![Exempellistan över filtrerade HTTP-begäranden och -svar](./media/application-proxy-working-with-proxy-servers/http-requests.png)

<span data-ttu-id="de3ed-205">Du nu söker specifikt hello Anslut begäranden som visar kommunikation med hello proxyserver.</span><span class="sxs-lookup"><span data-stu-id="de3ed-205">You're now specifically looking for hello CONNECT requests that show communication with hello proxy server.</span></span> <span data-ttu-id="de3ed-206">Vid lyckas, kan du få ett OK (200) för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-206">Upon success, you get an HTTP OK (200) response.</span></span>

<span data-ttu-id="de3ed-207">Om du ser andra svarskoder, till exempel 407 eller 502, är hello proxy kräver autentisering eller inte tillåter trafik för hello av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="de3ed-207">If you see other response codes, such as 407 or 502, hello proxy is requiring authentication or not allowing hello traffic for some other reason.</span></span> <span data-ttu-id="de3ed-208">Nu kan du kontaktar supportteamet proxy-server.</span><span class="sxs-lookup"><span data-stu-id="de3ed-208">At this point, you engage your proxy server support team.</span></span>

### <a name="identify-failed-tcp-connection-attempts"></a><span data-ttu-id="de3ed-209">Identifiera misslyckade TCP-anslutningsförsök</span><span class="sxs-lookup"><span data-stu-id="de3ed-209">Identify failed TCP connection attempts</span></span>

<span data-ttu-id="de3ed-210">hello andra vanligt scenario som du vill ha är när hello koppling försöker tooconnect direkt, men den inte.</span><span class="sxs-lookup"><span data-stu-id="de3ed-210">hello other common scenario that you may be interested in is when hello connector is trying tooconnect directly, but it's failing.</span></span>

<span data-ttu-id="de3ed-211">Ett annat Network Monitor-filter som hjälper tooeasily identifiera problemet är:</span><span class="sxs-lookup"><span data-stu-id="de3ed-211">Another Network Monitor filter that helps you tooeasily identify this problem is:</span></span>

<span data-ttu-id="de3ed-212">**Egenskapen. TCPSynRetransmit**</span><span class="sxs-lookup"><span data-stu-id="de3ed-212">**property.TCPSynRetransmit**</span></span>

<span data-ttu-id="de3ed-213">SYN-paket är hello första paketet skickas tooestablish en TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="de3ed-213">A SYN packet is hello first packet sent tooestablish a TCP connection.</span></span> <span data-ttu-id="de3ed-214">Om det här paketet inte returnerar ett svar, reattempted hello SYN.</span><span class="sxs-lookup"><span data-stu-id="de3ed-214">If this packet doesn’t return a response, hello SYN is reattempted.</span></span> <span data-ttu-id="de3ed-215">Du kan använda hello föregående filter toosee alla att SYN-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="de3ed-215">You can use hello preceding filter toosee any retransmitted SYNs.</span></span> <span data-ttu-id="de3ed-216">Du kan sedan kontrollera om dessa SYN-förfrågningar motsvarar tooany connector-relaterad trafik.</span><span class="sxs-lookup"><span data-stu-id="de3ed-216">Then, you can check whether these SYNs correspond tooany connector-related traffic.</span></span>

<span data-ttu-id="de3ed-217">hello följande exempel visas ett misslyckade anslutningsförsök tooService Bus port 9352:</span><span class="sxs-lookup"><span data-stu-id="de3ed-217">hello following example shows a failed connection attempt tooService Bus port 9352:</span></span>

 ![Exempelsvar misslyckade anslutningsförsök](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

<span data-ttu-id="de3ed-219">Om du ser något som liknar föregående svar hello försöker hello koppling toocommunicate direkt med hello Azure Service Bus-tjänst.</span><span class="sxs-lookup"><span data-stu-id="de3ed-219">If you see something like hello preceding response, hello connector is trying toocommunicate directly with hello Azure Service Bus service.</span></span> <span data-ttu-id="de3ed-220">Om du förväntar dig hello connector toomake direkta anslutningar toohello Azure-tjänster, svaret är en tydlig indikation på att du har ett problem med nätverket eller brandväggen.</span><span class="sxs-lookup"><span data-stu-id="de3ed-220">If you expect hello connector toomake direct connections toohello Azure services, this response is a clear indication that you have a network or firewall problem.</span></span>

>[!NOTE]
><span data-ttu-id="de3ed-221">Om du konfigurerade toouse en proxyserver, kan svaret innebära att Service Bus försöker en direkt TCP-anslutning innan du byter tooattempting en anslutning via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="de3ed-221">If you are configured toouse a proxy server, this response might mean that Service Bus is attempting a direct TCP connection before switching tooattempting a connection over HTTPS.</span></span>
>

<span data-ttu-id="de3ed-222">Nätverket trace analys är inte för alla.</span><span class="sxs-lookup"><span data-stu-id="de3ed-222">Network trace analysis is not for everyone.</span></span> <span data-ttu-id="de3ed-223">Men det kan vara ett ovärderligt verktyg tooget snabbt information om vad som händer med nätverket.</span><span class="sxs-lookup"><span data-stu-id="de3ed-223">But it can be a valuable tool tooget quick information about what's going on with your network.</span></span>

<span data-ttu-id="de3ed-224">Om du fortsätter toostruggle med connector anslutningsproblem kan du skapa en biljett med vårt supportteam.</span><span class="sxs-lookup"><span data-stu-id="de3ed-224">If you continue toostruggle with connector connectivity issues, create a ticket with our support team.</span></span> <span data-ttu-id="de3ed-225">hello-teamet kan hjälpa dig med felsökningen.</span><span class="sxs-lookup"><span data-stu-id="de3ed-225">hello team can assist you with further troubleshooting.</span></span>

<span data-ttu-id="de3ed-226">Information om hur du löser problem med Application Proxy Connector finns i [felsöka Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="de3ed-226">For information about resolving errors with Application Proxy Connector, see [Troubleshoot Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span></span>

## <a name="next-steps"></a><span data-ttu-id="de3ed-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de3ed-227">Next steps</span></span>

[<span data-ttu-id="de3ed-228">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="de3ed-228">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)<br><span data-ttu-id="de3ed-229">
[Hur toosilently installerar hello Azure AD Application Proxy Connector](active-directory-application-proxy-silent-installation.md)</span><span class="sxs-lookup"><span data-stu-id="de3ed-229">
[How toosilently install hello Azure AD Application Proxy Connector ](active-directory-application-proxy-silent-installation.md)</span></span>
