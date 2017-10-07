---
title: "aaaConfigure ett anpassat domännamn för din Azure Blob storage-slutpunkt | Microsoft Docs"
description: "Använd hello Azure portal toomap slutpunkt för Blob-lagring egna kanoniskt namn (CNAME) toohello i ett Azure Storage-konto."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: marsma
ms.openlocfilehash: bfee6d28039f389ba2e2ed6b28d43894b6e15469
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurera ett eget domännamn för din Blob Storage-slutpunkt

Du kan konfigurera en anpassad domän för åtkomst till blob-data i Azure storage-konto. Hej standardslutpunkten för Blob storage är `<storage-account-name>.blob.core.windows.net`. Om du mappar en anpassade domäner och underdomäner som **www.contoso.com** toohello blob-slutpunkten för ditt lagringskonto måste användarna kan sedan använda blob-data i ditt lagringskonto med domänen.

> [!IMPORTANT]
> Azure Storage har inte har inbyggt stöd HTTPS med anpassade domäner. Du kan för närvarande [använder hello Azure CDN tooaccess blobbar med anpassade domäner via HTTPS](storage-https-custom-domain-cdn.md).
>

hello i tabellen nedan visas några exempel-URL: er för blob-data som finns i ett lagringskonto med namnet **mittlagringskonto**. hello anpassade domänen som registrerats för hello storage-konto är **www.contoso.com**:

| Resurstyp | Standard-URL | URL: en anpassad domän |
| --- | --- | --- |
| Lagringskonto | http://mystorageaccount.BLOB.Core.Windows.NET | http://www.contoso.com |
| Blob |http://mystorageaccount.BLOB.Core.Windows.NET/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Rotbehållare | http://mystorageaccount.BLOB.Core.Windows.NET/myblob eller http://mystorageaccount.blob.core.windows.net/$ root/minblobb| http://www.contoso.com/myblob eller http://www.contoso.com/$ root/minblobb |

## <a name="direct-vs-intermediary-domain-mapping"></a>Direkt kontra mellanliggande domänmappning

Det finns två sätt toopoint domänen toohello blob-slutpunkten för ditt lagringskonto: Dirigera CNAME mappning och använder hello *asverify* mellanliggande underdomänen.

### <a name="direct-cname-mapping"></a>Direkt CNAME-mappning

hello första och enklaste metoden är toocreate en kanoniskt namn (CNAME)-post som mappar dina anpassade domäner och underdomäner direkt toohello blob-slutpunkten. En CNAME-post är en domain name system (DNS) funktion som mappar en domän tooa mål källdomänen. I det här fallet hello källdomänen är din egen anpassade domäner och underdomäner, till exempel *www.contoso.com*. hello måldomänen är Blobbtjänstens slutpunkt, till exempel  *mystorageaccount.BLOB.Core.Windows.NET*.

hello direkta metoden beskrivs i [registrera en anpassad domän](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Mellanliggande mappning med *asverify*

andra hello-metoden använder också CNAME-poster, men först använder en särskild underdomän som identifieras av Azure tooavoid driftstopp: **asverify**.

hello-processen för att matcha dina anpassade domäner tooa blob-slutpunkt kan resultera i en kort period driftstopp för hello domän när du registrerar den i hello [Azure-portalen](https://portal.azure.com). Om den anpassade domänen för närvarande stöder ett program med ett servicenivåavtal (SLA) som kräver driftstopp, så du kan använda hello Azure *asverify* underdomän som en mellanliggande registreringssteget. Det här mellanliggande steget säkerställer att användare kan tooaccess din domän är medan är hello DNS-mappning äger rum.

hello mellanliggande metoden beskrivs i [registrera en anpassad domän med hello *asverify* underdomän](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Registrera en anpassad domän
Använd den här proceduren tooregister domänen om du har några frågor om hello domänen som för tillfället inte tillgänglig tooyour användare, eller om den anpassade domänen inte är för närvarande är värd för ett program.

Om den anpassade domänen för närvarande stöder ett program som inte kan ha någon avbrottstid, följer du hello proceduren som beskrivs i [registrera en anpassad domän med hello *asverify* underdomän](#register-a-custom-domain-using-the-asverify-subdomain).

tooconfigure ett anpassat domännamn, måste du skapa en ny CNAME-post i DNS. hello CNAME-post anger ett alias för ett domännamn. I det här fallet mappas hello-adressen för din domän toohello Blob storage slutpunkt för ditt lagringskonto.

Normalt kan du hantera din domän DNS-inställningar på din domänregistrator webbplats. Varje register har en liknande metod för att ange en CNAME-post, men hello konceptet är hello samma. Vissa paket för registrering av grundläggande domän erbjuder inte DNS-konfiguration, så du kan behöva tooupgrade registreringspaket din domän innan du kan skapa hello CNAME-post.

1. Navigera tooyour storage-konto i hello [Azure-portalen](https://portal.azure.com).
1. Under **BLOB-tjänsten** på bladet för hello-menyn, Välj **anpassad domän** tooopen hello *anpassad domän* bladet.
1. Logga in tooyour domänregistrators webbplats och gå toohello sidan för hantering av DNS. Du kan hitta det här i ett avsnitt som till exempel **Domännamn**, **DNS**, eller **hantering av namnhantering**.
1. Hitta hello avsnitt för att hantera skapa CNAME-poster. Du kan ha toogo tooan avancerade Inställningssida och leta efter hello ord **CNAME**, **Alias**, eller **underdomäner**.
1. Skapa en ny CNAME-post och ange en underdomän alias som **www** eller **foton**. Ange ett värdnamn som är Blobbtjänstens slutpunkt, hello format **mystorageaccount.blob.core.windows.net** (där *mittlagringskonto* är hello namnet på ditt lagringskonto). hello värden namnet toouse visas i objektet #1 för hello *anpassad domän* bladet i hello [Azure-portalen](https://portal.azure.com).
1. I textrutan för hello på hello *anpassad domän* bladet i hello [Azure-portalen](https://portal.azure.com), ange hello namnet på din domän, inklusive hello underdomänen. Om din domän är till exempel **contoso.com** och underdomän-alias är **www**, ange **www.contoso.com**. Om din underdomän **foton**, ange **photos.contoso.com**. hello underdomän är *krävs*.
1. Välj **spara** på hello *anpassad domän* bladet tooregister domänen. Om hello registreringen lyckas visas en portalmeddelandet storage-konto har uppdaterats.

När din nya CNAME-posten har spridits via DNS kan användarna visa blob-data med hjälp av den anpassade domänen så länge som de har behörighet för hello.

## <a name="register-a-custom-domain-using-hello-asverify-subdomain"></a>Registrera en anpassad domän med hello *asverify* underdomän
Använd den här proceduren tooregister domänen om den anpassade domänen för närvarande stöder ett program med ett SLA som kräver att det finnas utan avbrott. Genom att skapa en CNAME-post som pekar från `asverify.<subdomain>.<customdomain>` för`asverify.<storageaccount>.blob.core.windows.net`, kan du registrera din domän med Azure. Du kan sedan skapa en andra CNAME-post som pekar från `<subdomain>.<customdomain>` för`<storageaccount>.blob.core.windows.net`, då trafik tooyour anpassad domän blir dirigerad tooyour slutpunkt för blob.

Hej **asverify** underdomän är en särskild underdomän som identifieras av Azure. Av tillagt `asverify` tooyour egna underdomän, du bevilja Azure toorecognize din anpassade domän utan att ändra hello DNS-post för hello domän. När du ändrar hello DNS-post för hello domän, kommer att vara mappade toohello slutpunkt för blob utan avbrott.

1. Navigera tooyour storage-konto i hello [Azure-portalen](https://portal.azure.com).
1. Under **BLOB-tjänsten** på bladet för hello-menyn, Välj **anpassad domän** tooopen hello *anpassad domän* bladet.
1. Logga in tooyour DNS-leverantörens webbplats och gå toohello sidan för hantering av DNS. Du kan hitta det här i ett avsnitt som till exempel **Domännamn**, **DNS**, eller **hantering av namnhantering**.
1. Hitta hello avsnitt för att hantera skapa CNAME-poster. Du kan ha toogo tooan avancerade Inställningssida och leta efter hello ord **CNAME**, **Alias**, eller **underdomäner**.
1. Skapa en ny CNAME-post och ange en underdomän alias som innehåller hello *asverify* underdomänen. Till exempel **asverify.www** eller **asverify.photos**. Ange ett värdnamn som är Blobbtjänstens slutpunkt, hello format **asverify.mystorageaccount.blob.core.windows.net** (där **mittlagringskonto** är hello namnet på ditt lagringskonto). hello värden namnet toouse visas i objektet #2 i hello *anpassad domän* bladet i hello [Azure-portalen](https://portal.azure.com).
1. I textrutan för hello på hello *anpassad domän* bladet i hello [Azure-portalen](https://portal.azure.com), ange hello namnet på din domän, inklusive hello underdomänen. Inkludera inte *asverify*. Om din domän är till exempel **contoso.com** och underdomän-alias är **www**, ange **www.contoso.com**. Om din underdomän **foton**, ange **photos.contoso.com**. hello underdomän krävs.
1. Välj hello **Använd indirekt CNAME-validering** kryssrutan.
1. Välj **spara** på hello *anpassad domän* bladet tooregister domänen. Om hello registreringen lyckas visas en portal meddelande om ditt lagringskonto har uppdaterats. Nu din anpassade domän har verifierats av Azure, men trafik tooyour domän ännu inte dirigeras tooyour storage-konto.
1. Returnerar tooyour DNS-leverantörens webbplats och skapa en annan CNAME-post som mappar underdomän tooyour Blobbtjänstens slutpunkt. Till exempel ange hello underdomän som **www** eller **foton** (utan hello *asverify*), och hello värdnamn som  **mystorageaccount.BLOB.Core.Windows.NET** (där **mittlagringskonto** är hello namnet på ditt lagringskonto). Hello registreringen av den anpassade domänen är klar med det här steget.
1. Slutligen kan du ta bort hello CNAME-post som du skapade med hello **asverify** underdomän som det var nödvändigt endast som en mellanliggande steg.

När din nya CNAME-posten har spridits via DNS kan användarna visa blob-data med hjälp av den anpassade domänen så länge som de har behörighet för hello.

## <a name="test-your-custom-domain"></a>Testa den anpassade domänen

tooconfirm din anpassade domän är faktiskt mappa tooyour Blob-tjänsteslutpunkt, skapa en blobb i en offentlig behållare inom ditt lagringskonto. I en webbläsare, Använd en URI i följande format tooaccess hello blob hello:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Du kan till exempel använda följande URI tooaccess ett webbformulär i hello hello **myforms** behållare i hello **photos.contoso.com** anpassade underdomän:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Avregistrera en anpassad domän

tooderegister en anpassad domän för din slutpunkt för Blob storage, med någon av följande procedurer hello.

### <a name="azure-portal"></a>Azure Portal

Utför följande hello i hello Azure portal tooremove hello domäninställningen:

1. Navigera tooyour storage-konto i hello [Azure-portalen](https://portal.azure.com).
1. Under **BLOB-tjänsten** på bladet för hello-menyn, Välj **anpassad domän** tooopen hello *anpassad domän* bladet.
1. Rensa hello innehållet i hello textruta som innehåller ditt domännamn.
1. Välj hello **spara** knappen.

När hello anpassad domän har tagits bort, visas ett portal meddelande om ditt lagringskonto har uppdaterats.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Använd hello [az lagring uppdatering](https://docs.microsoft.com/cli/azure/storage/account#update) CLI kommando och ange en tom sträng (`""`) för hello `--custom-domain` argumentet värdet tooremove en anpassad domän för registrering.

* Kommandoformatet:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Exemplet:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

Använd hello [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) PowerShell-cmdlet och ange en tom sträng (`""`) för hello `-CustomDomainName` argumentet värdet tooremove en anpassad domän för registrering.

* Kommandoformatet:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Exemplet:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Nästa steg
* [Mappa en anpassad domän tooan Azure Content Delivery Network (CDN) slutpunkt](../../cdn/cdn-map-content-to-custom-domain.md)
* [Med anpassade domäner via HTTPS hello Azure CDN tooaccess blobbar](storage-https-custom-domain-cdn.md)
