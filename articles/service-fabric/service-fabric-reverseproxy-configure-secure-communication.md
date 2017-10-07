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
# <a name="connect-tooa-secure-service-with-hello-reverse-proxy"></a>Anslut tooa säker service med hello omvänd proxy

Den här artikeln förklarar hur tooestablish säker anslutning mellan hello omvänd proxy och tjänster, vilket gör att en säker kanal för slutpunkt-tooend.

Anslutningstjänsterna toosecure stöds endast när omvänd proxy är konfigurerade toolisten på HTTPS. Resten av dokumentet hello antas hello fall.
Se för[omvänd proxy i Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello omvänd proxy i Service Fabric.

## <a name="secure-connection-establishment-between-hello-reverse-proxy-and-services"></a>Upprätta säker anslutning mellan hello omvänd proxy och tjänster 

### <a name="reverse-proxy-authenticating-tooservices"></a>Omvänd proxy som autentiserar tooservices:
hello omvänd proxy identifierar sig själv tooservices med dess certifikat som anges med ***reverseProxyCertificate*** egenskap i hello **klustret** [typen avsnittet](../azure-resource-manager/resource-group-authoring-templates.md). Tjänster kan implementera hello logik tooverify hello certifikatet som presenterades hello omvänd proxy. hello tjänster kan ange hello accepteras klientinformation för certifikat som konfigurationsinställningarna i hello konfigurationspaket. Detta kan läsas vid körning och används toovalidate hello certifikatet som presenterades hello omvänd proxy. Se för[hantera applikationsparametrarna](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello-konfigurationsinställningar. 

### <a name="reverse-proxy-verifying-hello-services-identity-via-hello-certificate-presented-by-hello-service"></a>Omvänd proxy Kontrollera hello tjänstens identitet via hello-certifikatet som presenterades av hello-tjänsten:
tooperform valideringen av servercertifikatet hello-certifikat som presenteras av hello tjänster, omvänd proxy stöder något av följande alternativ för hello: None, ServiceCommonNameAndIssuer och ServiceCertificateThumbprints.
tooselect något av följande tre alternativ ange hello **ApplicationCertificateValidationPolicy** under hello parametrar i ApplicationGateway/http-elementet under [fabricSettings](service-fabric-cluster-fabric-settings.md).

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

Mer information om ytterligare konfiguration för de här alternativen finns i toohello nästa avsnitt.

### <a name="service-certificate-validation-options"></a>Tjänstalternativ för certifikat 

- **Ingen**: omvänd proxy hoppar över kontroll av hello via proxy tjänstcertifikat och upprättar hello säker anslutning. Detta är standardbeteendet för hello.
Ange hello **ApplicationCertificateValidationPolicy** med värdet **ingen** under hello parametrar i ApplicationGateway/http-element.

- **ServiceCommonNameAndIssuer**: omvänd proxy kontrollerar hello certifikat som presenteras av hello tjänsten baserat på certifikatets nätverksnamn och omedelbar utfärdaren tumavtryck: Ange hello **ApplicationCertificateValidationPolicy**  med värdet **ServiceCommonNameAndIssuer** under hello parametrar i ApplicationGateway/http-element.

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

toospecify hello lista över vanliga tjänstnamn och utfärdaren tumavtryck lägga till en **ServiceCommonNameAndIssuer-ApplicationGateway/Http** elementet under fabricSettings, enligt nedan. Flera certifikatets unika namn och utfärdaren tumavtrycket par kan läggas till i hello parametrar matriselement. 

Om hello endpoint omvänd proxy ansluter toopresents ett certifikat som är vanliga namn och utfärdaren tumavtryck matchar hello-värden som anges här, SSL-kanal upprättas. När information om felet toomatch hello certifikatet misslyckas omvänd proxy hello klientbegäran med statuskod 502 (felaktig Gateway). hello HTTP-statusrad innehåller också hello frasen ”ogiltigt SSL-certifikatet”. 

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


- **ServiceCertificateThumbprints**: omvänd proxy ska verifiera hello via proxy tjänstcertifikat baserat på dess tumavtryck. Du kan välja toogo vägen när hello tjänster har konfigurerats med självsignerade certifikat: Ange hello **ApplicationCertificateValidationPolicy** med värdet **ServiceCertificateThumbprints**under hello parametrar i ApplicationGateway/http-element.

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

Ange hello tumavtryck med en **ServiceCertificateThumbprints** posten under avsnittet Parametrar för ApplicationGateway/http-elementet. Flera tumavtryck kan anges som en kommaavgränsad lista i hello värdefältet enligt nedan:

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

Om hello hello server certifikatets tumavtryck visas i den här posten config lyckas omvänd proxy hello SSL-anslutning. I annat fall det avslutas hello anslutning och misslyckas hello klientbegäran med 502 (felaktig Gateway). hello HTTP-statusrad innehåller också hello frasen ”ogiltigt SSL-certifikatet”.

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Slutpunkten markeringen logik när tjänster exponera säker samt oskyddat slutpunkter
Service fabric stöder konfiguration av flera slutpunkter för en tjänst. Se [Ange resurser i ett tjänstmanifest](service-fabric-service-manifest-resources.md).

Omvänd proxy väljer ett av hello slutpunkter tooforward hello begäran baserat på hello **ListenerName** Frågeparametern. Om detta inte anges välja den valfri slutpunkt hello slutpunkter listan. Nu kan det vara en HTTP- eller HTTPS-slutpunkt. Det kan finnas/krav för scenarier där du vill att hello omvänd proxy toooperate i ett ”säkert läge”, dvs vill du inte hello säker omvänd proxy tooforward begäranden toounsecured slutpunkter. Detta kan uppnås genom att ange hello **SecureOnlyMode** konfigurationspost med värdet **SANT** under hello parametrar i ApplicationGateway/http-element.   

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
> När du arbetar i **SecureOnlyMode**om klienten har angett en **ListenerName** motsvarande tooan HTTP(unsecured) endpoint omvänd proxy misslyckas hello begäran med en (inget hittas) http-status 404.

## <a name="setting-up-client-certificate-authentication-through-hello-reverse-proxy"></a>Konfigurera certifikat för klientautentisering med hello omvänd proxy
SSL-avslutning sker på hello omvänd proxy och alla hello klientens certifikat om data går förlorade. Ange hello för hello services tooperform autentisering av klientcertifikat, **ForwardClientCertificate** inställningen under hello parametrar i ApplicationGateway/http-element.

1. När **ForwardClientCertificate** har angetts för**FALSKT**, använda omvänd proxy inte begär för hello klientcertifikatet under dess SSL-handskakning med hello-klienten.
Detta är standardbeteendet för hello.

2. När **ForwardClientCertificate** har angetts för**SANT**, omvänd Proxybegäranden för hello klientcertifikat under dess SSL-handskakning med hello-klienten.
Sedan vidarebefordras hello klienten certifikatdata i en anpassad HTTP-huvud med namnet **X-klientcertifikatet**. hello huvudvärde är base64-kodade hello PEM formatsträng hello klientcertifikat för. hello-tjänsten kan lyckas eller inte hello begäran med lämpliga statuskoden efter att ha kontrollerat hello certifikatdata.
Om hello klienten inte ett certifikat, omvänd proxy vidarebefordrar ett tomt huvud och låta hello service referensen hello fallet.

> Omvänd proxy är enbart vidarebefordrare. Det kommer inte att utföra någon validering av hello klientcertifikat.


## <a name="next-steps"></a>Nästa steg
* Se för[konfigurera omvänd proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) för Azure Resource Manager mallen exempel tooconfigure säker omvänd proxy med hello olika service certifikat verifieringsalternativ.
* Se ett exempel på HTTP-kommunikation mellan tjänster i en [exempelprojektet på GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [RPC-anrop med Reliable Services fjärrkommunikation](service-fabric-reliable-services-communication-remoting.md)
* [Webb-API som använder OWIN i Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Hantera klustercertifikat](service-fabric-cluster-security-update-certs-azure.md)
