---
title: Arbeta med befintliga lokala proxyservrar och Azure AD | Microsoft Docs
description: Beskriver hur du arbetar med befintliga lokala proxyservrar.
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
ms.openlocfilehash: bdca442755507c4ffe8d43692c5b7f2aa3a746f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a><span data-ttu-id="9cc4f-103">Arbeta med befintliga lokala proxyservrar</span><span class="sxs-lookup"><span data-stu-id="9cc4f-103">Work with existing on-premises proxy servers</span></span>

<span data-ttu-id="9cc4f-104">Den här artikeln beskriver hur du konfigurerar Azure Active Directory (Azure AD) Application Proxy kopplingar att arbeta med utgående proxy-servrar.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-104">This article explains how to configure Azure Active Directory (Azure AD) Application Proxy connectors to work with outbound proxy servers.</span></span> <span data-ttu-id="9cc4f-105">Det är avsett för kunder med nätverksmiljöer som har befintliga proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-105">It is intended for customers with network environments that have existing proxies.</span></span>

<span data-ttu-id="9cc4f-106">Vi börjar med att titta på dessa huvudsakliga distributionsscenarier:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-106">We start by looking at these main deployment scenarios:</span></span>
* <span data-ttu-id="9cc4f-107">Konfigurera att kopplingar för att kringgå din lokala utgående proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-107">Configure connectors to bypass your on-premises outbound proxies.</span></span>
* <span data-ttu-id="9cc4f-108">Konfigurera att kopplingar ska använda en utgående proxy för att komma åt Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-108">Configure connectors to use an outbound proxy to access Azure AD Application Proxy.</span></span>

<span data-ttu-id="9cc4f-109">Mer information om hur kopplingar fungerar finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="9cc4f-109">For more information about how connectors work, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

## <a name="configure-the-outbound-proxy"></a><span data-ttu-id="9cc4f-110">Konfigurera utgående proxy</span><span class="sxs-lookup"><span data-stu-id="9cc4f-110">Configure the outbound proxy</span></span>

<span data-ttu-id="9cc4f-111">Om du har en utgående proxy i din miljö måste du använda ett konto med behörighet för att konfigurera utgående proxy.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-111">If you have an outbound proxy in your environment, use an account with appropriate permissions to configure the outbound proxy.</span></span> <span data-ttu-id="9cc4f-112">Eftersom installationsprogrammet körs i kontexten för den användare som utför installationen, kan du kontrollera konfigurationen med hjälp av Microsoft Edge eller en annan webbläsare.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-112">Because the installer runs in the context of the user who's doing the installation, you can check the configuration by using Microsoft Edge or another Internet browser.</span></span>

<span data-ttu-id="9cc4f-113">Konfigurera proxyinställningar i Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-113">To configure the proxy settings in Microsoft Edge:</span></span>

1. <span data-ttu-id="9cc4f-114">Gå till **inställningar** > **visa avancerade inställningar** > **öppna proxyinställningar** > **manuell Proxy installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-114">Go to **Settings** > **View Advanced Settings** > **Open Proxy Settings** > **Manual Proxy Setup**.</span></span>
2. <span data-ttu-id="9cc4f-115">Ange **använder en proxyserver** till **på**, Välj den **Använd inte proxyservern för lokala adresser (intranät)** kryssrutan och sedan ändra den adress och port för att avspegla din lokal proxyserver.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-115">Set **Use a proxy server** to **On**, select the **Don’t use the proxy server for local (intranet) addresses** check box, and then change the address and port to reflect your local proxy server.</span></span>
3. <span data-ttu-id="9cc4f-116">Fyll i de nödvändiga proxyinställningarna.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-116">Fill in the necessary proxy settings.</span></span>

   ![Dialogrutan för proxy-inställningar](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a><span data-ttu-id="9cc4f-118">Kringgå utgående proxyservrar</span><span class="sxs-lookup"><span data-stu-id="9cc4f-118">Bypass outbound proxies</span></span>

<span data-ttu-id="9cc4f-119">Kopplingar har underliggande OS-komponenter som begär utgående.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-119">Connectors have underlying OS components that make outbound requests.</span></span> <span data-ttu-id="9cc4f-120">De här komponenterna försöker automatiskt hitta en proxyserver i nätverket.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-120">These components automatically attempt to locate a proxy server on the network.</span></span> <span data-ttu-id="9cc4f-121">De kan använda Web Proxy Auto-Discovery (WPAD), om den är aktiverad i miljön.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-121">They use Web Proxy Auto-Discovery (WPAD), if it's enabled in the environment.</span></span>

<span data-ttu-id="9cc4f-122">OS-komponenter kommer att försöka hitta en proxyserver genom att utföra en DNS-sökning för wpad.domainsuffix.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-122">The OS components attempt to locate a proxy server by carrying out a DNS lookup for wpad.domainsuffix.</span></span> <span data-ttu-id="9cc4f-123">Om det löser i DNS, görs sedan en HTTP-begäran till IP-adressen för wpad.dat.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-123">If this resolves in DNS, an HTTP request is then made to the IP address for wpad.dat.</span></span> <span data-ttu-id="9cc4f-124">Denna begäran blir proxykonfigurationsskript i din miljö.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-124">This request becomes the proxy configuration script in your environment.</span></span> <span data-ttu-id="9cc4f-125">Anslutningen används det här skriptet för att välja en utgående proxyserver.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-125">The connector uses this script to select an outbound proxy server.</span></span> <span data-ttu-id="9cc4f-126">Dock kan connector trafik fortfarande inte går via, på grund av ytterligare konfigurationsinställningar som behövs på proxyservern.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-126">However, connector traffic might still not go through, because of additional configuration settings needed on the proxy.</span></span>

<span data-ttu-id="9cc4f-127">Du kan konfigurera anslutningen för att kringgå proxyservern lokalt för att säkerställa att den använder direkt anslutning till Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-127">You can configure the connector to bypass your on-premises proxy to ensure that it uses direct connectivity to the Azure services.</span></span> <span data-ttu-id="9cc4f-128">Vi rekommenderar den här metoden (om nätverksprincipen tillåter det), eftersom det innebär att du har en mindre konfiguration att underhålla.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-128">We recommend this approach (if your network policy allows for it), because it means that you have one less configuration to maintain.</span></span>

<span data-ttu-id="9cc4f-129">Om du vill inaktivera utgående proxy-användning för anslutningen, redigerar du filen C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config och lägga till den *system.net* avsnittet som visas i det här kodexemplet:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-129">To disable outbound proxy usage for the connector, edit the C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add the *system.net* section shown in this code sample:</span></span>

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
<span data-ttu-id="9cc4f-130">För att säkerställa att tjänsten Connector Updater också kringgår proxy, ändra liknande filen ApplicationProxyConnectorUpdaterService.exe.config i C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-130">To ensure that the Connector Updater service also bypasses the proxy, make a similar change to the ApplicationProxyConnectorUpdaterService.exe.config file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span></span>

<span data-ttu-id="9cc4f-131">Se till att göra kopior av de ursprungliga filerna om du behöver återgå till standard .config-filer.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-131">Be sure to make copies of the original files, in case you need to revert to the default .config files.</span></span>

## <a name="use-the-outbound-proxy-server"></a><span data-ttu-id="9cc4f-132">Utgående proxyserver används</span><span class="sxs-lookup"><span data-stu-id="9cc4f-132">Use the outbound proxy server</span></span>

<span data-ttu-id="9cc4f-133">Vissa miljöer kräver all utgående trafik att gå via en utgående proxy utan undantag.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-133">Some environments require all outbound traffic to go through an outbound proxy, without exception.</span></span> <span data-ttu-id="9cc4f-134">Kringgå proxy är därför inte ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-134">As a result, bypassing the proxy is not an option.</span></span>

<span data-ttu-id="9cc4f-135">Du kan konfigurera connector trafiken gå igenom utgående proxy, som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-135">You can configure the connector traffic to go through the outbound proxy, as shown in the following diagram:</span></span>

 ![Konfigurera anslutningen trafik att gå via en utgående proxy till Azure AD Application Proxy](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

<span data-ttu-id="9cc4f-137">Det finns behöver du inte konfigurera inkommande åtkomst via dina brandväggar på grund av att endast utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-137">As a result of having only outbound traffic, there's no need to configure inbound access through your firewalls.</span></span>

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a><span data-ttu-id="9cc4f-138">Steg 1: Konfigurera anslutningen och tillhörande tjänster gå igenom utgående proxy</span><span class="sxs-lookup"><span data-stu-id="9cc4f-138">Step 1: Configure the connector and related services to go through the outbound proxy</span></span>

<span data-ttu-id="9cc4f-139">Som omfattas tidigare, om WPAD är aktiverad i miljön och korrekt konfigurerad, kommer anslutningen automatiskt identifiera utgående proxyservern och försöka använda den.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-139">As covered earlier, if WPAD is enabled in the environment and configured appropriately, the connector will automatically discover the outbound proxy server and attempt to use it.</span></span> <span data-ttu-id="9cc4f-140">Du kan uttryckligen konfigurera kopplingen för att gå igenom en utgående proxy.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-140">However, you can explicitly configure the connector to go through an outbound proxy.</span></span>

<span data-ttu-id="9cc4f-141">Gör du genom att redigera filen C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config och lägga till den *system.net* avsnittet som visas i det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-141">To do so, edit the C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add the *system.net* section shown in this code sample.</span></span> <span data-ttu-id="9cc4f-142">Ändra *proxyserver:8080* återspeglar din lokala Proxyservernamn eller IP-adress och port som lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-142">Change *proxyserver:8080* to reflect your local proxy server name or IP address, and the port that it's listening on.</span></span>

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

<span data-ttu-id="9cc4f-143">Konfigurera Connector Updater-tjänsten om du vill använda proxyservern genom att göra en liknande ändring till filen i C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-143">Next, configure the Connector Updater service to use the proxy by making a similar change to the file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span></span>

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a><span data-ttu-id="9cc4f-144">Steg 2: Konfigurera proxyn för att tillåta trafik från koppling och relaterade tjänster kan passera</span><span class="sxs-lookup"><span data-stu-id="9cc4f-144">Step 2: Configure the proxy to allow traffic from the connector and related services to flow through</span></span>

<span data-ttu-id="9cc4f-145">Det finns fyra olika aspekter att tänka på utgående proxy:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-145">There are four aspects to consider at the outbound proxy:</span></span>
* <span data-ttu-id="9cc4f-146">Proxy utgående regler</span><span class="sxs-lookup"><span data-stu-id="9cc4f-146">Proxy outbound rules</span></span>
* <span data-ttu-id="9cc4f-147">Proxyautentisering</span><span class="sxs-lookup"><span data-stu-id="9cc4f-147">Proxy authentication</span></span>
* <span data-ttu-id="9cc4f-148">Proxy-portar</span><span class="sxs-lookup"><span data-stu-id="9cc4f-148">Proxy ports</span></span>
* <span data-ttu-id="9cc4f-149">SSL-kontroll</span><span class="sxs-lookup"><span data-stu-id="9cc4f-149">SSL inspection</span></span>

#### <a name="proxy-outbound-rules"></a><span data-ttu-id="9cc4f-150">Proxy utgående regler</span><span class="sxs-lookup"><span data-stu-id="9cc4f-150">Proxy outbound rules</span></span>
<span data-ttu-id="9cc4f-151">Tillåt åtkomst till följande slutpunkter för connector service åtkomst:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-151">Allow access to the following endpoints for connector service access:</span></span>

* <span data-ttu-id="9cc4f-152">*. msappproxy.net</span><span class="sxs-lookup"><span data-stu-id="9cc4f-152">*.msappproxy.net</span></span>
* <span data-ttu-id="9cc4f-153">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="9cc4f-153">*.servicebus.windows.net</span></span>

<span data-ttu-id="9cc4f-154">Tillåt åtkomst till följande slutpunkter för inledande registrering:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-154">For initial registration, allow access to the following endpoints:</span></span>

* <span data-ttu-id="9cc4f-155">login.windows.net</span><span class="sxs-lookup"><span data-stu-id="9cc4f-155">login.windows.net</span></span>
* <span data-ttu-id="9cc4f-156">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="9cc4f-156">login.microsoftonline.com</span></span>

<span data-ttu-id="9cc4f-157">Om du inte tillåter anslutningar av FQDN och måste du ange IP-adressintervall i stället kan du använda följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-157">If you can't allow connectivity by FQDN and need to specify IP ranges instead, use these options:</span></span>

* <span data-ttu-id="9cc4f-158">Tillåt anslutningen utgående åtkomst till alla måldatorer.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-158">Allow the connector outbound access to all destinations.</span></span>
* <span data-ttu-id="9cc4f-159">Tillåt anslutningen utgående åtkomst till [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="9cc4f-159">Allow the connector outbound access to [Azure datacenter IP ranges](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span></span> <span data-ttu-id="9cc4f-160">Utmaningen med hjälp av listan över IP-adressintervall för Azure-datacenter är uppdateras varje vecka.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-160">The challenge with using the list of Azure datacenter IP ranges is that it's updated weekly.</span></span> <span data-ttu-id="9cc4f-161">Du måste placera en process för att säkerställa att din åtkomstregler uppdateras.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-161">You need to put a process in place to ensure that your access rules are updated accordingly.</span></span>

#### <a name="proxy-authentication"></a><span data-ttu-id="9cc4f-162">Proxyautentisering</span><span class="sxs-lookup"><span data-stu-id="9cc4f-162">Proxy authentication</span></span>

<span data-ttu-id="9cc4f-163">Proxy-autentisering stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-163">Proxy authentication is not currently supported.</span></span> <span data-ttu-id="9cc4f-164">Vår aktuell rekommendation är att kopplingen anonym åtkomst till Internet-mål.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-164">Our current recommendation is to allow the connector anonymous access to the Internet destinations.</span></span>

#### <a name="proxy-ports"></a><span data-ttu-id="9cc4f-165">Proxy-portar</span><span class="sxs-lookup"><span data-stu-id="9cc4f-165">Proxy ports</span></span>

<span data-ttu-id="9cc4f-166">Kopplingen gör utgående SSL-baserad anslutningar med hjälp av metoden CONNECT.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-166">The connector makes outbound SSL-based connections by using the CONNECT method.</span></span> <span data-ttu-id="9cc4f-167">Den här metoden anger i princip en tunnel genom den utgående proxyn.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-167">This method essentially sets up a tunnel through the outbound proxy.</span></span> <span data-ttu-id="9cc4f-168">Konfigurera proxyservern så att portarna 443 och 80-tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-168">Configure the proxy server to allow tunneling to ports 443 and 80.</span></span>

>[!NOTE]
><span data-ttu-id="9cc4f-169">När Service Bus körs via HTTPS, använder port 443.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-169">When Service Bus runs over HTTPS, it uses port 443.</span></span> <span data-ttu-id="9cc4f-170">Dock försöker TCP-direktanslutningar Service Bus som standard och faller tillbaka till HTTPS endast om direkt anslutning misslyckades.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-170">However, by default, Service Bus attempts direct TCP connections and falls back to HTTPS only if direct connectivity fails.</span></span>

<span data-ttu-id="9cc4f-171">Se till att anslutningen inte kan ansluta direkt till Azure-tjänster för portar 9350, 9352 och 5671 för att säkerställa att Service Bus-trafik som också skickas via utgående proxyserver.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-171">To ensure that the Service Bus traffic is also sent through the outbound proxy server, ensure that the connector cannot directly connect to the Azure services for ports 9350, 9352, and 5671.</span></span>

#### <a name="ssl-inspection"></a><span data-ttu-id="9cc4f-172">SSL-kontroll</span><span class="sxs-lookup"><span data-stu-id="9cc4f-172">SSL inspection</span></span>
<span data-ttu-id="9cc4f-173">Använd inte SSL-kontroll för connector-trafik, eftersom den orsakar problem för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-173">Do not use SSL inspection for the connector traffic, because it causes problems for the connector traffic.</span></span>

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a><span data-ttu-id="9cc4f-174">Felsöka problem med anslutningen proxy och anslutningsproblem för tjänsten</span><span class="sxs-lookup"><span data-stu-id="9cc4f-174">Troubleshoot connector proxy problems and service connectivity issues</span></span>
<span data-ttu-id="9cc4f-175">Du bör nu se all trafik som passerar genom proxyn.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-175">Now you should see all traffic flowing through the proxy.</span></span> <span data-ttu-id="9cc4f-176">Om du har problem med bör följande felsökningsinformation hjälpa.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-176">If you have problems, the following troubleshooting information should help.</span></span>

<span data-ttu-id="9cc4f-177">Det bästa sättet att identifiera och felsöka problem med anslutningen nätverksanslutningen är att ta en nätverksbild på kopplingstjänsten när kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-177">The best way to identify and troubleshoot connector connectivity issues is to take a network capture on the connector service while starting the connector service.</span></span> <span data-ttu-id="9cc4f-178">Detta kan vara problematiskt, vi ska titta på snabba tips om att samla in och filtrera nätverksspår.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-178">This can be a daunting task, so let’s look at quick tips on capturing and filtering network traces.</span></span>

<span data-ttu-id="9cc4f-179">Du kan använda övervakningsverktyg önskat.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-179">You can use the monitoring tool of your choice.</span></span> <span data-ttu-id="9cc4f-180">Vi använde Microsoft Network Monitor 3.4 vid tillämpningen av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-180">For the purposes of this article, we used Microsoft Network Monitor 3.4.</span></span> <span data-ttu-id="9cc4f-181">Du kan [ladda ned det från Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span><span class="sxs-lookup"><span data-stu-id="9cc4f-181">You can [download it from Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span></span>

<span data-ttu-id="9cc4f-182">Exempel och filter som vi använder i följande avsnitt är specifika för Network Monitor, men principerna som kan tillämpas på alla verktyget.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-182">The examples and filters that we use in the following sections are specific to Network Monitor, but the principles can be applied to any analysis tool.</span></span>

### <a name="take-a-capture-by-using-network-monitor"></a><span data-ttu-id="9cc4f-183">Ta en avbildning med hjälp av Network Monitor</span><span class="sxs-lookup"><span data-stu-id="9cc4f-183">Take a capture by using Network Monitor</span></span>

<span data-ttu-id="9cc4f-184">Starta en avbildning:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-184">To start a capture:</span></span>

1. <span data-ttu-id="9cc4f-185">Öppna Network Monitor och klicka på **nya avbilda**.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-185">Open Network Monitor and click **New Capture**.</span></span>
2. <span data-ttu-id="9cc4f-186">Klicka på den **starta** knappen.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-186">Click the **Start** button.</span></span>

   ![Fönstret i Network Monitor](./media/application-proxy-working-with-proxy-servers/network-capture.png)

<span data-ttu-id="9cc4f-188">När du har slutfört en avbildning, klickar du på den **stoppa** för att avsluta den.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-188">After you complete a capture, click the **Stop** button to end it.</span></span>

### <a name="take-a-capture-of-connector-traffic"></a><span data-ttu-id="9cc4f-189">Ta en avbildning av connector-trafik</span><span class="sxs-lookup"><span data-stu-id="9cc4f-189">Take a capture of connector traffic</span></span>

<span data-ttu-id="9cc4f-190">Utför följande steg för inledande Felsökning:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-190">For initial troubleshooting, perform the following steps:</span></span>

1. <span data-ttu-id="9cc4f-191">Stoppa tjänsten Azure AD Application Proxy Connector från services.msc.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-191">From services.msc, stop the Azure AD Application Proxy Connector service.</span></span>
2. <span data-ttu-id="9cc4f-192">Starta network-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-192">Start the network capture.</span></span>
3. <span data-ttu-id="9cc4f-193">Starta tjänsten Azure AD Application Proxy Connector.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-193">Start the Azure AD Application Proxy Connector service.</span></span>
4. <span data-ttu-id="9cc4f-194">Stoppa insamlingen nätverk.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-194">Stop the network capture.</span></span>

   ![Azure AD Application Proxy Connector-tjänsten i services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-requests-from-the-connector-to-the-proxy-server"></a><span data-ttu-id="9cc4f-196">Titta på begäranden från anslutningen till proxyservern</span><span class="sxs-lookup"><span data-stu-id="9cc4f-196">Look at the requests from the connector to the proxy server</span></span>

<span data-ttu-id="9cc4f-197">Nu när du har en nätverksbild är du redo att filtrera den.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-197">Now that you’ve got a network capture, you're ready to filter it.</span></span> <span data-ttu-id="9cc4f-198">För att titta på spårningen förstå så här filtrerar du avbildningen.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-198">The key to looking at the trace is understanding how to filter the capture.</span></span>

<span data-ttu-id="9cc4f-199">Ett filter är (där 8080 är proxyporten som tjänst):</span><span class="sxs-lookup"><span data-stu-id="9cc4f-199">One filter is as follows (where 8080 is the proxy service port):</span></span>

<span data-ttu-id="9cc4f-200">**(http. Begäran eller http. Svar) och tcp.port==8080**</span><span class="sxs-lookup"><span data-stu-id="9cc4f-200">**(http.Request or http.Response) and tcp.port==8080**</span></span>

<span data-ttu-id="9cc4f-201">Om du anger det här filtret i den **visningsfilter** och välj **tillämpa**, filtrerar avbildade trafiken baserat på filtret.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-201">If you enter this filter in the **Display Filter** window and select **Apply**, it filters the captured traffic based on the filter.</span></span>

<span data-ttu-id="9cc4f-202">Föregående filtret visas endast de HTTP-begäranden och svar till eller från proxyporten.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-202">The preceding filter shows just the HTTP requests and responses to/from the proxy port.</span></span> <span data-ttu-id="9cc4f-203">För en koppling Start där anslutningen är konfigurerad för att använda en proxyserver, visas filtret ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-203">For a connector startup where the connector is configured to use a proxy server, the filter would show something like this:</span></span>

 ![Exempellistan över filtrerade HTTP-begäranden och -svar](./media/application-proxy-working-with-proxy-servers/http-requests.png)

<span data-ttu-id="9cc4f-205">Du söker nu specifikt för CONNECT-begäranden som visar kommunikation med proxyservern.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-205">You're now specifically looking for the CONNECT requests that show communication with the proxy server.</span></span> <span data-ttu-id="9cc4f-206">Vid lyckas, kan du få ett OK (200) för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-206">Upon success, you get an HTTP OK (200) response.</span></span>

<span data-ttu-id="9cc4f-207">Om du ser andra svarskoder, till exempel 407 eller 502, är proxyservern kräver autentisering eller inte tillåter trafiken av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-207">If you see other response codes, such as 407 or 502, the proxy is requiring authentication or not allowing the traffic for some other reason.</span></span> <span data-ttu-id="9cc4f-208">Nu kan du kontaktar supportteamet proxy-server.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-208">At this point, you engage your proxy server support team.</span></span>

### <a name="identify-failed-tcp-connection-attempts"></a><span data-ttu-id="9cc4f-209">Identifiera misslyckade TCP-anslutningsförsök</span><span class="sxs-lookup"><span data-stu-id="9cc4f-209">Identify failed TCP connection attempts</span></span>

<span data-ttu-id="9cc4f-210">Den vanligt scenario som du vill ha är när anslutningen försöker ansluta direkt, men den inte.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-210">The other common scenario that you may be interested in is when the connector is trying to connect directly, but it's failing.</span></span>

<span data-ttu-id="9cc4f-211">Ett annat Network Monitor-filter som hjälper dig att enkelt identifiera problemet är:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-211">Another Network Monitor filter that helps you to easily identify this problem is:</span></span>

<span data-ttu-id="9cc4f-212">**Egenskapen. TCPSynRetransmit**</span><span class="sxs-lookup"><span data-stu-id="9cc4f-212">**property.TCPSynRetransmit**</span></span>

<span data-ttu-id="9cc4f-213">SYN-paket är det första paketet skickas för att upprätta en TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-213">A SYN packet is the first packet sent to establish a TCP connection.</span></span> <span data-ttu-id="9cc4f-214">Om det här paketet inte returnerar ett svar, reattempted SYN.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-214">If this packet doesn’t return a response, the SYN is reattempted.</span></span> <span data-ttu-id="9cc4f-215">Du kan använda föregående filtret för att se alla att SYN-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-215">You can use the preceding filter to see any retransmitted SYNs.</span></span> <span data-ttu-id="9cc4f-216">Du kan sedan kontrollera om dessa SYN-förfrågningar motsvarar connector-relaterade trafik.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-216">Then, you can check whether these SYNs correspond to any connector-related traffic.</span></span>

<span data-ttu-id="9cc4f-217">I följande exempel visas ett misslyckade anslutningsförsök till Service Bus-port 9352:</span><span class="sxs-lookup"><span data-stu-id="9cc4f-217">The following example shows a failed connection attempt to Service Bus port 9352:</span></span>

 ![Exempelsvar misslyckade anslutningsförsök](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

<span data-ttu-id="9cc4f-219">Om du ser något som liknar föregående svaret försöker kopplingen kommunicera direkt med tjänsten Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-219">If you see something like the preceding response, the connector is trying to communicate directly with the Azure Service Bus service.</span></span> <span data-ttu-id="9cc4f-220">Om du räknar kopplingen för att ansluta direkt till Azure-tjänster är en tydlig indikation på att du har ett problem med nätverket eller brandväggen i svaret.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-220">If you expect the connector to make direct connections to the Azure services, this response is a clear indication that you have a network or firewall problem.</span></span>

>[!NOTE]
><span data-ttu-id="9cc4f-221">Om du har konfigurerats för att använda en proxyserver, kan svaret innebära att Service Bus försöker en direkt TCP-anslutning innan du växlar till försöker ansluta via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-221">If you are configured to use a proxy server, this response might mean that Service Bus is attempting a direct TCP connection before switching to attempting a connection over HTTPS.</span></span>
>

<span data-ttu-id="9cc4f-222">Nätverket trace analys är inte för alla.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-222">Network trace analysis is not for everyone.</span></span> <span data-ttu-id="9cc4f-223">Men det kan vara ett ovärderligt verktyg för att få snabb information om vad som händer med nätverket.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-223">But it can be a valuable tool to get quick information about what's going on with your network.</span></span>

<span data-ttu-id="9cc4f-224">Om du fortsätter att det blir problem med anslutningen anslutningsproblem, kan du skapa en biljett med vårt supportteam.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-224">If you continue to struggle with connector connectivity issues, create a ticket with our support team.</span></span> <span data-ttu-id="9cc4f-225">Gruppen kan hjälpa dig med felsökningen.</span><span class="sxs-lookup"><span data-stu-id="9cc4f-225">The team can assist you with further troubleshooting.</span></span>

<span data-ttu-id="9cc4f-226">Information om hur du löser problem med Application Proxy Connector finns i [felsöka Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="9cc4f-226">For information about resolving errors with Application Proxy Connector, see [Troubleshoot Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cc4f-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9cc4f-227">Next steps</span></span>

[<span data-ttu-id="9cc4f-228">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="9cc4f-228">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)<br><span data-ttu-id="9cc4f-229">
[Tyst installation av Azure AD Application Proxy Connector](active-directory-application-proxy-silent-installation.md)</span><span class="sxs-lookup"><span data-stu-id="9cc4f-229">
[How to silently install the Azure AD Application Proxy Connector ](active-directory-application-proxy-silent-installation.md)</span></span>
