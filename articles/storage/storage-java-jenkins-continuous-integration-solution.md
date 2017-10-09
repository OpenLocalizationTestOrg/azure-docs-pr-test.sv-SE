---
title: "aaaUsing Azure Storage med en Jenkins kontinuerlig integrationslösning | Microsoft Docs"
description: "Den här kursen visar hur toouse hello Azure blob-tjänst som en lagringsplats för skapa artefakter som skapats av en lösning för kontinuerlig integrering av Jenkins."
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: f4e5ca75-f6cb-4f74-981b-2aa06bb8de45
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: a8a8c6f3b03cf61d7ad98f0b38e9421dd0b47bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Använda Azure Storage med en Jenkins-baserad CI-lösning
## <a name="overview"></a>Översikt
hello visar följande information hur toouse Blob storage som en databas av build artefakter som skapats av en lösning för Jenkins kontinuerlig Integration (KO) eller som en källa för nedladdningsbara filer toobe används i en build-process. En av hello scenarier där det skulle vara praktiskt är när du kodning i en flexibel utvecklingsmiljö (med Java eller andra språk), versioner som körs baserat på kontinuerlig integration och du behöver en lagringsplats för build-artefakter, så att du kan , till exempel dela dem med andra organisationsmedlemmar i, dina kunder eller skapa ett arkiv. Ett annat scenario är om utskriftsjobbet build kräver andra filer, till exempel beroenden toodownload som en del av hello skapa indata.

I den här självstudiekursen kommer du använda hello Azure Storage-plugin-programmet för Jenkins CI som gjorts tillgängliga av Microsoft.

## <a name="overview-of-jenkins"></a>Översikt över Jenkins
Jenkins aktiverar kontinuerlig integration av programvara projektet genom att låta utvecklare tooeasily integrera sina kodändringar och har bygger producerade automatiskt och ofta, vilket ökar hello produktivitet hello-utvecklare. Versioner är en ny version och build artefakter kan vara överförda toovarious databaser. Det här avsnittet visas hur toouse Azure blob storage som hello databasen av hello build artefakter. Den visas också hur toodownload beroenden från Azure blob storage.

Mer information om Jenkins kan hittas på [uppfyller Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-hello-blob-service"></a>Fördelarna med att använda hello Blob-tjänsten
Fördelarna med att använda hello Blob-tjänsten toohost din flexibel utveckling build artefakter inkluderar:

* Hög tillgänglighet av build artefakter och/eller nedladdningsbara beroenden.
* Prestanda när Jenkins CI-lösningen överför build-artefakter.
* Prestanda när kunder och partners hämtar build-artefakter.
* Kontroll över principer för åtkomst, välja mellan anonym åtkomst, förfallodatum-baserad delad åtkomst signatur åtkomst, privat åtkomst, osv.

## <a name="prerequisites"></a>Krav
Du kommer måste hello följande toouse hello Blob-tjänsten med Jenkins CI-lösningen:

* Lösning för Jenkins kontinuerlig integrering.
  
    Du kan köra en Jenkins CI-lösning med hjälp av hello följande metod om du för närvarande inte har en Jenkins CI-lösning:
  
  1. På en dator i Java-aktiverade hämta jenkins.war från <http://jenkins-ci.org>.
  2. Vid en kommandotolk som är öppnade toohello mapp som innehåller jenkins.war, kör du:
     
      `java -jar jenkins.war`

  3. Öppna i din webbläsare `http://localhost:8080/`. Då öppnas hello Jenkins instrumentpanelen, som du kommer att använda tooinstall och konfigurera Azure Storage-plugin-programmet hello.
     
      När en typisk Jenkins CI-lösning skulle ställas in toorun som en tjänst, räcker kör hello Jenkins war hello kommandoraden för den här självstudiekursen.
* Ett Azure-konto. Du kan registrera dig för ett Azure-konto på <http://www.azure.com>.
* Ett Azure-lagringskonto. Om du inte redan har ett lagringskonto, kan du skapa en med hjälp av hello stegen i [skapa ett Lagringskonto](storage-create-storage-account.md#create-a-storage-account).
* Om du är bekant med hello Jenkins CI lösning rekommenderas men krävs inte, eftersom hello följande innehåll kommer att använda en grundläggande exempel tooshow du hello steg som krävs när du använder hello Blob-tjänsten som en lagringsplats för Jenkins CI bygga artefakter.

## <a name="how-toouse-hello-blob-service-with-jenkins-ci"></a>Hur toouse hello Blob-tjänsten med Jenkins CI
toouse hello Blob-tjänsten med Jenkins, behöver du tooinstall hello Azure Storage-plugin-programmet, och konfigurera hello plugin-programmet toouse storage-konto och sedan skapa en efter build-åtgärd som överför din build artefakter tooyour storage-konto. Dessa steg beskrivs i följande avsnitt hello.

## <a name="how-tooinstall-hello-azure-storage-plugin"></a>Hur tooinstall hello Azure Storage-plugin-programmet
1. Inom hello Jenkins instrumentpanelen, klickar du på **hantera Jenkins**.
2. I hello **hantera Jenkins** klickar du på **hantera plugin-program**.
3. Klicka på hello **tillgänglig** fliken.
4. I hello **artefakt Uploaders** avsnitt, kontrollera **plugin-program för Microsoft Azure Storage**.
5. Klicka på antingen **installera utan att starta om** eller **nu hämta och installera efter omstart**.
6. Starta om Jenkins.

## <a name="how-tooconfigure-hello-azure-storage-plugin-toouse-your-storage-account"></a>Hur hello tooconfigure Azure Storage-plugin-programmet toouse storage-konto
1. Inom hello Jenkins instrumentpanelen, klickar du på **hantera Jenkins**.
2. I hello **hantera Jenkins** klickar du på **konfigurera systemet**.
3. I hello **Microsoft Azure Storage-kontokonfigurationen** avsnitt:
   1. Ange namnet på ditt lagringskonto, som du kan hämta från hello [Azure Portal](https://portal.azure.com).
   2. Ange din lagringskontonyckel, även kan erhållas från hello [Azure Portal](https://portal.azure.com).
   3. Använd hello standardvärdet för **slutpunkts-URL för Blob-tjänsten** om du använder hello offentliga Azure-molnet. Om du använder en annan Azure-molnet använda hello slutpunkt som anges i hello [Azure Portal](https://portal.azure.com) för ditt lagringskonto. 
   4. Klicka på **verifiera autentiseringsuppgifterna för lagring** toovalidate ditt lagringskonto. 
   5. [Valfritt] Om du har ytterligare storage-konton som du vill gjorts tillgängliga tooyour Jenkins CI Klicka **lägga till flera Lagringskonton**.
   6. Klicka på **spara** toosave dina inställningar.

## <a name="how-toocreate-a-post-build-action-that-uploads-your-build-artifacts-tooyour-storage-account"></a>Hur toocreate en efter build-åtgärd som överför din build artefakter tooyour storage-konto
För instruktioner, först behöver vi toocreate ett jobb som skapar flera filer och Lägg sedan till i hello efter build åtgärd tooupload hello filer tooyour storage-konto.

1. Inom hello Jenkins instrumentpanelen, klickar du på **nytt objekt**.
2. Hello jobb **MyJob**, klickar du på **ett ledigt format programvara projekt**, och klicka sedan på **OK**.
3. I hello **skapa** avsnittet hello jobbkonfiguration, klickar du på **Lägg till build steg** och välj **köra Windows batch-kommandot**.
4. I **kommandot**, använda hello följande kommandon:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. I hello **efter build åtgärder** avsnittet hello jobbkonfiguration, klickar du på **Lägg till efter build åtgärd** och välj **överför artefakter tooAzure Blob storage**.
6. För **lagringskontonamnet**, Välj hello toouse för storage-konto.
7. För **behållarnamn**, ange hello behållarnamn. (hello behållare skapas om den inte redan finns när hello build artefakter överförs.) Du kan använda miljövariabler, så det här exemplet anger **${JOB_NAME}** som hello behållarnamn.
   
    **Tips**
   
    Nedan hello **kommandot** avsnitt som du angav ett skript för **köra Windows batch-kommandot** är en länk toohello miljövariabler som identifieras av Jenkins. Klicka på den länk toolearn hello miljövariabelnamn och beskrivningar. Observera att miljön variabler som innehåller specialtecken, till exempel hello **BUILD_URL** miljövariabeln, tillåts inte som en behållare eller vanliga virtuella sökvägen.
8. Klicka på **offentliggöra ny behållare som standard** för det här exemplet. (Om du vill toouse privata behållare, måste toocreate en delad åtkomst signatur tooallow åtkomst. Som ligger utanför hello i det här avsnittet. Du kan lära dig mer om signaturer för delad åtkomst på [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Valfritt] Klicka på **ren behållare innan du laddar upp** om du vill att hello behållaren toobe avmarkerad innehåll innan build artefakter överförs (lämna den oskyddad om du inte vill att tooclean hello innehållet i hello behållare).
10. För **lista artefakter tooupload**, ange  **text /*.txt**.
11. För **vanliga virtuella sökvägen för överförda artefakter**, för den här kursen ange **${skapa\_ID} / ${skapa\_nummer}**.
12. Klicka på **spara** toosave dina inställningar.
13. I hello Jenkins instrumentpanelen, klickar du på **skapa nu** toorun **MyJob**. Granska hello konsolens utdata för status. Statusmeddelanden för Azure storage ska tas med i hello konsolens utdata när hello efter build åtgärden startar tooupload build artefakter.
14. Du kan undersöka hello build artefakter genom att öppna hello offentlig blob vid slutförande av hello jobb.
    1. Inloggningen toohello [Azure Portal](https://portal.azure.com).
    2. Klicka på **lagring**.
    3. Klicka på hello lagringskontonamn som du använde för Jenkins.
    4. Klicka på **behållare**.
    5. Klicka på hello behållare med namnet **myjob**, vilket är hello gemen version av hello Jobbnamn som du tilldelade när du skapade hello Jenkins jobb. Namn på behållare och blobbnamnen är gemener (och skiftlägeskänsligt) i Azure-lagring. Inom hello lista över BLOB för hello behållare med namnet **myjob** bör du se **hello.txt** och **date.txt**. Kopiera hello URL för något av dessa objekt och öppna den i webbläsaren. Hello-textfil som överfördes visas som en build-artefakt.

Endast en efter build-åtgärd som överför artefakter tooAzure blob-lagring kan skapas per jobb. Observera att hello efter build åtgärd tooupload artefakter tooAzure blob-lagring kan ange olika filer (inklusive jokertecken) och sökvägar toofiles inom **lista artefakter tooupload** med ett semikolon som avgränsare. Producerar till exempel om din Jenkins skapa JAR-filer och TXT-filer i din arbetsyta **skapa** och du vill tooupload båda tooAzure blob-lagring, använda hello följande hello **lista artefakter tooupload** värde: **skapa /\*.jar; skapa /\*.txt**. Du kan också använda dubbla kolon syntax toospecify en sökväg toouse inom hello blob-namnet. Om du vill hello burkar tooget överförs med hjälp av till exempel **binärfiler** i hello-blobbsökvägen och tooget för hello TXT-filer överförs med hjälp av **meddelanden** hello blobbsökvägen använder hello följande för hello  **Lista över artefakter tooupload** värde: **skapa /\*. jar::binaries; skapa /\*. txt::notices**.

## <a name="how-toocreate-a-build-step-that-downloads-from-azure-blob-storage"></a>Hur toocreate ett build-steg som laddas ned från Azure blob storage
hello följande steg visar hur tooconfigure en build steg toodownload objekt från Azure blob storage. Detta kan vara användbart om du vill tooinclude objekt i din build exempelvis burkar som du behåller i Azure blob storage.

1. I hello **skapa** avsnittet hello jobbkonfiguration, klickar du på **Lägg till build steg** och välj **ladda ned från Azure Blob storage**.
2. För **lagringskontonamnet**, Välj hello toouse för storage-konto.
3. För **behållarnamn**, ange hello-behållare som har hello blob som du vill toodownload hello namn. Du kan använda miljövariabler.
4. För **blobbnamnet**, ange hello blob namn. Du kan använda miljövariabler. Du kan också använda en asterisk som jokertecken när du har angett hello första bokstaven i hello blob-namnet. Till exempel **projekt\***  anger alla BLOB vars namn börjar med **projekt**.
5. [Valfritt] För **hämtningssökvägen**, ange hello sökväg på hello Jenkins datorn där du vill att toodownload filer från Azure blob storage. Miljövariabler kan också användas. (Om du inte anger ett värde för **hämtningssökvägen**, hello filer från Azure blob storage blir hämtade toohello jobbet arbetsytan.)

Om du har ytterligare objekt som du vill toodownload från Azure blob storage kan skapa du ytterligare build steg.

När du kör en version, kan du kontrollera hello bygga historik konsolens utdata eller titta på din nedladdningsplatsen, toosee om hello blob som du förväntade dig har laddats ned.  

## <a name="components-used-by-hello-blob-service"></a>Komponenter som används av hello Blob-tjänsten
hello följande innehåller en översikt över hello Blob-tjänstens komponenter.

* **Lagringskontot**: komma åt tooAzure Storage görs genom ett lagringskonto. Detta är hello högsta nivån av hello namnområdet för åtkomst till blobbar. Ett konto kan innehålla ett obegränsat antal behållare, så länge som deras sammanlagda storlek är under 100TB.
* **Behållaren**: en behållare grupperar en uppsättning blobbar. Alla blobbar måste vara i en behållare. Ett konto kan innehålla ett obegränsat antal behållare. En behållare kan lagra ett obegränsat antal blobbar.
* **BLOB**: en fil av valfri typ och storlek. Det finns två typer av blobbar som kan lagras i Azure Storage: block- och sidblobbar. De flesta filer är blockblobbar. En enda blockblobb kan vara upp too200GB i storlek. Den här kursen använder blockblobar. Sidblobar, en annan blob-datatyp kan vara upp too1TB i storlek och är mer effektivt när adressintervall byte i en fil ändras ofta. Mer information om BLOB finns [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **Webbadressformatet**: Blobbar är adresserbara via följande URL-format hello:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (hello format ovan gäller toohello offentliga Azure-molnet. Om du använder en annan Azure-molnet använder hello slutpunkten inom hello [Azure Portal](https://portal.azure.com) toodetermine URL-slutpunkt.)
  
    Formatet hello ovan `storageaccount` representerar hello namnet på ditt lagringskonto `container_name` representerar hello namnet på din behållare och `blob_name` representerar hello namnet på din blob respektive. Du kan ha flera sökvägar, avgränsade med ett snedstreck inom hello behållarnamn  **/** . hello exempel behållarens namn i den här självstudiekursen har **MyJob**, och **${skapa\_ID} / ${skapa\_nummer}** användes för hello vanliga virtuell sökväg, vilket resulterar i hello blob med en URL till hello följande format:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Nästa steg
* [Uppfyller Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
* [Azure Storage SDK för Java](https://github.com/azure/azure-storage-java)
* [Referens för Azure Storage Client SDK](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API för Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)

Mer information finns också hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).
