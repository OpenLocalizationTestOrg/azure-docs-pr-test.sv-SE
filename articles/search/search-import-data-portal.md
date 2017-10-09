---
title: aaaImport data till Azure Search hello-portalen | Microsoft Docs
description: "Använd hello Azure Search i guiden Importera Data i hello Azure Portal toocrawl Azure data från NoSQL Azure Cosmos DB, Blob storage, tabellagring, SQL Database och SQL Server på virtuella Azure-datorer."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: Azure Portal
ms.assetid: f40fe07a-0536-485d-8dfa-8226eb72e2cd
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 00b0e59594560c0cdaea748df196518e9fba3834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-tooazure-search-using-hello-portal"></a>Importera data tooAzure Sök med hjälp av hello portal
hello Azure-portalen innehåller en **dataimport** guiden på hello Azure Search-instrumentpanelen för att läsa in data i ett index. 

  ![Importera Data i hello kommandofält][1]

Internt hello guiden konfigurerar och anropar en *indexeraren*, automatisera flera steg i hello indexering processen: 

* Ansluta tooan extern datakälla i hello samma Azure-prenumeration
* Generera en ändringsbar indexeringsschema baserat på hello datastruktur för källa
* Läsa in JSON-dokument i ett index med hjälp av en raduppsättning som hämtats från hello-datakälla

Du kan testa det här arbetsflödet med exempeldata i Azure Cosmos DB. Besök [Kom igång med Azure Search i hello Azure Portal](search-get-started-portal.md) anvisningar.

> [!NOTE]
> Du kan starta hello **dataimport** guiden från hello Azure Cosmos DB instrumentpanelen toosimplify indexering för datakällan. I det vänstra navigationsfältet gå för**samlingar** > **lägga till Azure Search** tooget igång.

## <a name="data-sources-supported-by-hello-import-data-wizard"></a>Datakällor som stöds av hello guiden Importera Data
hello importera Data guiden stöder hello följande datakällor: 

* Azure SQL Database
* SQL Server-relationsdata på en virtuell Azure-dator
* Azure Cosmos DB
* Azure Blob Storage
* Azure Table Storage

En utjämnad datauppsättning är en obligatorisk inmatning. Du kan bara importera från en enskild tabell, databasvy eller likvärdig datastruktur. Innan du kör guiden hello bör du skapa den här datastrukturen.

## <a name="connect-tooyour-data"></a>Ansluta tooyour data
1. Logga in toohello [Azure-portalen](https://portal.azure.com) och öppna hello service instrumentpanel. Du kan klicka på **fler tjänster** i hello hopp fältet toosearch för befintliga ”söktjänster” i hello nuvarande prenumeration. 
2. Klicka på **importera Data** på hello i kommandofältet tooslide öppna hello importera Data bladet.  
3. Klicka på **ansluta tooyour data** toospecify en definition av datakällan används av en indexerare. För intra-prenumeration datakällor hello guiden vanligtvis identifiera och läsa anslutningsinformationen, minimera övergripande konfigurationskrav.

|  |  |
| --- | --- |
| **Befintlig datakälla** |Om du redan har definierat indexerare i söktjänsten, kan du välja en definition av en befintlig datakälla för en annan import. |
| **Azure SQL Database** |Namn och autentiseringsuppgifter för en databasanvändare med läsbehörighet och ett databasnamn kan anges på sidan hello eller via en ADO.NET-anslutningssträngen. Välj hello anslutning sträng alternativet tooview eller anpassa egenskaper. <br/><br/>hello tabell eller vy som innehåller hello raduppsättningen måste anges på hello-sidan. Det här alternativet visas när hello anslutningen lyckas, vilket ger en listrutan så att du kan göra en markering. |
| **SQL Server på virtuella Azure-datorer** |Ange ett fullständigt tjänstnamn, användar-ID och lösenord, samt databasen som en anslutningssträng. toouse denna datakälla måste du ha tidigare installerat ett certifikat i hello lokala arkivet som krypterar hello-anslutning. Instruktioner finns i [SQL VM anslutning tooAzure Sök](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>hello tabell eller vy som innehåller hello raduppsättningen måste anges på hello-sidan. Det här alternativet visas när hello anslutningen lyckas, vilket ger en listrutan så att du kan göra en markering. |
| **Azure Cosmos DB** |Krav på innehåller hello kontot, databas och samling. Alla dokument i hello samlingen tas med i hello index. Du kan definiera en fråga tooflatten eller filtrera hello raduppsättningen eller toodetect ändrats dokument för efterföljande datauppdateringsåtgärderna. |
| **Azure Blob Storage** |Krav på innehåller hello storage-konto och en behållare. (Valfritt) om blobbnamnen följer en virtuell namngivningskonvention för gruppering ändamål, kan du ange hello virtuell katalog delen av hello namn som en mapp i behållaren. Se [Indexera Blob Storage](search-howto-indexing-azure-blob-storage.md) för mer information. |
| **Azure Table Storage** |Krav på innehåller hello storage-konto och ett tabellnamn. Du kan ange en fråga tooretrieve en delmängd av hello tabeller. Se [Indexera Table Storage](search-howto-indexing-azure-tables.md) för mer information. |

## <a name="customize-target-index"></a>Anpassa målindex
En preliminär index är vanligtvis härleda från hello dataset. Lägga till, redigera eller ta bort fält toocomplete hello schema. Dessutom ange attribut i hello fältet nivån toodetermine dess efterföljande Sök beteenden.

1. I **anpassa målindexet**, ange hello namn och en **nyckeln** används toouniquely identifierar varje dokument. hello nyckel måste vara en sträng. Om fältvärden innehålla mellanslag eller tankstreck Kontrollera tooset avancerade alternativ i **importera dina data** toosuppress hello valideringskontrollen för dessa tecken.
2. Granska och ändra hello återstående fälten. Fältnamnet och typen fylls vanligtvis i automatiskt. Du kan ändra hello tills hello indexet har skapats. Om du ändrar datatypen efter detta måste indexet återskapas.
3. Ange indexattribut för varje fält:
   
   * Strängfält returnerar hello-fältet i sökresultaten.
   * Filtrera kan hello fältet toobe som refereras i filteruttryck.
   * Sorterbar kan hello fältet toobe används i en sortering.
   * Facetable kan hello-fältet för fasetterad navigering.
   * Sökbar gör att du kan använda fulltextsökning.
4. Klicka på hello **Analyzer** fliken om du vill toospecify ett språk analyzer på hello fältnivå. Det går endast att ange språkanalysverktyg för närvarande. Om du använder ett anpassat analysverktyg eller en icke-språkanalys som t.ex. nyckelord, mönster och så vidare, kommer kod att krävas.
   
   * Klicka på **sökbara** toodesignate fulltext söka i hello fält och aktivera hello Analyzer nedrullningsbara listan.
   * Välj hello analyzer som du vill använda. Mer information finns i [Skapa ett index för dokument på flera språk](search-language-support.md).
5. Klicka på hello **Förslagsställarens** tooenable typ-ahead frågeförslag på markerade fälten.

## <a name="import-your-data"></a>Importera dina data
1. I **importera dina data**, ange ett namn för hello indexeraren. Återkalla hello produkten av hello importera Data guiden är en indexerare. Senare, om du vill tooview eller redigera den, väljer du det från hello-portal i stället för genom att köra guiden hello. 
2. Schemalägg hello, som baseras på hello regionala tidszonen som hello-tjänsten har etablerats.
3. Ange avancerade alternativ toospecify tröskelvärden för indexeringen kan om fortsätta om ett dokument har släppts. Du kan också ange om **nyckeln** fält tillåts toocontain blanksteg och snedstreck.  
4. Klicka på **OK** toocreate hello index och importera hello data.

Du kan övervaka indexering hello-portalen. När dokument laddas att växa hello dokumentantal för hello index som du har definierat. Ibland tar det några minuter för hello Portalsida toopick hello senaste uppdateringar.

hello index är klar tooquery som alla hello dokument har lästs in.

## <a name="query-an-index-using-search-explorer"></a>Köra frågor mot ett index med hjälp av Sökutforskaren

hello portal innehåller **Sök Explorer** så att du kan fråga efter ett index utan att behöva toowrite någon kod. Du kan använda [Sökutforskaren](search-explorer.md) för valfritt index.

hello sökinställningar är baserat på standardinställningarna, till exempel hello [enkel syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) och standard [searchMode frågeparameter (https://docs.microsoft.com/rest/api/searchservice/search-documents). 

Resultaten returneras i JSON, i formatet utförlig så att du kan inspektera hello hela dokumentet.

## <a name="edit-an-existing-indexer"></a>Redigera en befintlig indexerare
Enligt nedanstående hello i guiden Importera data skapas en **indexeraren**, som du kan ändra som en fristående konstruktion hello-portalen.

Dubbelklicka på hello indexeraren panelen tooslide en lista med alla indexerare som skapats för din prenumeration i instrumentpanelen för hello-tjänsten. Dubbelklicka på en av hello indexerare toorun, redigera eller ta bort den. Du kan ersätta hello index med en annan befintlig, ändra hello datakälla och ange alternativ för feltrösklarna under indexeringen.

## <a name="edit-an-existing-index"></a>Redigera ett befintligt index
hello guiden skapas också en **index**. I Azure Search kräver strukturella uppdateringar tooan index en återskapa indexet. Återskapning av en innebär att ta bort hello index, återskapa hello index med hjälp av en ändrad schema som innehåller hello ändringar som du vill och ladda om informationen. Strukturella uppdateringar innebär att man ändrar datatypen och byter namn på eller tar bort ett fält.

Ändringar som inte kräver att indexet återskapas omfattar att lägga till ett nytt fält, ändra bedömningsprofil, ändra förslagsställare eller ändra språkanalys. Mer information finns i [Uppdatera index](https://msdn.microsoft.com/library/azure/dn800964.aspx).


## <a name="next-steps"></a>Nästa steg
Granska dessa länkar toolearn mer om indexerare:

* [Indexera Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Indexera Azure Cosmos DB](search-howto-index-documentdb.md)
* [Indexera Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Indexera Table Storage](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

