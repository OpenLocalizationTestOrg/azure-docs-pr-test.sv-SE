---
title: aaaAzure hanterade program i hello Marketplace | Microsoft Docs
description: "Beskriver Azure hanterade program som är tillgängliga via hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b3cdf3f1fccdd47db699e4892ae8bce35118bfd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-in-hello-marketplace"></a>Azure hanterade program i hello Marketplace

 MSPs ISV: er och systemintegrerare (SIs) kan använda Azure hanterade program toooffer kunderna lösningar tooall Azure Marketplace. Sådana lösningar minska hello Service och underhåll kostnader för kunder. Utgivare kan sälja infrastruktur- och programvara via hello Marketplace. De kan koppla tjänster och operativa stöd toomanaged program. Mer information finns i [hanteras Programöversikt](managed-application-overview.md).

Den här artikeln beskrivs hur en MSP, ISV eller SI kan publicera ett program toohello Marketplace och gör den tillgänglig toocustomers.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Krav för att publicera ett hanterat program

Krav för toolisting i hello Marketplace:

* Tekniska

    *  Information om hello grundläggande struktur och syntaxen för Azure Resource Manager-mallar finns [Azure Resource Manager-mallar](resource-group-authoring-templates.md).
    *  tooview fullständig mallen lösningar finns [Azure-snabbstartsmallar](https://azure.microsoft.com/en-us/documentation/templates/) eller hello [Quickstart mallen databasen](https://github.com/azure/azure-quickstart-templates).
    *  Information om hur toocreate hello gränssnitt för kunder som distribuerar ditt program via hello Marketplace finns [skapa en fil för användaren gränssnittsdefinition](managed-application-createuidefinition-overview.md).

* Sökassistent (affärsbehov)

    *   Ditt företag eller dess dotterbolag måste finnas i ett land där försäljning stöds av hello Marketplace.
    *   Produkten måste ha licens på ett sätt som är kompatibel med fakturering modeller som stöds av hello Marketplace.
    *   Du är ansvarig för att teknisk support tillgängliga toocustomers på ett kommersiellt rimliga sätt. hello stöd kan vara fria, betald, eller via community stöder.
    *   Du är ansvarig för att licensiera programmet och eventuella beroenden för programvara från tredje part.
    *   Du måste ange innehåll som uppfyller villkoren för ditt erbjudande toobe som visas i hello Marketplace och hello Azure-portalen.
    *   Du måste acceptera toohello villkoren i hello Azure Marketplace deltagande principer eller utgivare avtal.
    *   Du måste acceptera toocomply med hello användningsvillkoren och sekretesspolicyn för Microsoft certifierad programmet avtalet för Microsoft Azure.

## <a name="create-a-new-azure-application-offer"></a>Skapa ett nytt erbjudande för Azure-program

Efter att du uppfyller hello förutsättningar du är klar toocreate erbjudandet hanterade program. Låt oss ta en snabb överblick över ett erbjudande och en SKU.

### <a name="offer"></a>Erbjudande

hello-erbjudande för ett hanterat program motsvarar tooa klass av produkten erbjudande från en utgivare. Om du har en ny typ av lösningen-program som du vill toomake som är tillgängliga i hello Marketplace kan konfigurera du den som ett nytt erbjudande. Ett erbjudande är en samling av SKU: er. Varje erbjudande visas som sin egen enhet i hello Marketplace.

### <a name="sku"></a>SKU

En SKU är hello minsta köpbara ett erbjudande. Du kan använda en SKU inom hello samma produkten klass (erbjudandet) toodifferentiate mellan:

* Olika funktioner som stöds.
* Om hello erbjudandet är hanterad eller ohanterad.
* Fakturering modeller som stöds.

En SKU visas under hello överordnade erbjudandet i hello Marketplace. Det verkar som sin egen köpbara entitet i hello Azure-portalen.

### <a name="set-up-an-offer"></a>Konfigurera ett erbjudande

1. Logga in toohello [molnpartnerportalen](https://cloudpartner.azure.com/).

2. Hello navigeringsfönstret hello vänster och välj **+ nytt erbjudande** > **Azure program**.

    ![Nytt erbjudande](./media/managed-application-author-marketplace/newOffer.png)

3. Fylla i hello formulär som visas på hello kvar i hello **Editor** vyn. Obligatoriska fält är markerade med en röd asterisk (*).

    ![Inställningar för erbjudande](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Fyra huvudsakliga formulär är används toocreate ett hanterat program:

    a. Inställningar för erbjudande

    b. SKU: er

    c. Marketplace

    d. Support

Formulären beskrivs mer detaljerat i följande avsnitt hello.

## <a name="offer-settings-form"></a>Erbjudande inställningar för formulär
Använd den här grundformat toospecify hello erbjudande inställningarna.

1. Fyll i hello **erbjuder inställningar** formuläret. hello olika fält är:

    a. **Erbjudande-ID**: den unika identifieraren identifierar hello erbjudandet i en profil för utgivaren. Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter. Det kan bara består av gemena alfanumeriska tecken och bindestreck (-). hello-ID får inte sluta med ett streck. Det är begränsad tooa högst 50 tecken. När ett erbjudande lanseras är i det här fältet låst.

    b. **Publicerings-ID**: Använd den här listrutan toochoose hello publisher profilen du vill toopublish erbjudandet under. När ett erbjudande lanseras är i det här fältet låst.

    c. **Namnet**: det här visningsnamnet för ditt erbjudande visas i hello Marketplace och hello portal. Det kan ha högst 50 tecken. Innehåller ett okänt varumärke för produkten. Inkludera inte ditt företagsnamn, om det inte är hur släpps. Om du marknadsföring erbjudandet på din egen webbplats, kan du kontrollera att hello-namn är exakt hur den visas på webbplatsen.

2. Välj **spara** toosave ditt arbete. 

## <a name="skus-form"></a>SKU: er formulär
hello nästa steg är tooadd SKU: er för ditt erbjudande.

1. Välj **SKU: er** > **nya SKU**. 

    ![Välj ny SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Ange en **SKU ID**. En SKU-ID är en unik identifierare för hello SKU inom ett erbjudande. Detta ID visas i produkten URL: er, Resource Manager-mallar och fakturering rapporter. Det kan bara består av gemena alfanumeriska tecken och bindestreck (-). hello-ID får inte sluta med ett streck och är begränsat tooa högst 50 tecken. När ett erbjudande lanseras är i det här fältet låst. Du kan ha flera SKU: er i ett erbjudande. En SKU måste du planera toopublish för varje avbildning.

3. Fylla hello **SKU information** avsnittet hello följande format:

    ![Ange nya SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Fyll ut hello följande fält:
    
    a. **Rubrik**: Ange en rubrik för denna SKU. Den här rubriken visas i hello-galleriet för det här objektet.

    b. **Översikt över**: Ange en kort sammanfattning för denna SKU. Den här texten visas under hello rubrik.

    c. **Beskrivning**: Ange en detaljerad beskrivning om hello SKU.

    d. **SKU-typen**: hello tillåtna värden är **hanterat program** och **Lösningsmallar**. Det här fallet markerar **hanterat program**.

4. Fylla hello **Paketinformation** avsnittet hello följande format:

    ![Paket](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Fyll ut hello följande fält:

    a. **Aktuell Version**: Ange en version för hello-paketet som du överför. Det måste vara i formatet för hello `{number}.{number}.{number}{number}`.

    b. **Välj en paketfil**: det här paketet innehåller hello följande filer som är komprimerade till en ZIP-fil:
    * **applianceMainTemplate.json**: hello distribution mallfilen som har använt toodeploy hello lösning/application. Information om hur toocreate mallfilerna för distribution, se [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: den här filen används av hello Azure portal toogenerate hello-användargränssnittet som har använt tooprovision lösning/programmet. Mer information finns i [Kom igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: den här mallen innehåller endast hello Microsoft.Solution/appliances resurs. Hej mainTemplate filen innehåller hello följande egenskaper:

        *  **typ**: Använd **Marketplace** för hanterade program i hello Marketplace.
        *  **ManagedResourceGroupId**: den här resursgruppen i hello kundens prenumeration är där alla hello-resurser som definierats i applianceMainTemplate.json har distribuerats.
        *  **PublisherPackageId**: denna sträng som unikt identifierar hello-paketet. Ange hello värde i hello-format för `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.

Hämta hello **erbjuder ID** och **Publicerings-ID** från hello publicering portal, enligt följande bild hello:

![Erbjudande-ID](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Hämta hello **SKU ID**som visas i följande bild hello:

![SKU-ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Hämtar hello paketet **Version**som visas i följande bild hello:

![Paketversion](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  Baserat på hello föregående exempel hello värdet för **PublisherPackageId** är `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  Exempel på mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify hello name of hello storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

Det här paketet innehåller kapslade mallar eller skript som är nödvändiga toosuccessfully etablera det här programmet. Hej måste mainTemplate.json, applianceMainTemplate.json och applianceCreateUIDefinition.json filer finnas hello rotmappen.

* **Tillstånd**: den här egenskapen definierar vilka som får åtkomst och hello åtkomstnivå toohello resurser i kundernas prenumerationer. hello publisher kan använda den toomanage hello program för hello kunds räkning.
* **PrincipalId**: den här egenskapen är hello Azure Active Directory (AD Azure) identifierare för en användare, grupp eller ett program som har beviljats vissa behörigheter för hello resurser i hello kundens prenumeration. hello rolldefinitionen beskriver hello behörigheter. 
* **Rolldefinitionen**: den här egenskapen är en lista över alla hello inbyggda rollbaserad åtkomstkontroll (RBAC) roller tillhandahålls av Azure AD. Du kan välja hello roll som är mest lämpliga toouse toomanage hello resurser för hello kunds räkning.

Du kan lägga till flera tillstånd. Vi rekommenderar att du skapar en grupp i AD-användare och ange dess ID i **PrincipalId**. På så sätt kan du kan lägga till fler användare toohello användargrupp utan hello måste tooupdate hello SKU.

Läs mer om hur RBAC [komma igång med RBAC på hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Marketplace-formulär

hello Marketplace formuläret begär fält som visas på hello [Azure Marketplace](https://azuremarketplace.microsoft.com) och på hello [Azure-portalen](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Prenumerations-ID: N för förhandsgranskning

Ange en lista över Azure-prenumeration ID: N som kan komma åt hello erbjudande när den har publicerats. Du kan använda dessa vitt visas prenumerationer tootest hello förhandsgranskas erbjudande innan du gör den live. Du kan sammanställa en lista för tillåten för in too100 prenumerationer i hello partnerportalen.

### <a name="suggested-categories"></a>Föreslagna kategorier

Välj toofive kategorier hello listan som erbjudandet bäst kan associeras med. Dessa kategorier är används toomap ditt erbjudande toohello produktkategorier som är tillgängliga i hello [Azure Marketplace](https://azuremarketplace.microsoft.com) och hello [Azure-portalen](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

hello sammanfattning av det hanterade programmet visar hello följande fält:

![Marketplace-sammanfattning](./media/managed-application-author-marketplace/publishvm10.png)

Hej **översikt över** för det hanterade programmet visar hello följande fält:

![Marketplace-översikt](./media/managed-application-author-marketplace/publishvm11.png)

Hej **planer + priser** för det hanterade programmet visar hello följande fält:

![Marketplace-planer](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

hello sammanfattning av det hanterade programmet visar hello följande fält:

![Översikt över Portal](./media/managed-application-author-marketplace/publishvm12.png)

hello översikt för det hanterade programmet visar hello följande fält:

![Portalen översikt](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Logotypen riktlinjer

Följ dessa riktlinjer för en logotyp som du överför i hello molnpartnerportalen:

*   hello Azure design har en enkel färgpalett. Begränsa hello antalet primära och sekundära färger i din logotyp.
*   hello temafärger hello portalen är vit och svart. Använd inte färgerna som hello bakgrundsfärg för din logotyp. Använd en färg som gör din logotyp framträdande hello-portalen. Vi rekommenderar enkla primära färger. *Om du använder en genomskinlig bakgrund, kontrollera att hello logotyp och texten inte vitt, svart eller blå.*
*   Använd inte en toning bakgrund på hello-logotypen.
*   Placera inte text på hello logotyp, inte ens företaget eller varumärke. hello utseende och känslan av din logotyp ska vara platt och undvika toningar.
*   Kontrollera att hello logotypen inte har sträckts ut.

#### <a name="hero-logo"></a>Hjälte-logotyp

hello hjälte logotypen är valfritt. hello publisher kan välja inte tooupload en hjälte logotyp. När hello hjälte ikonen har överförts kan inte tas bort. Hello partner måste följa hello Marketplace riktlinjer för hjälte ikoner som helst.

Följ dessa riktlinjer för hello hjälte logotypen ikon:

*   hello utgivarens namn, hello plan rubrik och hello erbjudande lång sammanfattning visas i vitt. Därför inte använda en ljusare hello bakgrunden hello hjälte ikon. En svart, vit eller genomskinlig bakgrund är inte tillåten för hjälte ikoner.
*   När hello erbjudande visas hello publisher visningsnamn, hello plan rubrik, hello erbjudande lång sammanfattning och hello **skapa** knappen inbäddad programmässigt i hello hjälte logotyp. Därför inte ange valfri text när du utformar hello hjälte logotyp. Lämna tomt utrymme på hello rätt eftersom hello text programmässigt ingår i detta utrymme. hello tomt utrymme för hello text ska 415 x 100 bildpunkter på hello rätt. Det är förskjutning med 370 bildpunkter från hello vänster.

    ![Hjälte logotypen exempel](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Stöd för formulär

Fyll i hello **stöder** formuläret med stöd för kontakter från ditt företag. Den här informationen kan vara engineering customer support kontakter.

## <a name="publish-an-offer"></a>Publicera ett erbjudande

När du fyller i alla hello avsnitt markerar **publicera** toostart hello process som gör att din tillgängliga toocustomers erbjudandet.

## <a name="next-steps"></a>Nästa steg

* En introduktion toomanaged program, se [hanteras Programöversikt](managed-application-overview.md).
* Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).
* Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).
* Information om att använda ett Tjänstkatalogen hanterade program, se [använder ett Tjänstkatalogen hanterade program](managed-application-consumption.md).
