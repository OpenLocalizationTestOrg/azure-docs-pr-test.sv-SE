---
title: "Azure Service Fabric-säkerhetsmetoder | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning av bästa praxis för Azure Service Fabric-säkerhet."
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
ms.openlocfilehash: 3132e42aac72c0132c7526ac56d80bc5eec269e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric-säkerhetsmetoder
Distribuera ett program på Azure är snabb, enkel och kostnadseffektiv. Innan du distribuerar molnapp i produktion praktiskt att ha en bästa praxis för att hjälpa dig att utvärdera programmet mot en lista över viktiga och rekommenderade metodtips.

Azure Service Fabric är en distribuerad systemplattform som gör det enkelt att paketera, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Service Fabric tar också itu med betydande utmaningar vid utveckling och hantering av molnprogram. Utvecklare och administratörer kan undvika komplexa infrastrukturproblem och fokusera på att implementera verksamhetskritiska, krävande arbetsbelastningar som är skalbara, tillförlitliga och hanterbara. 

För varje bästa praxis förklarar vi:

-   Vad som är bästa praxis
-   Varför du vill aktivera den bästa praxis
-   Vad kan vara resultatet om du inte aktivera bästa praxis
-   Hur du kan lära dig att aktivera bästa praxis

Vi har för närvarande på följande Azure Service Fabric säkerhetsmetoder:

-   Använd mallen för Azure Resource Manager och Service Fabric Azure PowerShell-modulen för att skapa säkra kluster
-   Använd X.509-certifikat
-   Konfigurera säkerhetsprinciper
-   Tillförlitliga aktörer säkerhetskonfiguration
-   Konfigurera SSL för Azure Service Fabric
-   Isolerade/nätverkssäkerhet med Azure Service Fabric
-   Ställ in nyckelvalv för säkerhet
-   Tilldela användare till roller


## <a name="best-practices-for-securing-your-cluster"></a>Metodtips för att skydda klustret

**Stor bild**

Använd alltid en säker kluster
-   Klustret säkerhet – använder certifikat
-   Klientåtkomst (Admin och skrivskyddad) – Använd AAD

Använd automatiserad distribution
-   Använda skript för att skapa, distribuera och rulla över hemligheter
-   Håll hemligheterna i KV, använda AD för andra klientåtkomst
-   Inga mänskliga ska ha åtkomst till dem utan autentisering.

Dessutom, Tänk på följande:
-   Skapa DMZs med Nätverkssäkerhetsgrupper (NSG: er)
-   Använda hopp servrar till RDP till virtuella datorer i klustret eller för att hantera klustret

Kluster måste skyddas för att förhindra att obehöriga användare från att ansluta till klustret, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt att skapa ett oskyddat kluster, blir gör det anonyma användare att ansluta till den, om det visar hanteringsslutpunkter till Internet.

Tekniker som används för att implementera de scenarierna. Den [kluster säkerhetsscenarier](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) är:

-   Nod till nod säkerhet – denna skyddar kommunikationen mellan virtuella datorer och datorer i klustret. Detta säkerställer att endast datorer som har behörighet att ansluta till klustret kan delta i värd för program och tjänster i klustret.
Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) eller [Windows-säkerhet](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) för Windows Server-datorer.
-   Klient-till-nod säkerhet – denna skyddar kommunikationen mellan en klient för Service Fabric och enskilda noder i klustret.
-   Rollbaserad åtkomstkontroll (RBAC) – du anger rollerna administratörs- och klienten när klustret har skapats med olika identiteter (certifikat, AAD etc.) för varje.
-   Säkerhetsrekommendationer-för Azure-kluster, rekommenderas att du använder AAD säkerhet för att autentisera klienter och certifikat för nod till nod säkerhet.

För att konfigurera fristående Windows-kluster, se [konfigurera inställningar för windows-kluster för fristående](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Använd Azure Resource Manager-mallar och Service Fabric Azure PowerShell-modulen för att skapa säkra kluster.
En stegvis guide får du hjälp med att skapa en säker Azure Service Fabric-kluster i Azure med Azure Resource Manager finns [här](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Använd Azure Resource Manager-mall för att anpassa ditt kluster
-   Installationsprogrammet hanteras lagringsutrymme för VM virtuella hårddiskar

Använd Azure Resource Manager-mall för enheten ändringar i resursgruppen
-   Enkelt konfigurationshantering
-   Granskning

Hantera klusterkonfigurationen som kod
-   Att ha planerat noggrant i kontrollerar de konfigurationer som du väljer att distribuera
-   Undvik att använda implicita kommandon för att justera dina resurser direkt

Många aspekter av den [Service Fabric application livscykel](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) kan automatiseras. [Service Fabric Azure PowerShell-modulen](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatiserar vanliga uppgifter för att distribuera, uppgradera, ta bort och testa Azure Service Fabric-program. Hanteras och HTTP-APIs för apphantering finns också tillgängliga.

## <a name="use-x509-certificates"></a>Använd X.509-certifikat
Kluster bör alltid säkras med X.509-certifikat eller Windows-säkerhet. Säkerhetsfunktioner konfigureras bara när kluster skapas, och du kan inte aktivera någon säkerhet i efterhand.

Om du anger en [klustret certifikat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), ange värdet för ClusterCredentialType till X509. Om du anger servercertifikat för utanför anslutningar kan du ange ServerCredentialType till X509.

-   Certifikat som används i kluster som kör produktionsarbetsbelastningar ska skapas med hjälp av en korrekt konfigurerade tjänsten för Windows Server-certifikat eller erhålls från en godkänd certifikatutfärdare (CA).
-   Använd aldrig några tillfälligt eller testa certifikat i produktion som skapas med verktyg som MakeCert.exe.
-   Du kan använda ett självsignerat certifikat, men det bör bara göra det för testkluster och inte i produktion.

Om klustret är oskyddat. Vem som helst kan ansluta anonymt och utföra hanteringsåtgärder, så produktionskluster bör alltid skyddas med X.509-certifikat eller Windows-säkerhet.

Mer information hur du aktiverar certifikat i service fabric-kluster finns i avsnittet [lägga till eller ta bort certifikat för service fabric-kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Konfigurera säkerhetsprinciper
Service Fabric hjälper också till att skydda resurser som används av program vid tidpunkten för distribution under användarkonton – till exempel, filer, kataloger och certifikat. Det gör att program som körs, även i en delad värdmiljö säkrare från varandra.

-   Använda en Active Directory-domän, grupp eller användare: du kan köra tjänsten under autentiseringsuppgifterna för ett Active Directory-användare eller grupp konto. Detta är Active Directory lokalt i din domän och är inte med Azure Active Directory (AD Azure). Genom att använda en domänanvändare eller grupp kan du sedan komma åt andra resurser i domänen (till exempel filresurser) som har behörighet.

-   Tilldela en säkerhetsprincip åtkomst för HTTP och HTTPS-slutpunkter: Om du använder en RunAs-princip i en tjänst och service manifest deklarerar endpoint resurser med HTTP-protokollet, måste du ange en SecurityAccessPolicy för att säkerställa att portar tilldelas dessa slutpunkter är korrekt åtkomst-kontrollerade som anges för det RunAs-konto som tjänsten körs under. Annars http.sys har inte åtkomst till tjänsten och du får ett fel med anrop från klienten.
Läs mer aktivera säkerhetsprinciper i service fabric finns [konfigurera säkerhetsprinciper för ditt program](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="reliable-actors-security-configuration"></a>Tillförlitliga aktörer säkerhetskonfiguration
Service Fabric Reliable Actors är en implementering av aktören designmönstret. Precis som med alla programvara designmönstret passar beslut om du vill använda ett specifikt mönster görs baserat på om huruvida en programvara utforma problemet mönstret.

Överväg att aktören mönstret modellera ditt problem eller scenario om som en allmän vägledning:
-   Sidan problem innebär att ett stort antal (tusentalsavgränsare eller mer) liten, oberoende och isolerade enheter av tillstånd och logik.
-   Du vill arbeta med Enkeltrådig objekt som inte kräver betydande interaktion från externa komponenter, inklusive frågor tillstånd på en uppsättning aktörer.
-   Aktören-instanser blockera inte anropare med oväntade fördröjningar genom att utfärda o-åtgärder.

I Service Fabric aktörer implementeras i Reliable Actors framework: ett programramverk för aktören-mönster-baserade byggt ovanpå [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Varje tillförlitliga aktören-tjänst som du skriver är faktiskt en partitionerad tillståndskänslig tillförlitlig tjänst.
Varje aktören definieras som en instans av aktörstyp, samma sätt som ett .NET-objekt är en instans av en .NET-typ. Till exempel kan det finnas en aktörstyp som implementerar funktionerna i en kalkylator och det kan finnas flera aktörer av den typen som distribueras på olika noder i ett kluster. Varje sådan aktören identifieras unikt genom ett aktören-ID.

[Replikatorn säkerhetskonfigurationer](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) används för att skydda kommunikationskanalen som används vid replikering. Det innebär att tjänster inte kan se varandras replikeringstrafik, säkerställer att de data som har gjorts hög tillgänglighet är säker. Som standard förhindrar en tom säkerhetskonfigurationsavsnittet replikeringssäkerhet.
Konfigurationer för replikatorn konfigurera replikatorn som ansvarar för att göra tillståndet aktören Tillståndsprovidern hög tillförlitlig.

## <a name="configure-ssl-for-azure-service-fabric"></a>Konfigurera SSL för Azure Service Fabric

Serverautentisering: [verifierar](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) att klustret management slutpunkter management-klienten så att klienten management vet pratar till verkliga klustret. Det här certifikatet ger också en [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) för HTTPS-hanterings-API och för Service Fabric Explorer via HTTPS.
Du måste skaffa ett anpassat domännamn för klustret. När du begär ett certifikat från en Certifikatutfärdare måste certifikatets ämnesnamn matcha det anpassade domännamnet som du använder för klustret.

Om du vill konfigurera SSL för ett program måste du först skaffa ett SSL-certifikat som har signerats av en certifikatet (CA), en betrodd tredje part som utfärdar certifikat för detta ändamål. Om du inte redan har en, måste du skaffa ett från ett företag som säljer SSL-certifikat.

Certifikatet måste uppfylla följande krav för SSL-certifikat i Azure:
-   Certifikatet måste innehålla en privat nyckel.
-   Certifikatet måste skapas för nyckelutbyte, kan exporteras till en Personal Information Exchange (.pfx)-fil.
-   Certifikatets ämnesnamn måste matcha den domän som används för åtkomst till Molntjänsten. Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för domänen cloudapp.net. Du måste skaffa ett anpassat domännamn som ska användas vid åtkomst till din tjänst. När du begär ett certifikat från en Certifikatutfärdare måste certifikatets ämnesnamn matcha det anpassade domännamnet som används för åtkomst till ditt program. Till exempel om ditt domännamn är contoso.com du vill begära ett certifikat från Certifikatutfärdaren för **. contoso.com** eller **www.contoso.com.**
-   Certifikatet måste använda minst 2048-bitars kryptering.

HTTP regleras avslyssna informationen eftersom de data som överförs från webbläsaren till webbservern eller mellan slutpunkter, skickas i klartext är osäker. Det innebär att angripare kan komma åt och visa känsliga data, till exempel kreditkortsinformation och inloggningar på kontot. När data skickas eller anslås via en webbläsare med hjälp av HTTPS, säkerställer SSL att sådan information är krypterad och säker obehöriga.

Mer information finns i, [Konfigurera SSL för azure-program](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="network-isolationsecurity-with-azure-service-fabric"></a>Isolerade/nätverkssäkerhet med Azure Service Fabric
Använd [Azure Resource Manager (ARM) mallen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) som ett exempel för att genomföra en säker tre nodetype-klustret och för att kontrollera inkommande och utgående nätverkstrafik som har med Nätverkssäkerhetsgrupper.

Mallen har en Nätverkssäkerhetsgrupp för varje virtuell dator skala set(VMSS) till styr trafiken till och från VMSS. Som standard är reglerna inställda så att all trafik som behövs av systemtjänsterna och program-portar som anges i mallen. Granska de här reglerna och göra ändringar så att de passar dina behov, inklusive lägga till nya för dina program.

Mer information finns i [Azure Service Fabric – vanliga scenarier för nätverk](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-a-key-vault-for-security"></a>Ställ in nyckelvalv för säkerhet
Certifikat används i Service Fabric till att autentisera och kryptera olika delar av ett kluster och de program som körs där.

Service Fabric använder X.509-certifikat för att skydda ett kluster och säkerhetsfunktioner för programmet. Du kan använda Nyckelvalv till [hantera certifikat](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) för Service Fabric-kluster i Azure. När ett kluster distribueras i Azure, Azure-resursprovider som ansvarar för att skapa Service Fabric-kluster hämtar certifikat från Nyckelvalvet och installerar dem på klustret virtuella datorer.

Relationen mellan [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), service fabric-kluster och Azure-resursprovider som använder certifikat som lagras i en nyckelvalvet när den skapar ett kluster.

**Skapa en resursgrupp** det första steget är att skapa en resursgrupp för nyckelvalvet. Vi rekommenderar att du placera nyckelvalvet i sin egen resursgruppen. Den här åtgärden kan du ta bort beräkning och lagring resursgrupper, inklusive resursgruppen som innehåller Service Fabric-klustret utan att förlora dina nycklar och hemligheter. Resursgruppen som innehåller ditt nyckelvalv måste vara i samma region som det kluster som använder den.

**Skapa en nyckelvalvet i den nya resursgruppen** nyckelvalvet måste vara aktiverat för distribution för att tillåta compute-resursprovidern så att hämta certifikat från det och installera det på instanser för virtuella datorer.
Mer information hur du ställer in Azure key vault finns [Kom igång med Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-roles"></a>Tilldela användarroller
När du har skapat de program som ska representera klustret tilldelar användarna till de roller som stöds av Service Fabric: skrivskyddade och administratör. Du kan tilldela roller med hjälp av den klassiska Azure-portalen.

>[!Note]
> Mer information om roller i Service Fabric finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna till en [Service Fabric-kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): administratörs- och. Åtkomstkontroll kan Klusteradministratören att begränsa åtkomsten till vissa klusteråtgärder för olika grupper av användare, vilket gör att klustret säkrare.

## <a name="next-steps"></a>Nästa steg
- Konfigurera Service Fabric [utvecklingsmiljö](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Lär dig mer om [Service Fabric supportalternativ](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

