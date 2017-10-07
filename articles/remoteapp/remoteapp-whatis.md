---
title: "aaaWhat är Azure RemoteApp? | Microsoft Docs"
description: "Lär dig hur tooshare appar och resurser tooany enhet via Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: d7a8a311-e70a-4463-ac85-c7f62c500921
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e13909f3a5698f58c7e4348f3ce53f43560f9b1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-remoteapp"></a>Vad är Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp ger hello funktionerna i hello lokalt Microsoft RemoteApp-program backas upp av Fjärrskrivbordstjänster tooAzure. Azure RemoteApp kan användarna få säker åtkomst tooapplications från många olika användarenheter. Azure RemoteApp i princip är värd för icke-beständig Terminal Server-sessioner i hello molnet, och du får toouse dem och dela dem med dina användare.

Med Azure RemoteApp kan du dela program och resurser med användarna på nästan vilken typ av enhet som helst. Vi värd för dina appar i hello moln, vilket innebär att vi tar hand om hello maskinvara och skalning toomeet användarkrav. Du har toodo bara överföra hello appar du vill tooshare och hämta dina användare toouse apparna. [Användarna får tookeep sina egna enheter](remoteapp-clients.md), medan du administrerar allt via hello Azure-portalen. Du har även hello möjlighet att använda företagets inloggningsuppgifter, så att du kan kontrollera hello säkerheten för program och data.

Fortsätt läsa för mer information om Azure RemoteApp, eller [prova nu](https://azure.microsoft.com/services/remoteapp/) om du redan vet vad du vill.

Har du frågor om Azure RemoteApp? Läs våra [vanliga frågor och svar](remoteapp-faq.md).

Azure RemoteApp är en del av hello [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nyhet!** Vill du toolearn mer om Azure RemoteApp? Eller vill toovalidate Azure RemoteApp i större skala? Ansluta till vår vecka [fråga hello experter webbseminariet](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp-samlingar
Det finns två typer av [Azure RemoteApp](remoteapp-collections.md)-samlingar: moln- och hybridsamlingar.

* En **molnsamling** är värd för och lagrar data för programmen i hello molnet. Användarna kan öppna programmen genom att logga in på sitt Microsoftkonto eller med sina inloggningsuppgifter till företaget, synkroniserat eller federerat med Azure Active Directory.
  
    Välj en molnsamling när hello-program som du vill tooshare inte kräver en anslutning tooany resurs företagets privata nätverk (till exempel via en VPN-enhet). Om programmet hello använder resurser på hello Internet, OneDrive eller Azure, fungerar en molnsamling för dig. Det är också hello snabbaste toocreate.
* En **hybridsamling** är värd för och lagrar data i hello Azure-molnet men kan användare komma åt data och resurser i ditt lokala nätverk. Användarna kan öppna programmen genom att logga in med sina inloggningsuppgifter till företaget , synkroniserat eller federerat med Azure Active Directory.
  
    Välj en hybridsamling om du behöver en anslutning tooresources i företagets privata nätverk. Om hello måste programmet åtkomst tooone av hello följande:
  
  * filservrar som finns i ert intranät
  * Quicken
  * databaser bakom en brandvägg
    
    Det här är lösningen passar bättre för stora företag med mycket resurser i sitt privata nät som inte kan flyttas toohello moln.

hello olika samlingar har olika alternativ, till exempel nätverk, så fundera på [vilken samling](remoteapp-collections.md) passar dig bäst. 

### <a name="updating-your-collection"></a>Uppdatera samlingen
En av hello viktiga skillnader mellan hello hybrid- och molnsamlingar är hur programuppdateringar hanteras. Med en molnsamling som använder hello förinstallerade Office 365 ProPlus eller Office 2013-avbildningen har inte tooworry om eventuella uppdateringar. hello service sker automatiskt och uppdateras kontinuerligt, tooboth appar och hello operativsystemet.

För hybridsamlingar, liksom molnsamlingar med egeninställd mallavbildning har ansvar för underhållet hello avbildningen och programmen. För domänanslutna avbildningar kan du styra uppdateringarna med verktyg som Windows Update, grupprinciper eller System Center.

När du har uppdaterat en egeninställd mallavbildning överför hello ny avbildning toohello Azure-molnet och uppdatera hello samling toouse hello ny avbildning. (Du kan göra detta från hello Azure RemoteApp **Snabbstart** sidan eller hello instrumentpanelen.)

Se artikeln om att [uppdatera samlingar ](remoteapp-update.md) för mer information.

## <a name="supported-remoteapp-clients"></a>Kompatibla RemoteApp-klienter
Azure RemoteApp stöds på hello RemoteApp-klientappar för Windows och Windows RT, samt hello Microsofts fjärrskrivbordsappar för Mac, iOS och Android. Användarna kan använda de här apparna på sina mobila eller compute enheter tooaccess hello nya Azure RemoteApp-program.

Se [åtkomst till dina appar i Azure RemoteApp](remoteapp-clients.md) för mer information om hello-klienter.

## <a name="next-steps"></a>Nästa steg
Sätt igång och prova! I de här artiklarna får du hjälp med att komma igång med Azure RemoteApp:

* [Vilken typ av samling behöver du för Azure RemoteApp?](remoteapp-collections.md)
* [Skapa en Azure RemoteApp-avbildning](remoteapp-imageoptions.md)
* [Hur toocreate en molnsamling av Azure RemoteApp](remoteapp-create-cloud-deployment.md)
* [Hur toocreate en hybridsamling av Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
* [Så här fungerar det med licensiering i Azure RemoteApp](remoteapp-licensing.md)
* [Tips på hur du arbetar med Azure RemoteApp](remoteapp-bestpractices.md)
* [Vanliga frågor och svar om Azure RemoteApp](remoteapp-faq.md)

### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att i tillägg toorating den här artikeln och skriva kommentarer nedan, kan du göra ändringar toohello själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Rulla uppåt och klicka på **Redigera på GitHub** eller **redigera** toomake ändringar - de kommer toous för granskning och sedan, när vi har godkänt dem, visas dina ändringar och förbättringar här.

