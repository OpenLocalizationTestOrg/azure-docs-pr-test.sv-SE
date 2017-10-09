---
title: "aaaUse hello Azure Batch-återgivning service toorender i hello molnet | Microsoft Docs"
description: "Rendera jobb på virtuella datorer i Azure direkt från Maya med betalning per användning."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.openlocfilehash: 3fb78d883311bbc3ab62743b7d1b111ffad177cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-rendering-service"></a>Kom igång med hello återgivning av Batch-tjänsten

hello Azure Batch-återgivning service har molnskala återgivning funktioner på grundval av per tillfälle. hello återgivning av Batch-tjänsten hanterar schemaläggning av jobb och kö, hantera fel och försök och automatisk skalning för jobbet render. hello återgivning av Batch-tjänsten stöder Autodesk Maya 3ds Max och Arnold med stöd för andra program kommer snart. hello Batch plugin-program för Maya 2017 gör det enkelt toostart ett återgivning jobb på Azure direkt från skrivbordet. 

## <a name="supported-applications"></a>Program som stöds

hello återgivning av Batch-tjänsten stöder för närvarande hello följande program:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Krav

toouse hello återgivning av Batch-tjänsten, behöver du:

- Ett [Azure-konto](https://azure.microsoft.com/free/). 
- **Ett Azure Batch-konto.** Anvisningar om hur du skapar ett Batch-konto i hello Azure-portalen finns [skapa ett batchkonto med hello Azure-portalen](batch-account-create-portal.md).
- **Ett Azure Storage-konto.** hello tillgångar som används för återgivning jobbet lagras i Azure Storage. Du kan skapa ett lagringskonto automatiskt när du skapar ditt Batch-konto. Du kan också använda ett befintligt lagringskonto. toolearn mer information om lagringskonton finns [hur toocreate, hantera eller ta bort ett lagringskonto i hello Azure-portalen](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

toouse hello Batch plugin-program för Maya, behöver du:

- **Maya 2017**
- **Arnold för Maya**

Du kan också använda hello [Azure-portalen](https://portal.azure.com) toocreate pooler med virtuella datorer som är förkonfigurerad med Maya 3ds Max och Arnold. Du kan använda hello portal toomonitor jobb och diagnos av misslyckade aktiviteter genom att hämta programloggarna och genom att fjärransluta tooindividual virtuella datorer med RDP eller SSH.

## <a name="basic-batch-concepts"></a>Grundläggande begrepp för Batch

Innan du börjar använda hello återgivning av Batch-tjänsten, är det bra toobe bekanta dig med några Batch-begrepp, inklusive compute-noder, pooler och jobb. Mer om Azure Batch i allmänhet finns i toolearn [köra filsystemen parallella arbetsbelastningar med Batch](batch-technical-overview.md).

### <a name="pools"></a>Pooler

Batch är en plattformstjänst för att köra beräkningsintensivt arbete, t.ex. rendering, i en **pool** med **beräkningsnoder**. Varje beräkningsnod i poolen är en virtuell Azure-dator (VM) som kör Linux eller Windows. 

Se mer information om Batch-pooler och datornoder hello [Pool](batch-api-basics.md#pool) och [Compute-nod](batch-api-basics.md#compute-node) avsnitten i [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md).

### <a name="jobs"></a>Jobb

En Batch **jobbet** är en samling aktiviteter som körs på hello compute-noder i en pool. När du skickar ett jobb för återgivning Batch dividerar hello jobbet i aktiviteter och distribuerar hello uppgifter toohello datornoderna hello poolen toorun.

Mer information om batchjobb finns hello [jobbet](batch-api-basics.md#job) i avsnittet [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md).

## <a name="use-hello-batch-plug-in-for-maya-toosubmit-a-render-job"></a>Använd hello Batch plugin-program för Maya toosubmit ett render-jobb

Du kan skicka ett jobb toohello återgivning av Batch-tjänsten direkt från Maya med hello plugin-program för Maya Batch. hello följande avsnitt beskrivs hur tooconfigure hello jobbet från hello plugin-program och skicka in den. 

### <a name="load-hello-batch-plug-in-in-maya"></a>Läsa in hello Batch plugin-programmet i Maya

hello Batch plugin-programmet är tillgängligt på [GitHub](https://github.com/Azure/azure-batch-maya/releases). Packa upp hello tooa dokumentarkiv önskat. Du kan läsa in plugin-programmet hello direkt från hello *azure_batch_maya* directory.

plugin-programmet i Maya tooload hello:

1. Kör Maya.
2. Öppna **Window** (Fönster)  > **Settings/Preferences** (Inställningar)  > **Plug-in Manager** (Plugin-hanteraren).
3. Klicka på **Browse** (Bläddra).
4. Navigera tooand Välj *azure_batch_maya/plug-in/AzureBatch.py*.

### <a name="authenticate-access-tooyour-batch-and-storage-accounts"></a>Autentisera åtkomst tooyour Batch- och Storage-konton

toouse hello plugin-program, behöver du tooauthenticate med ditt Azure Batch och nycklar för Azure Storage-konto. tooretrieve dina nycklar för kontot:

1. Visa hello plugin-programmet i Maya och välj hello **Config** fliken.
2. Navigera toohello [Azure-portalen](https://portal.azure.com).
3. Välj **Batch-konton** hello vänstra menyn. Om det behövs klickar du på **Fler tjänster** och filtrerar på _Batch_.
4. Leta upp hello önskad Batch-kontot i hello-listan.
5. Välj hello **nycklar** menyn objektet toodisplay ditt kontonamn och Kontowebbadressen snabbtangenter:
    - Klistra in URL: en för hello Batch-kontot i hello **Service** i hello Batch plugin-programmet.
    - Klistra in hello kontonamn i hello **Batch-kontot** fältet.
    - Klistra in hello primära konto nyckeln till hello **Batch nyckeln** fältet.
7. Välj Lagringskonton hello vänstra menyn. Om det behövs klickar du på **Fler tjänster** och filtrerar på _Storage_.
8. Leta upp hello önskad Storage-konto i hello-listan.
9. Välj hello **åtkomstnycklar** menyn objektet toodisplay hello lagringskontonamn och nycklar.
    - Klistra in hello lagringskontonamn i hello **Lagringskonto** i hello Batch plugin-programmet.
    - Klistra in hello primära konto nyckeln till hello **Lagringsnyckel** fältet.
10. Klicka på **autentisera** tooensure hello plugin-program kan komma åt båda kontona.

När du har autentiserats hello plugin-program anger hello statusfältet för**autentiserad**: 

![Autentisera dina Batch- och Storage-konton](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Konfigurera en pool för ett renderingsjobb

När du har autentiserat dina Batch- och Storage-konton skapar du en pool för renderingsjobbet. plugin-programmet hello sparar dina val mellan sessioner. När du har skapat din önskad konfiguration du behöver inte toomodify den såvida inte ändras.

hello följande avsnitt vägleder dig genom hello tillgängliga alternativ, tillgängliga på hello **skicka** fliken:

#### <a name="specify-a-new-or-existing-pool"></a>Ange en ny eller befintlig pool

toospecify en pool i vilket toorun hello render projekt, Välj hello **skicka** fliken. På den här fliken kan du välja mellan att skapa en ny pool eller använda en befintlig:

- Du kan **automatiskt etablera en pool för det här jobbet** (hello standardalternativet). När du väljer det här alternativet Batch skapar hello adresspool exklusivt för hello aktuella jobbet och automatiskt borttagningar hello pool när hello återge jobbet har slutförts. Det här alternativet är bäst när du har en enda render jobbet toocomplete.
- Du kan **återanvända en befintlig beständig pool**. Om du har en befintlig adresspool som är inaktiv, kan du ange att poolen för jobb som körs hello render genom att välja den hello listrutan. Återanvända en pool med befintliga beständiga sparar hello tid som krävs för tooprovision hello poolen.  
- Du kan **skapa en ny beständiga pool**. Det här alternativet skapas en ny pool för hello-jobbet. Hello poolen tas inte bort när hello jobbet har slutförts, så att du kan återanvända den för framtida jobb. Välj det här alternativet när du har en kontinuerlig måste toorun återge jobb. På efterföljande jobb, kan du välja **återanvända en pool med befintliga beständiga** toouse hello beständiga poolen som du skapade för hello första jobb.

![Ange pool, OS-avbildning, VM-storlek och licens](./media/batch-rendering-service/submit.png)

Mer information om hur kostnader påförs för virtuella Azure-datorer finns i hello [Linux priser vanliga frågor och svar](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) och [priser vanliga frågor om Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-hello-os-image-tooprovision"></a>Ange hello OS bilden tooprovision

Du kan ange hello typ av OS avbildningen toouse tooprovision beräkningsnoder i poolen hello på hello **Env** fliken (miljö). Batch stöder för närvarande hello följande alternativ för avbildning för att återge jobb:

|Operativsystem  |Bild  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Välja storlek för virtuella datorer

Du kan ange hello VM-storlek på hello **Env** fliken. Mer information om tillgängliga storlekar för virtuella datorer finns i [Linux VM sizes in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) (Storlekar för virtuella Linux-datorer i Azure) och [Windows VM sizes in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) (Storlekar för virtuella Windows-datorer i Azure). 

![Ange hello VM OS-avbildningen och storlek på hello Env fliken](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Ange licensieringsalternativ

Du kan ange hello licenser som du vill toouse på hello **Env** fliken. Alternativen är:

- **Maya**, som är aktiverat som standard.
- **Arnold**, som är aktiverad om Arnold identifieras hello active återgivningsmotorn i Maya.

 Om du vill använda din egen licens toorender måste konfigurera du din licens-slutpunkten genom att lägga till hello miljön variabler toohello tabell. Vara säker på att toodeselect hello standard licensalternativ om du gör detta.

> [!IMPORTANT]
> Du debiteras för användning av hello licenser när virtuella datorer som körs i hello poolen, även om hello virtuella datorer inte är för närvarande används för återgivning. tooavoid extra avgifter navigera toohello **pooler** fliken och ändra storlek på hello poolen too0 noder tills du är klar toorun ett annat jobb som render. 
>
>

#### <a name="manage-persistent-pools"></a>Hantera beständiga pooler

Du kan hantera en befintlig beständiga adresspool på hello **pooler** fliken. Att välja en pool hello listan visar hello hello poolen aktuella tillstånd.

Från hello **pooler** kan du också ta bort hello poolen och ändra storlek på hello antal virtuella datorer i hello pool. Du kan ändra storlek på poolen too0 noder tooavoid medför kostnader between arbetsbelastningar.

![Visa, ändra storlek på och ta bort pooler](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Konfigurera ett renderingsjobb för att skicka det

När du har angett hello parametrar för hello-pool som körs hello render, konfigurera hello jobbet sig själv. 

#### <a name="specify-scene-parameters"></a>Ange scenparametrar

hello Batch plugin-programmet identifierar vilka återgivningsmotor som du använder i Maya och visar hello lämpliga återge inställningar på hello **skicka** fliken. Inställningarna omfattar hello start-ram, end ram, utdata prefix och RAM steg. Du kan åsidosätta hello scen renderingsinställningar genom att ange olika inställningar i hello plugin-programmet. Ändringar du gör toohello plugin inställningar är inte beständiga tillbaka toohello scen Renderingsinställningar, så du kan göra ändringar på grundval av jobbet med jobb utan att behöva tooreupload hello scen filen.

plugin-programmet hello varnar dig om hello återge motor som du valde i Maya inte stöds.

Om du läser in en ny scen hello plugin-program är öppen klickar du på hello **uppdatera** knappen toomake att hello inställningarna uppdateras.

#### <a name="resolve-asset-paths"></a>Matcha sökvägar för resurser

När du läser in plugin-programmet hello söker hello scen filen efter referenser extern fil. Dessa referenser visas i hello **tillgångar** fliken. Om en refererade sökväg inte kan lösas, försöker hello plugin toolocate hello-filen i några standardplatserna, inklusive:

- hello platsen för hello scen fil 
- hello aktuella projektet _sourceimages_ directory
- hello aktuella arbetskatalogen. 

Om hello tillgången fortfarande inte kan hittas, är den i listan med en varningsikon:

![Resurser som saknas visas med en varningsikon](./media/batch-rendering-service/missing_assets.png)

Om du känner till en olöst filreferens hello plats, klickar du på hello varning ikonen toobe uppmanas tooadd sökvägen. hello plugin-programmet sedan använder den här sökningen sökvägen tooattempt tooresolve alla saknade tillgångar. Du kan lägga till valfritt antal ytterligare sökvägar.

När en referens har matchats visas den med en grön knappikon:

![Matchade resurser visas med en grön knappikon](./media/batch-rendering-service/found_assets.png)

Om din scen som kräver andra filer har som plugin-programmet hello inte identifierat, du kan lägga till ytterligare filer och kataloger. Använd hello **Lägg till filer** och **Lägg till katalog** knappar. Om du läser in en ny scen hello plugin-program är öppen vara säker på att tooclick **uppdatera** tooupdate hello scen referenser.

#### <a name="upload-assets-tooan-asset-project"></a>Överför tillgångar tooan tillgången projekt

När du skickar ett render jobb, hello refererar till filerna som visas i hello **tillgångar** fliken är automatiskt överförda tooAzure lagring som ett projekt för tillgången. Du kan också ladda upp hello tillgångsfiler oberoende av ett render jobb med hjälp av hello **överför** hello-knappen **tillgångar** fliken hello tillgången projektets namn har angetts i hello **projekt**fältet och heter efter hello aktuella Maya projekt som standard. När tillgången filerna har överförts behålls hello lokala filstruktur. 

När resurserna har laddats upp kan ett obegränsat antal renderingsjobb referera till dem. Alla överförda tillgångar är tillgängliga tooany jobb som refererar till hello tillgången projekt, oavsett om de ingår i hello scen. toochange hello tillgången projektet som refereras av din nästa jobb, ändra hello namn i hello **projekt** i hello **tillgångar** fliken. Om filerna som du vill tooexclude överför avmarkera dem med hjälp av hello gröna knappen bredvid hello lista.

#### <a name="submit-and-monitor-hello-render-job"></a>Skicka och övervaka hello återge jobb

När du har konfigurerat hello render jobbet i hello plugin-programmet, klickar du på hello **skicka jobbet** hello-knappen **skicka** fliken toosubmit hello jobbet tooBatch:

![Skicka hello render jobbet](./media/batch-rendering-service/submit_job.png)

Du kan övervaka ett jobb som pågår från hello **jobb** fliken på hello plugin-programmet. Välj ett jobb hello listan toodisplay hello aktuell status för hello jobb. Du kan också använda den här fliken toocancel och ta bort jobb, samt toodownload hello utdata och återges loggar. 

toodownload utdata, ändra hello **matar ut** fältet tooset hello önskade målkatalogen. Klicka på hello växeln ikonen toostart bakgrunden som bevakar hello jobb och hämtar utdata när den: 

![Visa jobbstatus och hämta utdata](./media/batch-rendering-service/jobs.png)

Du kan stänga Maya utan att störa hello hämtningen.

## <a name="next-steps"></a>Nästa steg

toolearn mer om batchen, se [köra filsystemen parallella arbetsbelastningar med Batch](batch-technical-overview.md).
