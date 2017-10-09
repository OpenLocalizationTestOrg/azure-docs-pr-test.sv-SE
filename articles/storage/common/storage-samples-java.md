---
title: "aaaAzure lagring exempel som använder Java | Microsoft Docs"
description: "Visa, hämta och köra exempelkod och program för Azure Storage. Identifiera komma igång prover för blobbar, köer, tabeller och filer med hjälp av hello Java storage, klientbiblioteken."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: 6aec326cbfedc1166fc61037ac39d33c15d28d2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-java"></a>Azure Storage-exempel som använder Java

## <a name="java-sample-index"></a>Index för Java-exempel

hello följande tabell ger en översikt över våra exempel databasen och hello scenarier beskrivs i varje prov. Klicka på hello länkar tooview hello motsvarande exempelkoden i GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Slutpunkt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Exempelkod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>BLOB</b></td>
<td>Lägg till Blob</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td> 
</tr> 
<tr> 
<td>Blockblob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Kryptering av klientsidan</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Komma igång med Azure Client Side Encryption i Java</a></td>
</tr> 
<tr> 
<td>Kopiera Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Skapa en behållare</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Ta bort blobben</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Ta bort behållaren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>BLOB Metadata/egenskaper/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Behållaren-ACL/metadataegenskaper</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Get-sidintervall</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Sidblob testar exempel</a></td>
</tr> 
<tr> 
<td>Lånet Blobbehållaren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Lista Blobbehållaren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td>Sidblob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS tester exempel</a></td>
</tr>   
<tr> 
<td>Tjänstegenskaper</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr>           
<tr> 
<td>Snapshot-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Komma igång med Azure Blob-tjänsten i Java</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Fil</b></td>
<td>Skapa resurser-kataloger-filer</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></td> 
</tr>
<tr> 
<td>Ta bort resurser-kataloger-filer</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></td> 
</tr> 
<tr> 
<td>Directory egenskaper/Metadata</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></td> 
</tr> 
<tr> 
<td>Hämta filer</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></td> 
</tr> 
<tr> 
<td>Filen mått-egenskaper/Metadata</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></td> 
</tr> 
<tr> 
<td>Egenskaper för filtjänsten</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></td> 
</tr> 
<tr> 
<td>Lista över kataloger och filer</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></td> 
</tr>
<tr> 
<td>Lista över resurser</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Komma igång med Azure File Service i Java</a></td> 
</tr>
<tr> 
<td>Dela Stats-egenskaper/Metadata</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Komma igång med Azure File Service i Java</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Kön</b></td>
<td>Lägga till meddelande</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Lagring Java klienten biblioteket prover</a></td> 
</tr> 
<tr> 
<td>Kryptering av klientsidan</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Lagring Java klienten biblioteket prover</a></td> 
</tr> 
<tr> 
<td>Skapa köer</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></td> 
</tr> 
<tr> 
<td>Ta bort meddelandekö /</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></td> 
</tr> 
<tr> 
<td>Granska meddelande</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></td> 
</tr> 
<tr> 
<td>Kön ACL/Metadata/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Komma igång med Azure-kötjänsten i Java</a></td> 
</tr> 
<tr> 
<td>Egenskaper för kön</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Komma igång med Azure-kötjänsten i Java</a></td> 
</tr> 
<tr> 
<td>Uppdatera meddelande</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Komma igång med Azure-kötjänsten i Java</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabell</b></td>
<td>Skapa tabell</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></td> 
</tr> 
<tr> 
<td>Ta bort en entitet/tabell</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></td> 
</tr> 
<tr> 
<td>Infoga/Merge/ersätta entitet</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Lagring Java klienten biblioteket prover</a></td> 
</tr> 
<tr> 
<td>Fråga entiteter</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></td> 
</tr> 
<tr> 
<td>Frågetabeller</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Komma igång med Azure-tabellen Service i Java</a></td> 
</tr> 
<tr> 
<td>ACL/Tabellegenskaper</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Komma igång med Azure-tabellen Service i Java</a></td> 
</tr> 
<tr> 
<td>Uppdatera entitet</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Lagring Java klienten biblioteket prover</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure-kodexempel-bibliotek

tooview hello fullständigt exempel bibliotek, gå toohello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=storage) biblioteket, som innehåller exempel för Azure Storage som du kan hämta och kör lokalt. hello kod exempel biblioteket innehåller exempelkod i ZIP-format. Du kan också bläddra och klona hello GitHub-lagringsplatsen för varje prov.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Komma igång guider

Kolla in hello följande guider om du letar efter information om hur tooinstall och komma igång med hello Azure Storage, Klientbiblioteken.

* [Komma igång med Azure Blob-tjänsten i Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Komma igång med Azure-kötjänsten i Java](../storage-java-how-to-use-queue-storage.md)
* [Komma igång med Azure-tabellen Service i Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Komma igång med Azure File Service i Java](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Nästa steg

Information om prover för andra språk:

* .NET: [azure Storage-exempel med hjälp av .NET](../storage-samples-dotnet.md)
* Alla andra språk: [Azure Storage-exempel](../storage-samples.md)
