---
title: "aaaGet igång med Lagringsutforskaren (förhandsversion) | Microsoft Docs"
description: "Hantera Azure-lagringsresurser med Lagringsutforskaren (förhandsversion)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/17/2017
ms.author: kraigb
ms.openlocfilehash: 57737b51baace92858eb07c7dbc3139bd7e041f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-storage-explorer-preview"></a>Kom igång med Lagringsutforskaren (förhandsversion)
## <a name="overview"></a>Översikt
Azure Lagringsutforskaren (förhandsversion) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux. I den här artikeln får du lära dig hello ansluter tooand hantera Azure storage-konton på olika sätt.

![Microsoft Azure Lagringsutforskaren (förhandsversion)][15]

## <a name="prerequisites"></a>Krav
* [Hämta och installera Lagringsutforskaren (förhandsversion)](http://www.storageexplorer.com)

## <a name="connect-tooa-storage-account-or-service"></a>Ansluta tooa storage-konto eller tjänst
Lagringsutforskaren (förhandsversion) innehåller flera olika sätt tooconnect toostorage konton. Du kan till exempel:
* Ansluta toostorage konton som är kopplade till dina Azure-prenumerationer.
* Ansluta toostorage konton och tjänster som delas från andra Azure-prenumerationer.
* Ansluta tooand hantera lokal lagring med hjälp av hello Azure Storage-emulatorn. 

Dessutom kan du arbeta med lagringskonton i globala och nationella Azure:

* [Ansluta tooan Azure-prenumeration](#connect-to-an-azure-subscription): hantera lagringsresurser som hör tooyour Azure-prenumeration.
* [Arbeta med lokal utveckling lagring](#work-with-local-development-storage): hantera lokal lagring med hjälp av hello Azure Storage-emulatorn.
* [Koppla tooexternal lagring](#attach-or-detach-an-external-storage-account): hantera lagringsresurser som hör tooanother Azure-prenumeration eller som är under nationella Azure moln med hjälp av hello lagringskontots namn, nyckel och slutpunkter.
* [Koppla ett lagringskonto med hjälp av en SAS](#attach-storage-account-using-sas): hantera lagringsresurser som hör tooanother Azure-prenumeration med hjälp av en signatur för delad åtkomst (SAS).
* [Koppla en tjänst med hjälp av en SAS](#attach-service-using-sas): hantera en specifik lagringstjänst (blobbehållare, kö eller tabell) som tillhör tooanother Azure-prenumeration med hjälp av en SAS.

## <a name="connect-tooan-azure-subscription"></a>Ansluta tooan Azure-prenumeration
> [!NOTE]
> Om du inte redan har ett Azure-konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. I Lagringsutforskaren (förhandsversion) väljer du **Inställningar för Azure-konto**.

    ![Inställningar för Azure-konto][0]

2. hello vänster visar alla hello Microsoft-konton som du har loggat in till. tooconnect tooanother kontot som väljer **Lägg till ett konto**, och följ sedan hello instruktioner toosign in med ett Microsoft-konto som är associerad med minst en aktiv Azure-prenumeration.

    >[!NOTE]
    >Ansluta toonational Azure (till exempel Azure Tyskland Azure Government och Azure Kina inloggning) stöds inte för närvarande. Se hello ”Anslut eller koppla från ett externt lagringskonto” avsnittet om hur tooconnect toonational Azure storage-konton.

3. När du loggar in med ett Microsoft-konto, hello vänster fönsterruta fylls med hello Azure-prenumerationer som är associerade med det kontot. Välj hello Azure-prenumerationer som du vill toowork med och välj sedan **tillämpa**. (Att välja **alla prenumerationer** knapparna för att välja alla eller inga av hello visas Azure-prenumerationer.)

    ![Välja Azure-prenumerationer][3]  
    hello vänster visar hello storage-konton som är associerade med hello valda Azure-prenumerationer.

    ![Valda Azure-prenumerationer][4]

## <a name="connect-tooan-azure-stack-subscription"></a>Anslut tooan Stack för Azure-prenumeration

Information om anslutande tooan Azure Stack-prenumerationen finns [ansluta Lagringsutforskaren tooan Azure Stack prenumeration](azure-stack/azure-stack-storage-connect-se.md).

## <a name="work-with-local-development-storage"></a>Arbeta med lokal utvecklingslagring
Med Lagringsutforskaren (förhandsversion) kan arbeta du mot lokal lagring med hjälp av hello Azure Storage-emulatorn. Den här metoden kan du skriva kod mot och testa lagring utan att ha ett lagringskonto som har distribuerats på Azure, eftersom hello lagringskontot emuleras av hello Azure Storage-emulatorn.

> [!NOTE]
> hello Azure Storage-emulatorn stöds för närvarande endast för Windows.
>
>

1. Hello vänstra rutan i Lagringsutforskaren (förhandsversion), expandera hello **(lokala och bifogad)** > **Lagringskonton** > **(utveckling)** nod.

    ![Noden Lokal utveckling][21]

2. Om du ännu inte har installerat hello Azure Storage-emulatorn är du tillfrågas toodo så via ett informationsfält. Om hello Informationsfältet visas väljer du **Download hello senaste versionen**, och sedan installera hello-emulatorn.

    ![Fråga om att hämta Azure Storage-emulatorn][22]

3. När hello emulatorn har installerats kan du skapa och arbeta med lokala blobbar, köer och tabeller. toolearn hur skriver toowork med varje lagringskonto, finns i hello följande:

    * [Hantera Azure-bloblagringsresurser](vs-azure-tools-storage-explorer-blobs.md)
    * Hantera Azure-filresurslagringsresurser: *kommer snart*
    * Hantera Azure-kölagringsresurser: *kommer snart*
    * Hantera Azure-tabellagringsresurser: *kommer snart*

## <a name="attach-or-detach-an-external-storage-account"></a>Ansluta eller koppla från ett externt lagringskonto
Du kan koppla tooexternal storage-konton så att storage-konton kan delas med Lagringsutforskaren (förhandsversion). Det här avsnittet beskrivs hur tooattach too(and detach from) externa lagringskonton.

### <a name="get-hello-storage-account-credentials"></a>Hämta hello lagringskontouppgifter
tooshare ett externt lagringskonto hello ägaren av det kontot måste först hämta hello autentiseringsuppgifter (kontonamnet och nyckeln) för hello-kontot och sedan dela informationen med hello person som vill tooattach toothat (externa) kontot. Du kan hämta hello lagringskontouppgifter via hello Azure-portalen genom att göra hello följande:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Välj **Bläddra**.

3. Välj **Lagringskonton**.

4. På hello **Lagringskonton** bladet, Välj hello önskad storage-konto.

5. På hello **inställningar** bladet för hello markerad storage-konto, markerar du **åtkomstnycklar**.

    ![Alternativ för åtkomstnycklar][5]

6. På hello **åtkomstnycklar** bladet, kopiera hello **lagringskontonamnet** och **key1** värden som ska användas när du ansluter toohello storage-konto.

    ![Åtkomstnycklar][6]

### <a name="attach-tooan-external-storage-account"></a>Koppla tooan externt lagringskonto
tooattach tooan externa lagringskonto måste hello kontots namn och nyckel. Hej förklaras ”Get hello lagringskontouppgifter” hur dessa värden från tooobtain hello Azure-portalen. Men i hello portal hello kontonyckel kallas **key1**. Så där Lagringsutforskaren (förhandsversion) begär en kontonyckel, ange hello **key1** värde.

1. I Lagringsutforskaren (förhandsversion), väljer **ansluta tooAzure lagring**.

    ![Ansluta tooAzure lagringsalternativ][23]

2. I hello **ansluta tooAzure lagring** dialogrutan Ange hello kontonyckel (hello **key1** värde från hello Azure-portalen), och välj sedan **nästa**.

    > [!NOTE]
    > Du kan ange hello lagringsanslutningssträngen från ett lagringskonto på nationella Azure. Till exempel ange tooconnect tooAzure Tyskland storage-konton, anslutning strängar liknande toohello följande: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<storage_account_key>
    >* EndpointSuffix=core.cloudapi.de
    
    >Du kan hämta hello anslutningssträngen från hello Azure portal i hello samma sätt som beskrivs i hello ”hämta hello lagringskontouppgifter” avsnittet.

    ![Ansluta tooAzure lagring dialogruta][24]

3. I hello **Anslut extern lagring** i dialogrutan hello **kontonamn** rutan, ange hello lagringskontonamn, ange andra inställningar och välj sedan **nästa**.

    ![Dialogrutan Anslut extern lagring][8]

4. I hello **anslutning sammanfattning** dialogrutan Kontrollera hello information. Om du vill toochange något annat markerar **tillbaka** och ange hello önskade inställningar. 

5. Välj **Anslut**.

6. När den har anslutits visas hello externt lagringskonto med **(externt)** läggs toohello lagringskontonamn.

    ![Resultatet av ansluter tooan externt lagringskonto][9]

### <a name="detach-from-an-external-storage-account"></a>Ansluta från ett externt lagringskonto
1. Högerklicka på hello externa lagringskonto som du vill toodetach och välj sedan **Detach**.

    ![Koppla bort lagringsalternativ][10]

2. Markera i hello bekräftelsemeddelandet **Ja** tooconfirm hello vill koppla bort hello externa lagringskontot.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Ansluta ett lagringskonto med hjälp av en SAS
En [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) kan Hej administratör för Azure-prenumeration bevilja tillfällig åtkomst tooa storage-konto utan att behöva tooprovide autentiseringsuppgifter för Azure-prenumeration.

tooillustrate det här scenariot kan vi säga att UserA är administratör för Azure-prenumeration och UserA vill tooallow b tooaccess en storage-konto under en begränsad tid med särskilda behörigheter:

1. Användare a genererar en SAS (som består av hello anslutningssträngen för hello lagringskontot) för en viss tid tidsperiod och med hello önskade behörigheter.

2. Resurser för UserA hello SAS med hello person (i vårt exempel b) som vill ha åtkomst toohello storage-konto.  

3. B använder Lagringsutforskaren (förhandsversion) tooattach toohello konto som tillhör tooUserA med hjälp av hello angivna SAS.

### <a name="get-an-sas-for-hello-account-you-want-tooshare"></a>Hämta en SAS för hello-konto som du vill tooshare
1. I Lagringsutforskaren (förhandsversion) högerklickar du på hello storage-konto som du vill dela och välj sedan **hämta signatur för delad åtkomst**.

    ![Snabbmenyalternativet Hämta SAS][13]

2. I hello **signatur för delad åtkomst** dialogrutan Ange hello tidsintervall och behörigheter som du vill använda för hello kontot och välj sedan **skapa**.

    Dialogrutan ![Hämta SAS][14]  
    Hej **signatur för delad åtkomst** dialogruta öppnas och visar hello SAS.

3. Nästa toohello **anslutningssträngen**väljer **kopiera** toocopy den toohello Urklipp och markera sedan **Stäng**.

### <a name="attach-toohello-shared-account-by-using-hello-sas"></a>Koppla toohello delade kontot med hjälp av hello SAS
1. I Lagringsutforskaren (förhandsversion), väljer **ansluta tooAzure lagring**.

    ![Ansluta tooAzure lagringsalternativ][23]

2. I hello **ansluta tooAzure lagring** dialogrutan Ange hello anslutningssträng och välj sedan **nästa**.

    ![Ansluta tooAzure lagring dialogruta][24]

3. I hello **anslutning sammanfattning** dialogrutan Kontrollera hello information. Välj toomake ändringar **tillbaka**, och ange sedan hello-inställningar som du vill använda. 

4. Välj **Anslut**.

5. När den är ansluten hello storage-konto visas med **(SAS)** läggs toohello kontonamn som du har angett.

    ![Resultatet av anslutna tooan konto med hjälp av SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a>Ansluta en tjänst med en SAS
Hej förklaras ”koppla ett lagringskonto med hjälp av en SAS” hur administratör Azure-prenumeration kan bevilja tillfällig åtkomst tooa storage-konto genom att generera och dela en SAS för hello storage-konto. På samma sätt kan en SAS genereras för en specifik tjänst (blobbehållare, kö eller tabell) i ett lagringskonto.  

### <a name="generate-an-sas-for-hello-service-that-you-want-tooshare"></a>Generera en SAS för hello-tjänsten som du vill tooshare
I det här scenariot kan en tjänst vara en blobbehållare, en kö eller en tabell. toogenerate hello SAS för en tjänst, se:

* [Hämta hello SAS för en blob-behållare](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Hämta hello SAS för en filresurs: *kommer snart*
* Hämta hello SAS för en kö: *kommer snart*
* Hämta hello SAS för en tabell: *kommer snart*

### <a name="attach-toohello-shared-account-service-by-using-hello-sas"></a>Koppla toohello delade kontot tjänsten med hjälp av hello SAS
1. I Lagringsutforskaren (förhandsversion), väljer **ansluta tooAzure lagring**.

    ![Ansluta tooAzure lagringsalternativ][23]

2. I hello **ansluta tooAzure lagring** dialogrutan Ange hello SAS-URI och välj sedan **nästa**.

    ![Ansluta tooAzure lagring dialogruta][24]

3. I hello **anslutning sammanfattning** dialogrutan Kontrollera hello information. Välj toomake ändringar **tillbaka**, och ange sedan hello-inställningar som du vill använda. 

4. Välj **Anslut**.

5. När den är ansluten hello tjänsten visas under hello **(tjänsten SAS)** nod.

    ![Resultatet av koppla tooa delade tjänsten med hjälp av en SAS][20]

## <a name="search-for-storage-accounts"></a>Söka efter lagringskonton
Om du har en lång lista med lagringskonton, är ett snabbt sätt toolocate ett visst lagringskonto toouse hello sökrutan överst hello i hello till vänster.

När du skriver i sökrutan hello vänster hello fönsterruta visar hello storage-konton som matchar hello sökvärdet som du har skrivit in toothat punkt. Till exempel en sökning för alla lagringskonton vars namn innehåller **tarcher** visas i följande skärmbild hello:

![Lagringskontosökning][11]

## <a name="next-steps"></a>Nästa steg
* [Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
