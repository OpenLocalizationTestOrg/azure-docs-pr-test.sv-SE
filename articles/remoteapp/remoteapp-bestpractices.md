---
title: "Metodtips för Azure RemoteApp | Microsoft Docs"
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
ms.openlocfilehash: ab9c2dafc622e9b6f3bcd2767218cdc03e844847
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Metodtips för att konfigurera och använda Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) för mer information.
> 
> 

Följande information kan hjälpa dig att konfigurera och använda Azure RemoteApp effektivt.

## <a name="connectivity"></a>Anslutning
* Använd alltid den senaste klientversionen. Med hjälp av äldre klienter leda till problem med nätverksanslutningen och andra försämrad upplevelser. Aktivera automatisk tillämpning uppdateringar för din enhet säkerställer att installeras alltid den senaste klienten.
* Använd alltid stabila och mest tillförlitliga Internetanslutningen tillgängliga för dig.  
* Endast stöd för att använda proxy-anslutningar för optimala prestanda.  SOCKS-proxy stöds inte.

## <a name="applications"></a>Program
* Spara och stänga RemoteApp-program när du är klar med programmet. Inte stänga programmet kan resultera i förlust av data.
* Verifiera anpassade program innan du använder dem i Azure RemoteApp. Detta inkluderar att säkerställa de fungerar på en plattform för flera sessioner och inte använda onödiga resurser, till exempel minne och processor som kan strypa en annan användare i samma samling. Information, hämta och granska de [Application Compatibility bästa praxis för Fjärrskrivbordstjänster](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfiguration och hantering
* Kontinuerligt mallavbildningar, programuppdateringar och andra kritiska korrigeringar installeras vid behov. Detta säkerställer att som automatiskt Azure RemoteApp-skalor för att uppfylla dina kapacitet, korrigeras varje instans.  
* Kontrollera distributionen av Active Directory Federation Services (AD FS) är säkert och tillförlitligt. Annars misslyckas klienten autentiseringar, hindrar användare från att komma åt Azure RemoteApp.
* Konfigurera mallavbildningarna med installerade program, roller eller funktioner som finns tillståndslösa. De bör inte lita på alla instanser av de virtuella datorerna i en RemoteApp-tjänsten är i ett beständigt tillstånd.
  * Lagra alla användardata i användarprofiler eller andra lagringsplatser som är externa för tjänsten, t.ex. lokala filen resurser eller OneDrive.
  * Lagra delade data i lagringsplatser som är externa för tjänsten, t.ex. lokala filen resurser eller OneDrive.
  * Konfigurera inställningar för hela systemet i mallavbildningen i stället för på enskilda virtuella datorer i en tjänst.
  * Inaktivera automatiska uppdateringar av publicerade program – i stället använda dem manuellt på mallavbildningen och testa dem innan du distribuerar från mallen.

