---
title: "Vilken typ av samling behöver du för Azure RemoteApp? | Microsoft Docs"
description: "Lär dig mer om vilka typer av samlingar som är tillgängliga med Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c13ec78d-07e9-4646-8194-cf3efafc1760
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 10f6c0533027767b6635ebff1e6a9872bde06a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Vilken typ av samling behöver du för Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Azure RemoteApp kan du dela appar och resurser med användare på alla enheter. Vi göra detta genom att skapa samlingar för att rymma appar och resurser och sedan du dela dessa samlingar med användare. Det finns 2 annan samling alternativ med olika nätverks- och autentiseringsalternativ - som är rätt för dig?

Låt oss gå igenom de olika överväganden och alternativ som du behöver göra att få ut mesta möjliga av Azure RemoteApp-samlingen. 

## <a name="quick-differences-between-the-collection-types"></a>Snabb skillnaderna mellan samlingstyper
|  | Molnet | Hybrid |
| --- | --- | --- |
| Använd ett befintligt virtuellt nätverk |Ja |Ja |
| Kräver AD-anslutna konton (DirSync) |Nej |Ja |
| Kräver domänanslutning |Nej |Ja |
| Kräver en domänkontrollant tillgänglig för virtuella nätverk |Nej |Ja |

## <a name="cloud-collections"></a>Molnsamlingar
* Snabbt att skapa - samlingen etableras snabbt, vilket innebär att dina appar får användare snabbare.
* Ta med egna appar eller dela oss. Du kan använda en anpassad avbildning (skapas från en Azure VM) eller en av avbildningarna som ingår i din prenumeration.
* Du behöver inte konfigurera en anslutning mellan din samling och den lokala domänen.
* Men du kan också använda ditt eget virtuella Azure-att ge åtkomst till din lokala miljö för delning av data eller att använda Windows-autentisering till resurser som SQL Server (med Databasautentisering).

OK, hur skapar jag en?

* Molnet? Skapa med de **Snabbregistrering** alternativet i portalen.
* Molnet + VNET? Skapa med hjälp av den **skapa med VNET** alternativet men inte väljer att ansluta till en domän.

## <a name="hybrid-collections"></a>Hybridsamlingar
* Ge full åtkomst till lokala nätverk + Azure VNET.
* Innehåller koppling domänåtkomst för appar och data. Fjärrprogram kan autentisering mot din lokala Active Directory - de kan sedan komma åt resurser i domänen.
* Aktivera avancerad övervakning och hantering av med befintliga System Center-lösningar och principer för Windows-grupp (via en anpassad avbildning som bygger på Windows Server 2012 R2)
* Stöd för [ExpressRoute](https://azure.microsoft.com/services/expressroute/) att ansluta ditt Azure VNET till ditt lokala VNET.

Skapa med hjälp av den **skapa med VNET** alternativet och väljer att ansluta till en domän.

## <a name="authentication-options"></a>Autentiseringsalternativ
Azure RemoteApp stöder både Microsoft-konton och Azure Active Directory-konton, men inte alla samlingar stöder alla metoder. 

| Kontotyp |  | Molnet | Molnet + VNET | Hybrid |
| --- | --- | --- | --- | --- |
| Microsoft-konto | |Ja |Ja |Nej |
| Azure Active Directory (AD Azure) | | | | |
| Azure AD |Ja |Ja |Nej | |
| AD Connect med Lösenordssynkronisering |Ja |Ja |Ja | |
| Anslut AD utan Lösenordssynkronisering |Ja |Ja |Nej | |
| AD Connect med AD FS |Ja |Ja |Ja | |
| 3-part stöds av Azure identitetsleverantörer (till exempel Ping) |Ja |Ja |Ja | |
| Multi-Factor Authentication | |Ja |Ja |Ja |

### <a name="cloud-and-cloud--vnet"></a>Molnet och molnet + VNET
Du kan använda Microsoft-konton, Azure AD-konton eller en blandning av båda med molnsamlingar. Använd de konton som passar bäst för dina användare.

Det finns inga särskilda krav för att använda Microsoft-konton. 

Om du vill använda Azure AD-konton måste du kontrollera att din Azure AD-klient matchar det som är associerade med prenumerationen. När du skapade din Azure RemoteApp-prenumeration, associerades Azure AD-klient som du använde automatiskt med din prenumeration. Alla Azure AD-användare som du ger behörighet att måste vara samma klient. Om det behövs kan du [ändra Azure AD-klient](remoteapp-changetenant.md) som är associerade med prenumerationen.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybrid (eller molnet + Azure AD + AD)
Använda Azure AD + lokala Active Directory är en förutsättning för en hybridsamling. Du måste använda AD Connect för att integrera två kataloger. Men du har vissa val när det gäller hur du konfigurerar AD Connect. 

Det finns 2 AD Connect scenarier - genom att använda Lösenordssynkronisering eller federation med AD. Kolla in den [AD Connect information](../active-directory/active-directory-aadconnect.md) ta reda på vilken av dessa fungerar bäst för dig.

Du kan också använda Azure AD + AD med en molnsamling. Kontrollera att du följer samma ställa in steg.

Checka ut [Azure AD + Active Directory-krav för Azure RemoteApp](remoteapp-ad.md) steg för att konfigurera Azure AD och Active Directory.

## <a name="go-create-your-collection"></a>Gå att skapa samlingen!
OK, tror jag att vi har förstått det nu, så det finns bara en sak kvar att göra - skapa din första Azure RemoteApp-samling.

[Skapa en molnsamling](remoteapp-create-cloud-deployment.md) eller [skapa en hybridsamling](remoteapp-create-hybrid-deployment.md) -bara hämta skapa.

