---
title: "aaaUse hello Azure Machine Learning-webbtjänster portal | Microsoft Docs"
description: "Hantera åtkomst tooAzure Machine Learning arbetsytor och distribuera och hantera ml – API-webbtjänster"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: jhubbard
editor: cgronlun
ms.assetid: b62cf2ca-dd2a-4a83-bb54-469f948fb026
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: v-donglo
ms.openlocfilehash: 04b49027fc0ab227382b320310088bb66aafacc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-service-using-hello-azure-machine-learning-web-services-portal"></a>Hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal
Du kan hantera dina Machine Learning nya och klassiska Web services med hjälp av hello Microsoft Azure Machine Learning-webbtjänster portal. Eftersom klassiska webbtjänster och nya Web services baseras på olika underliggande tekniker, ha lite olika hanteringsfunktioner för dem.

I hello Machine Learning-webbtjänster portalen kan du:

* Övervaka hur hello webbtjänsten används.
* Konfigurera hello beskrivning, uppdatera hello nycklarna för hello web service (nytt), uppdatera din lagring konto nyckel (endast nytt), aktivera loggning och aktivera eller inaktivera exempeldata.
* Ta bort hello-webbtjänsten.
* Skapa, ta bort eller uppdatera fakturering planer (nytt).
* Lägga till och ta bort slutpunkter (klassisk)

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="permissions-toomanage-new-resources-manager-based-web-services"></a>Behörigheter toomanage nya resurser Manager baserat webbtjänsterna

Nya webbtjänsterna distribueras som Azure-resurser. Du måste därför har hello behörigheter toodeploy och hantera nya webbtjänster.  toodeploy eller hantera nya webbtjänster som du måste tilldelas en deltagare eller administratörsroll hello prenumeration toowhich hello webbtjänsten har distribuerats. Om du bjuda in en annan användare tooa maskininlärning arbetsytan måste du tilldela dem tooa rollen deltagare eller administratör på hello prenumeration innan de kan distribuera eller hantera webbtjänster. 

Om hello användaren inte har hello korrigera behörigheter tooaccess resurser i hello Azure Machine Learning-webbtjänster portal, visas de hello följande fel vid toodeploy en webbtjänst:

*Det gick inte att Web Service-distributionen. Det här kontot har inte tillräcklig åtkomst toohello Azure-prenumeration som innehåller hello arbetsytan. Toodeploy en webbtjänst tooAzure hello måste vara samma konto uppmanas toohello arbetsytan i ordning och att tilldelas åtkomst toohello Azure-prenumeration som innehåller hello arbetsytan.*

Mer information om hur du skapar en arbetsyta finns [skapa och dela en Azure Machine Learning-arbetsytan](machine-learning-create-workspace.md).

Mer information om inställningen åtkomstbehörighet finns [visa åtkomst-tilldelningar för användare och grupper i hello Azure portal – förhandsversion](../active-directory/role-based-access-control-manage-assignments.md).


## <a name="manage-new-web-services"></a>Hantera nya webbtjänster
toomanage din nya webbtjänster:

1. Logga in toohello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portalen med ditt Microsoft Azure-konto - Använd hello-konto som är associerad med hello Azure-prenumeration.
2. På menyn hello **Web Services**.

Detta visar en lista över distribuerade webbtjänster för din prenumeration. 

toomanage en webbtjänst, klicka på webbtjänster. Hello webbtjänster sidan kan du:

* Klicka på hello web service toomanage den.
* Klicka på hello fakturering planera för hello web service tooupdate den.
* Ta bort en webbtjänst.
* Kopiera en webbtjänst och distribuerar den tooanother region.

När du klickar på en webbtjänst hello service Quickstart på webbsidan. hello-webbtjänstsida Snabbstart innehåller två menyalternativ som gör att du toomanage webbtjänsten:

* **INSTRUMENTPANELEN** -tillåter tooview Web service användning.
* **Konfigurera** – tillåter tooadd beskrivande text, uppdaterad hello nyckel för hello storage-konto som är associerade med hello webbtjänst, och aktivera eller inaktivera exempeldata.

### <a name="monitoring-how-hello-web-service-is-being-used"></a>Övervaka hur hello webbtjänsten används
Klicka på hello **INSTRUMENTPANELEN** fliken.

Du kan visa övergripande användning av webbtjänsten under en viss tidsperiod hello instrumentpanel. Du kan välja hello period tooview från hello Period listrutan i hello övre högra hörnet av hello användning diagram. hello instrumentpanelen visar hello följande information:

* **Begäranden över tid** visar ett steg diagram över hello antal begäranden via hello tidsperiod. Det hjälper dig identifiera om det uppstår toppar i användning.
* **Begäran och svar begäranden** visar hello Totalt antal anrop för begäran och svar hello-tjänsten har tagit emot och hello tidsperioden och hur många av dem misslyckades.
* **Genomsnittlig tid för begäran och svar Compute** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.
* **Batch-begäranden** visar hello Totalt antal gruppbegäranden hello-tjänsten har tagit emot och hello tidsperiod och hur många av dem misslyckades.
* **Genomsnittlig svarstid för jobbet** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.
* **Fel** visar hello sammanlagda antalet fel som har inträffat på anrop toohello-webbtjänsten.
* **Services kostnader** visar hello kostnader för hello faktureringsplan som associeras med hello-tjänsten.

### <a name="configuring-hello-web-service"></a>Konfigurera hello-webbtjänsten
Klicka på hello **konfigurera** menyalternativet.

Du kan uppdatera hello följande egenskaper:

* **Beskrivning** kan du tooenter en beskrivning för hello webbtjänsten.
* **Rubrik** kan du tooenter en rubrik för hello-webbtjänst
* **Nycklar** kan du toorotate dina primära och sekundära API-nycklar.
* **Lagringskontonyckel** kan du tooupdate hello nyckel för hello storage-konto som är associerade med hello Web-tjänsten ändras. 
* **Aktivera exempeldata** kan du tooprovide exempeldata som du kan använda tootest hello svar på begäranden tjänsten. Om du har skapat hello-webbtjänsten i Machine Learning Studio hello exempeldata hämtas från hello data din används tootrain din modell. Om du har skapat hello service programmässigt hämtas hello data från hello exempeldata som du angav som en del av hello JSON-paketet.

### <a name="managing-billing-plans"></a>Hantera fakturerings-planer
Klicka på hello **planer** menyalternativet från hello webbsida services Snabbstart. Du kan också klicka på hello plan som är kopplade till specifika Web service toomanage som planerar.

* **Nya** kan du toocreate en ny plan.
* **Lägg till/ta bort Plan instans** kan du för ”skala ut” en befintlig plan tooadd kapacitet.
* **Uppgradering/nedgradering** kan du för ”skala upp” en befintlig plan tooadd kapacitet.
* **Ta bort** kan du toodelete en plan.

Klicka på en plan tooview dess instrumentpanel. hello instrumentpanelen ger ett ögonblicksbild eller planera användning för en vald tidsperiod. tooselect Hej tid period tooview, klicka på hello **Period** listrutan i hello övre högra hörnet på instrumentpanelen. 

hello plan instrumentpanelen innehåller hello följande information:

* **Planera beskrivning** visar information om hello kostnader och kapacitet som är associerade med hello plan.
* **Planera användning** visar hello antal transaktioner och beräkningstimmar som har belastat hello plan.
* **Webbtjänster** visar hello antalet webbtjänster som använder den här planen.
* **De främsta av webbtjänstanrop** visar hello upp fyra webbtjänster som gör anrop som debiteras mot hello plan.
* **De främsta Web Services genom att beräkna timmar** visar hello upp fyra webbtjänster som använder beräkningsresurser som debiteras mot hello plan.

## <a name="manage-classic-web-services"></a>Hantera klassiska webbtjänster
> [!NOTE]
> hello procedurerna i det här avsnittet är relevanta toomanaging klassisk webbtjänster via hello Azure Machine Learning-webbtjänster portal. Information om hur du hanterar klassiska Web services via hello Machine Learning Studio och hello Azure klassiska portal, se [hantera en Azure Machine Learning-arbetsytan](machine-learning-manage-workspace.md).
> 
> 

toomanage klassiska webbtjänster:

1. Logga in toohello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/quickstart) portalen med ditt Microsoft Azure-konto - Använd hello-konto som är associerad med hello Azure-prenumeration.
2. På menyn hello **klassiska webbtjänster**.

toomanage klassiska webbtjänst, klickar du på **klassiska webbtjänster**. Hello klassiska webbtjänster sidan kan du:

* Klicka på tooview hello associerade slutpunkter för hello webbtjänster.
* Ta bort en webbtjänst.

När du hanterar en klassiska webbtjänst kan hantera du varje hello slutpunkter separat. När du klickar på en webbtjänst i hello webbtjänster sidan öppnas hello listan över slutpunkter som är associerade med hello-tjänsten. 

Hello klassiska webbtjänsten slutpunkt på sidan kan du lägga till och ta bort slutpunkter på hello-tjänsten. Mer information om att lägga till slutpunkter finns [skapar slutpunkter](machine-learning-create-endpoint.md).

Klicka på en hello slutpunkter tooopen hello service Quickstart webbsida. På sidan Snabbstart hello finns två menyalternativ som gör att du toomanage webbtjänsten:

* **INSTRUMENTPANELEN** -tillåter tooview Web service användning.
* **Konfigurera** -tillåter tooadd beskrivande text, aktivera fel loggar in och ut uppdaterad hello nyckel för hello storage-konto som associeras med hello webbtjänst, och aktivera och inaktivera exempeldata.

### <a name="monitoring-how-hello-web-service-is-being-used"></a>Övervaka hur hello webbtjänsten används
Klicka på hello **INSTRUMENTPANELEN** fliken.

Du kan visa övergripande användning av webbtjänsten under en viss tidsperiod hello instrumentpanel. Du kan välja hello period tooview från hello Period listrutan i hello övre högra hörnet av hello användning diagram. hello instrumentpanelen visar hello följande information:

* **Begäranden över tid** visar ett steg diagram över hello antal begäranden via hello tidsperiod. Det hjälper dig identifiera om det uppstår toppar i användning.
* **Begäran och svar begäranden** visar hello Totalt antal anrop för begäran och svar hello-tjänsten har tagit emot och hello tidsperioden och hur många av dem misslyckades.
* **Genomsnittlig tid för begäran och svar Compute** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.
* **Batch-begäranden** visar hello Totalt antal gruppbegäranden hello-tjänsten har tagit emot och hello tidsperiod och hur många av dem misslyckades.
* **Genomsnittlig svarstid för jobbet** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.
* **Fel** visar hello sammanlagda antalet fel som har inträffat på anrop toohello-webbtjänsten.
* **Services kostnader** visar hello kostnader för hello faktureringsplan som associeras med hello-tjänsten.

### <a name="configuring-hello-web-service"></a>Konfigurera hello-webbtjänsten
Klicka på hello **konfigurera** menyalternativet.

Du kan uppdatera hello följande egenskaper:

* **Beskrivning** kan du tooenter en beskrivning för hello webbtjänsten. Beskrivningen är ett obligatoriskt fält.
* **Loggning av** kan du tooenable eller inaktivera felloggning på hello slutpunkt. Mer information om loggning finns i Aktivera [loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).
* **Aktivera exempeldata** kan du tooprovide exempeldata som du kan använda tootest hello svar på begäranden tjänsten. Om du har skapat hello-webbtjänsten i Machine Learning Studio hello exempeldata hämtas från hello data din används tootrain din modell. Om du har skapat hello service programmässigt hämtas hello data från hello exempeldata som du angav som en del av hello JSON-paketet.

## <a name="grant-or-suspend-access-tooweb-services-for-users-in-hello-portal"></a>Bevilja eller pausa tooWeb tjänster för användare i hello-portalen
Använda hello klassiska Azure-portalen kan du tillåta eller neka åtkomst toospecific användare.

### <a name="access-for-users-of-new-web-services"></a>Åtkomst för användare av nya webbtjänster
tooenable andra användare toowork med webbtjänster i portalen för hello Azure Machine Learning-webbtjänster måste du lägga till dem som co-administratörer på Azure-prenumerationen.

Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med Microsoft Azure-konto, Använd hello-konto som är kopplad till hello Azure-prenumeration.

1. Hello navigeringsfönstret, klicka på **inställningar**, klicka på **administratörer**.
2. Hello längst ned i hello-fönstret klickar du på **Lägg till**. 
3. Ange hello hello person som du vill tooadd som medadministratör och välj sedan hello-prenumeration som du vill hello medadministratör tooaccess e-postadress i hello ADD A CO-administratör dialogrutan.
4. Klicka på **Spara**.

### <a name="access-for-users-of-classic-web-services"></a>Åtkomst för användare av klassiska webbtjänster
toomanage en arbetsyta:

Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med Microsoft Azure-konto, Använd hello-konto som är kopplad till hello Azure-prenumeration.

1. Klicka på hello Microsoft Azure-tjänster Kontrollpanelen **MASKININLÄRNING**.
2. Klicka på hello-arbetsyta som du vill toomanage.
3. Klicka på hello **konfigurera** fliken.

Från fliken Konfiguration hello du pausa åtkomst toohello Machine Learning-arbetsytan genom att klicka på **NEKA**. Användare kommer inte längre att kunna tooopen hello arbetsyta i Machine Learning Studio. toorestore åtkomst, klickar du på **TILLÅT**.

toospecific användare:

toomanage ytterligare konton som har åtkomst toohello arbetsyta i Machine Learning Studio klickar du på **inloggning tooML Studio** i hello **INSTRUMENTPANELEN** fliken. Detta öppnar hello arbetsyta i Machine Learning Studio. Klicka på hello från den här **inställningar** fliken och sedan **användare**. Du kan klicka på **bjuda in fler användare** toogive användare åtkomst toohello arbetsytan, eller välj en användare och klicka på **ta bort**.

> [!NOTE]
> Hej **inloggning tooML Studio** länken öppnar Machine Learning Studio med hello Account du för närvarande är inloggad på. Hej Account du använde toosign i toohello Azure klassiska portal toocreate en arbetsyta har automatiskt inte behörighet tooopen arbetsytan. tooopen en arbetsyta, måste du vara inloggad i toohello Account som har definierats som hello ägare av hello arbetsytan eller måste tooreceive en inbjudan från hello ägare toojoin hello arbetsytan.
> 
> 

