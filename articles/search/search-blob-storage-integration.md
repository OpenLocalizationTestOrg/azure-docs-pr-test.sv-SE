---
title: aaaAdding Azure Search tooBlob Storage | Microsoft Docs
description: Skapa ett index i kod med hello Azure Search http-REST API.
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: a3801790067bf169693b500e83799286b7387a77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Söka i Blob Storage med Azure Search

Söka i hello olika typer av innehåll lagras i Azure Blob storage kan vara en toosolve svårt problem. Du kan dock index och söka hello innehållet i dina Blobbar i bara några klickningar med hjälp av Azure Search. Sökning i Blob storage kräver etablering av en Azure Search-tjänst. Hej olika tjänstbegränsningarna och prisnivåer för Azure Search kan hittas på hello [sida med priser](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Vad är Azure Search?
[Azure Search](https://aka.ms/whatisazsearch) är en sökning-lösning som gör det lätt för utvecklare tooadd robust fulltextsökning inträffar tooweb och mobiltelefoner. Som tjänst Azure Search tar bort hello måste toomanage all Sök infrastruktur när erbjudande en [99,9% drifttid SLA](https://aka.ms/azuresearchsla).

Med Avancerad support för 56 språk Azure Search analysera innehållet och Intelligent hantera språkspecifika konstruktioner. Azure Search ger också hello verktyg toobuild en omfattande sökning användarupplevelse. Det är enkelt tooadd funktioner, till exempel fasetterad navigering och typeahead sökförslag träffar färgmarkera toouser gränssnitten med Azure Search. toolearn om Azure Search-funktioner kan du läsa hello service [dokumentationen](https://aka.ms/azsearchdocs).

## <a name="crack-open-and-search-through-hello-content-of-enterprise-document-formats"></a>Knäcka öppna och söka igenom hello innehållet i enterprise-dokumentformat
Med stöd för [dokumentera extrahering](https://aka.ms/azsblobindexer) i Azure Blob storage kan Azure Search index-hello innehållet i en mängd olika filtyper som är lagrade i blobar:
- PDF
- Microsoft Office: DOCX/DOC, XLSX/XLS, PPTX/PPT Ignorerad (Outlook e-postmeddelanden)
- HTML
- Filer med oformaterad text

Genom att extrahera text och metadata för de här filtyperna är det enkelt toosearch över flera filformat med en enskild fråga toofind hello relevant dokument oavsett typ. Genom att använda index hello innehåll och hello metadata för Microsoft Office-dokument, PDF-filer och e-postmeddelanden, möjliga toobuild en robust innehåll företagshanteringslösning med Blob storage och Azure Search.

## <a name="search-through-your-blob-metadata"></a>Söka igenom blobbmetadata
Ett vanligt scenario som gör det enkelt toosort via BLOB för content-type är tooindex hello anpassade, användardefinierade blobbmetadata samt hello Systemegenskaper för var och en av dina blobbar. På så sätt kan indexeras information för varje blob oavsett hello typ av dokument i hello blob så att du enkelt kan sortera och aspekten för alla dina Blob storage-innehåll.

[Lär dig mer om indexering blob-metadata.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Image-sökning
Azure Search fulltextsökning fasetterad navigering och sortering funktioner kan du nu vara tillämpade toohello metadata för bilder som är lagrade i blobar.

Om dessa avbildningar bearbetas före med hello [datorn Vision API](https://www.microsoft.com/cognitive-services/computer-vision-api) från Microsofts kognitiva Services, är det möjligt tooindex hello visuellt innehåll finns i varje avbildning inklusive OCR och handskriftsigenkänning. Vi arbetar på att lägga till OCR och andra bild bearbetningsfunktioner direkt tooAzure sökning, om du är intresserad av dessa funktioner så skicka en förfrågan på vår [UserVoice](https://aka.ms/azsuv) eller [mejla oss](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Index och sökning i JSON-blobbar
Azure Search kan vara konfigurerade tooextract strukturerad innehåll finns i BLOB som innehåller JSON. Azure Search kan läsa JSON-blobbar och parsa hello strukturerad innehåll till hello relevanta fält i ett Azure Search-dokument. Azure Search kan också dra blobbar som innehåller en matris av JSON-objekt och mappa varje element tooa separat Azure Search-dokument.

Observera att parsa JSON inte för närvarande kan konfigureras via hello-portalen. [Läs mer om JSON parsning i Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Snabbstart
Azure Search kan läggas tooblobs direkt från hello Blob storage-portal, blad.

![](./media/search-blob-storage-integration/blob-blade.png)

Klicka på hello ”Lägg till Azure” Sökalternativ startar ett flöde där du kan välja en befintlig Azure Search-tjänst eller skapa en ny tjänst. Om du skapar en ny tjänst öppnas utanför ditt lagringskonto-portaler. Du behöver toore-navigera toohello portalbladet för lagring och välj nytt hello ”Lägg till Azure” Sökalternativ, där du kan välja hello befintlig tjänst.

### <a name="next-steps"></a>Nästa steg
Mer information om hello Azure Blob sökindexeraren i hello fullständig [dokumentationen](https://aka.ms/azsblobindexer).
