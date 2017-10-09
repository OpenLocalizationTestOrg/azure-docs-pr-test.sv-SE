---
title: "aaaWhat som är nya i Azure RemoteApp? | Microsoft Docs"
description: "Lär dig mer om ändringar och förbättringar gjorts tooAzure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 40f1ba79-80f1-47bd-bf39-f86c03e2306a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c2417164c4e546b26c89767df04127c3f9a1ad60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-azure-remoteapp"></a>Vad är nytt i Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

En av hello fördelarna med Azure RemoteApp är att vi alltid fungerar tooimprove den. Varje gång vi gör att vi ska meddela de här ändringarna.

## <a name="future-updates"></a>Framtida uppdateringar
Hej - Visste du hello Azure RemoteApp-teamet skickar månatliga uppdateringar toohello RDS blogg? Du hittar inte bara vad är ändring i Azure RemoteApp, utan också annan information om hur toouse Fjärrskrivbordstjänster. Checka ut sin blogg [Remote Desktop Services-bloggen](https://blogs.msdn.microsoft.com/rds/), information. Till exempel några veckor sedan de upp en post om [lyfta och flytta arbetsbelastningar med Azure RemoteApp och Azure AD](https://blogs.msdn.microsoft.com/rds/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services/).

## <a name="september-2015"></a>September 2015
* Tillagda Infopath toohello Microsoft Office 365 mallen och galleriet bilden. Om du vill tooshare Infopath kontrollerar du att tooupdate samlingar med hello senaste avbildningen.
* Klientuppdateringar:
  * Windows-klienten uppdateras toomake det möjligt för användare tooshare feedback, särskilt runt anslutningen utfärdar.
  * iOS-klienten uppdateras toofix felmeddelandena och toofix problem där dina autentiseringsuppgifter har upphört att gälla tidigare än förväntat.
* Vi arbetar på att få stöd för Office 2016 testas. När du är klar kan du söka efter uppdaterade bilder.
* Publicerat en ny artikel om hello [skillnaderna mellan moln och hybrid samlingar](remoteapp-collections.md) -detta hjälper dig att välja hello samlingstyp som passar bäst för din apps - endast molnbaserad molnet + VNET eller hybrid.
* Vill du tooshare QuickBooks med Azure RemoteApp men inte är säker hello steg? Checka ut [Erics ny artikel](remoteapp-quickbooks.md) uppmanar exakt vilka toodo.

## <a name="august-2015"></a>Augusti 2015
Stor ändringar gjordes i augusti – följer hello visar:

* Nu kan du använda ett virtuellt Azure-nätverk med en molnsamling! Kolla in hello [moln skapa instruktioner](remoteapp-create-cloud-deployment.md) för hello nya åtgärder.
* Gör det möjligt tooadd appar toohello ** starta ** menyn för hello Windows RemoteApp-klienten. Appar kommer att visas i hello lista och du kan fästa dem toohello ** starta **-menyn i Windows.
* Lägga till en ny avbildning toohello Virtuella Azure-galleriet - Windows Server värd för fjärrskrivbordssession med Microsoft Office 365 ProPlus.
* Fast hello Mac-klienten så att appar med spärrad windows stoppas frysa.
* Beskrivs hur du kan använda din [Office 365 ProPlus-prenumeration](remoteapp-officesubscription.md) med Azure RemoteApp.
* Beskrivs hur du kan [secure hello appar och data](remoteapp-secure.md) i Azure RemoteApp-samlingen.

## <a name="july-2015"></a>Juli 2015
Juli ange hello etapp för ändringar kommer i augusti, så det inte finns mycket tootalk om nu, främst doc-uppdateringar. Här följer de senaste ändringarna i hello:

* Lägga till en **stöder** fliken toohello portalen så att du enkelt kan komma åt stöd resurser, t.ex. hello-forum.
* Gjorts hello felsökningsinformation för att skapa en hybridsamling. Checka ut [hello senaste och bästa](remoteapp-hybridtrouble.md) felsökningstips som hur tooidentify hello rätt portar tooconfigure för din VNET.
* Beskrivs hur [användardata](remoteapp-upd.md) skapas och sparas i Azure RemoteApp.
* Beskrivs hur för[Lås appar](remoteapp-secure.md).
* Publicerade hello [Azure RemoteApp-cmdlets](https://msdn.microsoft.com/library/mt428031.aspx).
* Och slutligen vi startat en konversation med vissa Azure RemoteApp-användare om terminologi. Sök efter ändringar toohello sätt vi refererar toohello annan samling alternativ.

## <a name="june-2015"></a>Juni 2015
Så många ändringar! hello-teamet har mycket upptagen i juni:

* Förbättrade hello Azure RemoteApp [landningssida](https://www.remoteapp.windowsazure.com/) -checka ut!
* Uppdatera hello programvara i alla hello avbildningar som är tillgängliga som en del av din prenumeration.
* Gjort förbättringar toohybrid samlingar, inklusive Tvingad tunneltrafik support och kontrollera IP-undernät storlek innan du försöker toocreate hello samling.
* Identifierade att hello * jokertecken fungerar inte för webbkameror. Du måste i stället toospecify hello instans-ID eller GUID. Vi kommer att uppdatera hello omdirigering information tooreflect som.
* Gjort det så att du kan lägga till anpassade antivirusprogram tooyour avbildningen när du skapar en mallavbildning från hello Azure-galleriet.

Vi har fler ändringar lansera i juli, så att vi igen med en annan uppdatering snart.

## <a name="may-2015"></a>Maj 2015
Det har ett antal tillägg (och månader) eftersom vi först skapas i det här avsnittet så att den här listan cheats lite och från hello början av mars via kan. Checka ut dessa nya funktioner:

* Automatisera allt - Azure RemoteApp har nu [cmdlets i hello Azure PowerShell-modulen](remoteapp-tutorial-arawithpowershell.md).
* [Skapa en Azure RemoteApp-avbildning från en virtuell Azure-dator](remoteapp-image-on-azurevm.md). Gör överför din anpassade bilden tooAzure mycket snabbare.
* Använd ett virtuellt Azure-nätverk i stället för en RemoteApp VNET tooconnect tooAzure för resurser i företagsnätverket. Vi har uppdaterat hello [anvisningarna för hybridsamling](remoteapp-create-hybrid-deployment.md) toowalk dig genom att skapa ett virtuellt Azure-nätverk (det är steg 1).
* Tal om Vnet kolla [hello ny vägledning](remoteapp-vnetsizing.md) runt VNET storlek gränser och begränsningar.
* Och tal om gränserna - precis vad är hello [tjänstbegränsningarna och standardprogram](../azure-subscription-service-limits.md)?

Vill du toolearn mer om Azure RemoteApp? hello RemoteApp-teamet gällde ut på Ignite några veckor sedan. Checka ut Eric har video- [hello grunderna för Microsoft Azure RemoteApp-hantering och Administration](http://channel9.msdn.com/Events/Ignite/2015/BRK3868).

Behöver toosee Azure RemoteApp i hello verkligt? Kolla in hello [kör en app på valfri enhet som helst](remoteapp-anyapp.md) kursen - den får du veta hur tooshare åtkomst med användarna, inklusive delade hello databasfiler. Vi har också en genomgång [gör Office 365](remoteapp-tutorial-o365anywhere.md) kör hello samma på alla enheter.

Tack för skulle fastna för oss - tillbaka nästa månad med flera uppdateringar.

### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att i tillägg toorating den här artikeln och skriva kommentarer nedan, kan du göra ändringar toohello själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Rulla uppåt och klicka på **Redigera på GitHub** toomake ändringar - de kommer toous för granskning och sedan, när vi har godkänt dem, visas dina ändringar och förbättringar här.

