---
title: "aaaApp krav för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om hello krav för appar som du vill toouse i Azure RemoteApp"
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
ms.openlocfilehash: 3fa2bcdaab457a6fbee8ac52a81d1c4154bbdce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-requirements"></a>Appkrav
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp stöder strömmande 32-bitars eller 64-bitars Windows-baserade program från en avbildning av Windows Server 2012 R2. De flesta 32-bitars eller 64-bitars Windows-baserade program som kör ”i befintligt skick” i Azure RemoteApp (Remote Desktop Services eller hette Terminal Services) miljö. Men det är skillnad mellan kör och kör väl – vissa program fungerar korrekt och utföra, medan andra inte. hello följande information ger vägledning för att utveckla program i en miljö med Remote Desktop Services och testa tooensure kompatibilitet.

Tips: Vi arbetar med att skapa arbetsexempel på appar du. Nya avsnitt som handlar om med hjälp av Microsoft Access, QuickBooks och App-V i RemoteApp visas.

## <a name="requirements"></a>Krav
Dessa tre krav om följt, för att köra väl i RemoteApp-programmet:

1. Program som uppfyller alla [certifieringskraven för Windows-skrivbordsappar](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) och för att följa[Fjärrskrivbordstjänster riktlinjer för programmering](https://msdn.microsoft.com/library/aa383490.aspx) har fullständig kompatibilitet med RemoteApp.
2. Program ska aldrig data lagras lokalt på hello bild- eller RemoteApp-instanser som kan gå förlorade.  När du skapar en RemoteApp-samling kan klonas hello instanser och är tillståndslösa och får endast innehålla program. Lagra data i en extern källa eller inom hello användarprofil.
3. Anpassade avbildningar bör aldrig innehåller data som kan gå förlorade.  

## <a name="testing-your-apps"></a>Testa dina appar
Använd dessa steg tootesting program:

1. Installera Windows Server 2012 R2 och ditt program
2. Aktivera Fjärrskrivbord
3. Skapa två användarkonton, användare a och b, som att lägga till både konton toohello fjärrskrivbord säkerhetsgrupp.
4. Kontrollera kompatibiliteten för flera sessioner genom att skapa två samtidiga RDS sessioner toohello dator när du startar programmet hello.
5. Validera app-beteende

## <a name="application-development-guidelines"></a>Riktlinjer för utveckling av program
Använd följande riktlinjer för hur du utvecklar program för RemoteApp hello.

### <a name="multiple-users"></a>Flera användare
* Installera en [för en enskild användare ](https://msdn.microsoft.com/library/aa380661.aspx)kan bli ett problem i ett nätverk.
* Program ska [lagra användarspecifik information](https://msdn.microsoft.com/library/aa383452.aspx) användarspecifika platser separat från globala information som gäller tooall användare.
* RemoteApp använder flera [namnområden för Kernelobjekt](https://msdn.microsoft.com/library/aa382954.aspx); en global namnrymd används främst av tjänster i klient/server-program.
* Det är inte säker tooassume som hello datornamn eller hello [IP-adress](https://msdn.microsoft.com/library/aa382942.aspx) tilldelade toohello dator som är associerade med en enskild användare eftersom flera användare kan ha loggat in samtidigt tooa värd för fjärrskrivbordssession (RD Session Host) server.

### <a name="performance"></a>Prestanda
* Inaktivera [grafiska effekter](https://msdn.microsoft.com/library/aa380822.aspx) innan du lägger till din app tooRemoteApp.
* toomaximize CPU tillgänglighet för alla användare, antingen inaktivera [background uppgifter ](https://msdn.microsoft.com/library/aa380665.aspx) eller skapa effektiva bakgrundsaktiviteter som inte är resurskrävande.
* Du bör finjustera och balansera program [tråd användning](https://msdn.microsoft.com/library/aa383520.aspx) för miljöer med flera användare, med flera processorer.
* toooptimize prestanda, är det bra för program för[identifiera](https://msdn.microsoft.com/library/aa380798.aspx) om de körs i en klientsession.

