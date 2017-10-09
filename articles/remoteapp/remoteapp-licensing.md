---
title: aaaAzure RemoteApp-licensiering | Microsoft Docs
description: "Läs om hur det fungerar med licensiering i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: ff8ebd20-61a1-4f10-87a6-234a170534c9
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: dfa808a65ea6b1a78bf74f3daddb9a84e186eebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Så här fungerar det med licensiering i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Så du har konfigurerat Azure RemoteApp-tjänsten, skapat mallar och är redo toopublish appar tooyour användare. Men det finns fortfarande en sista sak toofigure ut: licensiering. Hur fungerar precis licensiering fungerar för RemoteApp- och hello-appar som du delar via RemoteApp?

RemoteApp kräver varken Windows-licenser eller klientåtkomstlicenser för fjärrskrivbord. Din prenumeration hand tar om hello själva RemoteApp-sidan. (Kontrollera hello uppgifter om hello [prissättningar](https://azure.microsoft.com/pricing/details/remoteapp).)

Om du använder ett hello avbildningarna som ingår i din prenumeration kan dela du hello appar installerade på avbildningen utan att behöva en separat licens. Om du använder hello Windows Server 2012 R2 mallen avbildningen toobuild din samling kan dela du System Center Endpoint Protection med dina användare. hello endast undantag toothis regeln är Office 365 ProPlus, som kräver en separat prenumeration och Office 2013, som inte kan delas i en produktionssamling.

Om du vill toouse hello Office 365-mallavbildningen som medföljer RemoteApp, måste du ha en *befintliga* Office 365 ProPlus-plan. hello samma sak gäller för alla Office 365-appar som du vill publicera via en anpassad mall. Du behöver tooactivate hello appar med din egen prenumeration. Det här gäller både för utvärderingsprenumerationer och betalda prenumerationer. Om du vill toouse hello Office 365-mallavbildningen under hello utvärderingsversion *och du inte redan har en prenumeration*, gå toohello Office 365-sidan för[registrera](https://go.microsoft.com/fwlink/p/?LinkID=403802) för en utvärderingsprenumeration. Mer info finns i [Hur fungerar RemoteApp och Office tillsammans?](remoteapp-o365.md)

Om du inte vill tooget en Office 365-utvärderingsprenumeration under utvärderingsperioden hello, kan du använda hello Office 2013 Professional Plus-mallavbildningen som medföljer RemoteApp. Den här mallavbildningen kan bara användas i 30 dagar och det går inte att konvertera den till en betald samling.

Du måste toomake till att du har hello licens tooshare hello app för andra appar.

Det låter väl logiskt, eller hur? Du kan publicera en app som du har laglig rätt tooshare. Och du behöver toomake till att du verkligen har rätt tooshare dina program.

Observera att du inte kan använda en klientåtkomstlicens eller ett volymlicensavtal i en molnsamling. Du *kan* använder en Volume License avtal tooactivate program i hybridsamlingen (utom Office). Du behöver bara tooinstall dem i din mall bild av hello volymlicensmediet. Följ hello information från programmet hello leverantör tooinstall licenser i en miljö med fjärrskrivbord.

