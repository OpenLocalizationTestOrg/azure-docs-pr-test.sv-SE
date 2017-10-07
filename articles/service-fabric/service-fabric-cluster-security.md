---
title: aaaSecure ett Service Fabric-kluster | Microsoft Docs
description: "Beskriver hello säkerhetsscenarier för tooimplement för olika tekniker som används av klustret och hello ett Service Fabric dessa scenarier."
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
ms.openlocfilehash: 249a9e85b8fbe174e2accee85a94d95b2872a3af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>Säkerhetsscenarier för Service Fabric-kluster
Ett Service Fabric-kluster är en resurs som du äger. Kluster måste vara skyddad tooprevent obehöriga användare från att ansluta tooyour kluster, särskilt när den har produktionsarbetsbelastningar som körs på den. Även om det är möjligt toocreate ett oskyddat kluster på så sätt kan anonyma användare tooconnect tooit, om det visar management slutpunkter toohello offentliga Internet. 

Den här artikeln innehåller en översikt över hello säkerhetsscenarier för kluster som körs på Azure eller fristående och hello olika tekniker används tooimplement dessa scenarier. hello klustret scenarier är:

* Säkerhet för nod till nod
* Klient-till-nod-säkerhet
* Rollbaserad åtkomstkontroll (RBAC)

## <a name="node-to-node-security"></a>Säkerhet för nod till nod
Skyddar kommunikationen mellan hello virtuella datorer eller datorer i hello kluster. Detta säkerställer att endast datorer som är auktoriserade toojoin hello klustret kan delta i värd för program och tjänster i hello kluster.

![Diagram över nod till nod-kommunikation][Node-to-Node]

Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://msdn.microsoft.com/library/ff649801.aspx) eller [Windows-säkerhet](https://msdn.microsoft.com/library/ff649396.aspx) för Windows Server-datorer.

### <a name="node-to-node-certificate-security"></a>Nod till nod Certifikatsäkerhet
Service Fabric använder X.509-servercertifikat som du anger som en del av hello-nodtyp konfigurationer när du skapar ett kluster. En snabb överblick över dessa certifikat är och hur du kan hämta eller skapa dem tillhandahålls hello slutet av den här artikeln.

Certifikatsäkerhet konfigureras när du skapar klustret hello antingen via hello Azure-portalen, Azure Resource Manager-mallar eller en fristående JSON-mall. Du kan ange ett certifikat för primär och en valfri sekundärt certifikat som används för certifikatet överrullningar. hello primära och sekundära certifikat som du anger bör vara ett annat än hello administratörsklient och skrivskyddad klientcertifikat som du anger för [klient-till-nod säkerhet](#client-to-node-security).

För Azure läsa [ställer in ett kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) toolearn hur tooconfigure certifikat säkerhet i ett kluster.

För fristående Windows Server läsa [skydda ett fristående kluster på Windows med X.509-certifikat](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Nod till nod windows-säkerhet
För fristående Windows Server läsa [skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Klient-till-nod-säkerhet
Klienter och skyddar kommunikationen mellan en klient och enskilda noder i klustret hello. Den här typen av säkerhet som autentiserar och skyddar klientkommunikation, vilket garanterar att endast auktoriserade användare kan komma åt hello klustret och hello-program som distribueras på hello klustret. Klienter kan identifieras unikt genom sina autentiseringsuppgifter för Windows-säkerhet eller säkerhetsreferenser sina certifikat.

![Diagram över kommunikation från klient till nod][Client-to-Node]

Kluster som körs på Azure eller fristående kluster som körs på Windows kan använda antingen [Certifikatsäkerhet](https://msdn.microsoft.com/library/ff649801.aspx) eller [Windows-säkerhet](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Klient-till-nod Certifikatsäkerhet
 Certifikatsäkerhet för klient-till-nod konfigureras när du skapar klustret hello antingen via hello Azure-portalen, Resource Manager-mallar eller en fristående JSON-mall genom att ange ett klientcertifikat för admin och/eller ett klientcertifikat.  hello klient- och administratörsklientcertifikat du anger bör vara annorlunda än hello primära och sekundära certifikat som du anger för [nod till nod säkerhet](#node-to-node-security) som bästa praxis. Som standard läggs hello klustercertifikat för nod till nod säkerhet toohello tillåtna klienten Admin certifikat lista.

Klienter som ansluter toohello kluster med hjälp av hello admin certifikat har fullständig åtkomst toomanagement.  Klienter som ansluter toohello kluster med hjälp av hello skrivskyddade klientcertifikat har bara läsbehörighet toomanagement. Med andra ord används dessa certifikat för hello rollen baser åtkomstkontroll (RBAC) i den här artikeln.

För Azure läsa [ställer in ett kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) toolearn hur tooconfigure certifikat säkerhet i ett kluster.

För fristående Windows Server läsa [skydda ett fristående kluster på Windows med X.509-certifikat](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Klient-till-nod Azure Active Directory (AAD) säkerheten på Azure
Kluster som körs på Azure kan även skydda åtkomst toohello management slutpunkter med hjälp av Azure Active Directory (AAD). Se [ställer in ett kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) för information om hur toocreate hello nödvändiga AAD-artefakter, hur toopopulate dem vid klustret skapas och hur tooconnect toothose kluster efteråt.

## <a name="security-recommendations"></a>Säkerhetsrekommendationer
Det rekommenderas att du använder AAD säkerhet tooauthenticate klienter och certifikat för nod till nod säkerhet för Azure-kluster.

För fristående Windows Server hanterade kluster rekommenderas att du använder Windows-säkerhet med grupp-konton (GMA) om du har Windows Server 2012 R2 och Active Directory. Annars fortfarande använda Windows-säkerhet med Windows-konton.

## <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare. Två olika åtkomstkontroll typer stöds för klienter som ansluter tooa kluster: administratörsrollen och användarrollen.

Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.

Du anger Hej administratör och klienten användarroller Hej när klustret skapas med olika identiteter (certifikat, AAD etc.) för varje. Mer information om inställningar för åtkomstkontroll för hello standard och hur toochange hello standardinställningar finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-certifikat och Service Fabric
X.509 digitala certifikat används ofta tooauthenticate klienter och servrar och tooencrypt och signera meddelanden. Mer information om dessa certifikat finns för[arbetar med certifikat](http://msdn.microsoft.com/library/ms731899.aspx).

Några viktiga saker tooconsider:

* Certifikat som används i kluster som kör produktionsarbetsbelastningar ska skapas med hjälp av en korrekt konfigurerade tjänsten för Windows Server-certifikat eller hämtas från en godkänd [certifikatutfärdare (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Använd aldrig några tillfälligt eller testa certifikat i produktion som skapas med verktyg som MakeCert.exe.
* Du kan använda ett självsignerat certifikat, men det bör bara göra det för testkluster och inte i produktion.

### <a name="server-x509-certificates"></a>Servern X.509-certifikat
Servercertifikat ha hello främsta uppgiften för att autentisera en server (nod) tooclients eller autentisera en server (nod) tooa server (nod). En hello inledande kontroller när en klient eller en nod autentiserar en nod är toocheck hello värdet för hello nätverksnamn i hello ämne. Den här nätverksnamn eller en av hello certifikat Alternativt ämnesnamn måste finnas i hello listan över tillåtna namn.

hello följande artikeln beskriver hur toogenerate certifikat med alternativa ämnesnamn (SAN): [hur tooadd tooa för alternativt namn för ett ämne skydda LDAP-certifikat](http://support.microsoft.com/kb/931351).

hello ämnesfältet kan innehålla flera värden, var och en med en värdetyp för initiering tooindicate hello prefixet. Oftast är hello initieringen ”CN” för nätverksnamn; till exempel ”CN = www.contoso.com”. Det är också möjligt för hello ämne fältet toobe tomt. Om hello valfria Alternativt ämnesnamn fylls, måste den innehålla både hello eget namn för hello certifikat och en post per Alternativt ämnesnamn. Dessa anges som värden för DNS-namn.

hello-värdet för hello avsedda syften fältet hello certifikat ska innehålla ett lämpligt värde, till exempel ”serverautentisering” eller ”klientautentisering”.

### <a name="client-x509-certificates"></a>X.509-klientcertifikat
Klientcertifikat utfärdas inte normalt av en tredjeparts certifikatutfärdare. I stället innehåller hello datorarkivet av hello aktuella plats vanligtvis klientcertifikat som placerats där av en rotcertifikatutfärdare, med syftet ”klientautentisering”. hello-klienten kan använda ett sådant certifikat när ömsesidig autentisering krävs.

> [!NOTE]
> Alla hanteringsåtgärder på ett Service Fabric-kluster kräver certifikat. Klientcertifikat kan inte användas för hantering.
> 
> 

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->


## <a name="next-steps"></a>Nästa steg
Den här artikeln innehåller information om säkerhet i klustret. Nästa,


1.  [Skapa ett kluster i Azure med hjälp av en Resource Manager-mall](service-fabric-cluster-creation-via-arm.md) 
2.  [Azure-portalen](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
