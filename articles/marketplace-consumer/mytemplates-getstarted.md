---
title: "aaaGet igång med privata mallar | Microsoft Docs"
description: "Lägga till, hantera och dela dina privata mallar med hjälp av hello Azure-portalen, hello Azure CLI eller PowerShell."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.openlocfilehash: 1fe2c6422f62a98f7ae9ba5c61b9639d993f0bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-private-templates-on-hello-azure-portal"></a>Kom igång med privata mallar på hello Azure-portalen
En [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) mallen är en deklarativ mall används toodefine din distribution. Du kan definiera hello resurser toodeploy för en lösning och ange parametrar och variabler som gör att du tooinput värden för olika miljöer. hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution.

Du kan använda hello nya **mallar** funktionen hello [Azure Portal](https://portal.azure.com) tillsammans med hello **Microsoft.Gallery** resursprovidern som ett tillägg för hello [ Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable användare toocreate, hantera och distribuera privata mallar från personliga bibliotek.

Det här dokumentet vägleder dig genom att lägga till, hantera och dela en privat **mallen** med hello Azure-portalen.

## <a name="guidance"></a>Riktlinjer
hello följande rekommendationer hjälper dig att dra full nytta av **mallar** när du arbetar med dina lösningar:

* En **Mall** är en inkapslande resurs som innehåller en Resource Manager-mall och ytterligare metadata. Den fungerar väldigt likt tooan objekt i hello Marketplace. hello viktigaste skillnaden är att den är privat som skillnad från toohello offentliga Marketplace-objekt.
* Hej **mallar** biblioteket fungerar bra för användare som behöver toocustomize distributioner.
* **Mallar** fungerar bra för användare som behöver en enkel lagringsplats i Azure.
* Börja med en befintlig Resource Manager-mall Hitta mallar i [github](https://github.com/Azure/azure-quickstart-templates) eller [Exportera mallen](../azure-resource-manager/resource-manager-export-template.md) från en befintlig resursgrupp.
* **Mallar** är bundet toohello användare som publicerar dem.. hello utgivarens namn är synligt tooeveryone som har läsbehörighet tooit.
* **Mallar** är Resource Manager-resurser och det går inte att byta namn på dem efter att de publicerats.

## <a name="add-a-template-resource"></a>Lägg till en mallresurs
Det finns två sätt toocreate en **mallen** resurs i hello Azure-portalen.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Metod 1: Skapa en ny mallresurs från en resursgrupp som körs
1. Navigera tooan befintlig resursgrupp hello Azure-portalen. Välj **Exportera mall** i **Inställningar**.
2. När hello Resource Manager-mall har exporterats kan du använda hello **Spara mall** knappen toosave den toohello **mallar** databasen. Hitta komplett information för att Exportera mall [här](../azure-resource-manager/resource-manager-export-template.md).
   <br /><br />
   ![Exportera resursgrupp](media/rg-export-portal1.PNG)  <br />
3. Välj hello **spara tooTemplate** kommandoknapp.
   <br /><br />
4. Ange hello följande information:
   
   * Namn – namnet på hello mallobjektet (Obs: Detta är ett Azure Resource Manager-baserat namn. Alla namngivningsbegränsningar gäller och namn kan inte ändras efter att de skapats).
   * Beskrivning – snabb sammanfattning om hello mallen.
     
     ![Spara mall](media/save-template-portal1.PNG)  <br />
5. Klicka på **Spara**.
   
   > [!NOTE]
   > hello visar Export mall bladet meddelanden när hello exporterade Resource Manager-mallen innehåller fel, men du kommer fortfarande att kunna toosave mallar för toohello denna Resource Manager-mall. Se till att du kontrollerar och korrigerar eventuella Resource Manager mallen problem innan omdistribuera hello exporterade Resource Manager-mall.
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a>Metod 2: Lägg till en ny Mall-resurs från bläddra
Du kan också lägga till en ny **mallen** med hello kommandoknappen + Lägg till i **Bläddra > mallar**. Du behöver tooprovide ett namn, beskrivning och hello Resource Manager-mallens JSON.

![Lägg till mall](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> Microsoft.Gallery är en klientbaserad Azure-resursprovider. hello är mallresursen bundet toohello användare som skapade den. Det är inte bunden tooany viss prenumeration. En prenumeration måste toobe valt endast när du distribuerar en mall.
> 
> 

## <a name="view-template-resources"></a>Visa mallresurser
Alla **mallar** tillgängliga tooyou kan ses i **Bläddra > mallar**. Detta omfattar de **mallar** du har skapat samt de som har delats med dig med olika behörighetsnivåer. Mer information finns i hello [åtkomstkontroll](#access-control-for-a-tenant-resource-provider) nedan.

![Visa mall](media/view-template-portal1.PNG)  <br />

Du kan visa hello information om en **mallen** genom att klicka på ett objekt i listan hello.

![Visa mall](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Redigera en mallresurs
Du kan initiera hello redigera flödet för en **mallen** genom att högerklicka på hello artikeln på hello bläddringslistan eller genom att välja kommandoknappen Redigera för hello.

![Redigera mall](media/edit-template-portal1a.PNG)  <br />

Du kan redigera hello beskrivningen eller Resource Manager-mallens text. Du kan inte redigera hello namn eftersom det är ett resursnamn för resource Manager. När du redigerar hello Resource Manager-mallens JSON kommer vi verifiera tooensure att den är giltig JSON. Välj **OK** och sedan **spara** toosave den uppdaterade mallen.

![Redigera mall](media/edit-template-portal2a.PNG)  <br />

En gång hello **mallen** har sparats visas ett meddelande om bekräftelse.

![Redigera mall](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Distribuera en mallresurs
Du kan distribuera alla **mallar** som du har **läs**behörigheter på. Hej distributionsflödet startar hello standard Azure-mallens distributionsblad. Fyll i hello värden för Resource Manager hello mallen parametrar tooproceed med hello-distribution.

![Distribuera mallen](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Dela en mallresurs
En **mall**resurs kan delas med kollegor. Delning fungerar på samma sätt för[rolltilldelning för en resurs på Azure](../active-directory/role-based-access-control-configure.md). Hej **mallen** ägare ger tooother användare som kan interagera med en mallresurs. hello person eller grupp med personer som du delar hello **mallen** med blir kan toosee hello Resource Manager-mall och dess egenskaper.

### <a name="access-control-for-hello-microsoftgallery-resources"></a>Åtkomstkontroll för hello Microsoft.Gallery-resurser
| Roll | Behörigheter |
| --- | --- |
| Ägare |Tillåter fullständig behörighet för hello mallresursen inklusive delning |
| Läsare |Tillåter Läs- och mallresursen på hello mall-resurs |
| Deltagare |Tillåter redigera och ta bort på hello mall-resurs. Användare kan inte dela hello mallen med andra |

Välj **resursen** på hello objektet genom att högerklicka eller på hello vybladet för ett specifikt objekt. Detta startar en delningsupplevelse.

![Dela mall](media/share-template-portal1a.png)  <br />

 Nu kan du välja en roll och en användare eller grupp tooprovide åtkomst tooa viss **mallen**. hello tillgängliga roller är ägare, läsare och deltagare. Mer information finns i hello [åtkomstkontroll](#access-control-for-a-tenant-resource-provider) ovan.

![Dela mall](media/share-template-portal2b.png)  <br />

![Dela mall](media/share-template-portal3b.png)  <br />

Klicka på **Välj** och **Ok**. Du kan nu se hello användare eller grupper du har lagt till toohello resurs.

![Dela mall](media/share-template-portal4b.png)  <br />

> [!NOTE]
> En mall kan endast delas med användare och grupper i hello samma Azure Active Directory-klient. Om du delar en mall med en e-postadress som inte är i din klient skickas en inbjudan ber användaren hello toojoin hello klientorganisationen som en gäst.
> 
> 

## <a name="next-steps"></a>Nästa steg
* toolearn om hur du skapar Resource Manager-mallar finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md)
* toounderstand hello funktioner som kan användas i en Resource Manager-mallen finns [Mallfunktioner](../azure-resource-manager/resource-group-template-functions.md)
* Anvisningar om hur du skapar mallar finns [Metodtips för att utforma Azure Resource Manager-mallar](../azure-resource-manager/best-practices-resource-manager-design-templates.md)

