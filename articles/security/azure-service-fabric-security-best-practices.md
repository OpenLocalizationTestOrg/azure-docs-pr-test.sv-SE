---
title: "aaaAzure Service Fabric säkerhetsmetoder | Microsoft Docs"
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
ms.openlocfilehash: 483a21240da17d56bb4641653093ddcbad379d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric-säkerhetsmetoder
Distribuera ett program på Azure är snabb, enkel och kostnadseffektiv. Innan du distribuerar cloud program i produktion användbara toohave en god rutin tooassist vid utvärdering av programmet mot en lista över viktiga och rekommenderade metodtips.

Azure Service Fabric är en plattform för distribuerade system som gör det enkelt toopackage, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Service Fabric löser också hello betydande problem för utveckling och hantering av molnprogram. Utvecklare och administratörer kan undvika komplexa infrastrukturproblem och fokusera på att implementera verksamhetskritiska, krävande arbetsbelastningar som är skalbara, tillförlitliga och hanterbara. 

För varje bästa praxis förklarar vi:

-   Vilka hello bra
-   Varför du vill tooenable som bästa praxis
-   Vad kan vara hello resultat om du inte bästa praxis för tooenable hello
-   Hur du kan lära dig tooenable hello bästa praxis

Vi har för närvarande följande säkerhetsmetoder för Azure Service Fabric hello:

-   Använd Azure Resource Manager-mall och Service Fabric Azure PowerShell-modulen toocreate säker kluster
-   Använd X.509-certifikat
-   Konfigurera säkerhetsprinciper
-   Tillförlitliga aktörer säkerhetskonfiguration
-   Konfigurera SSL för Azure Service Fabric
-   Isolerade/nätverkssäkerhet med Azure Service Fabric
-   Ställ in nyckelvalv för säkerhet
-   Tilldela användare tooroles


## <a name="best-practices-for-securing-your-cluster"></a>Metodtips för att skydda klustret

**Stor bild**

Använd alltid en säker kluster
-   Klustret säkerhet – använder certifikat
-   Klientåtkomst (Admin och skrivskyddad) – Använd AAD

Använd automatiserad distribution
-   Använda skript toogenerate, distribuera och rulla över hemligheter
-   Tänk KV hello hemligheter, använda AD för andra klientåtkomst
-   Inga mänskliga ska ha åtkomst toothem utan autentisering.

Tänk dessutom hello följande:
-   Skapa DMZs med Nätverkssäkerhetsgrupper (NSG: er)
-   Använd hopp servrar tooRDP till virtuella datorer i klustret eller toomanage klustret

Kluster måste vara skyddad tooprevent obehöriga användare från att ansluta tooyour kluster, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt toocreate ett oskyddat kluster på så sätt kan anonyma användare tooconnect tooit, om det visar management slutpunkter toohello offentliga Internet.

Tekniker som används tooimplement dessa scenarier. Hej [kluster säkerhetsscenarier](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) är:

-   Nod till nod säkerhet – denna skyddar kommunikationen mellan hello virtuella datorer och datorer i hello-klustret. Detta säkerställer att endast datorer som är auktoriserade toojoin hello klustret kan delta i värd för program och tjänster i hello kluster.
Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) eller [Windows-säkerhet](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) för Windows Server-datorer.
-   Klient-till-nod säkerhet – denna skyddar kommunikationen mellan en klient för Service Fabric och enskilda noder i klustret hello.
-   Rollbaserad åtkomstkontroll (RBAC) – du anger Hej administratör och klienten användarroller Hej när klustret skapas med olika identiteter (certifikat, AAD etc.) för varje.
-   Säkerhetsrekommendationer-för Azure-kluster, rekommenderas att du använder AAD säkerhet tooauthenticate klienter och certifikat för nod till nod säkerhet.

tooconfigure hello fristående Windows-kluster, se [konfigurera inställningar för windows-kluster för fristående](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Använda mallar för Azure Resource Manager och Service Fabric Azure PowerShell-modulen toocreate säker klustret.
En stegvis guide får du hjälp med att skapa en säker Azure Service Fabric-kluster i Azure med Azure Resource Manager finns [här](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Använd hello Azure Resource Manager-mall toocustomize klustret
-   Installationsprogrammet hanteras lagringsutrymme för VM virtuella hårddiskar

Använd hello Azure Resource Manager-mall toodrive ändringar tooyour resursgruppen.
-   Enkelt konfigurationshantering
-   Granskning

Hantera klusterkonfigurationen som kod
-   Att ha planerat noggrant i kontrollerar hello-konfigurationer som du väljer toodeploy
-   Undvik att använda implicita kommandon tootweak dina resurser direkt

Många aspekter av hello [Service Fabric application livscykel](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) kan automatiseras. [Service Fabric Azure PowerShell-modulen](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatiserar vanliga uppgifter för att distribuera, uppgradera, ta bort och testa Azure Service Fabric-program. Hanteras och HTTP-APIs för apphantering finns också tillgängliga.

## <a name="use-x509-certificates"></a>Använd X.509-certifikat
Kluster bör alltid säkras med X.509-certifikat eller Windows-säkerhet. Säkerheten är bara konfigurerad vid tidpunkten för skapandet av klustret och den är inte möjligt tooenable säkerhet hello klustret har skapats.

Om du anger en [klustret certifikat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), ange ClusterCredentialType tooX509 hello-värde. Ange hello ServerCredentialType tooX509 om du anger servercertifikat för externa anslutningar.

-   Certifikat som används i kluster som kör produktionsarbetsbelastningar ska skapas med hjälp av en korrekt konfigurerade tjänsten för Windows Server-certifikat eller erhålls från en godkänd certifikatutfärdare (CA).
-   Använd aldrig några tillfälligt eller testa certifikat i produktion som skapas med verktyg som MakeCert.exe.
-   Du kan använda ett självsignerat certifikat, men det bör bara göra det för testkluster och inte i produktion.

Om hello kluster är oskyddat. Vem som helst kan ansluta anonymt och utföra hanteringsåtgärder, så produktionskluster bör alltid skyddas med X.509-certifikat eller Windows-säkerhet.

toolearn mer hur tooenable certifikat i service fabric-kluster finns [lägga till eller ta bort certifikat för service fabric-kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Konfigurera säkerhetsprinciper
Service Fabric hjälper också till att säkra hello-resurser som används av program när hello distribution under hello användarkonton – till exempel, filer, kataloger och certifikat. Det gör att program som körs, även i en delad värdmiljö säkrare från varandra.

-   Använda en Active Directory-domän, grupp eller användare: du kan köra hello tjänsten hello autentiseringsuppgifterna för ett Active Directory-användare eller grupp.. Detta är Active Directory lokalt i din domän och är inte med Azure Active Directory (AD Azure). Genom att använda en domänanvändare eller grupp kan du sedan komma åt andra resurser i hello domän (till exempel filresurser) som har behörighet.

-   Tilldela en säkerhetsprincip åtkomst för HTTP och HTTPS-slutpunkter: Om du använder en RunAs-princip tooa tjänst och hello tjänstmanifestet deklarerar endpoint resurser med hello HTTP-protokollet, måste du ange en SecurityAccessPolicy tooensure att portar allokeras toothese slutpunkter är korrekt åtkomst-kontrolleras för hello RunAs-konto som hello-tjänsten körs under. Annars http.sys har inte åtkomst toohello tjänsten och du får fel med anrop från hello-klient.
toolearn mer aktivera säkerhetsprinciper i service fabric finns [konfigurera säkerhetsprinciper för ditt program](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="reliable-actors-security-configuration"></a>Tillförlitliga aktörer säkerhetskonfiguration
Service Fabric Reliable Actors är en implementering av hello aktören designmönstret. Precis som med alla programvara designmönstret passar hello beslut om görs toouse ett specifikt mönster baserat på om huruvida en programvara utforma problemet hello mönster.

Som en allmän vägledning Tänk hello aktören mönster toomodel ditt problem eller scenario om:
-   Sidan problem innebär att ett stort antal (tusentalsavgränsare eller mer) liten, oberoende och isolerade enheter av tillstånd och logik.
-   Vill du toowork med Enkeltrådig objekt som inte kräver betydande interaktion från externa komponenter, inklusive frågor tillstånd på en uppsättning aktörer.
-   Aktören-instanser blockera inte anropare med oväntade fördröjningar genom att utfärda o-åtgärder.

I Service Fabric aktörer implementeras i hello Reliable Actors framework: ett programramverk för aktören-mönster-baserade byggt ovanpå [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Varje tillförlitliga aktören-tjänst som du skriver är faktiskt en partitionerad tillståndskänslig tillförlitlig tjänst.
Varje aktören har definierats som en instans av en aktörstyp av, identiska toohello sätt en .NET-objekt är en instans av en .NET-typen. Till exempel kan det finnas en aktörstyp som implementerar hello funktionerna i en kalkylator och det kan finnas flera aktörer av den typen som distribueras på olika noder i ett kluster. Varje sådan aktören identifieras unikt genom ett aktören-ID.

[Replikatorn säkerhetskonfigurationer](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) är används toosecure hello kommunikationskanalen som används vid replikering. Det innebär att tjänster inte kan se varandras replikeringstrafik, säkerställer att hello data som görs högtillgänglig också säker. Som standard förhindrar en tom säkerhetskonfigurationsavsnittet replikeringssäkerhet.
Konfigurationer för replikatorn konfigurera hello replikatorn som ansvarar för att göra hello aktören Tillståndsprovidern tillstånd hög tillförlitlig.

## <a name="configure-ssl-for-azure-service-fabric"></a>Konfigurera SSL för Azure Service Fabric

Serverautentisering: [verifierar](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) hello cluster management slutpunkter tooa management-klienten så att hello Hanteringsklient vet pratar toohello verkliga klustret. Det här certifikatet ger också en [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) för hello HTTPS hanterings-API och för Service Fabric Explorer via HTTPS.
Du måste skaffa ett anpassat domännamn för klustret. När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.

tooconfigure SSL för ett program måste du först tooget ett SSL-certifikat som har signerats av en certifikatet (CA), en betrodd tredje part som utfärdar certifikat för detta ändamål. Om du inte redan har en, måste tooobtain något på ett företag som säljer SSL-certifikat.

hello certifikat måste uppfylla följande krav för SSL-certifikat i Azure hello:
-   hello certifikatet måste innehålla en privat nyckel.
-   hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.
-   hello måste certifikatets ämnesnamn matcha hello domän används tooaccess hello-Molntjänsten. Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello cloudapp.net domän. Du måste skaffa en anpassad domän namnet toouse när komma åt tjänsten. När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domänen namnet som används för tooaccess ditt program. Till exempel om ditt domännamn är contoso.com du vill begära ett certifikat från Certifikatutfärdaren för **. contoso.com** eller **www.contoso.com.**
-   hello certifikat måste använda minst 2048-bitars kryptering.

HTTP är osäker och ämne tooeavesdropping attacker eftersom hello data som överförs från hello webbläsare toohello web webbserver eller mellan slutpunkter, skickas i klartext. Det innebär att angripare kan komma åt och visa känsliga data, till exempel kreditkortsinformation och inloggningar på kontot. När data skickas eller anslås via en webbläsare med hjälp av HTTPS, säkerställer SSL att sådan information är krypterad och säker obehöriga.

Det finns fler toolearn, [Konfigurera SSL för azure-program](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="network-isolationsecurity-with-azure-service-fabric"></a>Isolerade/nätverkssäkerhet med Azure Service Fabric
Använd [Azure Resource Manager (ARM) mallen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) som ett exempel för att ställa in tre nodetype säker klustret och toocontrol hello inkommande och utgående nätverkstrafik med Nätverkssäkerhetsgrupper.

hello-mallen innehåller en Nätverkssäkerhetsgrupp för varje hello skala set(VMSS) toocontrol hello trafik för virtuella datorer till och från hello VMSS. Som standard anges hello regler tooallow alla hello trafik som behövs av hello system tjänster och hello programmet portar som angetts i hello mallen. Granska de här reglerna och gör ändringar toofit dina behov, inklusive lägga till nya för dina program.

Mer information finns i [Azure Service Fabric – vanliga scenarier för nätverk](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-a-key-vault-for-security"></a>Ställ in nyckelvalv för säkerhet
Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program.

Service Fabric använder X.509-certifikat toosecure ett kluster och säkerhetsfunktioner för programmet. Du kan använda Key Vault för[hantera certifikat](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) för Service Fabric-kluster i Azure. När ett kluster distribueras i Azure hämtar certifikat från Nyckelvalvet hello Azure-resursprovider som ansvarar för att skapa Service Fabric-kluster och installerar dem på hello klustrets virtuella datorer.

Hej förhållandet mellan [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), service fabric-kluster och hello Azure-resursprovider som använder certifikat som lagras i en nyckelvalvet när den skapar ett kluster.

**Skapa en resursgrupp** hello första steget är toocreate en resursgrupp för nyckelvalvet. Vi rekommenderar att du placerar hello nyckelvalv i sin egen resursgrupp. Den här åtgärden kan du ta bort hello beräkning och lagring resursgrupper, inklusive hello resursgruppen som innehåller Service Fabric-klustret utan att förlora dina nycklar och hemligheter. hello resursgruppen som innehåller ditt nyckelvalv måste vara i hello samma region som hello-kluster som använder den.

**Skapa en nyckelvalvet i hello ny resursgrupp** hello nyckelvalv måste vara aktiverat för distribution tooallow hello beräkning resource provider tooget certifikat från den och installera det på instanser för virtuella datorer.
toolearn mer hur tooset upp Azure key vault finns [Kom igång med Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-roles"></a>Tilldela användarroller
När du har skapat hello program toorepresent klustret, tilldela dina användare toohello roller som stöds av Service Fabric: skrivskyddade och administratör. Du kan tilldela hello roller med hjälp av hello klassiska Azure-portalen.

>[!Note]
> Mer information om roller i Service Fabric finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa [Service Fabric-kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): administratörs- och. Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.

## <a name="next-steps"></a>Nästa steg
- Konfigurera Service Fabric [utvecklingsmiljö](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Lär dig mer om [Service Fabric supportalternativ](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

