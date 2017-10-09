---
title: "aaa ”skapa ett index (portal - Azure Search) | Microsoft Docs ”"
description: Skapa ett index med hello Azure-portalen.
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: 4c5d663499bf73a8883690aa9482b6ba59d1ddf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-azure-portal"></a>Skapa ett Azure Search-index med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Översikt](search-what-is-an-index.md)
> * [Portalen](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Använd hello inbyggda index designer i Azure portal tooprototype eller skapa en [sökindex](search-what-is-an-index.md) toorun på ditt Azure Search-tjänsten. 

## <a name="prerequisites"></a>Krav

Den här artikeln förutsätter en [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) och [Azure Search-tjänsten](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Hitta din söktjänst
1. Logga in toohello Azure Portalsida och granska hello [söktjänster för din prenumeration](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Välj din Azure Search-tjänst.

## <a name="name-hello-index"></a>Namnet hello index

1. Klicka på hello **Lägg till index** knappen i hello kommandofältet hello överst på hello sidan.
2. Namnge ditt Azure Search-index. 
   * Börja med en bokstav.
   * Använd endast gemener, siffror eller bindestreck (”-”).
   * Begränsa hello namn too60 tecken.

  hello Indexnamnet blir en del av hello slutpunkts-URL som används på anslutningar toohello index och för att skicka HTTP-begäranden med hello Azure Search REST API.

## <a name="define-hello-fields-of-your-index"></a>Definiera hello fält i ditt index

Index sammansättning innehåller en *fält samling* som definierar hello sökbara data i ditt index. Mer specifikt anger hello strukturen för dokument som du överför separat. Hej fältsamlingen innehåller obligatoriska och valfria fält med namnet och skrivits med index attribut toodetermine hur hello-fält kan användas.

1. I hello **lägga till indexet** bladet, klickar du på **fält >** tooslide öppna hello fältet definition bladet. 

2. Acceptera hello genereras *nyckeln* fältet av typen Edm.String. Som standard heter hello fältet *id* men du kan byta namn på det så länge hello sträng uppfyller [namngivningsregler](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Ett nyckelfält är obligatoriskt för varje Azure Search index och det måste vara en sträng.

3. Lägga till fält toofully ange hello-dokument som du kommer att överföra. Om dokument som består av en *id*, *hotell namnet*, *adress*, *Stad*, och *region*, skapa en motsvarande fält för var och en i hello index. Granska hello [utforma riktlinjerna i hello nedan](#design) för hjälp med att attribut.

4. Du kan också lägga till alla fält som används internt i filteruttryck. Attribut för hello-fält kan ställas in tooexclude fält från sökningar.

5. När du är klar klickar du på **OK** toosave och skapa hello index.

## <a name="tips-for-adding-fields"></a>Tips för att lägga till fält

Skapa ett index i hello-portalen är beräkningsintensiva tangentbord. Minimera steg genom att följa det här arbetsflödet:

1. Skapa först hello fältlistan genom att ange namn och ange datatyper.

2. Sedan Använd hello kryssrutorna hello överst i varje attributet toobulk hello inställningen för alla fält och sedan avmarkerar selektivt kryssrutorna för hello få fält som inte kräver den. Till exempel är strängfält vanligtvis sökbar. Därför kan du klicka på **Retrievable** och **sökbara** tooboth returnerade hello värdena för hello fältet i sökresultat, samt tillåter fulltextsökning i hello fält. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Riktlinjer för designen för att ange attribut

Du kan lägga till nya fält när som helst, är befintliga fältdefinitioner skrivskyddade hello livstid hello index. Därför använder utvecklare vanligtvis hello portal för att skapa enkla index, testa idéer eller använder hello portalens sidor toolook upp en inställning. Ofta iteration via en design för indexet är effektivare om du följer en kodbaserad metod så att du enkelt kan återskapa hello index.

Analyzers och suggesters är kopplade till fält innan hello index sparas. Vara säker på att tooclick via varje fliksida tooadd språk analyzers eller suggesters tooyour indexdefinitionen.

Strängfält ofta är markerade som **sökbara** och **Retrievable**.

Fält som används för toonarrow sökresultaten innehåller **Sortable**, **Filterable**, och **Facetable**.

Fältattribut avgör hur ett fält används, till exempel om den används i fulltextsökning, fasetterad navigering, sorteringsåtgärder och så vidare. hello i den följande tabellen beskrivs varje attribut.

|Attribut|Beskrivning|  
|---------------|-----------------|  
|**sökbara**|Fulltext-sökbara, ämne toolexical analys, till exempel radbrytning under indexeringen. Om du anger ett värde till sökbara fält tooa exempel ”skiner” delas internt den upp i hello enskilda token ”Soligt” och ”dag”. Mer information finns i [hur full text search fungerar](search-lucene-query-architecture.md).|  
|**Filtrera**|Refereras till i **$filter** frågor. Filtrera fält av typen `Edm.String` eller `Collection(Edm.String)` inte genomgår radbrytning, så att jämförelser för endast exakt matchar. Om du ställer in sådant fält f för till exempel ”skiner” `$filter=f eq 'sunny'` hittar inga matchningar men `$filter=f eq 'sunny day'` kommer. |  
|**sorterbar**|Som standard sorterar hello system resultat poäng, men du kan konfigurera sortering som baseras på fält i hello dokument. Fält av typen `Collection(Edm.String)` får inte vara **sorterbar**. |  
|**facetable**|Används vanligtvis i en presentation av sökresultat som innehåller ett antal träffar efter kategori (till exempel hotell på en viss ort). Det här alternativet kan inte användas med fält av typen `Edm.GeographyPoint`. Fält av typen `Edm.String` som är **filtrera**, **sorterbar**, eller **facetable** får vara högst 32 kB längd. Mer information finns i [Create Index (REST-API)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**nyckel**|Unik identifierare för dokument inom hello index. Exakt ett fält måste väljas som hello nyckelfältet och den måste vara av typen `Edm.String`.|  
|**strängfält**|Anger om fältet hello kan returneras i sökresultatet. Detta är användbart när du vill toouse ett fält (som *vinst*) som ett filter, sortera eller bedömningen mekanism, men gör inte vill hello fältet toobe synliga toohello slutanvändaren. Det här attributet måste vara `true` för `key` fält.|  

## <a name="create-hello-hotels-index-used-in-example-api-sections"></a>Skapa hello hotell index som används i exempel API-avsnitt

Azure Search-API-dokumentationen innehåller kodexempel med en enkel *hotell* index. I hello skärmbilderna nedan ser du hello indexdefinitionen, inklusive hello franska analyzer anges under indexdefinitionen, som du kan återskapa som en övning i hello-portalen.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Nästa steg

När du har skapat ett Azure Search-index, kan du flytta toohello nästa steg: [överföra sökbara data till hello index](search-what-is-data-import.md).

Alternativt kan du också ta en djupare inblick i index. Dessutom toohello fält samling, ett index anger också analyzers suggesters, resultatfunktioner profiler och CORS-inställningarna. hello portal innehåller sidor för att definiera de vanligaste hello-element: fält, analyzers och suggesters. toocreate eller ändra andra element, kan du använda hello REST API eller .NET SDK.

## <a name="see-also"></a>Se även

 [Så här fungerar fulltextsökning](search-lucene-query-architecture.md)  
 [Söktjänsten REST API](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

