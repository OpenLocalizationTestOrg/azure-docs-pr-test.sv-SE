---
title: "Metodtips för aaaAzure RemoteApp | Microsoft Docs"
description: "Metodtips för att konfigurera och använda Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b851865b-bec4-4f29-82c0-7b9770c1a520
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f4d09ef30816eaebb74b69f26f3242c69ea27591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Metodtips för att konfigurera och använda Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) mer information.
> 
> 

hello följande information kan hjälpa dig att konfigurera och använda Azure RemoteApp effektivt.

## <a name="connectivity"></a>Anslutning
* Använd alltid hello senaste klientversion. Med hjälp av äldre klienter leda till problem med nätverksanslutningen och andra försämrad upplevelser. Aktivera automatisk tillämpning uppdateringar för din enhet säkerställer installeras alltid den senaste hello-klienten.
* Använd alltid hello stabila och mest tillförlitliga internet-anslutning tillgängliga tooyou.  
* Endast stöd för att använda proxy-anslutningar för optimala prestanda.  hello SOCKS-proxy stöds inte.

## <a name="applications"></a>Program
* Spara och stänga RemoteApp-program när du är klar med hello program. Inte stänga programmet hello kan resultera i förlust av data.
* Verifiera anpassade program innan du använder dem i Azure RemoteApp. Detta inkluderar att säkerställa de fungerar på en plattform för flera sessioner och inte använda onödiga resurser, till exempel minne och processor som kan strypa en annan användare i hello samma samling. Hämta information och granska hello [Application Compatibility bästa praxis för Fjärrskrivbordstjänster](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfiguration och hantering
* Behåll din mallavbildningarna in toodate, programuppdateringar och andra kritiska korrigeringar installeras vid behov. Detta säkerställer att som Azure RemoteApp auto-skalor toomeet korrigeras kapacitet varje instans.  
* Kontrollera distributionen av Active Directory Federation Services (AD FS) är säkert och tillförlitligt. Annars misslyckas klienten autentiseringar, hindrar användare från att komma åt Azure RemoteApp.
* Konfigurera mallavbildningarna med installerade program, roller eller funktioner som finns tillståndslösa. De bör inte lita på alla instanser av hello virtuella datorer i en RemoteApp-tjänsten är i ett beständigt tillstånd.
  * Lagra alla användardata i användarprofiler eller andra platser externa toohello lagringstjänsten, t.ex. lokala filen resurser eller OneDrive.
  * Lagra delade data i platser externa toohello lagringstjänsten, t.ex. lokala filen resurser eller OneDrive.
  * Konfigurera inställningar för hela systemet i hello mallavbildningen i stället för på enskilda virtuella datorer i en tjänst.
  * Inaktivera automatiska uppdateringar av publicerade program – i stället använda dem manuellt toohello mallavbildningen och testa dem innan du distribuerar från hello mall.

