---
title: aaaHow toouse Office 365-prenumerationen med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur du använder Office 365-prenumeration i Azure RemoteApp tooshare Office-appar."
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
ms.openlocfilehash: 2648868dd28cbcd93e38461ae6dce25eaa5d5e99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-your-office-365-subscription-with-azure-remoteapp"></a>Hur toouse din Office 365-prenumeration med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Visste du att du kan använda din befintliga Office 365-prenumeration i Azure RemoteApp tooshare Office-appar från hello molnet? Läsa på information om dina Office 365 + Azure RemoteApp alternativ, inklusive länkar tooarticles om Office 365 som hjälper dig se hello mest för din prenumeration.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Hur använder Office 365-konton för Azure RemoteApp?
Kolla in Peters ny artikel för alla hello information: [hur toouse Azure RemoteApp med Office 365-användarkonton](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-toorun-office-applications-in-azure-remoteapp"></a>Kan jag använda min Office 365-prenumeration toorun Office-program i Azure RemoteApp?
Visst! Faktum är är med hjälp av Office 365-prenumeration hello endast sätt toobring RemoteApp för tooAzure dina Office-program.

(Obs: om distributionen av Azure RemoteApp är levererad av en värd partner, de kanske kan tooprovide dig med Office licenser baserat på en [Service Provider Licensing Agreement](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))

hello fantastiska med Office 365-prenumeration är att den kan du använda hello samma användarlicens på många olika plattformar och miljöer, inklusive hello Azure-molnet. När du använder Office-program i Azure RemoteApp kan du inte behöver toopurchase ytterligare licenser eller konfigurera befintliga licenser på något särskilt sätt. Allt du behöver är en Office 365-prenumeration som omfattar [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus aktiverar [delade aktivering](https://technet.microsoft.com/library/Dn782860.aspx) – den här funktionen kan tillfälliga användare-baserad aktivering för Office i virtuella och molnmiljöer som Azure RemoteApp (och Remote Desktop Services).

Vilka planer för Office 365 innehålla Office 365 ProPlus? Kolla in hello [tjänsttillgängligheten inom varje plan](https://technet.microsoft.com/library/office-365-plan-options.aspx) tabell. Observera att inte alla planer omfattar Office 365 ProPlus (till exempel hello Office 365 Business plan). Om din plan inte om du uppgraderar tooa plan som har (till exempel Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK hur är min Office 365 ProPlus licenser som används med Azure RemoteApp?
Varje användarlicens för Office 365 ProPlus kan en enskild användare aktivera Office-program på datorer too5 plus surfplattor och telefoner. Varje aktivering har registrerats med hello användare förrän de inaktivera Office på hello enhet. (Användare kan hantera sina enheter i hello [Office 365-portalen](https://portal.office365.com/).)

Med Azure RemoteApp en enskild användare kan logga in på flera datorer på hello samma dag utan att inse att den. Det beror hello tjänsten hanterar automatiskt och skalas resurser i hello molnet, medan hello användaren ser endast hello appar och program som du har delat. För det här scenariot som Office 365 ProPlus har en delad dator aktivering läge – det innebär att användaren inte behöver toodo någon licens management tooaccess resurserna och att enskilda datorer hello räknas inte mot hello 5 gränsen för aktivering av datorn.

Så länge du (hello admin) tilldela Office 365 ProPlus licenser tooyour användare kan de använda Office på sina personliga enheter, samt via Azure RemoteApp-samlingen.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Vilka Office-program kan använda med Office 365 och Azure RemoteApp?
Du kan använda din Office 365-prenumeration tooactivate och dela Office 2013 i Azure RemoteApp-distributioner. Vi stöder för närvarande inte hello användning av andra versioner av Office med Azure RemoteApp. Detta inkluderar Office 2003, Office 2007, Office 2010 och Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Nyheter om Visio Pro och Project Pro?
hello Office 365 ProPlus-avbildningen som ingår i RemoteApp-prenumeration innehåller både Visio Pro och Project Pro. Men du kan inte använda din Office 365 ProPlus-prenumeration tooactivate dessa program – alla har en egen licens. Du kan aktivera dem i hello [Office 365-portalen](https://portal.office365.com/). 

Du har inte toolicense programmen om du inte vill toouse dem. Aktivera bara hello program toouse - och om du vill hoppa över hello andra. Du fortfarande ser dem i hello bild, men du kan inte använda dem. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Hur kommer jag igång med Office 365 och Azure RemoteApp?
Nu när du vet hello information om licensiering för Office 365 få ska igång med i Azure RemoteApp – det är lätt:

När du skapar Azure RemoteApp-samlingen använder hello **Office 365 ProPlus (prenumeration krävs)** bild.

![Azure RemoteApp-avbildning med Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)

Den här avbildningen innehåller hello senaste versionen av Windows Server och Office 365 ProPlus. När du har konfigurerat din samling (inklusive publishing appar), användarna bara logga in på Azure RemoteApp (med hjälp av sina RemoteApp-klienten) och ange sina autentiseringsuppgifter för Office 365 för alla Office-program. Licenser levereras automatiskt utan sådana eller management krävs.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Kan jag skapa en anpassad avbildning med Office 365 ProPlus?
Du kan skapa en anpassad avbildning för din samling som innehåller Office 365 ProPlus. Det finns alternativ 2 – Använd hello Azure-galleriet bild vi ger eller du kan skapa en egen anpassad avbildning och installera Office 365 ProPlus det.

### <a name="use-hello-azure-gallery-image"></a>Använd hello Azure-galleriet bild
hello enklaste sättet toodeploy Office 365 ProPlus tooa samlingen är för[börja med en av hello Azure galleriavbildningar](remoteapp-image-on-azurevm.md) ingår i Azure RemoteApp-prenumeration. Se till att välja hello **Windows Server värd för fjärrskrivbordssession med Office 365 ProPlus förinstallerat** bild. Sedan installerar några andra appar som du vill använda på avbildningen, och du är bra toogo.

### <a name="use-a-custom-image"></a>Använda en anpassad avbildning
Du kan alltid skapa en anpassad avbildning – du kan skapa en [Azure VM](remoteapp-image-on-azurevm.md) eller [skapa hello avbildning lokalt](remoteapp-create-custom-image.md) och ladda upp den tooAzure. Se till att installera Office 365 ProPlus med hello delade aktivering datornod i båda fallen. Använd hello [distributionsverktyg för Office](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) och följ hello [instruktioner](https://technet.microsoft.com/library/Dn782858.aspx) för installation.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Inaktivera automatiska uppdateringar för Office 365 ProPlus i den anpassade avbildningen - viktigt
Den anpassade avbildningen används av Azure RemoteApp som en mall för att lägga till ytterligare resurser som hello efterfrågan från dina användare ökar. tooprevent fördröjningar och anslutningsproblem, inaktivera automatiska uppdateringar för Office hello bild. Om du inte sedan uppdateras alla resurser som skapats med mallen automatiskt när den startas. Använd i stället hello Azure RemoteApp-standardprocessen för att uppdatera den anpassade avbildningen. På så sätt som du uppdaterar hello Office-program en gång på hello mallavbildningen och därefter låta Azure RemoteApp ta hand om hello uppdateringar hämtas tooyour användare.

toodisable automatiska uppdateringar, Lägg till hello följande toohello distributionsverktyg för Office-konfigurationsfilen:

        <Updates Enabled="FALSE" />

Nu bör din konfigurationsfil innehålla dessa rader:

        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Hur kan jag uppdatera en bild med Office 365 ProPlus?
Det finns många anledningar tooupdate hello avbildningen i samlingen. Här följer några:

* Hämta hello senaste uppdateringarna för Windows 
* Hämta hello senaste Office 365 ProPlus programuppdateringar
* Uppdatera ditt anpassade program
* Ändra andra inställningar för själva hello-bilden

Hello slutpunkt till slutpunkt anvisningar för att uppdatera samlingen toouse hello avbildningen du uppdaterade finns [här](remoteapp-update.md). Men för information om hur tooupdate hello avbildningen och Office 365 ProPlus, kolla hello följande information.

Du har två alternativ för att uppdatera avbildningen - ersätta bilden med en helt ny eller manuellt uppdatera befintliga avbildningen.

### <a name="replace-your-image-with-hello-latest-azure-gallery-image--add-customizations"></a>Ersätta bilden med hello senaste Azure-galleriet avbildning + Lägg till anpassningar
Med det här alternativet kan du låta Microsoft tar hand om hello Windows Server och Office 365 ProPlus-uppdateringar. I stället för att uppdatera din befintliga bilden ska du skapa en helt ny avbildning baserat på hello senaste bild i galleriet. Upprepa sedan de steg som du gjorde före toocustomize hello image - installera anpassade appar, ändra hello avbildningen konfiguration, osv.

Hej galleriavbildningar uppdateras regelbundet, så du kan vara lätt och veta att dina appar för Windows Server och Office 365 ProPlus är in toodate. Naturligtvis är hello kompromiss att du har tooapply anpassningarna varje gång du får en ny avbildning. Du kan skapa skript tooautomate inställningen dina anpassningar.

### <a name="manually-update-your-existing-image"></a>Manuellt uppdatera befintliga avbildningen
Med det här alternativet använder du standard Windows verktyg tooapply uppdateringar toohello bild. För Office 365 ProPlus, Använd hello distribution av Office verktyget toodownload och installera hello senaste uppdateringar eller versioner av Office 365 ProPlus.

> [!IMPORTANT]
> Kom ihåg - inaktivera hello Office 365 ProPlus automatiska uppdateringar.
> 
> 

Behöver du mer information om hur du använder hello distributionsverktyg för Office för uppdateringar?

* [Distribuera Klicka på Kör för Office 365-produkter med hjälp av hello distributionsverktyg för Office](https://technet.microsoft.com/library/JJ219423.aspx)
* [Distribuera och uppdatering av Office 365 ProPlus med hello distributionsverktyg för Office](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
* [Konfigurera inställningar för Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)

