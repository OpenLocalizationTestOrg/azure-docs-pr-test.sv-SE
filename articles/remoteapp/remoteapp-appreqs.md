---
title: "Appars krav för Azure RemoteApp | Microsoft Docs"
description: "Läs om krav för appar som du vill använda i Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 4427eef6-288a-49e1-97eb-fee67d99f26a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 13d42df97ea2b090180f5865a4eac25945f9f34c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="app-requirements"></a>Appkrav
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Azure RemoteApp stöder strömmande 32-bitars eller 64-bitars Windows-baserade program från en avbildning av Windows Server 2012 R2. De flesta 32-bitars eller 64-bitars Windows-baserade program som kör ”i befintligt skick” i Azure RemoteApp (Remote Desktop Services eller hette Terminal Services) miljö. Men det är skillnad mellan kör och kör väl – vissa program fungerar korrekt och utföra, medan andra inte. Följande information ger vägledning för att utveckla program i en miljö med Remote Desktop Services och tester för att säkerställa kompatibilitet.

Tips: Vi arbetar med att skapa arbetsexempel på appar du. Nya avsnitt som handlar om med hjälp av Microsoft Access, QuickBooks och App-V i RemoteApp visas.

## <a name="requirements"></a>Krav
Dessa tre krav om följt, för att köra väl i RemoteApp-programmet:

1. Program som uppfyller alla [certifieringskraven för Windows-skrivbordsappar](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) och följa [Fjärrskrivbordstjänster riktlinjer för programmering](https://msdn.microsoft.com/library/aa383490.aspx) har fullständig kompatibilitet med RemoteApp.
2. Program ska aldrig data lagras lokalt på bilden eller RemoteApp-instanser som kan gå förlorade.  När du skapar en RemoteApp-samling kan klonas instanserna och är tillståndslösa och får endast innehålla program. Lagra data i en extern källa eller i användarens profil.
3. Anpassade avbildningar bör aldrig innehåller data som kan gå förlorade.  

## <a name="testing-your-apps"></a>Testa dina appar
Följ dessa steg för att testa program:

1. Installera Windows Server 2012 R2 och ditt program
2. Aktivera Fjärrskrivbord
3. Skapa två användarkonton, användare a och b, att både användarkonton läggs till i säkerhetsgruppen för fjärrskrivbord.
4. Kontrollera kompatibiliteten för flera sessioner genom att skapa två samtidiga RDS-sessioner till datorn när du startar programmet.
5. Validera app-beteende

## <a name="application-development-guidelines"></a>Riktlinjer för utveckling av program
Använd följande riktlinjer för att utveckla program för RemoteApp.

### <a name="multiple-users"></a>Flera användare
* Installera en [för en enskild användare ](https://msdn.microsoft.com/library/aa380661.aspx)kan bli ett problem i ett nätverk.
* Program ska [lagra användarspecifik information](https://msdn.microsoft.com/library/aa383452.aspx) användarspecifika platser separat från globala information som gäller för alla användare.
* RemoteApp använder flera [namnområden för Kernelobjekt](https://msdn.microsoft.com/library/aa382954.aspx); en global namnrymd används främst av tjänster i klient/server-program.
* Det är inte säkert att anta att datornamnet eller [IP-adress](https://msdn.microsoft.com/library/aa382942.aspx) tilldelats datorn är associerade med en enskild användare eftersom flera användare kan ha loggat in samtidigt till en server som värd för fjärrskrivbordssession (RD Session Host).

### <a name="performance"></a>Prestanda
* Inaktivera [grafiska effekter](https://msdn.microsoft.com/library/aa380822.aspx) innan du lägger till en app i RemoteApp.
* Om du vill maximera CPU tillgänglighet för alla användare, antingen inaktivera [background uppgifter ](https://msdn.microsoft.com/library/aa380665.aspx) eller skapa effektiva bakgrundsaktiviteter som inte är resurskrävande.
* Du bör finjustera och balansera program [tråd användning](https://msdn.microsoft.com/library/aa383520.aspx) för miljöer med flera användare, med flera processorer.
* För att optimera prestanda, är det bra för program att [identifiera](https://msdn.microsoft.com/library/aa380798.aspx) om de körs i en klientsession.

