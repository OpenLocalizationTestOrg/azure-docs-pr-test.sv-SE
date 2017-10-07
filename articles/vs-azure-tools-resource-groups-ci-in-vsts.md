---
title: aaaContinuous integrering i VS Team Services med Azure-resursgruppsprojekt | Microsoft Docs
description: Beskriver hur tooset in kontinuerlig integration i Visual Studio Team Services med Azure-resursgrupp distribution projekt i Visual Studio.
services: visual-studio-online
documentationcenter: na
author: mlearned
manager: erickson-doug
editor: 
ms.assetid: b81c172a-be87-4adc-861e-d20b94be9e38
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: mlearned
ms.openlocfilehash: 0fe4a4b8989ee323e8ef2206fa4ebed503025670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Kontinuerlig integration i Visual Studio Team Services med hjälp av projekt för distribution av Azure-resursgrupp
toodeploy en Azure-mall du utför uppgifter i olika faser: skapa, testa, kopiera tooAzure (även kallat ”mellanlagring”) och distribuerar mallen. Det finns två olika sätt toodeploy mallar tooVisual Studio Team Services (VS Team Services). Båda metoderna och ange hello samma resultat, så Välj hello som bäst passar ditt arbetsflöde.

1. Lägg till en enda steg tooyour build-definition som kör hello PowerShell-skript som ingår i projekt för distribution av Azure-resursgrupp hello (distribuera AzureResourceGroup.ps1). hello skriptet kopierar artefakter och distribuerar sedan hello mallen.
2. Lägga till flera VS Team Services skapa steg, var och en utförs steget.

Den här artikeln visar båda alternativen. hello första alternativet har hello fördelen med hello samma skript används av utvecklare i Visual Studio och ger konsekvens i hela hello livscykel. andra alternativ för hello erbjuder ett enkelt alternativ toohello inbyggda skript. Båda procedurer förutsätter att du redan har ett projekt för distribution av Visual Studio checkas in VS Team Services.

## <a name="copy-artifacts-tooazure"></a>Kopiera artefakter tooAzure
Om du har alla artefakter som behövs för malldistribution av, oavsett hello scenario måste du ge Azure Resource Manager åtkomst toothem. Dessa artefakter kan innehålla filer som:

* Kapslade mallar
* Konfigurationsskript och DSC-skript
* Binärfiler

### <a name="nested-templates-and-configuration-scripts"></a>Kapslade mallar och konfigurationsskript
När du använder hello mallar som ingår i Visual Studio (eller skapats med Visual Studio kodavsnitt) hello PowerShell-skript skapar inte bara hello artefakter, den också parameterizes hello URI för hello resurser för olika distributioner. hello skript och sedan kopierar hello artefakter tooa säkra behållare i Azure, skapas en SaS-token för behållaren och skickar sedan informationen på toohello malldistribution. Se [skapar en för malldistribution](https://msdn.microsoft.com/library/azure/dn790564.aspx) toolearn mer om kapslade mallar.  När du använder uppgifter i VS Team Services, måste du väljer hello lämpliga uppgifter för din för malldistribution och eventuellt ange parametervärden från hello mellanlagring steg toohello malldistribution.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Ställ in kontinuerlig distribution i VS Team Services
toocall hello PowerShell-skriptet i VS Team Services behöver du tooupdate build-definition. I korthet är hello stegen: 

1. Redigera hello build-definition.
2. Ställ in Azure auktorisering VS Team Services.
3. Lägg till ett Azure PowerShell build steg som refererar till hello PowerShell-skript i projekt för distribution av hello Azure-resursgrupp.
4. Värdet för hello hello *- ArtifactsStagingDirectory* parametern toowork med ett projekt som skapats i VS Team Services.

### <a name="detailed-walkthrough-for-option-1"></a>Detaljerad genomgång av alternativ 1
hello vägleder följande procedurer dig genom hello steg nödvändiga tooconfigure kontinuerlig distribution i VS Team Services med hjälp av en enskild aktivitet som körs hello PowerShell-skript i projektet. 

1. Redigera din VS Team Services build-definition och lägger till en Azure PowerShell build-steget. Välj hello build definition under hello **skapa definitioner** kategori och välj sedan hello **redigera** länk.
   
   ![Redigera definition av build][0]
2. Lägg till en ny **Azure PowerShell** skapa steg toohello build definition och välj sedan hello **Lägg till build steg...** till.
   
   ![Lägg till build steg][1]
3. Välj hello **distribuera aktiviteten** kategori, Välj hello **Azure PowerShell** uppgift och välj sedan dess **Lägg till** knappen.
   
   ![Lägg till aktiviteter][2]
4. Välj hello **Azure PowerShell** skapa steg och fyll sedan i dess värden.
   
   1. Om du redan har en Azure-tjänsteslutpunkt lagts tooVS Team Services, Välj hello prenumeration i hello **Azure-prenumeration** nedrullningsbara listrutan och hoppa över toohello nästa avsnitt. 
      
      Om du inte har en Azure-tjänsteslutpunkt i VS Team Services, måste tooadd en. Detta underavsnitt tar dig igenom processen hello. Om ditt Azure-konto använder ett Microsoft-konto (till exempel Hotmail), måste du utföra följande steg tooget hello tjänstens huvudnamn-autentisering.
   2. Välj hello **hantera** länka nästa toohello **Azure-prenumeration** nedrullningsbara listrutan.
      
      ![Hantera Azure-prenumerationer][3]
   3. Välj **Azure** i hello **nya tjänstslutpunkten** nedrullningsbara listrutan.
      
      ![Nya tjänstslutpunkten][4]
   4. I hello **Lägg till Azure-prenumeration** dialogrutan, Välj hello **tjänstens huvudnamn** alternativet.
      
      ![Tjänstens huvudnamn alternativet][5]
   5. Lägg till din Azure-prenumeration information toohello **Lägg till Azure-prenumeration** dialogrutan. Du behöver tooprovide hello följande objekt:
      
      * Prenumerations-Id
      * Prenumerationsnamn
      * Tjänstens huvudnamn Id
      * Tjänstens huvudnamn nyckel
      * Klient-Id
   6. Lägga till ett namn för ditt val toohello **prenumeration** namnrutan. Det här värdet som visas längre fram i hello **Azure-prenumeration** listrutan i VS Team Services. 
   7. Om du inte vet Azure prenumerations-ID, du kan använda något av följande kommandon tooretrieve hello den.
      
      Använd för PowerShell-skript:
      
      `Get-AzureRmSubscription`
      
      Om du använder Azure CLI använder du:
      
      `azure account show`
   8. tooget Service Principal-ID, Service Principal Key och klient-ID, följ hello procedur i [Skapa Active Directory-program och tjänstens huvudnamn med hjälp av portalen](resource-group-create-service-principal-portal.md) eller [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](resource-group-authenticate-service-principal.md).
   9. Lägg till hello tjänstens huvudnamn ID, Service Principal Key och klient-ID värden toohello **Lägg till Azure-prenumeration** dialogrutan och sedan väljer hello **OK** knappen.
      
      Nu har du en giltig Service Principal toouse toorun hello Azure PowerShell-skript.
5. Redigera definition av hello build och välj hello **Azure PowerShell** skapa steg. Välj hello prenumeration i hello **Azure-prenumeration** nedrullningsbara listrutan. (Om hello prenumeration inte visas välja hello **uppdatera** knappen Nästa hello **hantera** länken.) 
   
   ![Konfigurera Azure PowerShell build-aktivitet][8]
6. Ange en sökväg toohello distribuera AzureResourceGroup.ps1 PowerShell-skript. toodo, Välj hello ellips (...)-knappen Nästa toohello **skriptsökvägen** går toohello distribuera AzureResourceGroup.ps1 PowerShell-skript i hello **skript** mapp i ditt projekt, markerar den och välj sedan hello **OK** knappen.    
   
   ![Välj sökväg tooscript][9]
7. När du har valt hello skript uppdateringsskript hello sökvägen toohello så att den körs från hello Build.StagingDirectory (hello samma katalog som *ArtifactsLocation* har angetts till). Du kan göra detta genom att lägga till ”$(Build.StagingDirectory)/” toohello början av hello skriptets sökväg.
   
    ![Redigera sökvägen tooscript][10]
8. I hello **skriptargument** ange hello följande parametrar (i en enda rad). När du kör hello skript i Visual Studio, kan du se hur VS använder hello parametrar i hello **utdata** fönster. Du kan använda den som en startpunkt för att ange parametervärden för hello i build-steget.
   
   | Parameter | Beskrivning |
   | --- | --- |
   | -ResourceGroupLocation |Hej geografiska plats värdet var hello resursgruppen finns, som **eastus** eller **'Östra USA'**. (Lägg till enkla citattecken om det finns ett blanksteg i hello namn.) Se [Azure-regioner](https://azure.microsoft.com/en-us/regions/) för mer information. |
   | -ResourceGroupName |hello namnet på hello resursgruppens namn används för den här distributionen. |
   | -UploadArtifacts |Den här parametern när anger att artefakter som behöver toobe upp tooAzure från hello lokalt system. Du behöver bara tooset den här växeln om mallen distributionen kräver extra artefakter som du vill använda hello PowerShell-skript (exempelvis konfigurationsskript eller kapslade mallar) toostage. |
   | -StorageAccountName |hello namnet på lagringskontot för hello används toostage artefakter för den här distributionen. Den här parametern används bara om du Förproduktion artefakter för distribution. Om den här parametern anges skapas ett nytt lagringskonto om hello skriptet inte har skapat något under en tidigare distribution. Om hello-parametern anges finnas hello lagringskontot redan. |
   | -StorageAccountResourceGroupName |hello namnet på hello resursgrupp som är associerade med hello storage-konto. Den här parametern krävs endast om du anger ett värde för hello StorageAccountName parameter. |
   | -TemplateFile |hello sökvägen toohello mallfil i projekt för distribution av hello Azure-resursgrupp. tooenhance flexibilitet, Använd en sökväg för den här parametern är relativ toohello plats av hello PowerShell-skript i stället för en absolut sökväg. |
   | -TemplateParametersFile |hello sökvägen toohello parameterfilen i projekt för distribution av hello Azure-resursgrupp. tooenhance flexibilitet, Använd en sökväg för den här parametern är relativ toohello plats av hello PowerShell-skript i stället för en absolut sökväg. |
   | -ArtifactStagingDirectory |Den här parametern kan hello PowerShell-skript vet hello mapp var hello projektet binära filer ska kopieras. Det här värdet åsidosätter standardvärdet för hello används av hello PowerShell-skript. För VS Team Services använder värdet hello: - ArtifactStagingDirectory $(Build.StagingDirectory) |
   
   Här är ett exempel på skript argument (bruten för att läsa rad):
   
   ```    
   -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
   -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
   –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)'    
   ```
   
   När du är klar hello **skriptargument** rutan bör likna hello följande lista:
   
   ![Skriptargument][11]
9. När du har lagt till alla hello nödvändiga objekt toohello Azure PowerShell skapa steg, väljer hello **kön** knappen toobuild hello projekt. Hej **skapa** skärmen visar hello utdata från hello PowerShell-skript.

### <a name="detailed-walkthrough-for-option-2"></a>Detaljerad genomgång av alternativ 2
hello vägleder följande procedurer dig genom hello steg nödvändiga tooconfigure kontinuerlig distribution i VS Team Services med hjälp av inbyggda hello-uppgifter.

1. Redigera din VS Team Services build definition tooadd två nya build steg. Välj hello build definition under hello **skapa definitioner** kategori och välj sedan hello **redigera** länk.
   
   ![Redigera build attributdefinitionstabellen][12]
2. Lägg till hello nya skapa steg toohello build-definitionen skapades med hello **Lägg till build steg...** till.
   
   ![Lägg till build steg][13]
3. Välj hello **distribuera** aktivitetskategori, Välj hello **Azure filkopiering** uppgift och välj sedan dess **Lägg till** knappen.
   
   ![Lägg till Azure filkopiering aktivitet][14]
4. Välj hello **Azure resurs för Gruppdistributionen** uppgift och välj sedan dess **Lägg till** knappen och sedan **Stäng** hello **aktivitet katalogen**.
   
   ![Lägg till Azure-resursgrupp distribution aktivitet][15]
5. Välj hello **Azure filkopiering** uppgift och Fyll i dess värden.
   
   Om du redan har en Azure-tjänsteslutpunkt lagts tooVS Team Services, Välj hello prenumeration i hello **Azure-prenumeration** nedrullningsbara listrutan. Om du inte har en prenumeration, se [alternativ 1](#detailed-walkthrough-for-option-1) anvisningar för att konfigurera en i VS Team Services.
   
   * Käll - ange **$(Build.StagingDirectory)**
   * Azure anslutningstypen - Välj **Azure Resource Manager**
   * Azure RM-prenumeration – Välj hello prenumerationen för hello storage-konto som du vill toouse i hello **Azure-prenumeration** nedrullningsbara listrutan. Om hello prenumeration inte visas välja hello **uppdatera** knappen Nästa hello **hantera** länk.
   * Måltypen - Välj **Azure Blob**
   * RM Lagringskonto - Välj hello storage-konto du vill toouse för mellanlagring artefakter
   * Behållarens namn - Ange hello namn för hello behållare som toouse för mellanlagring; Det kan vara vilket behållarnamn som giltiga, men använder en dedikerad toothis build-definition
   
   För hello utdatavärden:
   
   * Ange lagringsbehållaren URI - **artifactsLocation**
   * Storage-behållare SAS-Token - ange **artifactsLocationSasToken**
   
   ![Konfigurera Azure filkopiering aktivitet][16]
6. Välj hello **Azure Resource distribution** skapa steg och fyll sedan i dess värden.
   
   * Azure anslutningstypen - Välj **Azure Resource Manager**
   * Azure RM-prenumeration – Välj hello prenumeration för distribution i hello **Azure-prenumeration** nedrullningsbara listrutan. Det här är vanligtvis hello samma prenumeration som används i föregående steg i hello
   * Åtgärd - väljer **skapa eller uppdatera resursgruppen.**
   * Resursgrupp – Välj en resursgrupp eller ange hello namnet på en ny resursgrupp för hello distribution
   * Plats – Välj hello plats för hello resursgrupp
   * Mall - ange hello sökvägen och namnet på hello mallen toobe distribueras tillagt **$(Build.StagingDirectory)**, till exempel: **$(Build.StagingDirectory/DSC-CI/azuredeploy.json)**
   * Mallparametrar - ange hello sökvägen och namnet på hello parametrar toobe används, tillagt **$(Build.StagingDirectory)**, till exempel: **$(Build.StagingDirectory/DSC-CI/azuredeploy.parameters.json)**
   * Åsidosätt mallparametrar – ange eller kopiera och klistra in följande kod hello:
     
     ```    
     -_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken (ConvertTo-SecureString -String "$(artifactsLocationSasToken)" -AsPlainText -Force)
     ```
     ![Konfigurera Azure-resursgrupp distribution aktivitet][17]
7. När du har lagt till alla objekt i hello krävs, spara hello build definition och välj **kö nya build** hello överst.

## <a name="next-steps"></a>Nästa steg
Läs [översikt över Azure Resource Manager](azure-resource-manager/resource-group-overview.md) toolearn mer om Azure Resource Manager och Azure-resursgrupper.

[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
[12]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough13.png
[13]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough14.png
[14]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough15.png
[15]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough16.png
[16]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough17.png
[17]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough18.png
