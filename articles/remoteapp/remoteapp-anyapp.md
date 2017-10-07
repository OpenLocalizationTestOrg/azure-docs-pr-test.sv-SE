---
title: "aaaRun en Windows-app på valfri enhet med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooshare en Windows-app med dina användare via Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 961d40ca-9673-4977-aa54-d6b22fc61ce1
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 750f3b881e0cb671ff6e8f3e851bccdf2262d156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Köra en Windows-app på valfri enhet med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Du kan köra ett Windows-program var som helst på valfri enhet genom att helt enkelt använda Azure RemoteApp. Om det är ett anpassat program som skrevs för 10 år sedan eller en Office-app har användarna inte längre toobe knutna tooa specifikt operativsystem (till exempel Windows XP) för några få program.

Med Azure RemoteApp kan användarna kan också använda sina egna Android eller Apple-enheter och få hello samma användarupplevelse som på Windows (eller Windows Phone-enheter). Du åstadkommer detta genom att värdhantera ditt Windows-program i en samling virtuella Windows-datorer på Azure – vilket innebär att användarna kan komma åt dem från valfri plats med en internetanslutning. 

Innehåller information om ett exempel på exakt hur toodo detta.

I den här artikeln ska vi tooshare åtkomst med alla våra användare. Men du kan använda VALFRI app. Så länge som du kan installera appen på en dator med Windows Server 2012 R2 måste dela du den med hjälp av hello stegen nedan. Du kan granska hello [appkrav](remoteapp-appreqs.md) toomake att appen fungerar.

Observera att eftersom Access är en databas, och vi vill att databasen toobe användbar, går vi igenom några extra steg toolet användare gå hello Access-dataresursen. Om din app inte är en databas eller om du inte behöver dina användare toobe kan tooaccess en filresurs, kan du hoppa över de stegen i den här självstudiekursen

> [!NOTE]
> <a name="note"></a>Du behöver ett Azure-konto toocomplete den här kursen:
> 
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): du får kredit du kan använda tootry ut betald Azure-tjänster och även när de används upp du kan behålla hello kontot och använda kostnadsfria Azure-tjänster, till exempel Websites. Ditt kreditkort kommer aldrig att debiteras, såvida du inte uttryckligen ändrar dina inställningar och be toobe debiteras.
> * Du kan [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): din MSDN-prenumeration ger dig krediter varje månad som kan användas för Azure-betaltjänster.
> 
> 

## <a name="create-a-collection-in-remoteapp"></a>Skapa en samling i RemoteApp
Börja med att skapa en samling. hello samlingen fungerar som en behållare för dina appar och användare. Varje samling är baserad på en avbildning – du kan skapa en egen eller använda en som ingår i din prenumeration. För den här kursen använder vi hello Office 2013-utvärderingsavbildningen - hello appen som vi vill tooshare innehåller.

1. I hello Azure-portalen, bläddrar du nedåt i hello vänstra navigeringsträdet tills du ser RemoteApp. Öppna sidan.
2. Klicka på **Create a RemoteApp collection** (Skapa en RemoteApp-samling).
3. Klicka på **Quick create** (Snabbregistrering) och ange ett namn för samlingen.
4. Välj hello region som du vill toouse toocreate din samling. För bästa möjliga upplevelse med hello, väljer du hello region som ligger närmast geografiskt toohello plats där användarna ska använda hello app. I den här självstudiekursen kommer exempelvis hello användare ligga i Redmond, Washington. hello närmaste Azure-regionen är **västra USA**.
5. Välj hello faktureringsplan som du vill toouse. hello grundläggande faktureringsplanen har 16 användare på en stor Azure VM, medan hello standardfaktureringsplanen har 10 användare på en stor Azure VM. En allmän exempel fungerar hello grundläggande planen bra för arbetsflöde med datainmatning data. För en produktivitetsapp som Office, vill du hello standardplan.
6. Välj slutligen hello Office 2013 Professional-avbildningen. Den här avbildningen innehåller Office 2013-appar. Kom ihåg att den här avbildningen enbart kan användas för utvärderingssamlingar och POC. Det går inte att använda den här avbildningen i en produktionssamling.
7. Klicka på **Create RemoteApp collection** (Skapa RemoteApp-samling).

![Skapa en molnsamling i RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Detta börjar skapa samlingen, men det kan ta upp tooan timme.

Nu kan du är klar tooadd användarna.

## <a name="share-hello-app-with-users"></a>Dela hello appen med användare
När din samling har skapats, är tid toopublish åtkomst toousers och lägga till hello-användare som ska ha åtkomst tooit.

Om du har navigerat bort från hello Azure RemoteApp-noden medan hello samling skapades, börjar du med att gå tillbaka tooit från hello Azure-startsidan.

1. Klicka på hello-samling som du skapade tidigare tooaccess ytterligare alternativ och konfigurera hello samling.
   ![En ny RemoteApp-molnsamling](./media/remoteapp-anyapp/ra-anyappcollection.png)
2. På hello **Publishing** klickar du på **publicera** längst hello hello-skärmen och klicka sedan på **publicera Start-menyn program**.
   ![Publicera ett RemoteApp-program](./media/remoteapp-anyapp/ra-anyapppublish.png)
3. Välj hello-appar som du vill använda toopublish hello-listan. I det här exemplet väljer vi Access. Klicka på **Complete** (Slutför). Vänta tills hello appar toofinish publicering.
   ![Publicera Access i RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)
4. När hello appen har publicerats gå toohello **användaråtkomst** fliken tooadd alla hello-användare som behöver komma åt tooyour appar. Ange användarnas användarnamn (e-postadress) och klicka sedan på **Save** (Spara).

![Lägg till användare tooRemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)

1. Nu är det tid tootell användarna om de nya apparna och hur tooaccess dem. toodo genom att skicka ett e-postmeddelande peka dem toohello fjärrskrivbord nedladdnings-URL.
   ![hello klientens nedladdnings-URL för RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-tooaccess"></a>Konfigurera åtkomst tooAccess
Vissa appar behöver ytterligare konfiguration efter att du har distribuerat dem via RemoteApp. I synnerhet för åtkomst vi kommer toocreate en filresurs på Azure som alla användare har åtkomst till. (Om du inte vill toodo att du kan skapa en [hybridsamling](remoteapp-create-hybrid-deployment.md) [i stället för molnsamlingen] som låter användarna åtkomst till filer och information om det lokala nätverket.) Sedan måste tootell våra användare toomap en lokal enhet på sin dator toohello Azure-filsystemet.

hello första delen du som Hej administratör göra. Sedan har vi några steg för användarna.

1. Börja med att publicera hello-kommandoradsgränssnittet (cmd.exe). I hello **Publishing** väljer **cmd**, och klicka sedan på **publicera > Publicera program med sökväg**.
2. Ange namn på hello hello app och hello sökväg. Använda ”File Explorer” exemplet som hello namn och ”% SYSTEMDRIVE%\windows\explorer.exe” som hello sökväg.
   ![Publicera hello cmd.exe fil.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Nu måste toocreate en Azure [lagringskonto](../storage/common/storage-create-storage-account.md). Vi valde namnet ”accessstorage”, så Välj ett namn som är meningsfullt tooyou. (toomisquote highlander –, det kan finnas endast ett ”accessstorage”.) ![Vårt Azure storage-konto](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Gå nu tillbaka tooyour instrumentpanelen så att du kan få hello sökvägen tooyour lagring (endpoint plats). Var noga med att kopiera och spara sökvägen eftersom du kommer att använda den om en liten stund.
   ![hello sökvägen för lagringskontot](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. När hello storage-konto har skapats måste sedan hello primärnyckeln. Klicka på **hantera åtkomstnycklar**, och sedan kopiera hello primärnyckeln.
6. Nu ange hello kontext för hello storage-konto och skapa en ny filresurs för Access. Kör följande cmdlets i ett upphöjt Windows PowerShell-fönster hello:
   
        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx
   
    För vår resurs kan dessa så är vi kör hello-cmdlets:
   
        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx

Det är nu hello användarens tur.. Uppmana först användarna att installera en [RemoteApp-klient](remoteapp-clients.md). Därefter hello användare behöver toomap som en enhet från deras konto toothat Azure filen delar du skapade och lägga till sina Access-filer. Det gör de på följande sätt:

1. I hello RemoteApp-klienten åtkomst hello publicerade appar. Starta programmet för hello cmd.exe.
2. Kör följande kommando toomap hello en enhet från datorn toohello filresursen:
   
        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>
   
    Om du ställer in hello **/ persistent** parametern tooyes hello mappade enheten beständig mellan sessioner.
3. Starta appen i hello Utforskaren från RemoteApp. Kopiera alla Access-filer som du vill toouse i hello delade appen toohello filresurs.
   ![Placera Access-filer i en Azure-resurs](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
4. Slutligen öppna Access och öppna sedan hello-databas som du precis har delat. Du bör se dina data i komma igång från hello molnet.
   ![En verklig Access-databas som körs från molnet hello](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Nu kan du använda Access på valfri enhet där en RemoteApp-klient finns installerad.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig att skapa en samling kan du försöka skapa en [samling som använder Office 365](remoteapp-tutorial-o365anywhere.md). Du kan även skapa en [hybridsamling](remoteapp-create-hybrid-deployment.md) som kan komma åt ditt lokala nätverk.

<!--Image references-->

