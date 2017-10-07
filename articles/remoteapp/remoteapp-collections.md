---
title: "aaaWhat typ av samling gör du behöver för Azure RemoteApp? | Microsoft Docs"
description: "Läs mer om hello typer av samlingar som är tillgängliga med Azure RemoteApp."
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
ms.openlocfilehash: f00b5fe41af597cf75e26300bf7842c3a8ff94fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Vilken typ av samling behöver du för Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp kan du dela appar och resurser med användare på alla enheter. Vi göra detta genom att skapa samlingar toohold hello appar och resurser och sedan du dela dessa samlingar med användare. Det finns 2 annan samling alternativ med olika nätverks- och autentiseringsalternativ - som är rätt för dig?

Låt oss gå igenom hello olika överväganden och alternativ som du behöver toomake tooget hello mest utanför Azure RemoteApp-samlingen. 

## <a name="quick-differences-between-hello-collection-types"></a>Snabb skillnaderna mellan hello samlingstyper
|  | Molnet | Hybrid |
| --- | --- | --- |
| Använd ett befintligt virtuellt nätverk |Ja |Ja |
| Kräver AD-anslutna konton (DirSync) |Nej |Ja |
| Kräver domänanslutning |Nej |Ja |
| Kräver domain controller tillgänglig tooVNET |Nej |Ja |

## <a name="cloud-collections"></a>Molnsamlingar
* Snabb toocreate - hello samling etableras snabbt, vilket innebär att dina appar får toousers snabbare.
* Ta med egna appar eller dela oss. Du kan använda en anpassad avbildning (skapas från en Azure VM) eller en av hello avbildningarna som ingår i din prenumeration.
* Du behöver inte tooconfigure en anslutning mellan din samling och den lokala domänen.
* Men du kan eventuellt använda din egen Azure VNET tooprovide åtkomst i din lokala miljö för data delas eller toouse icke-Windows-autentisering till resurser som SQL Server (med Databasautentisering).

OK, hur skapar jag en?

* Molnet? Skapa med hello **Snabbregistrering** alternativet i hello-portalen.
* Molnet + VNET? Skapa med hjälp av hello **skapa med VNET** alternativet men inte väljer toojoin en domän.

## <a name="hybrid-collections"></a>Hybridsamlingar
* Ger fullständig åtkomst tooon lokala nätverk + Azure VNET.
* Innehåller koppling domänåtkomst för appar och data. Fjärrprogram kan autentisering mot din lokala Active Directory - de kan sedan komma åt resurser i domänen.
* Aktivera avancerad övervakning och hantering av med befintliga System Center-lösningar och principer för Windows-grupp (via en anpassad avbildning som bygger på Windows Server 2012 R2)
* Stöd för [ExpressRoute](https://azure.microsoft.com/services/expressroute/) tooconnect Azure VNET-tooyour virtuella lokala nätverk.

Skapa med hjälp av hello **skapa med VNET** alternativet och välj toojoin en domän.

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
Du kan använda Microsoft-konton, Azure AD-konton eller en blandning av hello två med molnsamlingar. Använd hello-konton som passar bäst för dina användare.

Det finns inga särskilda krav för att använda Microsoft-konton. 

Om du vill toouse Azure AD-konton, måste toomake till att din Azure AD-klient matchar hello en associerad med din prenumeration. När du skapade din Azure RemoteApp-prenumeration associerades hello Azure AD-klient som du använde automatiskt med din prenumeration. Azure AD-användare ger behörighet tooneeds toobe samma klient. Om det behövs kan du [ändra hello Azure AD-klienten](remoteapp-changetenant.md) som är associerade med prenumerationen.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybrid (eller molnet + Azure AD + AD)
Använda Azure AD + lokala Active Directory är en förutsättning för en hybridsamling. Du måste toouse AD Connect toointegrate hello två kataloger. Men du har vissa val när det gäller toohow som du konfigurerar AD Connect. 

Det finns 2 AD Connect scenarier - genom att använda Lösenordssynkronisering eller federation med AD. Kolla in hello [AD Connect information](../active-directory/active-directory-aadconnect.md) toofigure reda på om de passar bäst för dig.

Du kan också använda Azure AD + AD med en molnsamling. Kontrollera att du följer hello samma konfigurera steg.

Checka ut [Azure AD + Active Directory-krav för Azure RemoteApp](remoteapp-ad.md) för hello steg krävs tooconfigure Azure AD och Active Directory.

## <a name="go-create-your-collection"></a>Gå att skapa samlingen!
Jag tror OK, vi har förstått det nu, så det finns bara en sak vänstra toodo – skapa din första Azure RemoteApp-samling.

[Skapa en molnsamling](remoteapp-create-cloud-deployment.md) eller [skapa en hybridsamling](remoteapp-create-hybrid-deployment.md) -bara hämta skapa.

