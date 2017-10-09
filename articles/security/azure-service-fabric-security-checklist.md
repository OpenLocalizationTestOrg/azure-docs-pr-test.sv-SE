---
title: "Checklista för aaaAzure service fabric-säkerhet | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning checklista för Azure-strukturen säkerhet säkerhet."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 10ffaea9e7e4de6d758b0a57a79e269c87bfd14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-checklist"></a>Checklista för Azure Service Fabric-säkerhet
Den här artikeln innehåller en checklista för enkelt att använda som hjälper dig att skydda din Azure Service Fabric-miljö.

## <a name="introduction"></a>Introduktion
Azure Service Fabric är en plattform för distribuerade system som gör det enkelt toopackage, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Service Fabric löser också hello betydande problem för utveckling och hantering av molnprogram. Utvecklare och administratörer kan undvika komplexa infrastrukturproblem och fokusera på att implementera verksamhetskritiska, krävande arbetsbelastningar som är skalbara, tillförlitliga och hanterbara.

## <a name="checklist"></a>Checklista
Använd följande checklista toohelp du se till att du inte har missat några viktiga problem i hantering och konfiguration av en säker Azure Service Fabric-lösning hello.


|Checklista för kategori| Beskrivning |
| ------------ | -------- |
|[Rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.</li><li>Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). </li><li>   Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.</li></ul>|
|[X.509-certifikat och Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Certifikat](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/working-with-certificates) används i kluster körs produktionsarbetsbelastningar ska skapas med hjälp av en korrekt konfigurerade tjänsten för Windows Server-certifikat eller hämtas från en godkänd [certifikatutfärdare (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Använd aldrig något [tillfälliga eller testa certifikat](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) i produktion som skapas med verktyg som [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>Du kan använda en [självsignerat certifikat](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) men du bör bara göra det för testkluster och inte i produktion.</li></ul>|
|[Klustersäkerhet](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>hello klustret säkerhetsscenarier är nod till nod säkerhet, klient-till-nod säkerhet [rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Autentisering för klustret](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Autentiserar [nod till nod kommunikation](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) för klustret. </li></ul>|
|[Server-autentisering](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Autentiserar hello [cluster management slutpunkter](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-portal) tooa management-klienten.</li></ul>|
|[Programsäkerhet](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Kryptering och dekryptering av konfigurationsvärden för programmet.</li><li> Kryptering av data mellan noderna under replikeringen.</li></ul>|
|[Certifikat för kluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Det här certifikatet är obligatoriska toosecure hello kommunikation mellan hello noder i ett kluster.</li><li>  Ange hello tumavtryck hello primära certifikat i hello tumavtrycket avsnittet och som hello sekundär i hello ThumbprintSecondary variabler.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Det här certifikatet visas toohello klienten när den försöker tooconnect toothis klustret. Du kan använda två olika servercertifikat, en primär och sekundär för uppgradering.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Det här är en uppsättning certifikat som du vill tooinstall på hello autentiserade klienter. </li></ul>|
|ClientCertificateCommonNames| <ul><li>Ange hello nätverksnamnet för hello första klientcertifikatet för hello CertificateCommonName. Hej CertificateIssuerThumbprint är hello tumavtrycket för hello utfärdaren av det här certifikatet. </li></ul>|
|ReverseProxyCertificate| <ul><li>Detta är ett valfritt certifikat som kan anges om du vill toosecure din [omvänd Proxy](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Key Vault| <ul><li>Toomanage certifikat som används för Service Fabric-kluster i Azure.  </li></ul>|


## <a name="next-steps"></a>Nästa steg
- [Uppgraderingsprocessen för Service Fabric-kluster och förväntningar från dig](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-upgrade)
- [Hantera dina Service Fabric-program i Visual Studio](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Tjänstens hälsa för Fabric modellen introduktion](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-health-introduction).
