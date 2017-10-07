---
title: aaaEncryption i Azure Data Lake Store | Microsoft Docs
description: "Så här fungerar kryptering och nyckelrotation i Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.openlocfilehash: a9f3a2dce8232deba93005594d1e6a21e9c0cbee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encryption-of-data-in-azure-data-lake-store"></a>Kryptera data i Azure Data Lake Store

Du kan skydda dina data, implementera säkerhetsprinciper och uppfylla efterlevnadskrav med hjälp av kryptering i Azure Data Lake Store. Den här artikeln innehåller en översikt över hello design och beskrivs några av hello tekniska aspekter för implementering.

Data Lake Store har stöd för kryptering av vilande data och data under överföringen. Data Lake Store stöder transparent kryptering av vilande data som är på som standard. En liten förklaring av vad detta betyder:

* **På som standard**: när du skapar ett nytt Data Lake Store-konto hello standardinställningen aktiverar kryptering. Därefter är data som lagras i Data Lake Store alltid krypterat tidigare toostoring på beständiga media. Det är hello som gäller för alla data och kan inte ändras efter att ett konto skapas.
* **Transparent**: Data Lake Store automatiskt data tidigare toopersisting krypterar och dekrypterar data tidigare tooretrieval. hello kryptering konfigureras och hanteras på hello Data Lake Store nivå av en administratör. Inga ändringar görs toohello data API: er. Därför krävs inga ändringar i de program och tjänster som interagerar med Data Lake Store på grund av krypteringen.

Data under överföring (även kallat data i rörelse) krypteras också alltid i Data Lake Store. Dessutom tooencrypting data tidigare toostoring toopersistent media hello data alltid skyddas under överföringen med hjälp av HTTPS. HTTPS är hello enda protokoll som stöds för hello Data Lake Store REST-gränssnitt. hello följande diagram visar hur data blir krypterade i Data Lake Store:

![Diagram över kryptering i Data Lake Store](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-store"></a>Konfigurera kryptering med Data Lake Store

Kryptering för Data Lake Store ställs in när kontot skapas, och är alltid aktiverat som standard. Du kan antingen hantera hello nycklar själv eller tillåta Data Lake Store toomanage dem du (detta är hello standard).

Mer information finns i [Komma igång](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="how-encryption-works-in-data-lake-store"></a>Så här fungerar kryptering i Data Lake Store

hello beskriver följande information hur toomanage master krypteringsnycklar och det förklarar hello tre olika typer av nycklar som du kan använda datakryptering för Data Lake Store.

### <a name="master-encryption-keys"></a>Huvudkrypteringsnycklar

Data Lake Store har två lägen för hantering av huvudkrypteringsnycklar (Master, Encryption Keys, MEK). Anta att hello huvudkrypteringsnyckel är hello högsta nyckeln för tillfället. Åtkomst toohello huvudkrypteringsnyckel är obligatoriska toodecrypt några data som lagras i Data Lake Store.

hello två lägen för att hantera hello huvudkrypteringsnyckel är följande:

*   Tjänsthanterade nycklar
*   Kundhanterade nycklar

I båda lägena skyddas hello huvudkrypteringsnyckel genom att lagra det i Azure Key Vault. Key Vault är en helt hanterad tjänst mycket säkert på Azure som kan använda toosafeguard kryptografiska nycklar. Mer information finns i [Key Vault](https://azure.microsoft.com/services/key-vault).

Här är en kort jämförelse av funktioner som tillhandahålls av hello två lägen för att hantera hello MEKs.

|  | Tjänsthanterade nycklar | Kundhanterade nycklar |
| --- | --- | --- |
|Hur lagras data?|Alltid krypterade tidigare toobeing lagras.|Alltid krypterade tidigare toobeing lagras.|
|Var lagras hello kryptering huvudnyckel?|Key Vault|Key Vault|
|Är kryptering nycklar lagrade i hello Rensa utanför Key Vault? |Nej|Nej|
|Kan hello MEK hämtas av Key Vault?|Nej. Efter hello MEK lagras i Nyckelvalvet, kan den bara användas för kryptering och dekryptering.|Nej. Efter hello MEK lagras i Nyckelvalvet, kan den bara användas för kryptering och dekryptering.|
|Vem äger hello Key Vault-instans och hello MEK?|hello Data Lake Store-tjänsten|Du äger hello Key Vault-instansen, som ingår i din egen Azure-prenumeration. Hej MEK i Key Vault kan hanteras av programvara eller maskinvara.|
|Kan du återkalla åtkomst toohello MEK för hello Data Lake Store-tjänsten?|Nej|Ja. Du kan hantera åtkomstkontrollistor i Nyckelvalvet och ta bort tjänstidentiteten för access control poster toohello för hello Data Lake Store-tjänsten.|
|Kan du ta bort permanent hello MEK?|Nej|Ja. Om du tar bort hello MEK från Nyckelvalvet kan hello data i hello Data Lake Store-konto inte dekrypteras av vem som helst, inklusive hello Data Lake Store-tjänsten. <br><br> Om du har uttryckligen säkerhetskopierade hello MEK tidigare toodeleting den från Nyckelvalvet, hello MEK kan återställas och hello data kan återställas. Men om du inte säkerhetskopierat hello MEK tidigare toodeleting den från Nyckelvalvet, kan hello data i hello Data Lake Store-konto aldrig dekrypteras därefter.|


Utöver den här skillnaden mellan som hanterar hello MEK och hello Key Vault-instans där den finns, hello resten av hello design är hello samma för båda lägena.

Det är viktigt tooremember hello följande när du väljer hello-läge för hello master krypteringsnycklar:

*   Du kan välja om toouse kundhanterad nycklar eller för tjänsten hanteras när du etablerar ett Data Lake Store-konto.
*   När ett Data Lake Store-konto har etablerats kan inte hello-läge ändras.

### <a name="encryption-and-decryption-of-data"></a>Kryptering och dekryptering av data

Det finns tre typer av nycklar som används i hello utformning av datakryptering. hello följande tabell innehåller en översikt:

| Nyckel                   | Förkortning | Kopplad till | Lagringsplats                             | Typ       | Anteckningar                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Huvudkrypteringsnyckel | MEK          | Ett Data Lake Store-konto | Key Vault                              | Asymmetrisk | Det kan hanteras av Data Lake Store eller dig.                                                              |
| Datakrypteringsnyckel   | DEK          | Ett Data Lake Store-konto | Beständig lagring – hanteras av Data Lake Store-tjänsten | Symmetrisk  | Hej DEK krypteras med hello MEK. hello krypterade DEK är vad som lagras på beständiga media. |
| Blockkrypteringsnyckel  | BEK          | Ett datablock | Ingen                                         | Symmetrisk  | Hej BEK härleds från hello DEK och hello datablock.                                                      |

hello följande diagram illustrerar de här koncepten:

![Nycklar i datakryptering](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-toobe-decrypted"></a>Pseudokolumner algoritm när en fil toobe dekrypteras:
1.  Kontrollera om hello DEK för hello Data Lake Store-konto är cachelagrade och redo att användas.
    - Om inte, Läs hello krypterade DEK från beständig lagring och skicka den tooKey valvet toobe dekrypteras. Cache hello dekryptera DEK i minnet. Det är nu redo toouse.
2.  För varje datablock i hello-filen:
    - Läsa hello krypterat datablock från beständig lagring.
    - Generera hello BEK från hello DEK och hello krypterat datablock.
    - Använda hello BEK toodecrypt data.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-toobe-encrypted"></a>Pseudokolumner algoritmen när ett datablock är krypterad toobe:
1.  Kontrollera om hello DEK för hello Data Lake Store-konto är cachelagrade och redo att användas.
    - Om inte, Läs hello krypterade DEK från beständig lagring och skicka den tooKey valvet toobe dekrypteras. Cache hello dekryptera DEK i minnet. Det är nu redo toouse.
2.  Generera en unik BEK för hello datablock från hello DEK.
3.  Kryptera hello datablock med hello BEK, med hjälp av AES 256-kryptering.
4.  Lagra hello krypterade data datablock i permanent lagringsutrymme.

> [!NOTE] 
> Av prestandaskäl hello DEK i hello Rensa cachelagras i minnet för en kort tid och raderas omedelbart efteråt. På beständiga medier lagras den alltid krypterats av hello MEK.

## <a name="key-rotation"></a>Nyckelrotation

Du kan rotera hello MEK när du använder en kundhanterad nycklar. hur tooset upp ett Data Lake Store-konto med kundhanterad nycklar, se toolearn [komma igång](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Krav

När du ställer in hello Data Lake Store-konto har du valt toouse egna nycklar. Det här alternativet kan inte ändras efter hello-konto har skapats. hello följande steg förutsätter att du använder kundhanterad nycklar (det vill säga du egna nycklar från Nyckelvalvet).

Observera att om du använder hello standardalternativen för kryptering av dina data alltid krypteras med hjälp av nycklar som hanteras av Data Lake Store. I det här alternativet kan du inte hello möjlighet toorotate nycklar som de hanteras av Data Lake Store.

### <a name="how-toorotate-hello-mek-in-data-lake-store"></a>Hur toorotate hello MEK i Data Lake Store

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Bläddra toohello Key Vault-instans som lagrar dina nycklar som associeras med Data Lake Store-konto. Välj **Nycklar**.

    ![Skärmbild av Key Vault](./media/data-lake-store-encryption/keyvault.png)

3.  Välj hello nyckeln som associeras med ditt Data Lake Store-konto och skapa en ny version av den här nyckeln. Observera att Data Lake Store stöder för närvarande endast viktiga rotation tooa ny version av en nyckel. Det stöder inte roterar tooa annan nyckel.

   ![Skärmbild av nyckelfönstret med den nya versionen markerad](./media/data-lake-store-encryption/keynewversion.png)

4.  Bläddra toohello Data Lake Store storage-konto och markera **kryptering**.

    ![Skärmbild av Data Lake Store-lagringskontofönstret, med kryptering markerat](./media/data-lake-store-encryption/select-encryption.png)

5.  Ett meddelande om att en ny nyckel version av hello nyckeln är tillgänglig. Klicka på **rotera nyckeln** tooupdate hello viktiga toohello ny version.

    ![Skärmbild av Data Lake Store-fönstret med meddelande och nyckelrotering markerat](./media/data-lake-store-encryption/rotatekey.png)

Den här åtgärden tar mindre än två minuter och det finns inga förväntad avbrottstid på grund av tookey rotation. När hello åtgärd har slutförts, används hello ny version av hello nyckel.
