---
title: aaaManage Machine Learning-arbetsytan | Microsoft Docs
description: "Hantera åtkomst tooAzure Machine Learning arbetsytor och distribuera och hantera ml – API-webbtjänster"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 7bd378d82aa46f4340ca974c7dc5a612c089cdca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Hantera en Azure Machine Learning-arbetsyta

> [!NOTE]
> Information om hur du hanterar Web services hello Machine Learning-webbtjänster portal finns [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).
> 
> 

Du kan hantera Machine Learning arbetsytor i antingen hello Azure-portalen eller hello klassiska Azure-portalen.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-hello-azure-portal"></a>Använd hello Azure-portalen

toomanage en arbetsyta i hello Azure-portalen:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) med ett administratörskonto för Azure-prenumeration.
2. Ange i sökrutan överst hello på hello sidan hello ”datorn learning arbetsytor” och välj sedan **Machine Learning arbetsytor**.
3. Klicka på hello-arbetsyta som du vill toomanage.

Dessutom toohello standard management resursinformation och alternativ som är tillgängliga, kan du:

- Visa **egenskaper** – den här sidan visar hello arbetsytan och resursen och du kan ändra hello prenumeration och resursgrupp som den här arbetsytan är kopplad till.
- **Omsynkronisering Lagringsnycklar** -hello arbetsytan underhåller nycklar toohello storage-konto. Om hello storage-konto ändras nycklar och du kan klicka på **omsynkronisering nycklar** toosynchronize hello nycklar med hello-arbetsyta.

toomanage hello webbtjänster som är associerade med den här arbetsytan kan använda hello Machine Learning-webbtjänster portal. Se [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md) fullständig information.

> [!NOTE]
> toodeploy eller hantera nya webbtjänster som du måste tilldelas en deltagare eller administratörsroll hello prenumeration toowhich hello webbtjänsten har distribuerats. Om du bjuda in en annan användare tooa maskininlärning arbetsytan måste du tilldela dem tooa rollen deltagare eller administratör på hello prenumeration innan de kan distribuera eller hantera webbtjänster. 
> 
>Mer information om inställningen åtkomstbehörighet finns [visa åtkomst-tilldelningar för användare och grupper i hello Azure portal – förhandsversion](../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-hello-azure-classic-portal"></a>Använd hello klassiska Azure-portalen

Du kan hantera dina Machine Learning arbetsytor att med hello klassiska Azure-portalen:

* Övervaka hur hello arbetsytan används
* Konfigurera hello arbetsytan tooallow eller nekar åtkomst
* Hantera webbtjänster som skapats i hello arbetsyta
* Ta bort hello-arbetsytan

Dessutom innehåller hello instrumentpanelen en översikt över arbetsytan-användning och en snabb överblick över din arbetsyta information.  

> [!TIP]
> I Azure Machine Learning Studio, på hello **WEB SERVICES** fliken, du kan lägga till, uppdatera eller ta bort en Machine Learning-webbtjänst.
> 
> 

toomanage en arbetsyta i hello klassiska Azure-portalen:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med Microsoft Azure-konto, Använd hello-konto som är kopplad till hello Azure-prenumeration.
2. Klicka på hello Microsoft Azure-tjänster Kontrollpanelen **MASKININLÄRNING**.
3. Klicka på hello-arbetsyta som du vill toomanage.

hello arbetsytssidan har tre flikar:

* **INSTRUMENTPANELEN** -gör att du tooview arbetsytan användning och information
* **Konfigurera** -tillåter toomanage åtkomst toohello arbetsytan
* **WEBBTJÄNSTER** -tillåter toomanage webbtjänster som har publicerats från arbetsytan

### <a name="toomonitor-how-hello-workspace-is-being-used"></a>toomonitor hur hello arbetsytan används
Klicka på hello **INSTRUMENTPANELEN** fliken.

Du kan visa övergripande användning av arbetsytan och få en snabb överblick över arbetsytan information från hello instrumentpanelen.

* Hej **COMPUTE** diagrammet visar hello beräkningsresurser som används av hello arbetsytan. Du kan ändra hello visa toodisplay relativa eller absoluta värden och du kan ändra hello tidsram som visas i diagrammet hello.
* **Användningsöversikten** visar Azure-lagring som används av hello arbetsytan.
* **Snabböversikten** innehåller en översikt över arbetsyteinformation och länkar.

> [!NOTE]
> Hej **inloggning tooML Studio** länken öppnar Machine Learning Studio med hello Account du för närvarande är inloggad på. Hej Account du använde toosign i toohello Azure klassiska portal toocreate en arbetsyta har automatiskt inte behörighet tooopen arbetsytan. tooopen en arbetsyta, måste du vara inloggad i toohello Account som har definierats som hello ägare av hello arbetsytan eller måste tooreceive en inbjudan från hello ägare toojoin hello arbetsytan.
> 
> 

### <a name="toogrant-or-suspend-access-for-users"></a>toogrant eller pausar åtkomst för användare
Klicka på hello **konfigurera** fliken.

Från fliken för hello-konfiguration kan du:

* Pausa åtkomst toohello Machine Learning-arbetsytan genom att klicka på NEKA. Användare kommer inte längre att kunna tooopen hello arbetsyta i Machine Learning Studio. toorestore åtkomst till, klickar du på TILLÅT.

toomanage ytterligare konton som har åtkomst toohello arbetsyta i Machine Learning Studio klickar du på **inloggning tooML Studio** i hello **INSTRUMENTPANELEN** fliken (se anmärkning för hello som föregår rörande  **Sign-in tooML Studio**). Detta öppnar hello arbetsyta i Machine Learning Studio. Klicka på hello från den här **inställningar** fliken och sedan **användare**. Du kan klicka på **bjuda in fler användare** toogive användare åtkomst toohello arbetsytan, eller välj en användare och klicka på **ta bort**.

### <a name="toomanage-web-services-in-this-workspace"></a>toomanage webbtjänster i den här arbetsytan
Klicka på hello **WEB SERVICES** fliken.

Detta visar en lista över webbtjänster som publicerats från den här arbetsytan.
toomanage en webbtjänst, klicka på hello i hello listan tooopen hello-webbtjänstsida.

En webbtjänst kan ha en eller flera slutpunkter som definierats.

* Du kan definiera flera slutpunkter i tillägg toohello ”standard” slutpunkt. tooadd Hej slutpunkt, klickar du på **hantera slutpunkter** längst hello hello instrumentpanelen tooopen hello Azure Machine Learning-webbtjänster portal.
* toodelete slutpunkt (du inte kan ta bort hello ”standard” endpoint), markerar du kryssrutan för hello hello början av hello endpoint raden och klicka på **ta bort**. Detta tar bort hello endpoint från hello webbtjänsten.
  
  > [!NOTE]
  > Om ett program använder hello webbtjänstslutpunkt när hello slutpunkt har tagits bort, får hello programmet ett fel hello nästa gång den försöker tooaccess hello-tjänsten.
  > 
  > 

Klicka på en Web service endpoint tooopen hello namn den. 

Du kan visa övergripande användning av webbtjänsten under en viss tidsperiod hello instrumentpanel. Du kan välja hello period tooview från hello Period listrutan i hello övre högra hörnet av hello användning diagram. hello instrumentpanelen visar hello följande information:

* **Begäranden över tid** visar ett steg diagram över hello antal begäranden via hello tidsperiod. Det hjälper dig identifiera om det uppstår toppar i användning.
* **Begäran och svar begäranden** visar hello Totalt antal anrop för begäran och svar hello-tjänsten har tagit emot och hello tidsperioden och hur många av dem misslyckades.
* **Genomsnittlig tid för begäran och svar Compute** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.
* **Batch-begäranden** visar hello Totalt antal gruppbegäranden hello-tjänsten har tagit emot och hello tidsperiod och hur många av dem misslyckades.
* **Genomsnittlig svarstid för jobbet** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.
* **Fel** visar hello sammanlagda antalet fel som har inträffat på anrop toohello-webbtjänsten.
* **Services kostnader** visar hello kostnader för hello faktureringsplan som associeras med hello-tjänsten.

Hello Konfigurera sidan kan du uppdatera hello följande egenskaper:

* **Beskrivning** kan du tooenter en beskrivning för hello webbtjänsten. Beskrivningen är ett obligatoriskt fält.
* **Loggning av** kan du tooenable eller inaktivera felloggning på hello slutpunkt. Mer information om loggning finns i Aktivera [loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).
* **Aktivera exempeldata** kan du tooprovide exempeldata som du kan använda tootest hello svar på begäranden tjänsten. Om du har skapat hello-webbtjänsten i Machine Learning Studio hello exempeldata hämtas från hello data din används tootrain din modell. Om du har skapat hello service programmässigt hämtas hello data från hello exempeldata som du angav som en del av hello JSON-paketet.

