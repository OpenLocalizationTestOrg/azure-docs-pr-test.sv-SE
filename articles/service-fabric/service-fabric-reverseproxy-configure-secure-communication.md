---
title: "aaaAzure Service Fabric omvänd proxy säker kommunikation | Microsoft Docs"
description: "Konfigurera omvänd proxy tooenable säker slutpunkt till slutpunkt-kommunikation."
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
ms.openlocfilehash: e1248dffe2c324373ad0d09d3f5f094db74480d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-secure-service-with-hello-reverse-proxy"></a><span data-ttu-id="3dbbb-103">Anslut tooa säker service med hello omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="3dbbb-103">Connect tooa secure service with hello reverse proxy</span></span>

<span data-ttu-id="3dbbb-104">Den här artikeln förklarar hur tooestablish säker anslutning mellan hello omvänd proxy och tjänster, vilket gör att en säker kanal för slutpunkt-tooend.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-104">This article explains how tooestablish secure connection between hello reverse proxy and services, thus enabling an end tooend secure channel.</span></span>

<span data-ttu-id="3dbbb-105">Anslutningstjänsterna toosecure stöds endast när omvänd proxy är konfigurerade toolisten på HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-105">Connecting toosecure services is supported only when reverse proxy is configured toolisten on HTTPS.</span></span> <span data-ttu-id="3dbbb-106">Resten av dokumentet hello antas hello fall.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-106">Rest of hello document assumes this is hello case.</span></span>
<span data-ttu-id="3dbbb-107">Se för[omvänd proxy i Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello omvänd proxy i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-107">Refer too[Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello reverse proxy in Service Fabric.</span></span>

## <a name="secure-connection-establishment-between-hello-reverse-proxy-and-services"></a><span data-ttu-id="3dbbb-108">Upprätta säker anslutning mellan hello omvänd proxy och tjänster</span><span class="sxs-lookup"><span data-stu-id="3dbbb-108">Secure connection establishment between hello reverse proxy and services</span></span> 

### <a name="reverse-proxy-authenticating-tooservices"></a><span data-ttu-id="3dbbb-109">Omvänd proxy som autentiserar tooservices:</span><span class="sxs-lookup"><span data-stu-id="3dbbb-109">Reverse proxy authenticating tooservices:</span></span>
<span data-ttu-id="3dbbb-110">hello omvänd proxy identifierar sig själv tooservices med dess certifikat som anges med ***reverseProxyCertificate*** egenskap i hello **klustret** [typen avsnittet](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3dbbb-110">hello reverse proxy identifies itself tooservices using its certificate, specified with ***reverseProxyCertificate*** property in hello **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="3dbbb-111">Tjänster kan implementera hello logik tooverify hello certifikatet som presenterades hello omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-111">Services can implement hello logic tooverify hello certificate presented by hello reverse proxy.</span></span> <span data-ttu-id="3dbbb-112">hello tjänster kan ange hello accepteras klientinformation för certifikat som konfigurationsinställningarna i hello konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-112">hello services can specify hello accepted client certificate details as configuration settings in hello configuration package.</span></span> <span data-ttu-id="3dbbb-113">Detta kan läsas vid körning och används toovalidate hello certifikatet som presenterades hello omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-113">This can be read at runtime and used toovalidate hello certificate presented by hello reverse proxy.</span></span> <span data-ttu-id="3dbbb-114">Se för[hantera applikationsparametrarna](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello-konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-114">Refer too[Manage application parameters](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello configuration settings.</span></span> 

### <a name="reverse-proxy-verifying-hello-services-identity-via-hello-certificate-presented-by-hello-service"></a><span data-ttu-id="3dbbb-115">Omvänd proxy Kontrollera hello tjänstens identitet via hello-certifikatet som presenterades av hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="3dbbb-115">Reverse proxy verifying hello service's identity via hello certificate presented by hello service:</span></span>
<span data-ttu-id="3dbbb-116">tooperform valideringen av servercertifikatet hello-certifikat som presenteras av hello tjänster, omvänd proxy stöder något av följande alternativ för hello: None, ServiceCommonNameAndIssuer och ServiceCertificateThumbprints.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-116">tooperform server certificate validation of hello certificates presented by hello services, reverse proxy supports one of hello following options: None, ServiceCommonNameAndIssuer, and ServiceCertificateThumbprints.</span></span>
<span data-ttu-id="3dbbb-117">tooselect något av följande tre alternativ ange hello **ApplicationCertificateValidationPolicy** under hello parametrar i ApplicationGateway/http-elementet under [fabricSettings](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="3dbbb-117">tooselect one of these three options, specify hello **ApplicationCertificateValidationPolicy** in hello parameters section of ApplicationGateway/Http element under [fabricSettings](service-fabric-cluster-fabric-settings.md).</span></span>

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

<span data-ttu-id="3dbbb-118">Mer information om ytterligare konfiguration för de här alternativen finns i toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-118">Refer toohello next section for details about additional configuration for each of these options.</span></span>

### <a name="service-certificate-validation-options"></a><span data-ttu-id="3dbbb-119">Tjänstalternativ för certifikat</span><span class="sxs-lookup"><span data-stu-id="3dbbb-119">Service certificate validation options</span></span> 

- <span data-ttu-id="3dbbb-120">**Ingen**: omvänd proxy hoppar över kontroll av hello via proxy tjänstcertifikat och upprättar hello säker anslutning.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-120">**None**: Reverse proxy skips verification of hello proxied service certificate and establishes hello secure connection.</span></span> <span data-ttu-id="3dbbb-121">Detta är standardbeteendet för hello.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-121">This is hello default behavior.</span></span>
<span data-ttu-id="3dbbb-122">Ange hello **ApplicationCertificateValidationPolicy** med värdet **ingen** under hello parametrar i ApplicationGateway/http-element.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-122">Specify hello **ApplicationCertificateValidationPolicy** with value **None** in hello parameters section of ApplicationGateway/Http element.</span></span>

- <span data-ttu-id="3dbbb-123">**ServiceCommonNameAndIssuer**: omvänd proxy kontrollerar hello certifikat som presenteras av hello tjänsten baserat på certifikatets nätverksnamn och omedelbar utfärdaren tumavtryck: Ange hello **ApplicationCertificateValidationPolicy**  med värdet **ServiceCommonNameAndIssuer** under hello parametrar i ApplicationGateway/http-element.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-123">**ServiceCommonNameAndIssuer**: Reverse proxy verifies hello certificate presented by hello service based on certificate's common name and immediate issuer's thumbprint: Specify hello **ApplicationCertificateValidationPolicy** with value **ServiceCommonNameAndIssuer** in hello parameters section of ApplicationGateway/Http element.</span></span>

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

<span data-ttu-id="3dbbb-124">toospecify hello lista över vanliga tjänstnamn och utfärdaren tumavtryck lägga till en **ServiceCommonNameAndIssuer-ApplicationGateway/Http** elementet under fabricSettings, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-124">toospecify hello list of service common name and issuer thumbprints, add a **ApplicationGateway/Http/ServiceCommonNameAndIssuer** element under fabricSettings, as shown below.</span></span> <span data-ttu-id="3dbbb-125">Flera certifikatets unika namn och utfärdaren tumavtrycket par kan läggas till i hello parametrar matriselement.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-125">Multiple certificate common name and issuer thumbprint pairs can be added in hello parameters array element.</span></span> 

<span data-ttu-id="3dbbb-126">Om hello endpoint omvänd proxy ansluter toopresents ett certifikat som är vanliga namn och utfärdaren tumavtryck matchar hello-värden som anges här, SSL-kanal upprättas.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-126">If hello endpoint reverse proxy is connecting toopresents a certificate who's common name and  issuer thumbprint matches any of hello values specified here, SSL channel is established.</span></span> <span data-ttu-id="3dbbb-127">När information om felet toomatch hello certifikatet misslyckas omvänd proxy hello klientbegäran med statuskod 502 (felaktig Gateway).</span><span class="sxs-lookup"><span data-stu-id="3dbbb-127">Upon failure toomatch hello certificate details, reverse proxy fails hello client's request with a 502 (Bad Gateway) status code.</span></span> <span data-ttu-id="3dbbb-128">hello HTTP-statusrad innehåller också hello frasen ”ogiltigt SSL-certifikatet”.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-128">hello HTTP status line will also contain hello phrase "Invalid SSL Certificate."</span></span> 

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


- <span data-ttu-id="3dbbb-129">**ServiceCertificateThumbprints**: omvänd proxy ska verifiera hello via proxy tjänstcertifikat baserat på dess tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-129">**ServiceCertificateThumbprints**: Reverse proxy will verify hello proxied service certificate based on its thumbprint.</span></span> <span data-ttu-id="3dbbb-130">Du kan välja toogo vägen när hello tjänster har konfigurerats med självsignerade certifikat: Ange hello **ApplicationCertificateValidationPolicy** med värdet **ServiceCertificateThumbprints**under hello parametrar i ApplicationGateway/http-element.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-130">You can choose toogo this route when hello services are configured with self signed certificates: Specify hello **ApplicationCertificateValidationPolicy** with value **ServiceCertificateThumbprints** in hello parameters section of ApplicationGateway/Http element.</span></span>

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

<span data-ttu-id="3dbbb-131">Ange hello tumavtryck med en **ServiceCertificateThumbprints** posten under avsnittet Parametrar för ApplicationGateway/http-elementet.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-131">Also specify hello thumbprints with a **ServiceCertificateThumbprints** entry under parameters section of ApplicationGateway/Http element.</span></span> <span data-ttu-id="3dbbb-132">Flera tumavtryck kan anges som en kommaavgränsad lista i hello värdefältet enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="3dbbb-132">Multiple thumbprints can be specified as a comma-separated list in hello value field, as shown below:</span></span>

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

<span data-ttu-id="3dbbb-133">Om hello hello server certifikatets tumavtryck visas i den här posten config lyckas omvänd proxy hello SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-133">If hello thumbprint of hello server certificate is listed in this config entry, reverse proxy succeeds hello SSL connection.</span></span> <span data-ttu-id="3dbbb-134">I annat fall det avslutas hello anslutning och misslyckas hello klientbegäran med 502 (felaktig Gateway).</span><span class="sxs-lookup"><span data-stu-id="3dbbb-134">Otherwise, it terminates hello connection and fails hello client's request with a 502 (Bad Gateway).</span></span> <span data-ttu-id="3dbbb-135">hello HTTP-statusrad innehåller också hello frasen ”ogiltigt SSL-certifikatet”.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-135">hello HTTP status line will also contain hello phrase "Invalid SSL Certificate."</span></span>

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a><span data-ttu-id="3dbbb-136">Slutpunkten markeringen logik när tjänster exponera säker samt oskyddat slutpunkter</span><span class="sxs-lookup"><span data-stu-id="3dbbb-136">Endpoint selection logic when services expose secure as well as unsecured endpoints</span></span>
<span data-ttu-id="3dbbb-137">Service fabric stöder konfiguration av flera slutpunkter för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-137">Service fabric supports configuring  multiple endpoints for a service.</span></span> <span data-ttu-id="3dbbb-138">Se [Ange resurser i ett tjänstmanifest](service-fabric-service-manifest-resources.md).</span><span class="sxs-lookup"><span data-stu-id="3dbbb-138">See [Specify resources in a service manifest](service-fabric-service-manifest-resources.md).</span></span>

<span data-ttu-id="3dbbb-139">Omvänd proxy väljer ett av hello slutpunkter tooforward hello begäran baserat på hello **ListenerName** Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-139">Reverse proxy selects one of hello endpoints tooforward hello request based on hello  **ListenerName** query parameter.</span></span> <span data-ttu-id="3dbbb-140">Om detta inte anges välja den valfri slutpunkt hello slutpunkter listan.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-140">If this is not specified, it can pick any endpoint from hello endpoints list.</span></span> <span data-ttu-id="3dbbb-141">Nu kan det vara en HTTP- eller HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-141">Now this can be an HTTP or HTTPS endpoint.</span></span> <span data-ttu-id="3dbbb-142">Det kan finnas/krav för scenarier där du vill att hello omvänd proxy toooperate i ett ”säkert läge”, dvs</span><span class="sxs-lookup"><span data-stu-id="3dbbb-142">There might be scenarios/requirements where you want hello reverse proxy toooperate in a "secure only mode", i.e</span></span> <span data-ttu-id="3dbbb-143">vill du inte hello säker omvänd proxy tooforward begäranden toounsecured slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-143">you don't want hello secure reverse proxy tooforward requests toounsecured endpoints.</span></span> <span data-ttu-id="3dbbb-144">Detta kan uppnås genom att ange hello **SecureOnlyMode** konfigurationspost med värdet **SANT** under hello parametrar i ApplicationGateway/http-element.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-144">This can be achieved by specifying hello **SecureOnlyMode** configuration entry with value **true** in hello parameters section of ApplicationGateway/Http element.</span></span>   

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
> <span data-ttu-id="3dbbb-145">När du arbetar i **SecureOnlyMode**om klienten har angett en **ListenerName** motsvarande tooan HTTP(unsecured) endpoint omvänd proxy misslyckas hello begäran med en (inget hittas) http-status 404.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-145">When operating in **SecureOnlyMode**, if client has specified a **ListenerName** corresponding tooan HTTP(unsecured) endpoint, reverse proxy fails hello request with a 404 (Not Found) HTTP status code.</span></span>

## <a name="setting-up-client-certificate-authentication-through-hello-reverse-proxy"></a><span data-ttu-id="3dbbb-146">Konfigurera certifikat för klientautentisering med hello omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="3dbbb-146">Setting up client certificate authentication through hello reverse proxy</span></span>
<span data-ttu-id="3dbbb-147">SSL-avslutning sker på hello omvänd proxy och alla hello klientens certifikat om data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-147">SSL termination happens at hello reverse proxy and all hello client certificate data is lost.</span></span> <span data-ttu-id="3dbbb-148">Ange hello för hello services tooperform autentisering av klientcertifikat, **ForwardClientCertificate** inställningen under hello parametrar i ApplicationGateway/http-element.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-148">For hello services tooperform client certificate authentication, set hello **ForwardClientCertificate** setting in hello parameters section of ApplicationGateway/Http element.</span></span>

1. <span data-ttu-id="3dbbb-149">När **ForwardClientCertificate** har angetts för**FALSKT**, använda omvänd proxy inte begär för hello klientcertifikatet under dess SSL-handskakning med hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-149">When **ForwardClientCertificate** is set too**false**, reverse proxy will not request for hello client certificate during its SSL handshake with hello client.</span></span>
<span data-ttu-id="3dbbb-150">Detta är standardbeteendet för hello.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-150">This is hello default behavior.</span></span>

2. <span data-ttu-id="3dbbb-151">När **ForwardClientCertificate** har angetts för**SANT**, omvänd Proxybegäranden för hello klientcertifikat under dess SSL-handskakning med hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-151">When **ForwardClientCertificate** is set too**true**, reverse proxy requests for hello client's certificate during its SSL handshake with hello client.</span></span>
<span data-ttu-id="3dbbb-152">Sedan vidarebefordras hello klienten certifikatdata i en anpassad HTTP-huvud med namnet **X-klientcertifikatet**.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-152">It will then forward hello client certificate data in a custom HTTP header named **X-Client-Certificate**.</span></span> <span data-ttu-id="3dbbb-153">hello huvudvärde är base64-kodade hello PEM formatsträng hello klientcertifikat för.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-153">hello header value is hello base64 encoded PEM format string of hello client's certificate.</span></span> <span data-ttu-id="3dbbb-154">hello-tjänsten kan lyckas eller inte hello begäran med lämpliga statuskoden efter att ha kontrollerat hello certifikatdata.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-154">hello service can succeed/fail hello request with appropriate status code after inspecting hello certificate data.</span></span>
<span data-ttu-id="3dbbb-155">Om hello klienten inte ett certifikat, omvänd proxy vidarebefordrar ett tomt huvud och låta hello service referensen hello fallet.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-155">If hello client does not present a certificate, reverse proxy forwards an empty header and let hello service handle hello case.</span></span>

> <span data-ttu-id="3dbbb-156">Omvänd proxy är enbart vidarebefordrare.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-156">Reverse proxy is a mere forwarder.</span></span> <span data-ttu-id="3dbbb-157">Det kommer inte att utföra någon validering av hello klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-157">It will not perform any validation of hello client's certificate.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3dbbb-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3dbbb-158">Next steps</span></span>
* <span data-ttu-id="3dbbb-159">Se för[konfigurera omvänd proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) för Azure Resource Manager mallen exempel tooconfigure säker omvänd proxy med hello olika service certifikat verifieringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="3dbbb-159">Refer too[Configure reverse proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples tooconfigure secure reverse proxy with hello different service certificate validation options.</span></span>
* <span data-ttu-id="3dbbb-160">Se ett exempel på HTTP-kommunikation mellan tjänster i en [exempelprojektet på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="3dbbb-160">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="3dbbb-161">RPC-anrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="3dbbb-161">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="3dbbb-162">Webb-API som använder OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="3dbbb-162">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="3dbbb-163">Hantera klustercertifikat</span><span class="sxs-lookup"><span data-stu-id="3dbbb-163">Manage cluster certificates</span></span>](service-fabric-cluster-security-update-certs-azure.md)
