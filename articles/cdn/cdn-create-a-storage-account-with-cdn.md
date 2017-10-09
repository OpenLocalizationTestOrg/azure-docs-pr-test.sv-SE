---
title: aaaIntegrate ett Azure storage-konto med Azure CDN | Microsoft Docs
description: "Lär dig hur toouse hello Azure innehåll innehållsleveransnätverk (CDN) toodeliver hög bandbredd innehåll genom cachelagring av BLOB-objekt från Azure Storage."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e44716969d6a784265cc4b1da34f0d021a17b38d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Integrera Azure storage-konto med Azure CDN
CDN kan vara aktiverad toocache innehåll från ditt Azure storage. Den erbjuder utvecklare en global lösning för att leverera innehåll med hög bandbredd genom cachelagring av blobbar och compute-instanser på fysiska noder i hello USA, Europa, Asien, Australien och Sydamerika statiskt innehåll.

## <a name="step-1-create-a-storage-account"></a>Steg 1: Skapa ett lagringskonto
Använd följande procedur toocreate ett nytt lagringskonto för en Azure-prenumeration hello. Ett lagringskonto ger åtkomst till Azure storage-tjänster. Hej lagringskonto representerar hello högsta nivå av hello namnområdet för åtkomst till var och en av hello Azure storage tjänstkomponenter: Blob-tjänster, Queue-tjänster och tabellen tjänster. Mer information finns i toohello [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).

toocreate ett lagringskonto, du måste vara hello tjänstadministratör eller en medadministratör för hello associerade prenumeration.

> [!NOTE]
> Det finns flera metoder du kan använda toocreate ett lagringskonto, inklusive hello Azure Portal och Powershell.  För den här självstudiekursen kommer vi att använda hello Azure-portalen.  
> 
> 

**toocreate ett lagringskonto för en Azure-prenumeration**

1. Logga in toohello [Azure Portal](https://portal.azure.com).
2. I hello övre vänstra hörnet, Välj **ny**. I hello **ny** väljer **Data + lagring**, klicka på **lagringskonto**.
    
    Hej **skapa lagringskonto** bladet visas.   

    ![Skapa Lagringskonto][create-new-storage-account]  

3. I hello **namnet** skriver du ett underdomännamn. Den här posten kan innehålla 3 till 24 gemena bokstäver och siffror.
   
    Det här värdet blir hello värdnamnet inom hello URI som används för att adressera Blob, kö eller tabell resurser för hello prenumeration. För att lösa en behållare resurs i hello Blob-tjänsten har du använder en URI i hello följande format, där  *&lt;StorageAccountLabel&gt;*  refererar toohello värdet du angav i **ange en URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;minbehållare&gt;*
   
    **Viktigt:** hello URL etikett formulär hello underdomän hello lagringskonto URI och måste vara unika bland alla värdbaserade tjänster i Azure.
   
    Det här värdet används också som hello namnet på det här lagringskontot i hello-portalen eller vid åtkomst till det här kontot programmässigt.
4. Lämna hello standardvärden för **distributionsmodellen**, **konto kind**, **prestanda**, och **replikering**. 
5. Välj hello **prenumeration** att hello storage-konto kommer att användas med.
6. Välj eller skapa en **Resursgrupp**.  Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Välj en plats för ditt lagringskonto.
8. Klicka på **Skapa**. hello processen att skapa hello storage-konto kan ta flera minuter toocomplete.

## <a name="step-2-enable-cdn-for-hello-storage-account"></a>Steg 2: Aktivera CDN för hello storage-konto

Med senaste hello-integration kan du nu aktivera CDN för ditt lagringskonto utan att lämna lagring-portaltillägg. 

1. Välj hello lagringskonto, söka ”CDN” eller rulla nedåt från hello vänstra navigeringsmenyn och klicka på ”Azure CDN”.
    
    Hej **Azure CDN** bladet visas.

    ![CDN aktivera navigering][cdn-enable-navigation]
    
2. Skapa en ny slutpunkt genom att ange hello krävs information
    - **CDN-profilen**: du kan skapa en ny eller använda en befintlig profil.
    - **Prisnivån**: behöver du bara tooselect en prisnivå om du skapar en ny CDN-profil.
    - **Namnet på slutpunkten CDN**: Ange en slutpunktsnamn per valet.

    > [!TIP]
    > hello skapa CDN-slutpunkten använder hello värdnamnet för ditt lagringskonto som ursprung som standard.

    ! [cdn-slutpunkten skapandet av ny] [cdn--slutpunkt-skapa nya]

3. När den har skapats visas hello ny slutpunkt i hello endpoint listan ovan.

    ![CDN-slutpunkten ny lagring][cdn-storage-new-endpoint]

> [!NOTE]
> Du kan även gå tooAzure CDN tillägget tooenable CDN. [Kursen](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>Steg 3: Aktivera ytterligare CDN-funktioner

Klicka på hello CDN-slutpunkten från hello listan tooopen CDN configuration bladet storage-konto ”Azure CDN” bladet. Du kan aktivera ytterligare CDN-funktioner för leveransen, till exempel komprimering, frågesträngen geo filtrering. Du kan också lägga till anpassade domäner mappning tooyour CDN-slutpunkten och aktivera anpassad domän för HTTPS.
    
![cdn-CDN lagringskonfigurationen][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>Steg 4: Åtkomst CDN innehåll
tooaccess cachelagrat innehåll på hello CDN, Använd hello CDN-URL som angetts i hello-portalen. hello-adress för en cachelagrade blob är liknande toohello följande:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> När du aktiverar CDN åtkomst tooa storage-konto, är alla offentligt tillgängliga objekt berättigade för cachelagring av CDN kant. Om du ändrar ett objekt som för tillfället är cachelagrade i hello CDN hello nytt innehåll inte tillgängliga via hello CDN förrän hello CDN uppdateras dess innehåll när hello cachelagrat innehåll time to live-period har löpt ut.
> 
> 

## <a name="step-5-remove-content-from-hello-cdn"></a>Steg 5: Ta bort innehåll från hello CDN
Om du inte längre vill toocache ett objekt i hello Azure Content Delivery Network (CDN) måste göra du något av följande hello:

* Du kan göra hello behållaren privat i stället för allmänheten. Se [Hantera anonym läsbehörighet toocontainers och blobbar](../storage/blobs/storage-manage-access-to-resources.md) för mer information.
* Du kan inaktivera eller ta bort hello CDN-slutpunkten med hello-hanteringsportalen.
* Du kan ändra din värdbaserade tjänsten toono längre svara toorequests för hello-objektet.

Ett objekt som redan har cachelagrats i hello CDN förblir cachelagrade tills hello time to live för hello objektet ut eller hello endpoint tas bort. När hello time to live period har löpt ut kontrollerar hello CDN toosee om hello CDN-slutpunkten fortfarande är giltigt och hello objekt anonymt tillgängliga. Om det inte sedan cachelagras hello objektet inte längre.

## <a name="additional-resources"></a>Ytterligare resurser
* [Hur tooMap innehåll till CDN tooa anpassad domän](cdn-map-content-to-custom-domain.md)
* [Aktivera HTTPS för den anpassade domänen](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
