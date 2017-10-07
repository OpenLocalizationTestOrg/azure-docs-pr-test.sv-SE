---
title: "aaaSecure ett kluster som körs på Windows med hjälp av Windows-säkerhet | Microsoft Docs"
description: "Lär dig hur säkerhet för tooconfigure nod till nod och klient-till-nod på en fristående kluster som körs på Windows med hjälp av Windows-säkerhet."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 44f3011eb630357f342052a48d6c852b17dccec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet
tooprevent obehörig åtkomst tooa Service Fabric-kluster, måste du skydda hello klustret. Säkerhet är särskilt viktigt när hello klustret körs produktionsarbetsbelastningar. Den här artikeln beskriver hur säkerhet för tooconfigure nod till nod och klient-till-nod med hjälp av Windows-säkerhet i hello *ClusterConfig.JSON* fil.  hello process motsvarar toohello konfigurera säkerhetssteg i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md). Mer information om hur Service Fabric använder Windows-säkerhet finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).

> [!NOTE]
> Bör du hello val av nod till nod säkerhet noggrant eftersom det finns inget klusteruppgradering från en säkerhet val tooanother. toochange hello säkerhet val du har toorebuild hello fullständig kluster.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Konfigurera Windows-säkerhet som använder gMSA  
hello exempel *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurationsfilen hämtas med hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) fristående klusterpaket innehåller en mall för att konfigurera Windows-säkerhet med [Grupphanterat tjänstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  
  
| **Konfigurationsinställningen** | **Beskrivning** |  
| --- | --- |  
| WindowsIdentities |Innehåller hello klustret och klienten identiteter. |  
| ClustergMSAIdentity |Konfigurerar nod till noden säkerhet. En grupp Hanterat tjänstkonto. |  
| ClusterSPN |Fullständigt kvalificerat domännamn SPN för gMSA-kontot|  
| ClientIdentities |Konfigurerar säkerhet för klient-till-nod. En matris med användarkonton för klienten. |  
| Identitet |hello klientens identitet, en domänanvändare. |  
| IsAdmin |TRUE anger hello domänanvändaren har administratörsbehörighet för klienten, false för klientåtkomst för användaren. |  
  
[Noden toonode säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras genom att ange **ClustergMSAIdentity** när service fabric måste toorun under gMSA. I ordning toobuild förtroenderelationer mellan noder, måste de göras medveten om varandra. Detta kan åstadkommas på två olika sätt: Ange hello Grupphanterat tjänstkonto som innehåller alla noder i klustret hello eller ange hello datorn domängrupp som innehåller alla noder i klustret hello. Vi rekommenderar starkt med hello [Grupphanterat tjänstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) metod, särskilt för större kluster (fler än 10 noder) eller för kluster som är sannolikt toogrow eller minska.  
Den här metoden kräver inte hello skapandet av en domängrupp som klusteradministratörer har beviljats åtkomst rättigheter tooadd och ta bort medlemmar. Dessa konton är också användbara för automatisk lösenordshantering. Mer information finns i [komma igång med Grupphanterade tjänstkonton](http://technet.microsoft.com/library/jj128431.aspx).  
 
[Toonode klientsäkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**. I ordning tooestablish förtroende mellan klienten och hello-kluster, måste du konfigurera hello klustret tooknow som klienten identiteter som den kan lita på. Detta kan göras på två olika sätt: Ange hello gruppen Domänanvändare som kan ansluta eller ange hello nod domänanvändare som kan ansluta. Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och. Åtkomstkontroll ger hello möjligheten för hello administratören toolimit åtkomst toocertain klustertyper för klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.  Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster. Mer information om åtkomstkontroller finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).  
 
Hej följande exempel **säkerhet** avsnittet konfigurerar Windows-säkerhet som använder gMSA och anger att hello datorer i *ServiceFabric.clusterA.contoso.com* gMSA tillhör hello klustret och att *CONTOSO\usera* har åtkomst till admin-klienten:  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a>Konfigurera Windows-säkerhet med hjälp av en grupp datorer  
hello exempel *ClusterConfig.Windows.MultiMachine.JSON* konfigurationsfilen hämtas med hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) fristående klusterpaket innehåller en mall för att konfigurera Windows-säkerhet.  Windows-säkerhet har konfigurerats i hello **egenskaper** avsnitt: 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| **Konfigurationsinställningen** | **Beskrivning** |
| --- | --- |
| ClusterCredentialType |**ClusterCredentialType** har angetts för*Windows* om ClusterIdentity anger en Active Directory datorn gruppnamn. |  
| ServerCredentialType |Ställa in också*Windows* tooenable Windows-säkerhet för klienter.<br /><br />Detta anger att hello klienter på hello och hello klustret själva körs inom en Active Directory-domän. |  
| WindowsIdentities |Innehåller hello klustret och klienten identiteter. |  
| ClusterIdentity |Använd en dator gruppnamn, domain\machinegroup, tooconfigure nod till noden säkerhet. |  
| ClientIdentities |Konfigurerar säkerhet för klient-till-nod. En matris med användarkonton för klienten. |  
| Identitet |Lägg till hello domänanvändare domän\användarnamn för hello klientens identitet. |  
| IsAdmin |Ange tootrue toospecify som hello domänanvändare har klienten administratörsåtkomst eller false för klientåtkomst för användaren. |  

[Noden toonode säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras med hjälp av inställningen **ClusterIdentity** om du vill toouse en grupp datorer inom en Active Directory-domän. Mer information finns i [skapar en datorgruppen i Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[Klient-till-nod säkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**. tooestablish förtroende mellan klienten och hello kluster, måste du konfigurera hello klustret tooknow hello klient kan lita på identiteter som hello klustret. Du kan upprätta förtroende på två olika sätt:

- Ange hello gruppen Domänanvändare som kan ansluta.
- Ange hello nod domänanvändare som kan ansluta.

Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och. Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustertyper för klusteråtgärder för olika grupper av användare, vilket gör att hello klustret är säkrare.  Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.  

Hej följande exempel **säkerhet** avsnittet konfigurerar Windows-säkerhet, anger den hello datorer i *ServiceFabric/clusterA.contoso.com* tillhör hello kluster och anger att  *CONTOSO\usera* har åtkomst till admin-klienten:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric bör inte distribueras på en domänkontrollant. Kontrollera att ClusterConfig.json inte omfatta hello IP-adress för hello domänkontrollant när du använder en dator grupp eller grupphanterat tjänstkonto (gMSA).
>
>

## <a name="next-steps"></a>Nästa steg
När du har konfigurerat Windows-säkerhet i hello *ClusterConfig.JSON* fil, återuppta hello skapar kluster i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md).

Mer information om hur nod till noden säkerhet, klient-till-nod säkerhet och rollbaserad åtkomstkontroll finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).

Se [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md) exempel på ansluter med hjälp av PowerShell eller FabricClient.
