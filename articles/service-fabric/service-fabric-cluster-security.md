---
title: Skydda ett Service Fabric-kluster | Microsoft Docs
description: "Beskriver säkerhetsscenarier för Service Fabric-klustret och de olika tekniker som används för att implementera de scenarierna."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: chackdan
ms.openlocfilehash: 5afbe575a8affc37b8f902c0988585a83921e3d2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>Säkerhetsscenarier för Service Fabric-kluster
Ett Service Fabric-kluster är en resurs som du äger. Kluster måste skyddas för att förhindra att obehöriga användare från att ansluta till klustret, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt att skapa ett oskyddat kluster, blir gör det anonyma användare att ansluta till den, om det visar hanteringsslutpunkter till Internet. 

Den här artikeln innehåller en översikt över säkerhetsscenarier för kluster som körs på Azure eller fristående och de olika tekniker som används för att implementera de scenarierna. Säkerhetsscenarier för klustret är:

* Säkerhet för nod till nod
* Klient-till-nod-säkerhet
* Rollbaserad åtkomstkontroll (RBAC)

## <a name="node-to-node-security"></a>Säkerhet för nod till nod
Skyddar kommunikationen mellan virtuella datorer eller datorer i klustret. Detta säkerställer att endast datorer som har behörighet att ansluta till klustret kan delta i värd för program och tjänster i klustret.

![Diagram över nod till nod-kommunikation][Node-to-Node]

Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://msdn.microsoft.com/library/ff649801.aspx) eller [Windows-säkerhet](https://msdn.microsoft.com/library/ff649396.aspx) för Windows Server-datorer.

### <a name="node-to-node-certificate-security"></a>Nod till nod Certifikatsäkerhet
Service Fabric använder X.509-servercertifikat som du anger som en del av konfigurationer för nodtypen när du skapar ett kluster. En snabb överblick över dessa certifikat är och hur du kan hämta eller skapa dem tillhandahålls i slutet av den här artikeln.

Certifikatsäkerhet konfigureras när du skapar klustret Azure-portalen, Azure Resource Manager-mallar eller en fristående JSON-mall. Du kan ange ett certifikat för primär och en valfri sekundärt certifikat som används för certifikatet överrullningar. De primära och sekundära certifikat som du anger bör vara ett annat än administratörsklient och skrivskyddad klientcertifikat som du anger för [klient-till-nod säkerhet](#client-to-node-security).

För Azure läsa [ställer in ett kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) information om hur du konfigurerar Certifikatsäkerhet i ett kluster.

För fristående Windows Server läsa [skydda ett fristående kluster på Windows med X.509-certifikat](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Nod till nod windows-säkerhet
För fristående Windows Server läsa [skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Klient-till-nod-säkerhet
Klienter och skyddar kommunikationen mellan en klient och enskilda noder i klustret. Den här typen av säkerhet som autentiserar och skyddar klientkommunikation, vilket garanterar att endast behöriga användare har åtkomst till klustret och program som har distribuerats på klustret. Klienter kan identifieras unikt genom sina autentiseringsuppgifter för Windows-säkerhet eller säkerhetsreferenser sina certifikat.

![Diagram över kommunikation från klient till nod][Client-to-Node]

Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://msdn.microsoft.com/library/ff649801.aspx) eller [Windows-säkerhet](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Klient-till-nod Certifikatsäkerhet
 Certifikatsäkerhet för klient-till-nod konfigureras när du skapar klustret antingen via Azure-portalen Resource Manager-mallar eller en fristående JSON-mall genom att ange ett klientcertifikat för admin och/eller ett klientcertifikat.  Admin klient- och klientcertifikat du anger måste skilja sig från de primära och sekundära certifikat som du anger för [nod till nod säkerhet](#node-to-node-security) som bästa praxis. Som standard läggs klustercertifikat för nod till nod säkerhet till i listan över tillåtna klienter Admin certifikat.

Klienter som ansluter till klustret med certifikatet som administratören har fullständig åtkomst till hanteringsmöjligheter.  Klienter som ansluter till klustret med klientcertifikatet skrivskyddade användaren har läsbehörighet till hanteringsfunktioner. Med andra ord används dessa certifikat för den roll baser åtkomstkontroll (RBAC) i den här artikeln.

För Azure läsa [ställer in ett kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) information om hur du konfigurerar Certifikatsäkerhet i ett kluster.

För fristående Windows Server läsa [skydda ett fristående kluster på Windows med X.509-certifikat](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Klient-till-nod Azure Active Directory (AAD) säkerheten på Azure
Kluster som körs på Azure kan också säker åtkomst till management-slutpunkter som använder Azure Active Directory (AAD). Se [ställer in ett kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) information om hur du skapar nödvändiga AAD-artefakter, hur du fylla i dem när klustret skapas och hur du ansluter till dessa kluster efteråt.

## <a name="security-recommendations"></a>Säkerhetsrekommendationer
Det rekommenderas att du använder AAD säkerhet för att autentisera klienter och certifikat för nod till nod säkerhet för Azure-kluster.

För fristående Windows Server hanterade kluster rekommenderas att du använder Windows-säkerhet med grupp-konton (GMA) om du har Windows Server 2012 R2 och Active Directory. Annars fortfarande använda Windows-säkerhet med Windows-konton.

## <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Åtkomstkontroll kan Klusteradministratören att begränsa åtkomsten till vissa klusteråtgärder för olika grupper av användare, vilket gör att klustret säkrare. Två olika åtkomstkontroll typer stöds för klienter som ansluter till ett kluster: administratörsrollen och användarrollen.

Administratörer har fullständig åtkomst till funktioner för hantering (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet till funktioner för hantering (till exempel frågefunktioner) och möjligheten att lösa program och tjänster.

Du anger rollerna administratörs- och klienten när klustret har skapats med olika identiteter (certifikat, AAD etc.) för varje. Läs mer om inställningar för åtkomstkontroll standard och hur du ändrar standardinställningarna [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-certifikat och Service Fabric
Digitala X.509-certifikat används ofta för att autentisera klienter och servrar och för att kryptera och digitalt signera meddelanden. Mer information om dessa certifikat, gå till [arbetar med certifikat](http://msdn.microsoft.com/library/ms731899.aspx).

Några viktiga saker att tänka på:

* Certifikat som används i kluster som kör produktionsarbetsbelastningar ska skapas med hjälp av en korrekt konfigurerade tjänsten för Windows Server-certifikat eller hämtas från en godkänd [certifikatutfärdare (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Använd aldrig några tillfälligt eller testa certifikat i produktion som skapas med verktyg som MakeCert.exe.
* Du kan använda ett självsignerat certifikat, men det bör bara göra det för testkluster och inte i produktion.

### <a name="server-x509-certificates"></a>Servern X.509-certifikat
Servercertifikat ha den främsta uppgiften för att autentisera en server (nod) till klienter eller autentisera en server (nod) till en server (nod). En av de första kontrollerna när en klient eller en nod autentiserar en nod är att kontrollera värdet för det gemensamma namnet i fältet Ämne. Den här nätverksnamn eller en av de certifikat Alternativt ämnesnamn måste finnas i listan över tillåtna namn.

I följande artikel som beskriver hur du skapar certifikat med alternativa ämnesnamn (SAN): [lägga till ett Alternativt ämnesnamn i ett certifikat för säker LDAP](http://support.microsoft.com/kb/931351).

Fältet ämne kan innehålla flera värden, var och en med en initieringen att ange värdetypen prefixet. Oftast är initieringen ”CN” för nätverksnamn; till exempel ”CN = www.contoso.com”. Det är också möjligt för fältet ämne är tom. Om valfria Alternativt ämnesnamn fylls fältet i, måste den innehålla en post per Alternativt ämnesnamn och vanliga namn för certifikatet. Dessa anges som värden för DNS-namn.

Värdet för fältet avsett syfte för certifikatet bör innehålla ett lämpligt värde, till exempel ”serverautentisering” eller ”klientautentisering”.

### <a name="client-x509-certificates"></a>X.509-klientcertifikat
Klientcertifikat utfärdas inte normalt av en tredjeparts certifikatutfärdare. I stället innehåller det personliga arkivet för den aktuella platsen vanligtvis klientcertifikat som placerats där av en rotcertifikatutfärdare, med syftet ”klientautentisering”. Klienten kan använda ett sådant certifikat när ömsesidig autentisering krävs.

> [!NOTE]
> Alla hanteringsåtgärder på ett Service Fabric-kluster kräver certifikat. Klientcertifikat kan inte användas för hantering.
> 
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Nästa steg
Den här artikeln innehåller information om säkerhet i klustret. Nästa,


1.  [Skapa ett kluster i Azure med hjälp av en Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) 
2.  [Azure-portalen](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
