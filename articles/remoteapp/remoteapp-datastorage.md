---
title: "aaaNever lagra känsliga data på anpassade avbildningar för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om hello riktlinjer för att lagra data i anpassade avbildningar i Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 86a0fea218f8826d6d25f50d3c4c36e368e26fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a>Lagra inte känsliga data på anpassade avbildningar
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

När du är värd för programmet i Azure RemoteApp, är hello första steget toocreate en anpassad avbildning. Vi använder de anpassade bilden toocreate VM-instanser som hanterar dina appar tooyour användare. hello anpassad avbildning bör innehålla endast program och aldrig känsliga data som kan gå förlorade, till exempel SQL-databaser, supportpersonal filer eller särskilda filer som QuickBooks företagets filer. Alla känsliga data bör finnas externa tooAzure RemoteApp på en filserver och en annan Azure VM, eller i SQL Azure. hello avbildningen ska bara hello värdprogrammet som ansluter toohello datakälla och visar hello data. Granska [krav för Azure RemoteApp bilder](remoteapp-imagereqs.md) för mer information. 

toounderstand varför inte bör du lagra känsliga data, behöver du toounderstand hur fungerar Azure RemoteApp. När en samling skapas eller uppdateras, bakgrunden hello skapas flera kloner eller kopior av hello bild. Alla dessa VM-instanser är exakt repliker av hello anpassade bilden. När användarna startar program är de anslutna tooone av dessa VM-instanser. Men hello samma instans är inte säkert och bör inte roll eftersom de är icke-beständig. Hej VM-instanser värd hello program är inte beständiga och kan vara raderats eller tagits bort baserat, till exempel under uppdateringen. 

När hello samling etableras och startar användarna ansluter toohello VMs användardata är permanent och skyddas eftersom den har sparats i separata lagringsutrymme i en virtuell Hårddisk som vi kallar en [användarprofil-disk (UPD)](remoteapp-upd.md), vilket är hello användarprofil i c:\users\<userprofile >. När ett program börjar hello UPD monteras och behandlas precis som en lokal användarprofil hello-operativsystemet. Läs mer om hur [Azure RemoteApp sparar användardata och inställningar](remoteapp-upd.md).

Exempeldata som inte ska placeras i hello avbildningen:

* Delade data för användare tooaccess
* SQL-databas eller QuickBooks DB
* Inga data i D:\

Exempeldata som kan finnas i hello standard profil toobe kopieras till varje användare UPD:

* Konfigurationsfiler per användare
* Skript som användarna behöver bevaras i sina UPD

Viktiga punkter:

* Lagra inte känsliga data som kan gå förlorade på hello avbildningen när du skapar en anpassad avbildning.
* Känsliga data bör alltid finns på en separat filserver, avgränsa Azure VM, på hello moln och alltid externa toohello VM-instanser som värd för dina program i Azure RemoteApp. 
* Informationen sparas och kvarstår i hello användarprofil-disk (UPD)

