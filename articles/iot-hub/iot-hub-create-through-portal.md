---
title: aaaUse hello Azure portal toocreate en IoT-hubb | Microsoft Docs
description: "Hur toocreate, hantera och ta bort Azure IoT Hub via hello Azure-portalen. Innehåller information om prisnivåer, skalning, säkerhet, och messaging konfiguration."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: 383968c90ee7ef3bff85a6c90efbf5f0e8fbb208
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-portal"></a>Skapa en IoT-hubb med hello Azure-portalen

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Den här artikeln beskrivs:

* Hur hello toofind IoT-hubb tjänst i hello Azure-portalen.
* Hur toocreate och hantera IoT-hubbar.

## <a name="where-toofind-hello-iot-hub-service"></a>Där toofind hello IoT-hubb-tjänsten

Du kan hitta hello IoT-hubb service i hello följande platser i hello portal:

* Välj **+ ny**, Välj **Sakernas Internet**.
* Välj i hello Marketplace, **Sakernas Internet**.

## <a name="create-an-iot-hub"></a>Skapa en IoT Hub

Du kan skapa en IoT-hubb med hello följande metoder:

* Hej **+ ny** hello bladet som visas i följande skärmbild som visar hello öppnas. hello stegen för att skapa hello IoT-hubb via den här metoden och via hello marketplace är identiska.
* Välj i hello Marketplace, **skapa** tooopen hello bladet som visas i följande skärmbild som visar hello.

hello följande avsnitt beskrivs hello flera steg toocreate en IoT-hubb:

### <a name="choose-hello-name-of-hello-iot-hub"></a>Välj hello namnet på hello IoT-hubb

toocreate en IoT-hubb, måste du namnge hello IoT-hubb. Det här namnet måste vara unikt inom alla IoT-hubbar.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-hello-pricing-tier"></a>Välj hello prisnivån

Du kan välja mellan fyra nivåer: **lediga**, **Standard 1** och **Standard 2**, och **Standard S3**. hello kostnadsfria nivån kan endast 500 enheter toobe anslutna toohello IoT-hubb och uppåt too8 000 meddelanden per dag.

**Standard S1**: Använd hello S1 edition för IoT-lösningar med ett stort antal enheter som varje generera små mängder data. Varje enhet hello S1 edition kan in too400 000 meddelanden per dag på alla anslutna enheter.

**Standard S2**: Använd hello S2 edition för IoT-lösningar som enheter generera stora mängder data. Varje enhet hello S2 edition kan in too6 miljoner meddelanden per dag mellan alla anslutna enheter.

**Standard S3**: Använd hello S3 edition för IoT-lösningar som genererar stora mängder data. Varje enhet hello S3 edition kan in too300 miljoner meddelanden per dag mellan alla anslutna enheter.

![][4]

> [!NOTE]
> IoT-hubb kan endast en kostnadsfri hubb per Azure-prenumeration.

### <a name="iot-hub-units"></a>IoT-hubbenheter

hello antalet meddelanden som tillåts per enhet per dag beror på din hubb prisnivån. Till exempel om du vill att hello IoT-hubb toosupport ingång av 700 000 meddelanden du två S1 nivå enheter.

### <a name="device-toocloud-partitions-and-resource-group"></a>Enheten toocloud partitioner och resursgruppen.

Du kan ändra hello antalet partitioner för en IoT-hubb. hello standardantalet partitioner är 4 kan du välja ett annat antal hello nedrullningsbara listan.

Du behöver inte tooexplicitly skapar du en tom resursgrupp. När du skapar en resurs kan du välja antingen toocreate en ny eller Använd en befintlig resursgrupp.

![][5]

### <a name="choose-subscription"></a>Välj prenumeration

Azure IoT-hubb automatiskt visar hello Azure-prenumerationer hello-användarkontot är kopplad till. Du kan välja hello Azure-prenumeration tooassociate hello IoT-hubb till.

### <a name="choose-hello-location"></a>Välj hello plats

Hej platsalternativ innehåller en lista över hello regioner där IoT-hubben är tillgängliga.

### <a name="create-hello-iot-hub"></a>Skapa hello IoT-hubb

När alla föregående steg har slutförts kan skapa du hello IoT-hubb. Klicka på **skapa** toostart hello serverdelsprocess toocreate och distribuera hello IoT-hubb med hello-alternativ som du har valt.

Det kan ta några minuter toocreate hello IoT-hubb som det tar tid för hello backend-distribution toorun på hello lämplig plats servrar.

## <a name="change-hello-settings-of-hello-iot-hub"></a>Ändra hello inställningar för hello IoT-hubb

Du kan ändra hello inställningarna för en befintlig IoT-hubb när den har skapats från hello IoT-hubb-bladet.

![][8]

**Delade åtkomstprinciper**: dessa principer definiera hello behörigheter för enheter och tjänster tooconnect tooIoT hubb. Du kan komma åt dessa principer genom att klicka på **principer för delad åtkomst** under **allmänna**. I det här bladet kan du ändra befintliga principer eller lägga till en ny princip.

### <a name="create-a-policy"></a>Skapa en princip

* Klicka på **Lägg till** tooopen ett blad. Här kan du ange hello nya principnamn och hello behörigheter som du vill tooassociate med den här principen enligt hello följande bild:

    Det finns flera behörigheter som kan associeras med dessa principer för delad. Hej **läsa registret** och **registret skrivåtgärder** principer ger Läs- och skrivbehörighet rättigheter toohello identitetsregistret. Att välja alternativ för skrivning av hello automatiskt väljer hello läsa alternativet.

    Hej **tjänsten ansluta** princip ger behörighet tooaccess slutpunkter som **får enhet till moln**. Hej **enhet ansluta** princip ger behörighet för att skicka och ta emot meddelanden med hello IoT-hubb enheten sida slutpunkter.

* Klicka på **skapa** tooadd detta nyskapad princip toohello befintlig lista.

![][10]

## <a name="endpoints"></a>Slutpunkter

Klicka på **slutpunkter** toodisplay en lista över slutpunkter för hello IoT-hubb som du ändrar. Det finns två typer av slutpunkter: slutpunkter som är inbyggda i hello IoT-hubb och slutpunkter som du lägger till toohello IoT-hubb när skapandet.

![][11]

### <a name="built-in-endpoints"></a>Inbyggda slutpunkter

Det finns två inbyggda slutpunkter: **molnet toodevice feedback** och **händelser**.

* **Molnet toodevice feedback** inställningar: den här inställningen har två subsettings: **molnet tooDevice TTL** (time-to-live) och **kvarhållningstiden** (i timmar) för hälsningsmeddelande. När först skapar en IoT-hubb kan ha båda de här inställningarna hello standardvärdet 1 timme. tooadjust inställningarna, använda Hej reglage eller Skriv hello-värden.
* **Händelser** inställningar: den här inställningen har flera subsettings, vilket är skrivskyddade. hello följande lista beskrivs de här inställningarna:

  * **Partitioner**: en är standardvärdet när hello IoT-hubben har skapats. Du kan ändra hello antalet partitioner via den här inställningen.

  * **Händelsenamn hubb-kompatibel och slutpunkten**: när hello IoT-hubb skapas en Händelsehubb har skapats internt att du kanske behöver åtkomst till toounder vissa omständigheter. Du kan anpassa hello Event Hub-kompatibelt namn och en slutpunkt värden men du kan kopiera dem genom att klicka på **kopiera**.

  * **Kvarhållningstiden**: Ange tooone dagen som standard men du kan ändra den med hjälp av hello nedrullningsbara listan. Det här värdet är i dagar för inställningen för hello-enhet till moln.

  * **Konsumentgrupper**: konsumentgrupper aktivera flera läsare tooread meddelanden oberoende av hello IoT-hubb. Alla IoT-hubben har skapats med en förinställd konsumentgrupp. Du kan lägga till eller ta bort konsumenten grupper tooyour IoT Hub genom att använda den här inställningen.

  > [!NOTE]
  > hello förinställd konsumentgrupp inte redigeras eller tas bort.

### <a name="custom-endpoints"></a>Anpassade slutpunkter

Du kan lägga till anpassade slutpunkter på din IoT-hubb med hello-portalen. Från hello **slutpunkter** bladet, klickar du på **Lägg till** på hello översta tooopen hello **lägga till slutpunkten** bladet. Ange information om hello som krävs och klicka sedan på **OK**. Din anpassade slutpunkt visas nu i hello huvudsakliga **slutpunkter** bladet.

![][13]

Du kan läsa mer om anpassade slutpunkter i [referens - IoT-hubbslutpunkter][lnk-devguide-endpoints].

## <a name="routes"></a>Vägar

Klicka på **vägar** toomanage hur IoT-hubb skickar meddelanden från din enhet till moln.

![][14]

Du kan lägga till vägar tooyour IoT-hubb genom att klicka på **Lägg till** hello överst i hello **vägar*** bladet att ange hello krävs information och klicka **OK**. Rutten visas sedan i hello huvudsakliga **vägar** bladet. Du kan redigera en väg genom att klicka på hello listan över vägar. tooenable en väg klickar du på hello lista över vägar och ange hello **aktiverad** växla för**av**. toosave hello ändras, klickar du på **OK** längst hello hello-bladet.

![][15]

## <a name="pricing-and-scale"></a>Priser och skalning

hello priser för en befintlig IoT-hubb kan ändras via hello **priser** inställningar med hello följande undantag:

* I aktuella hello-implementering måste en IoT-hubb med en kostnadsfri SKU kan inte ändra nivåerna tooone av hello betald SKU: er, och vice versa.
* Det kan bara finnas en kostnadsfria nivån IoT-hubb i hello Azure-prenumeration.

![][12]

Du kan flytta från en högre nivå toolower endast när hello antal meddelanden skickade den dagen överstiger hello kvot för hello lägre nivå. Till exempel om hello antalet meddelanden per dag överskrider 400 000, sedan hello-nivå för hello IoT-hubb kan ändras. Men om du ändrar toohello S1 nivå begränsas sedan hello IoT-hubb för dagen.

## <a name="delete-hello-iot-hub"></a>Ta bort hello IoT-hubb

Du kan bläddra toohello IoT-hubb som du vill toodelete genom att klicka på **Bläddra**, och sedan välja hello lämpliga hubb toodelete. toodelete Hej IoT-hubben, klickar du på hello **ta bort** nedan hello IoT-hubbnamnet.

## <a name="next-steps"></a>Nästa steg

Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:

* [Massinläsning hantera IoT-enheter][lnk-bulk]
* [IoT-hubb mått][lnk-metrics]
* [Åtgärder som övervakning][lnk-monitor]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med IoT kant][lnk-iotedge]
* [Skydda din IoT-lösning från hello bakgrund][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
