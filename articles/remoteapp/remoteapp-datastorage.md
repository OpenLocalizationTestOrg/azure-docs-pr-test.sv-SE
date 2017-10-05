---
title: "Lagra inte känsliga data på anpassade avbildningar för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om riktlinjerna för att lagra data i anpassade avbildningar i Azure RemoteApp"
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
ms.openlocfilehash: 75d5415d33324d957617426e75909a6c6c58b1f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a>Lagra inte känsliga data på anpassade avbildningar
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

När du är värd för programmet i Azure RemoteApp, är det första steget att skapa en anpassad avbildning. Vi använder den anpassade bilden för att skapa VM-instanser som hanterar dina appar till användarna. Den anpassade avbildningen ska innehålla endast program och aldrig känsliga data som kan gå förlorade, till exempel SQL-databaser, supportpersonal filer eller särskilda filer som QuickBooks företagets filer. Alla känsliga data bör finnas utanför Azure RemoteApp på en filserver och en annan Azure VM, eller i SQL Azure. Bilden bör bara vara värd för det program som ansluter till datakällan och visar data. Granska [krav för Azure RemoteApp bilder](remoteapp-imagereqs.md) för mer information. 

För att förstå varför bör du inte lagra känsliga data, måste du förstå hur fungerar Azure RemoteApp. När en samling skapas eller uppdateras flera kloner eller kopior av bilden i bakgrunden skapas. Alla dessa VM-instanser är exakt repliker av den anpassade bilden. När användarna startar program som de är anslutna till något av dessa VM-instanser. Men samma instans är inte säkert och bör inte roll eftersom de är icke-beständig. VM-instanser som hyser program är inte beständiga och kan förstörs eller tas bort baserat, till exempel under uppdateringen. 

När samlingen är etablerad och startar användarna ansluter till de virtuella datorerna kan informationen är permanent och skyddas eftersom den har sparats i separata lagringsutrymme i en virtuell Hårddisk som vi kallar en [användarprofil-disk (UPD)](remoteapp-upd.md), vilket är användarens profil i c:\users\<userprofile >. När ett program börjar UPD monteras och behandlas precis som en lokal användarprofil av operativsystemet. Läs mer om hur [Azure RemoteApp sparar användardata och inställningar](remoteapp-upd.md).

Exempeldata som inte ska placeras i avbildningen:

* Delade data för användare att komma åt
* SQL-databas eller QuickBooks DB
* Inga data i D:\

Exempeldata som kan finnas i standardprofilen som ska kopieras till varje användare UPD:

* Konfigurationsfiler per användare
* Skript som användarna behöver bevaras i sina UPD

Viktiga punkter:

* Aldrig lagra känsliga data som kan gå förlorade på avbildningen när du skapar en anpassad avbildning.
* Känsliga data ska alltid placeras på en separat filserver separat Azure VM, i molnet, och alltid externa för VM-instanser som är värd för dina program i Azure RemoteApp. 
* Informationen sparas och kvarstår i användarprofildisk (UPD)

