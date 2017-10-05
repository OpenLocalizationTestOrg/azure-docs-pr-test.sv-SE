---
title: "Översikt över säkerheten i Azure service fabric | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över Azure service fabric-säkerhet."
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
ms.openlocfilehash: 4cbd2791649c6d2dd005521cedb44c17aa874073
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-service-fabric-security-overview"></a>Översikt över säkerheten i Azure Service Fabric
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) är en plattform för distribuerade system som gör det enkelt att paketera, distribuera och hantera skalbara och tillförlitliga micro-tjänster. Service Fabric-adresser betydande utmaningarna med att utveckla och hantera molnprogram. Utvecklare och administratörer kan undvika komplexa infrastrukturproblem och fokusera på att implementera verksamhetskritiska, krävande arbetsbelastningar som är skalbara, tillförlitliga och hanterbara.

Översikt över Azure Service Fabric säkerhet artikeln fokuserar på följande områden:

-   Skydda ditt kluster
-   Övervakning och diagnostik
-   Skydda med hjälp av certifikat
-   Rollbaserad åtkomstkontroll (RBAC)
-   Skydda kluster med hjälp av Windows-säkerhet
-   Konfigurera programsäkerhet i Service Fabric
-   Säker kommunikation för tjänster i Azure Service Fabric-säkerhet

## <a name="securing-your-cluster"></a>Skydda ditt kluster
Azure Service Fabric är en orchestrator av tjänster i ett kluster på datorer, kluster måste skyddas för att förhindra att obehöriga användare från att ansluta till klustret, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt att skapa ett oskyddat kluster, blir gör det anonyma användare att ansluta till den, om det visar hanteringsslutpunkter till Internet.

Det här avsnittet innehåller en översikt över säkerhetsscenarier för kluster som körs på Azure eller fristående och de olika tekniker som används för att implementera de scenarierna. Säkerhetsscenarier för klustret är:

-   Säkerhet för nod till nod
-   Klient-till-nod-säkerhet

### <a name="node-to-node-security"></a>Säkerhet för nod till nod
Skyddar kommunikationen mellan virtuella datorer eller datorer i klustret. Detta säkerställer att endast datorer som har behörighet att ansluta till klustret kan delta i värd för program och tjänster i klustret.

Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://msdn.microsoft.com/library/ff649801.aspx) eller [Windows-säkerhet](https://msdn.microsoft.com/library/ff649396.aspx) för Windows Server-datorer.

**Nod till nod Certifikatsäkerhet**

Service Fabric använder X.509-servercertifikat som du anger som en del av konfigurationer för nodtypen när du skapar ett kluster. En snabb överblick över dessa certifikat är och [hur du kan hämta eller skapa dem finns i den här artikeln](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Certifikatsäkerhet konfigureras när du skapar klustret Azure-portalen, Azure Resource Manager-mallar eller en fristående JSON-mall. Du kan ange ett certifikat för primär och en valfri sekundärt certifikat som används för certifikatet överrullningar. De primära och sekundära certifikat som du anger bör vara ett annat än administratörsklient och skrivskyddad klientcertifikat som du anger för [klient-till-nod säkerhet](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Klient-till-nod-säkerhet
Klient till noden säkerhet konfigureras med hjälp av klienten identiteter. Om du vill upprätta förtroende mellan en klient och klustret måste du konfigurera klustret så att du vet vilken klient identiteter som den kan lita på. Detta kan göras på två olika sätt:

-   Ange de gruppanvändare som kan ansluta eller
-   Ange nod domänanvändare som kan ansluta.

Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna till ett Service Fabric-kluster:

-   Administratör
-   Användare

Åtkomstkontroll gör möjligheten att begränsa åtkomsten till vissa typer av klusteråtgärder för olika grupper av användare, vilket gör att klustret säkrare Klusteradministratören. Administratörer har fullständig åtkomst till funktioner för hantering (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet till funktioner för hantering (till exempel frågefunktioner) och möjligheten att lösa program och tjänster.

**Klient-till-nod Certifikatsäkerhet**

Certifikatsäkerhet för klient-till-nod konfigureras när du skapar klustret antingen via Azure-portalen Resource Manager-mallar eller en fristående JSON-mall genom att ange ett klientcertifikat för admin och/eller ett klientcertifikat. Admin klient- och klientcertifikat du anger måste skilja sig från de primära och sekundära certifikat som du anger för nod till nod säkerhet.

Klienter som ansluter till klustret med certifikatet som administratören har fullständig åtkomst till hanteringsmöjligheter. Klienter som ansluter till klustret med klientcertifikatet skrivskyddade användaren har läsbehörighet till hanteringsfunktioner. Baserar åtkomstkontroll (RBAC) i andra word dessa certifikat används för rollen.

För Azure läsa [ställer in ett kluster med en Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) information om hur du konfigurerar Certifikatsäkerhet i ett kluster.

**Klient-till-nod Azure Active Directory (AAD) säkerheten på Azure**

Kluster som körs på Azure kan också säker åtkomst till management-slutpunkter som använder Azure Active Directory (AAD). Se [ställer in ett kluster med en Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) information om hur du skapar nödvändiga AAD-artefakter, hur du fylla i dem när klustret skapas och hur du ansluter till dessa kluster efteråt.

AAD kan organisationer (kallas även klienter) hantera användarnas åtkomst till program som är indelade i program med en webbaserad inloggning UI och program med en native client-upplevelse.

Ett Service Fabric-kluster erbjuder flera startpunkter till dess hanteringsfunktioner, inklusive webbaserade Service Fabric Explorer och Visual Studio. Därför kan skapa du två AAD-program för att styra åtkomst till klustret, ett webbprogram och en programspecifika.
Det rekommenderas att du använder AAD säkerhet för att autentisera klienter och certifikat för nod till nod säkerhet för Azure-kluster.

För fristående Windows Server-kluster rekommenderas det att du använder Windows-säkerhet med hanterade konton (GMA) om du har Windows Server 2012 R2 och Active Directory. Annars fortfarande använda Windows-säkerhet med Windows-konton.

## <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Övervaknings- och diagnostikfunktionerna för Azure Service Fabric
[Övervaknings- och diagnostikfunktionerna](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) är viktigt att utveckla, testa och distribuera program och tjänster i en miljö. Service Fabric-lösningar som fungerar bäst när du planerar och implementerar övervakning och diagnostik för att kontrollera program och tjänster fungerar som förväntat i en miljö för lokal utveckling eller i produktion.

Från ett säkerhetsperspektiv är de huvudsakliga målen med övervakning och diagnostik att:

-   Identifiera och diagnostisera problem med maskinvara och infrastruktur som kan bero på att en säkerhetshändelse.
-   Identifiera problem med programvara och appar som kan tillhandahålla indikator för kompromettering (IoC).
-   Förstå resursanvändningen för att förhindra oavsiktlig DOS-attacker.

Det totala arbetsflödet för övervakning och diagnostik består av tre steg:

-   **Händelsegenerering:** Detta innefattar händelser (loggar, spårningar, anpassade händelser) på både infrastruktur (kluster) och ett program / service-nivå. Läs mer om [infrastrukturhändelser](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) och [nivå programhändelser](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) att förstå vad som tillhandahålls och hur du lägger till ytterligare instrumentation.
-   **Händelsen aggregering:** genererade händelser behöver samlas in och sammanställs innan de kan visas. Vi rekommenderar vanligtvis använder [Azure-diagnostik](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (mer liknar agent-baserade Logginsamling) eller [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (pågående Logginsamling).
-   **Analys:** händelser måste vara visualiserade och tillgänglig i vissa format för analys och visa efter behov. Det finns flera bra plattformar som finns på marknaden när det gäller analys och visualisering av data för övervakning och diagnostik. De två rekommenderar vi är [OMS](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) och [Programinsikter](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) på grund av deras bättre integrering med Service Fabric.

Du kan också använda [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) att övervaka många av de Azure-resurser som en Service Fabric-klustret har skapats.

En watchdog är en separat tjänst som kan titta på hälsa och läsa in över tjänster och rapporten hälsa för något i hierarkin hälsa modellen. Detta kan hjälpa att förhindra fel som inte skulle kunna identifieras baserat på vyn för en enskild tjänst. Watchdogs är också bra att värd-kod som utför vidtar åtgärder utan interaktion från användaren (t.ex, rensa loggfiler i lagring vid vissa intervall). Du hittar en implementering av exemplet watchdog [här](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-using-certificates"></a>Skydda med hjälp av certifikat
Använda certifikat, visar den hur för säker kommunikation mellan olika noder i fristående Windows-kluster samt hur att autentisera klienter som ansluter till det här klustret med X.509-certifikat. Detta säkerställer att endast auktoriserade användare kan komma åt klustret, distribuerade program och utföra administrativa uppgifter. Certifikatsäkerhet ska aktiveras på klustret när klustret skapas.

### <a name="x509-certificates-and-service-fabric"></a>X.509-certifikat och Service Fabric
Digitala X.509-certifikat används ofta för att autentisera klienter och servrar och för att kryptera och digitalt signera meddelanden.

I följande tabell visas de certifikat som du behöver på din konfiguration för klustret:

|Inställning för certifikat |Beskrivning|
|-------------------------------|-----------|
|ClusterCertificate|    Detta certifikat krävs för att skydda kommunikationen mellan noder i ett kluster. Du kan använda två olika certifikat, en primär och sekundär för uppgradering.|
|ServerCertificate| Det här certifikatet visas till klienten när den försöker ansluta till det här klustret. Du kan använda två olika servercertifikat, en primär och sekundär för uppgradering.|
|ClientCertificateThumbprints|  Det här är en uppsättning certifikat som du vill installera på de autentiserade klienterna.|
|ClientCertificateCommonNames|  Ange namnet på det första klientcertifikatet för CertificateCommonName. CertificateIssuerThumbprint är tumavtrycket för den här certifikatutfärdaren.|
|ReverseProxyCertificate|   Detta är ett valfritt certifikat som kan anges om du vill skydda din [omvänd Proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Mer information om hur du skyddar certifikat, [Klicka här](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Åtkomstkontroll kan Klusteradministratören att begränsa åtkomsten till vissa klusteråtgärder för olika grupper av användare, vilket gör att klustret säkrare. Två olika åtkomstkontroll typer stöds för klienter som ansluter till ett kluster: administratörsrollen och användarrollen.

Administratörer har fullständig åtkomst till funktioner för hantering (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet till funktioner för hantering (till exempel frågefunktioner) och möjligheten att lösa program och tjänster.

Du anger rollerna administratörs- och klienten när klustret har skapats med olika identiteter (certifikat, AAD etc.) för varje. Läs mer om inställningar för åtkomstkontroll standard och hur du ändrar standardinställningarna [rollbaserad åtkomstkontroll för Service Fabric-klienter](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-using-windows-security"></a>Säker fristående kluster med hjälp av Windows-säkerhet
För att förhindra obehörig åtkomst till ett Service Fabric-kluster, måste du skydda klustret. Säkerhet är särskilt viktigt när klustret körs produktionsarbetsbelastningar. Det beskriver hur du konfigurerar säkerhet nod till nod och klient-till-nod med hjälp av Windows-säkerhet i filen ClusterConfig.JSON.

**Konfigurera Windows-säkerhet som använder gMSA**

Nod till noden säkerhet konfigureras genom att ange [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) när service fabric måste köras under gMSA. För att skapa betrodda relationer mellan noder, måste de göras medveten om varandra.

Klient till noden säkerhet konfigureras med hjälp av ClientIdentities. För att upprätta förtroende mellan en klient och klustret måste du konfigurera klustret så att du vet vilken klient identiteter som den kan lita på.

**Konfigurera Windows-säkerhet med hjälp av en grupp datorer**

Nod till noden säkerhet har konfigurerats med inställningen med ClusterIdentity om du vill använda en grupp datorer inom en Active Directory-domän. Mer information finns i [skapar en datorgruppen i Active Directory](https://msdn.microsoft.com/library/aa545347).

Klient-till-nod-säkerheten är konfigurerad med hjälp av ClientIdentities. Om du vill upprätta förtroende mellan en klient och klustret måste du konfigurera klustret för att kunna veta klienten identiteter kan lita på klustret. Du kan upprätta förtroende på två olika sätt:

-   Ange de gruppanvändare som kan ansluta.
-   Ange nod domänanvändare som kan ansluta.

## <a name="configure-application-security-in-service-fabric"></a>Konfigurera programsäkerhet i Service Fabric
### <a name="managing-secrets-in-service-fabric-applications"></a>Hantera hemligheter i Service Fabric-program
Den här metoden kan hantera hemligheter i ett Service Fabric-program. Hemligheter kan vara känslig information, till exempel storage-anslutningssträngar, lösenord eller andra värden som inte ska hanteras i oformaterad text.

Den här metoden använder [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) att hantera nycklar och hemligheter. Med hjälp av hemligheter i ett program är dock moln-plattformsoberoende så att program som ska distribueras till ett kluster som värd var som helst. Det finns fyra steg i det här flödet:

-   Skaffa ett certifikat för chiffrering av data.
-   Installera certifikatet i klustret.
-   Kryptera hemliga värden när du distribuerar ett program med certifikatet och mata in dem i en tjänst Settings.xml konfigurationsfilen.
-   Läsa krypterade värden utanför Settings.xml genom att dekryptera med samma chiffrering av certifikat.

>[!Note]
>Lär dig mer om [hantera hemligheter i Service Fabric program](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Konfigurera säkerhetsprinciper för ditt program
Med hjälp av Azure Service Fabric-säkerhet kan du säkra program som körs i kluster under olika användarkonton. Service Fabric säkerhet hjälper också till att skydda resurser som används av program vid tidpunkten för distribution under användarkonton – till exempel, filer, kataloger och certifikat. Det gör att program som körs, även i en delad värdmiljö säkrare från varandra.
Stegen omfattar:

-   Konfigurera principen för en startpunkt för installationen av tjänsten.
-   Starta PowerShell-kommandon från en startpunkt för installationen.
-   Använda omdirigering till konsolen för lokala felsökning.
-   Konfigurera en princip för servicepaket kod.
-   Tilldela en säkerhetsprincip åtkomst för HTTP och HTTPS-slutpunkter.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Säker kommunikation för tjänster i Azure Service Fabric-säkerhet
Säkerhet är en av de viktigaste aspekterna av kommunikation. Application framework Reliable Services innehåller några fördefinierade kommunikation stackar och verktyg som kan användas för att förbättra säkerheten.

-   [Skydda en tjänst när du använder tjänsten fjärrkommunikation](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).
-   [Skydda en tjänst när du använder en WCF-baserad kommunikation stack](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack).

## <a name="next-steps"></a>Nästa steg
- Konceptuell information om säkerhet i klustret finns [skapar ett kluster i Azure med hjälp av en Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) och [Azure-portalen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Lär dig mer om, se [Service Fabric-Klustersäkerhet](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
