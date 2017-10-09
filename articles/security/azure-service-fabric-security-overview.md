---
title: "Översikt över aaaAzure service fabric säkerhet | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hello Azure service fabric-säkerhet."
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
ms.openlocfilehash: ec5355983c5d59f4e0c3b855965f03ac47f1a4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-overview"></a>Översikt över säkerheten i Azure Service Fabric
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) är en plattform för distribuerade system som gör det enkelt toopackage, distribuera och hantera skalbara och tillförlitliga micro-tjänster. Service Fabric-adresser hello betydande utmaningarna med att utveckla och hantera molnprogram. Utvecklare och administratörer kan undvika komplexa infrastrukturproblem och fokusera på att implementera verksamhetskritiska, krävande arbetsbelastningar som är skalbara, tillförlitliga och hanterbara.

Översikt över Azure Service Fabric säkerhet artikeln fokuserar på hello följande områden:

-   Skydda ditt kluster
-   Övervakning och diagnostik
-   Skydda med hjälp av certifikat
-   Rollbaserad åtkomstkontroll (RBAC)
-   Skydda kluster med hjälp av Windows-säkerhet
-   Konfigurera programsäkerhet i Service Fabric
-   Säker kommunikation för tjänster i Azure Service Fabric-säkerhet

## <a name="securing-your-cluster"></a>Skydda ditt kluster
Azure Service Fabric är en orchestrator av tjänster i ett kluster på datorer, kluster måste vara skyddad tooprevent obehöriga användare från att ansluta tooyour kluster, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt toocreate ett oskyddat kluster på så sätt kan anonyma användare tooconnect tooit, om det visar management slutpunkter toohello offentliga Internet.

Det här avsnittet innehåller en översikt över hello säkerhetsscenarier för kluster som körs på Azure eller fristående och hello olika tekniker används tooimplement dessa scenarier. hello klustret scenarier är:

-   Säkerhet för nod till nod
-   Klient-till-nod-säkerhet

### <a name="node-to-node-security"></a>Säkerhet för nod till nod
Skyddar kommunikationen mellan hello virtuella datorer eller datorer i hello kluster. Detta säkerställer att endast datorer som är auktoriserade toojoin hello klustret kan delta i värd för program och tjänster i hello kluster.

Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://msdn.microsoft.com/library/ff649801.aspx) eller [Windows-säkerhet](https://msdn.microsoft.com/library/ff649396.aspx) för Windows Server-datorer.

**Nod till nod Certifikatsäkerhet**

Service Fabric använder X.509-servercertifikat som du anger som en del av hello-nodtyp konfigurationer när du skapar ett kluster. En snabb överblick över dessa certifikat är och [hur du kan hämta eller skapa dem finns i den här artikeln](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Certifikatsäkerhet konfigureras när du skapar klustret hello antingen via hello Azure-portalen, Azure Resource Manager-mallar eller en fristående JSON-mall. Du kan ange ett certifikat för primär och en valfri sekundärt certifikat som används för certifikatet överrullningar. hello primära och sekundära certifikat som du anger bör vara ett annat än hello administratörsklient och skrivskyddad klientcertifikat som du anger för [klient-till-nod säkerhet](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Klient-till-nod-säkerhet
Toonode klientsäkerhet konfigureras med hjälp av klienten identiteter. tooestablish förtroende mellan klienten och hello kluster, måste du konfigurera hello klustret tooknow som klienten identiteter som den kan lita på. Detta kan göras på två olika sätt:

-   Ange hello gruppen Domänanvändare som kan ansluta eller
-   Ange hello nod domänanvändare som kan ansluta.

Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster:

-   Administratör
-   Användare

Åtkomstkontroll ger hello möjligheten för hello administratören toolimit åtkomst toocertain klustertyper för klusteråtgärder för olika grupper av användare, göra hello klustret säkrare. Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.

**Klient-till-nod Certifikatsäkerhet**

Certifikatsäkerhet för klient-till-nod konfigureras när du skapar klustret hello antingen via hello Azure-portalen Resource Manager-mallar eller en fristående JSON-mall genom att ange ett klientcertifikat för admin och/eller ett klientcertifikat. Hej administratör klient- och klientcertifikat du anger måste skilja sig från hello primära och sekundära certifikat som du anger för nod till nod säkerhet.

Klienter som ansluter toohello kluster med hjälp av hello admin certifikat har fullständig åtkomst toomanagement. Klienter som ansluter toohello kluster med hjälp av hello skrivskyddade klientcertifikat har bara läsbehörighet toomanagement. I andra word dessa certifikat används för hello baserar rollen åtkomstkontroll (RBAC).

För Azure läsa [ställer in ett kluster med en Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) toolearn hur tooconfigure certifikat säkerhet i ett kluster.

**Klient-till-nod Azure Active Directory (AAD) säkerheten på Azure**

Kluster som körs på Azure kan även skydda åtkomst toohello management slutpunkter med hjälp av Azure Active Directory (AAD). Se [ställer in ett kluster med en Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) för information om hur toocreate hello nödvändiga AAD-artefakter, hur toopopulate dem vid klustret skapas och hur tooconnect toothose kluster efteråt.

AAD gör det möjligt för organisationer (kallas även klienter) toomanage användaren åtkomst tooapplications, som är indelade i program med en webbaserad inloggning UI och program med en native client-upplevelse.

Ett Service Fabric-kluster erbjuder flera posten pekar tooits hanteringsfunktioner, inklusive hello webbaserade Service Fabric Explorer och Visual Studio. Därför kan skapa du två AAD program toocontrol åtkomst toohello klustret, ett webbprogram och en programspecifika.
Det rekommenderas att du använder AAD säkerhet tooauthenticate klienter och certifikat för nod till nod säkerhet för Azure-kluster.

För fristående Windows Server-kluster rekommenderas det att du använder Windows-säkerhet med hanterade konton (GMA) om du har Windows Server 2012 R2 och Active Directory. Annars fortfarande använda Windows-säkerhet med Windows-konton.

## <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Övervaknings- och diagnostikfunktionerna för Azure Service Fabric
[Övervaknings- och diagnostikfunktionerna](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) är kritiska toodeveloping, testa och distribuera program och tjänster i en miljö. Service Fabric-lösningar som fungerar bäst när du planerar och implementerar övervakning och diagnostik för att kontrollera program och tjänster fungerar som förväntat i en miljö för lokal utveckling eller i produktion.

Hej huvudsakliga målen med övervakning från ett säkerhetsperspektiv och diagnostik är att:

-   Identifiera och diagnostisera problem med maskinvara och infrastruktur som kan bero på att tooa säkerhetshändelse.
-   Identifiera problem med programvara och appar som kan tillhandahålla indikator för kompromettering (IoC).
-   Förstå resource förbrukning toohelp förhindra oavsiktlig DOS-attacker.

Hej totala arbetsflödet för övervakning och diagnostik består av tre steg:

-   **Händelsegenerering:** Detta innefattar händelser (loggar, spårningar, anpassade händelser) på både hello-infrastruktur (kluster) och ett program / service. Läs mer om [infrastrukturhändelser](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) och [nivå programhändelser](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) toounderstand vilken finns och hur tooadd ytterligare instrumentation.
-   **Händelsen aggregering:** genererade händelser måste toobe samlas in och sammanställs innan de kan visas. Vi rekommenderar vanligtvis använder [Azure-diagnostik](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (mer liknande tooagent-baserade Logginsamling) eller [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (pågående Logginsamling).
-   **Analys:** händelser måste toobe visualiserade och är tillgänglig i vissa format, tooallow för analys och visa efter behov. Det finns flera bra plattformar som finns på marknaden hello när det gäller toohello analys och visualisering av data för övervakning och diagnostik. hello två som vi rekommenderar är [OMS](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) och [Programinsikter](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) på grund av tootheir bättre integrering med Service Fabric.

Du kan också använda [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) toomonitor många av hello Azure-resurser som en Service Fabric-klustret har skapats.

En watchdog är en separat tjänst som kan titta på hälsa och läsa in över tjänster och rapporten hälsa för någonting i hello hälsa modellen hierarki. Detta kan hjälpa att förhindra fel som inte skulle kunna identifieras baserat på hello vy av en enskild tjänst. Watchdogs är också en bra toohost kod som utför vidtar åtgärder utan interaktion från användaren (t.ex, rensa loggfiler i lagring vid vissa intervall). Du hittar en implementering av exemplet watchdog [här](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-using-certificates"></a>Skydda med hjälp av certifikat
Använda certifikat, visar den hur toosecure hello kommunikation mellan hello olika noder för fristående Windows-kluster och om hur tooauthenticate anslutande klienter toothis kluster med X.509-certifikat. Detta säkerställer att endast auktoriserade användare kommer åt hello klustret hello distribuerade program och utföra administrativa uppgifter. Certifikatsäkerhet ska aktiveras på hello klustret när hello klustret har skapats.

### <a name="x509-certificates-and-service-fabric"></a>X.509-certifikat och Service Fabric
X.509 digitala certifikat används ofta tooauthenticate klienter och servrar och tooencrypt och signera meddelanden.

hello visar följande tabell hello certifikat som du behöver på din konfiguration:

|Inställning för certifikat |Beskrivning|
|-------------------------------|-----------|
|ClusterCertificate|    Det här certifikatet är obligatoriska toosecure hello kommunikation mellan hello noder i ett kluster. Du kan använda två olika certifikat, en primär och sekundär för uppgradering.|
|ServerCertificate| Det här certifikatet visas toohello klienten när den försöker tooconnect toothis klustret. Du kan använda två olika servercertifikat, en primär och sekundär för uppgradering.|
|ClientCertificateThumbprints|  Det här är en uppsättning certifikat som du vill tooinstall på hello autentiserade klienter.|
|ClientCertificateCommonNames|  Ange hello nätverksnamnet för hello första klientcertifikatet för hello CertificateCommonName. Hej CertificateIssuerThumbprint är hello tumavtrycket för hello utfärdaren av det här certifikatet.|
|ReverseProxyCertificate|   Detta är ett valfritt certifikat som kan anges om du vill toosecure din [omvänd Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Mer information om hur du skyddar certifikat, [Klicka här](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare. Två olika åtkomstkontroll typer stöds för klienter som ansluter tooa kluster: administratörsrollen och användarrollen.

Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.

Du anger Hej administratör och klienten användarroller Hej när klustret skapas med olika identiteter (certifikat, AAD etc.) för varje. Mer information om inställningar för åtkomstkontroll för hello standard och hur toochange hello standardinställningar finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-using-windows-security"></a>Säker fristående kluster med hjälp av Windows-säkerhet
tooprevent obehörig åtkomst tooa Service Fabric-kluster, måste du skydda hello klustret. Säkerhet är särskilt viktigt när hello klustret körs produktionsarbetsbelastningar. Beskriver hur säkerhet för tooconfigure nod till nod och klient-till-nod med hjälp av Windows-säkerhet i hello ClusterConfig.JSON fil.

**Konfigurera Windows-säkerhet som använder gMSA**

Noden toonode säkerhet konfigureras genom att ange [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) när service fabric måste toorun under gMSA. I ordning toobuild förtroenderelationer mellan noder, måste de göras medveten om varandra.

Toonode klientsäkerhet konfigureras med hjälp av ClientIdentities. I ordning tooestablish förtroende mellan klienten och hello-kluster, måste du konfigurera hello klustret tooknow som klienten identiteter som den kan lita på.

**Konfigurera Windows-säkerhet med hjälp av en grupp datorer**

Noden toonode säkerheten är konfigurerad med inställningen med ClusterIdentity om du vill toouse en grupp datorer inom en Active Directory-domän. Mer information finns i [skapar en datorgruppen i Active Directory](https://msdn.microsoft.com/library/aa545347).

Klient-till-nod-säkerheten är konfigurerad med hjälp av ClientIdentities. tooestablish förtroende mellan klienten och hello kluster, måste du konfigurera hello klustret tooknow hello klient kan lita på identiteter som hello klustret. Du kan upprätta förtroende på två olika sätt:

-   Ange hello gruppen Domänanvändare som kan ansluta.
-   Ange hello nod domänanvändare som kan ansluta.

## <a name="configure-application-security-in-service-fabric"></a>Konfigurera programsäkerhet i Service Fabric
### <a name="managing-secrets-in-service-fabric-applications"></a>Hantera hemligheter i Service Fabric-program
Den här metoden kan hantera hemligheter i ett Service Fabric-program. Hemligheter kan vara känslig information, till exempel storage-anslutningssträngar, lösenord eller andra värden som inte ska hanteras i oformaterad text.

Den här metoden använder [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) toomanage nycklar och hemligheter. Använder hemligheter i ett program är dock molnet plattformsoberoende tooallow program distribueras toobe tooa klustret finns någonstans. Det finns fyra steg i det här flödet:

-   Skaffa ett certifikat för chiffrering av data.
-   Installera hello certifikat i klustret.
-   Kryptera hemliga värden när du distribuerar ett program med hello certifikat och mata in dem i en tjänst Settings.xml konfigurationsfilen.
-   Läs krypterade värden utanför Settings.xml genom att dekryptera med hello samma chiffrering av certifikat.

>[!Note]
>Lär dig mer om [hantera hemligheter i Service Fabric program](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Konfigurera säkerhetsprinciper för ditt program
Med hjälp av Azure Service Fabric-säkerhet kan du säkra program som körs i hello kluster under olika användarkonton. Service Fabric Security kan också säker hello-resurser som används av program när hello distribution under hello användarkonton – till exempel, filer, kataloger och certifikat. Det gör att program som körs, även i en delad värdmiljö säkrare från varandra.
hello stegen innefattar:

-   Konfigurera hello princip för en startpunkt för installationen av tjänsten.
-   Starta PowerShell-kommandon från en startpunkt för installationen.
-   Använda omdirigering till konsolen för lokala felsökning.
-   Konfigurera en princip för servicepaket kod.
-   Tilldela en säkerhetsprincip åtkomst för HTTP och HTTPS-slutpunkter.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Säker kommunikation för tjänster i Azure Service Fabric-säkerhet
Säkerhet är hello mest viktiga aspekter av kommunikation. hello Reliable Services-programmet innehåller några fördefinierade kommunikation stackar och verktyg som kan använda tooimprove säkerhet.

-   [Skydda en tjänst när du använder tjänsten fjärrkommunikation](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).
-   [Skydda en tjänst när du använder en WCF-baserad kommunikation stack](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack).

## <a name="next-steps"></a>Nästa steg
- Konceptuell information om säkerhet i klustret finns [skapar ett kluster i Azure med hjälp av en Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) och [Azure-portalen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Lär dig mer om, se [Service Fabric-Klustersäkerhet](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
