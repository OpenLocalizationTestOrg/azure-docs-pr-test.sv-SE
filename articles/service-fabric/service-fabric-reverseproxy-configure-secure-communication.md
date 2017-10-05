---
title: "Azure Service Fabric omvänd proxy säker kommunikation | Microsoft Docs"
description: "Konfigurera omvänd proxy för att aktivera säker slutpunkt till slutpunkt-kommunikation."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: 568f9638c59282bcd7d3fae058a1588a889c22dc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a><span data-ttu-id="650c1-103">Ansluta till en säker tjänst med omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="650c1-103">Connect to a secure service with the reverse proxy</span></span>

<span data-ttu-id="650c1-104">Den här artikeln förklarar hur du upprättar säker anslutning mellan omvänd proxy och tjänster, vilket gör att en säker kanal för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="650c1-104">This article explains how to establish secure connection between the reverse proxy and services, thus enabling an end to end secure channel.</span></span>

<span data-ttu-id="650c1-105">Ansluter till säker tjänster stöds endast när omvänd proxy har konfigurerats för att lyssna på HTTPS.</span><span class="sxs-lookup"><span data-stu-id="650c1-105">Connecting to secure services is supported only when reverse proxy is configured to listen on HTTPS.</span></span> <span data-ttu-id="650c1-106">Resten av dokumentet förutsätter så är fallet.</span><span class="sxs-lookup"><span data-stu-id="650c1-106">Rest of the document assumes this is the case.</span></span>
<span data-ttu-id="650c1-107">Referera till [omvänd proxy i Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) konfigurera omvänd proxy i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="650c1-107">Refer to [Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) to configure the reverse proxy in Service Fabric.</span></span>

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a><span data-ttu-id="650c1-108">Upprätta säker anslutning mellan omvänd proxy och tjänster</span><span class="sxs-lookup"><span data-stu-id="650c1-108">Secure connection establishment between the reverse proxy and services</span></span> 

### <a name="reverse-proxy-authenticating-to-services"></a><span data-ttu-id="650c1-109">Omvänd proxy som autentiserar till tjänster:</span><span class="sxs-lookup"><span data-stu-id="650c1-109">Reverse proxy authenticating to services:</span></span>
<span data-ttu-id="650c1-110">Omvänd proxy identifierar sig själv för tjänster med hjälp av dess certifikatet som anges med ***reverseProxyCertificate*** egenskap i den **klustret** [typen avsnittet](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="650c1-110">The reverse proxy identifies itself to services using its certificate, specified with ***reverseProxyCertificate*** property in the **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="650c1-111">Tjänster kan implementera logik för att verifiera certifikatet som presenterades av omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="650c1-111">Services can implement the logic to verify the certificate presented by the reverse proxy.</span></span> <span data-ttu-id="650c1-112">Tjänsterna kan ange godkända klienten certifikatinformation som konfigurationsinställningarna i konfigurationspaketet.</span><span class="sxs-lookup"><span data-stu-id="650c1-112">The services can specify the accepted client certificate details as configuration settings in the configuration package.</span></span> <span data-ttu-id="650c1-113">Detta kan läsa vid körning och används för att validera certifikatet som presenterades av omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="650c1-113">This can be read at runtime and used to validate the certificate presented by the reverse proxy.</span></span> <span data-ttu-id="650c1-114">Referera till [hantera applikationsparametrarna](service-fabric-manage-multiple-environment-app-configuration.md) att lägga till konfigurationsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="650c1-114">Refer to [Manage application parameters](service-fabric-manage-multiple-environment-app-configuration.md) to add the configuration settings.</span></span> 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a><span data-ttu-id="650c1-115">Omvänd proxy verifierar tjänstidentitet via certifikatet som presenterades av tjänsten:</span><span class="sxs-lookup"><span data-stu-id="650c1-115">Reverse proxy verifying the service's identity via the certificate presented by the service:</span></span>
<span data-ttu-id="650c1-116">För att utföra valideringen av servercertifikatet för de certifikat som presenteras av tjänsterna omvänd proxy stöder något av följande alternativ: None, ServiceCommonNameAndIssuer och ServiceCertificateThumbprints.</span><span class="sxs-lookup"><span data-stu-id="650c1-116">To perform server certificate validation of the certificates presented by the services, reverse proxy supports one of the following options: None, ServiceCommonNameAndIssuer, and ServiceCertificateThumbprints.</span></span>
<span data-ttu-id="650c1-117">Välj något av följande tre alternativ att ange den **ApplicationCertificateValidationPolicy** i avsnittet Parametrar för ApplicationGateway/http-elementet under [fabricSettings](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="650c1-117">To select one of these three options, specify the **ApplicationCertificateValidationPolicy** in the parameters section of ApplicationGateway/Http element under [fabricSettings](service-fabric-cluster-fabric-settings.md).</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "None"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="650c1-118">Referera till nästa avsnitt för mer information om ytterligare konfiguration för var och en av dessa alternativ.</span><span class="sxs-lookup"><span data-stu-id="650c1-118">Refer to the next section for details about additional configuration for each of these options.</span></span>

### <a name="service-certificate-validation-options"></a><span data-ttu-id="650c1-119">Tjänstalternativ för certifikat</span><span class="sxs-lookup"><span data-stu-id="650c1-119">Service certificate validation options</span></span> 

- <span data-ttu-id="650c1-120">**Ingen**: omvänd proxy hoppar över kontroll av servercertifikatet via proxy och upprättar säker anslutning.</span><span class="sxs-lookup"><span data-stu-id="650c1-120">**None**: Reverse proxy skips verification of the proxied service certificate and establishes the secure connection.</span></span> <span data-ttu-id="650c1-121">Detta är standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="650c1-121">This is the default behavior.</span></span>
<span data-ttu-id="650c1-122">Ange den **ApplicationCertificateValidationPolicy** med värdet **ingen** i avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="650c1-122">Specify the **ApplicationCertificateValidationPolicy** with value **None** in the parameters section of ApplicationGateway/Http element.</span></span>

- <span data-ttu-id="650c1-123">**ServiceCommonNameAndIssuer**: omvänd proxy kontrollerar certifikatet som presenterades av tjänsten baserat på certifikatets nätverksnamn och omedelbar utfärdaren tumavtryck: Ange den **ApplicationCertificateValidationPolicy** med värdet **ServiceCommonNameAndIssuer** i avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="650c1-123">**ServiceCommonNameAndIssuer**: Reverse proxy verifies the certificate presented by the service based on certificate's common name and immediate issuer's thumbprint: Specify the **ApplicationCertificateValidationPolicy** with value **ServiceCommonNameAndIssuer** in the parameters section of ApplicationGateway/Http element.</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCommonNameAndIssuer"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="650c1-124">Lista över vanliga tjänstnamn och utfärdaren tumavtryck lägger du till en **ServiceCommonNameAndIssuer-ApplicationGateway/Http** elementet under fabricSettings, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="650c1-124">To specify the list of service common name and issuer thumbprints, add a **ApplicationGateway/Http/ServiceCommonNameAndIssuer** element under fabricSettings, as shown below.</span></span> <span data-ttu-id="650c1-125">Flera certifikatets unika namn och utfärdaren tumavtrycket par kan läggas till i matriselementet parametrar.</span><span class="sxs-lookup"><span data-stu-id="650c1-125">Multiple certificate common name and issuer thumbprint pairs can be added in the parameters array element.</span></span> 

<span data-ttu-id="650c1-126">Om slutpunkten omvänd proxy ansluter till anger ett certifikat som är vanliga namn och utfärdaren tumavtryck matchar någon av de värden som anges här, SSL-kanal upprättas.</span><span class="sxs-lookup"><span data-stu-id="650c1-126">If the endpoint reverse proxy is connecting to presents a certificate who's common name and  issuer thumbprint matches any of the values specified here, SSL channel is established.</span></span> <span data-ttu-id="650c1-127">Vid fel att matcha certifikatinformation misslyckas omvänd proxy klientbegäran med statuskod 502 (felaktig Gateway).</span><span class="sxs-lookup"><span data-stu-id="650c1-127">Upon failure to match the certificate details, reverse proxy fails the client's request with a 502 (Bad Gateway) status code.</span></span> <span data-ttu-id="650c1-128">HTTP-statusrad innehåller också frasen ”ogiltigt SSL-certifikatet”.</span><span class="sxs-lookup"><span data-stu-id="650c1-128">The HTTP status line will also contain the phrase "Invalid SSL Certificate."</span></span> 

```json
{
"fabricSettings": [
          ...
          {
             "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
            "parameters": [
              {
                "name": "WinFabric-Test-Certificate-CN1",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
              },
              {
                "name": "WinFabric-Test-Certificate-CN2",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
              }
            ]
          }
        ],
        ...
}
```


- <span data-ttu-id="650c1-129">**ServiceCertificateThumbprints**: omvänd proxy ska verifiera Tjänstcertifikatet via proxy baserat på dess tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="650c1-129">**ServiceCertificateThumbprints**: Reverse proxy will verify the proxied service certificate based on its thumbprint.</span></span> <span data-ttu-id="650c1-130">Du kan välja att gå vägen när tjänsterna som är konfigurerade med self signerade certifikat: Ange den **ApplicationCertificateValidationPolicy** med värdet **ServiceCertificateThumbprints** i avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="650c1-130">You can choose to go this route when the services are configured with self signed certificates: Specify the **ApplicationCertificateValidationPolicy** with value **ServiceCertificateThumbprints** in the parameters section of ApplicationGateway/Http element.</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCertificateThumbprints"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="650c1-131">Ange tumavtryck med en **ServiceCertificateThumbprints** posten under avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="650c1-131">Also specify the thumbprints with a **ServiceCertificateThumbprints** entry under parameters section of ApplicationGateway/Http element.</span></span> <span data-ttu-id="650c1-132">Flera tumavtryck kan anges som en kommaavgränsad lista i värdefältet enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="650c1-132">Multiple thumbprints can be specified as a comma-separated list in the value field, as shown below:</span></span>

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "ServiceCertificateThumbprints",
                "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
              }
            ]
          }
        ],
        ...
}
```

<span data-ttu-id="650c1-133">Om tumavtrycket för certifikatet visas i den här posten config lyckas omvänd proxy SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="650c1-133">If the thumbprint of the server certificate is listed in this config entry, reverse proxy succeeds the SSL connection.</span></span> <span data-ttu-id="650c1-134">Annars avslutar anslutningen och misslyckas klientbegäran med 502 (felaktig Gateway).</span><span class="sxs-lookup"><span data-stu-id="650c1-134">Otherwise, it terminates the connection and fails the client's request with a 502 (Bad Gateway).</span></span> <span data-ttu-id="650c1-135">HTTP-statusrad innehåller också frasen ”ogiltigt SSL-certifikatet”.</span><span class="sxs-lookup"><span data-stu-id="650c1-135">The HTTP status line will also contain the phrase "Invalid SSL Certificate."</span></span>

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a><span data-ttu-id="650c1-136">Slutpunkten markeringen logik när tjänster exponera säker samt oskyddat slutpunkter</span><span class="sxs-lookup"><span data-stu-id="650c1-136">Endpoint selection logic when services expose secure as well as unsecured endpoints</span></span>
<span data-ttu-id="650c1-137">Service fabric stöder konfiguration av flera slutpunkter för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="650c1-137">Service fabric supports configuring  multiple endpoints for a service.</span></span> <span data-ttu-id="650c1-138">Se [Ange resurser i ett tjänstmanifest](service-fabric-service-manifest-resources.md).</span><span class="sxs-lookup"><span data-stu-id="650c1-138">See [Specify resources in a service manifest](service-fabric-service-manifest-resources.md).</span></span>

<span data-ttu-id="650c1-139">Omvänd proxy väljer en av slutpunkterna att vidarebefordra begäran baserat på de **ListenerName** Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="650c1-139">Reverse proxy selects one of the endpoints to forward the request based on the  **ListenerName** query parameter.</span></span> <span data-ttu-id="650c1-140">Om detta inte anges välja den valfri slutpunkt från listan över slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="650c1-140">If this is not specified, it can pick any endpoint from the endpoints list.</span></span> <span data-ttu-id="650c1-141">Nu kan det vara en HTTP- eller HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="650c1-141">Now this can be an HTTP or HTTPS endpoint.</span></span> <span data-ttu-id="650c1-142">Det kan finnas/krav för scenarier där du vill att fungera i ett ”säkert läge”, dvs omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="650c1-142">There might be scenarios/requirements where you want the reverse proxy to operate in a "secure only mode", i.e</span></span> <span data-ttu-id="650c1-143">vill du inte säker omvänd proxy vidarebefordrar begäranden till oskyddad slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="650c1-143">you don't want the secure reverse proxy to forward requests to unsecured endpoints.</span></span> <span data-ttu-id="650c1-144">Detta kan uppnås genom att ange den **SecureOnlyMode** konfigurationspost med värdet **SANT** i avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="650c1-144">This can be achieved by specifying the **SecureOnlyMode** configuration entry with value **true** in the parameters section of ApplicationGateway/Http element.</span></span>   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> 
> <span data-ttu-id="650c1-145">När du arbetar i **SecureOnlyMode**om klienten har angett en **ListenerName** motsvarar en HTTP(unsecured) slutpunkt omvänd proxy misslyckas begäran med en (inget hittas) http-status 404.</span><span class="sxs-lookup"><span data-stu-id="650c1-145">When operating in **SecureOnlyMode**, if client has specified a **ListenerName** corresponding to an HTTP(unsecured) endpoint, reverse proxy fails the request with a 404 (Not Found) HTTP status code.</span></span>

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a><span data-ttu-id="650c1-146">Konfigurera certifikat för klientautentisering via omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="650c1-146">Setting up client certificate authentication through the reverse proxy</span></span>
<span data-ttu-id="650c1-147">SSL-avslutning sker på omvänd proxy och alla data för klient-certifikatet har gått förlorade.</span><span class="sxs-lookup"><span data-stu-id="650c1-147">SSL termination happens at the reverse proxy and all the client certificate data is lost.</span></span> <span data-ttu-id="650c1-148">Tjänster för autentisering av klientcertifikat, ange den **ForwardClientCertificate** i avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="650c1-148">For the services to perform client certificate authentication, set the **ForwardClientCertificate** setting in the parameters section of ApplicationGateway/Http element.</span></span>

1. <span data-ttu-id="650c1-149">När **ForwardClientCertificate** är inställd på **FALSKT**, använda omvänd proxy inte begär för klientcertifikatet under dess SSL-handskakning med klienten.</span><span class="sxs-lookup"><span data-stu-id="650c1-149">When **ForwardClientCertificate** is set to **false**, reverse proxy will not request for the client certificate during its SSL handshake with the client.</span></span>
<span data-ttu-id="650c1-150">Detta är standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="650c1-150">This is the default behavior.</span></span>

2. <span data-ttu-id="650c1-151">När **ForwardClientCertificate** är inställd på **SANT**, omvänd Proxybegäranden för klientens certifikat under dess SSL-handskakning med klienten.</span><span class="sxs-lookup"><span data-stu-id="650c1-151">When **ForwardClientCertificate** is set to **true**, reverse proxy requests for the client's certificate during its SSL handshake with the client.</span></span>
<span data-ttu-id="650c1-152">Sedan vidarebefordras klienten certifikatdata i en anpassad HTTP-huvud med namnet **X-klientcertifikatet**.</span><span class="sxs-lookup"><span data-stu-id="650c1-152">It will then forward the client certificate data in a custom HTTP header named **X-Client-Certificate**.</span></span> <span data-ttu-id="650c1-153">Huvudets värde är base64-kodade PEM Formatsträngen för klientens certifikat.</span><span class="sxs-lookup"><span data-stu-id="650c1-153">The header value is the base64 encoded PEM format string of the client's certificate.</span></span> <span data-ttu-id="650c1-154">Tjänsten kan lyckas eller inte begäran med lämpliga statuskoden efter att ha kontrollerat certifikatdata.</span><span class="sxs-lookup"><span data-stu-id="650c1-154">The service can succeed/fail the request with appropriate status code after inspecting the certificate data.</span></span>
<span data-ttu-id="650c1-155">Om klienten inte ett certifikat, omvänd proxy vidarebefordrar ett tomt huvud och låta tjänsten hantera fallet.</span><span class="sxs-lookup"><span data-stu-id="650c1-155">If the client does not present a certificate, reverse proxy forwards an empty header and let the service handle the case.</span></span>

> <span data-ttu-id="650c1-156">Omvänd proxy är enbart vidarebefordrare.</span><span class="sxs-lookup"><span data-stu-id="650c1-156">Reverse proxy is a mere forwarder.</span></span> <span data-ttu-id="650c1-157">Det kommer inte att utföra någon validering av klientens certifikat.</span><span class="sxs-lookup"><span data-stu-id="650c1-157">It will not perform any validation of the client's certificate.</span></span>


## <a name="next-steps"></a><span data-ttu-id="650c1-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="650c1-158">Next steps</span></span>
* <span data-ttu-id="650c1-159">Referera till [konfigurera omvänd proxy för att ansluta till säkra tjänster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) för Azure Resource Manager mallen exempel för att konfigurera secure omvänd proxy med olika Tjänstcertifikatet verifieringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="650c1-159">Refer to [Configure reverse proxy to connect to secure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples to configure secure reverse proxy with the different service certificate validation options.</span></span>
* <span data-ttu-id="650c1-160">Se ett exempel på HTTP-kommunikation mellan tjänster i en [exempelprojektet på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="650c1-160">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="650c1-161">RPC-anrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="650c1-161">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="650c1-162">Webb-API som använder OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="650c1-162">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="650c1-163">Hantera klustercertifikat</span><span class="sxs-lookup"><span data-stu-id="650c1-163">Manage cluster certificates</span></span>](service-fabric-cluster-security-update-certs-azure.md)
