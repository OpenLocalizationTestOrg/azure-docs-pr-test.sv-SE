---
title: "Hur du använder Office 365-prenumeration med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du kan använda Office 365-prenumeration i Azure RemoteApp för att dela Office-appar."
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: f82b6e23-2b71-47be-846d-fd93f2652f3c
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e58ee0444517074d6b756652d03fc79c2370e69e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Hur du använder Office 365-prenumeration med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Visste du att du kan använda din befintliga prenumeration på Office 365 i Azure RemoteApp för att dela Office-appar från molnet? Läsa på information om dina Office 365 + Azure RemoteApp alternativ, se inklusive länkar till artiklar om Office 365 som hjälper dig det mesta av din prenumeration.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Hur använder Office 365-konton för Azure RemoteApp?
Kolla in Peters ny artikel för all information: [hur du använder Azure RemoteApp med Office 365-användarkonton](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Kan jag använda min prenumeration på Office 365 för att köra Office-program i Azure RemoteApp?
Visst! Faktum är är med hjälp av Office 365-prenumeration det enda sättet att ta dina Office-program till Azure RemoteApp.

(Obs: om distributionen av Azure RemoteApp är levererad av en värd partner, de kanske kan förse dig med Office-licenser baseras på en [Service Provider Licensing Agreement](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))

Fantastiska med Office 365-prenumeration är att den kan du använda samma användarlicens på många olika plattformar och miljöer, inklusive Azure-molnet. När du använder Office-program i Azure RemoteApp behöver du inte köpa ytterligare licenser eller konfigurera befintliga licenser på något särskilt sätt. Allt du behöver är en Office 365-prenumeration som omfattar [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus aktiverar [delade aktivering](https://technet.microsoft.com/library/Dn782860.aspx) – den här funktionen kan tillfälliga användare-baserad aktivering för Office i virtuella och molnmiljöer som Azure RemoteApp (och Remote Desktop Services).

Vilka planer för Office 365 innehålla Office 365 ProPlus? Kolla in den [tjänsttillgängligheten inom varje plan](https://technet.microsoft.com/library/office-365-plan-options.aspx) tabell. Observera att inte alla planer omfattar Office 365 ProPlus (till exempel Office 365 Business plan). Om din plan inte om du uppgraderar till en plan som har (till exempel Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK hur är min Office 365 ProPlus licenser som används med Azure RemoteApp?
Varje användarlicens för Office 365 ProPlus kan en enskild användare aktivera Office-program på upp till 5 datorer plus surfplattor och telefoner. Varje aktivering har registrerats med användaren tills de inaktivera Office på enheten. (Användare kan hantera sina enheter i den [Office 365-portalen](https://portal.office365.com/).)

Med Azure RemoteApp en enskild användare kan logga in på flera datorer på samma dag utan att inse att den. Det är eftersom tjänsten hanterar automatiskt och skalas resurser i molnet, medan användaren ser endast de appar och program som du har delat. För det här scenariot som Office 365 ProPlus har en delad dator aktivering läge – det innebär att användaren inte behöver göras några licenshantering för att komma åt de resurserna och att räknas enskilda datorer inte mot 5 datorn gränsen för aktivering.

Så länge du (admin) tilldela Office 365 ProPlus licenser till dina användare kan de använda Office på sina personliga enheter, samt via Azure RemoteApp-samlingen.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Vilka Office-program kan använda med Office 365 och Azure RemoteApp?
Du kan använda Office 365-prenumeration för att aktivera och dela Office 2013 i Azure RemoteApp-distributioner. Vi stöder för närvarande inte användning av andra versioner av Office med Azure RemoteApp. Detta inkluderar Office 2003, Office 2007, Office 2010 och Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Nyheter om Visio Pro och Project Pro?
Office 365 ProPlus-avbildningen ingår i RemoteApp-prenumeration innehåller både Visio Pro och Project Pro. Men du kan inte aktivera dessa program med hjälp av din prenumeration på Office 365 ProPlus - alla har en egen licens. Du kan aktivera dem i den [Office 365-portalen](https://portal.office365.com/). 

Du saknar licensierar dessa program om du inte vill använda dem. Aktivera bara de program som du vill använda - och hoppa över de andra. Du får se dem i avbildningen, men du kan inte använda dem. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Hur kommer jag igång med Office 365 och Azure RemoteApp?
Nu när du vet information om licensiering för Office 365 få ska igång med i Azure RemoteApp – det är lätt:

När du skapar Azure RemoteApp-samlingen använder den **Office 365 ProPlus (prenumeration krävs)** bild.

![Azure RemoteApp-avbildning med Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)

Den här avbildningen innehåller den senaste versionen av Windows Server och Office 365 ProPlus. När du har konfigurerat din samling (inklusive publishing appar), användarna bara logga in på Azure RemoteApp (med hjälp av sina RemoteApp-klienten) och ange sina autentiseringsuppgifter för Office 365 för alla Office-program. Licenser levereras automatiskt utan sådana eller management krävs.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Kan jag skapa en anpassad avbildning med Office 365 ProPlus?
Du kan skapa en anpassad avbildning för din samling som innehåller Office 365 ProPlus. 2 val – använda Azure-galleriet avbildning som vi tillhandahåller eller du kan skapa en egen anpassad avbildning och installera Office 365 ProPlus det.

### <a name="use-the-azure-gallery-image"></a>Använda Azure-galleriet avbildning
Det enklaste sättet att distribuera Office 365 ProPlus till en samling är att [börja med en av avbildningarna Azure-galleriet](remoteapp-image-on-azurevm.md) ingår i Azure RemoteApp-prenumeration. Se till att välja den **Windows Server värd för fjärrskrivbordssession med Office 365 ProPlus förinstallerat** bild. Sedan installerar några andra appar som du vill använda på avbildningen och du är redo att börja.

### <a name="use-a-custom-image"></a>Använda en anpassad avbildning
Du kan alltid skapa en anpassad avbildning – du kan skapa en [Azure VM](remoteapp-image-on-azurevm.md) eller [skapa avbildningen lokalt](remoteapp-create-custom-image.md) och överföra den till Azure. I båda fallen kan du se till att installera Office 365 ProPlus med hjälp av noden delad dator aktivering. Använd den [distributionsverktyg för Office](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) och följ de [instruktioner](https://technet.microsoft.com/library/Dn782858.aspx) för installation.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Inaktivera automatiska uppdateringar för Office 365 ProPlus i den anpassade avbildningen - viktigt
Den anpassade avbildningen används av Azure RemoteApp som en mall för att lägga till ytterligare resurser som efterfrågan från dina användare ökar. Inaktivera automatiska uppdateringar för att förhindra fördröjningar och anslutningsproblem för Office i avbildningen. Om du inte sedan uppdateras alla resurser som skapats med mallen automatiskt när den startas. Använd i stället standardprocessen för Azure RemoteApp för att uppdatera den anpassade avbildningen. På så sätt du uppdaterar Office-program en gång på mallavbildningen och därefter låta Azure RemoteApp ta hand för att få uppdateringar för dina användare.

Om du vill inaktivera automatiska uppdateringar, lägger du till följande distributionsverktyg för Office-konfigurationsfilen:

        <Updates Enabled="FALSE" />

Nu bör din konfigurationsfil innehålla dessa rader:

        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Hur kan jag uppdatera en bild med Office 365 ProPlus?
Det finns många anledningar för uppdatering av avbildningen i samlingen. Här följer några:

* Hämta de senaste uppdateringarna för Windows 
* Hämta de senaste uppdateringarna för Office 365 ProPlus-program
* Uppdatera ditt anpassade program
* Ändra andra inställningar för själva bilden

Slutpunkt till slutpunkt-anvisningar för att uppdatera din samling om du vill använda den avbildning du uppdaterade finns [här](remoteapp-update.md). Men för information om hur du uppdaterar avbildningen och Office 365 ProPlus, Kolla in följande information.

Du har två alternativ för att uppdatera avbildningen - ersätta bilden med en helt ny eller manuellt uppdatera befintliga avbildningen.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Ersätta bilden med den senaste Azure-galleri avbildningen + Lägg till anpassningar
Med det här alternativet kan du låta Microsoft tar hand om Windows Server och Office 365 ProPlus-uppdateringar. I stället för att uppdatera din befintliga bilden ska du skapa en helt ny avbildning baserat på senaste bild i galleriet. Upprepa sedan de steg som du gjorde före att anpassa avbildningen - installera anpassade appar ändra det bildkonfiguration, osv.

Galleriavbildningar uppdateras regelbundet, så du kan vara lätt och veta att dina appar för Windows Server och Office 365 ProPlus är uppdaterade. Naturligtvis är en kompromiss att du måste tillämpa anpassningarna varje gång du får en ny avbildning. Du kan skapa skript som automatiserar inställningen dina anpassningar.

### <a name="manually-update-your-existing-image"></a>Manuellt uppdatera befintliga avbildningen
Med det här alternativet kan använda du standardverktyg för Windows för att tillämpa uppdateringar på avbildningen. Använd distributionsverktyget för Office för Office 365 ProPlus, hämta och installera de senaste uppdateringarna eller versioner av Office 365 ProPlus.

> [!IMPORTANT]
> Kom ihåg - inaktivera Office 365 ProPlus automatiska uppdateringar.
> 
> 

Behöver du mer information om hur du använder distributionsverktyget för Office för uppdateringar?

* [Distribuera Klicka på Kör för Office 365-produkter med hjälp av distributionsverktyget för Office](https://technet.microsoft.com/library/JJ219423.aspx)
* [Distribuera och uppdatera Office 365 ProPlus med hjälp av distributionsverktyget för Office](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
* [Konfigurera inställningar för Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)

