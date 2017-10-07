---
title: "aaaConfigure hello roller för en Azure cloud service med Visual Studio | Microsoft Docs"
description: "Lär dig hur tooset upp och konfigurera roller för Azure-molntjänster med Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: d3c62eb57040ebe987787e73b17b468bb82122bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>Konfigurera roller för Azure cloud service med Visual Studio
En Azure-molntjänst kan ha en eller flera worker webbroller. För varje roll måste toodefine hur rollen har ställts in och även konfigurera hur rollen körs. toolearn mer information om roller i molntjänster, se hello video [introduktion tooAzure molntjänster](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

hello-information för din molntjänst lagras i hello följande filer:

- **ServiceDefinition.csdef** -hello tjänstdefinitionsfilen definierar hello körningsinställningar för Molntjänsten, inklusive vilka roller är obligatoriska, slutpunkter och storlek på virtuell dator. Ingen av hello data som lagras i `ServiceDefinition.csdef` kan ändras när den körs.
- **ServiceConfiguration.cscfg** - hello tjänstkonfigurationsfilen konfigurerar hur många instanser av en roll körs och hello värden för hello inställningar för en roll. Hej data som lagras i `ServiceConfiguration.cscfg` kan ändras när din roll körs.

toostore olika värden för hello inställningar som styr hur en roll körs, kan du definiera flera konfigurationer. Du kan använda en annan tjänstkonfiguration för varje distributionsmiljö. Du kan till exempel ange din lagring konto anslutning sträng toouse hello lokala Azure storage-emulatorn i en lokal tjänst-konfiguration och skapa en annan service configuration toouse Azure storage i hello molnet.

När du skapar en Azure-molntjänst i Visual Studio två tjänstkonfiguration skapas automatiskt och läggas till tooyour Azure-projekt:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Konfigurera en Azure-molntjänst
Du kan konfigurera en Azure-molntjänst från Solution Explorer i Visual Studio enligt hello följande steg:

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **egenskaper**.
   
    ![Snabbmenyn för Solution Explorer-projekt](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Välj hello i hello projektets egenskapssida **Development** fliken. 

    ![Projektet egenskapssidan - utveckling fliken](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. I hello **tjänstkonfiguration** listan, Välj hello namnet på tjänstkonfigurationen för hello som du vill tooedit. (Om du vill toomake ändras tooall hello tjänstkonfiguration för den här rollen, Välj **alla konfigurationer av**.)
   
    > [!IMPORTANT]
    > Om du väljer en specifik tjänstkonfiguration har vissa egenskaper inaktiverats eftersom de kan bara anges för alla konfigurationer. tooedit dessa egenskaper, måste du välja **alla konfigurationer av**.
    > 
    > 
   
    ![Tjänstkonfigurationen listan för en Azure-molntjänst](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-hello-number-of-role-instances"></a>Ändra hello antalet rollinstanser
Du kan ändra hello antal instanser av en roll som körs, baserat på hello antalet användare eller hello belastning som förväntas för en viss roll tooimprove hello prestanda för Molntjänsten. En separat virtuell dator skapas för varje instans av rollen när hello molnbaserad tjänst som körs i Azure. Detta påverkar hello fakturering för hello distribution av den här Molntjänsten. Mer information om fakturering finns [förstå fakturan för Microsoft Azure](billing/billing-understand-your-bill.md).

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, expandera hello projektnoden. Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Välj hello **Configuration** fliken.

    ![Konfigurationsfliken](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. I hello **tjänstkonfiguration** listan, Välj hello tjänstkonfiguration som du vill tooupdate.
   
    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. I hello **instansen antal** text Ange hello antalet instanser som du vill toostart för den här rollen. När du publicerar hello cloud service tooAzure körs varje instans på en separat virtuell dator.

    ![Uppdatera hello antal instanser](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. Från hello Visual Studio i verktygsfältet väljer **spara**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Hantera anslutningssträngar för storage-konton
Du kan lägga till, ta bort eller ändra anslutningssträngar för service-konfigurationer. Du kan exempelvis lokala anslutningssträngen för en lokal tjänst-konfiguration som har värdet `UseDevelopmentStorage=true`. Du kan också tooconfigure en tjänstkonfiguration i molnet som använder ett lagringskonto i Azure.

> [!WARNING]
> När du anger hello Azure storage-konto viktig information för en anslutningssträng för storage-konto, lagras informationen lokalt i konfigurationsfilen för hello-tjänsten. Men den här informationen lagras för närvarande inte som krypterad text.
> 
> 

Genom att använda ett annat värde för varje service-konfiguration kan du inte har toouse olika anslutningssträngar i Molntjänsten eller ändra koden när du publicerar ditt moln tjänsten tooAzure. Du kan använda hello samma namn för hello anslutningssträngen i koden och hello-värdet är olika, baserat på hello tjänstkonfiguration som du väljer när du skapar din molntjänst eller när du publicerar den.

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, expandera hello projektnoden. Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Välj hello **inställningar** fliken.

    ![Fliken Inställningar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. I hello **tjänstkonfiguration** listan, Välj hello tjänstkonfiguration som du vill tooupdate.

    ![Tjänstkonfiguration](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Välj tooadd en anslutningssträng **Lägg till inställning**.

    ![Lägg till anslutningssträng](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. När en ny inställning för hello har lagts toohello lista, uppdatera hello rad i hello lista hello nödvändig information.

    ![Ny anslutningssträng](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Namnet** -ange hello namn som du vill toouse för hello anslutningssträngen.
    - **Typen** – Välj **anslutningssträngen** hello nedrullningsbara listan.
    - **Värdet** -du kan antingen ange hello anslutningssträngen direkt i hello **värdet** cell eller välj hello ellips (...) toowork i hello **skapa Lagringsanslutningssträng** dialogrutan.  

1. I hello **skapa Lagringsanslutningssträng** väljer ett alternativ för **ansluta med**. Följ sedan instruktionerna för hello för hello alternativ du väljer:

    - **Microsoft Azure storage-emulatorn** -om du väljer det här alternativet hello återstående inställningar i dialogrutan hello har inaktiverats eftersom de gäller endast tooAzure. Välj **OK**.
    - **Din prenumeration** – om du väljer det här alternativet, väljer du Använd hello listrutan tooeither och logga in ett Microsoft-konto eller Lägg till ett Microsoft-konto. Välj ett Azure-prenumeration och storage-konto. Välj **OK**.
    - **Autentiseringsuppgifterna anges manuellt** – ange hello lagringskontonamn och antingen hello primärnyckel eller andra. Välj ett alternativ för **anslutning** (HTTPS rekommenderas för de flesta fall.) Välj **OK**.

1. toodelete en anslutningssträng Välj hello anslutningssträngen och välj sedan **ta bort inställningen**.

1. Från hello Visual Studio i verktygsfältet väljer **spara**.

## <a name="programmatically-access-a-connection-string"></a>Komma åt en anslutningssträng

hello följande steg visar hur tooprogrammatically åt en anslutningssträng med C#.

1. Lägg till följande hello med direktiven tooa C#-filen där du ska toouse hello inställningen:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. hello följande kod visar ett exempel på hur tooaccess en anslutningssträng. Ersätt hello &lt;ConnectionStringName > med hello lämpligt värde. 

    ```csharp
    // Setup hello connection tooAzure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-toouse-in-your-azure-cloud-service"></a>Lägga till anpassade inställningar toouse i Azure-molntjänst
Anpassade inställningar i konfigurationsfilen för hello service kan du lägga till ett namn och värde för en sträng för en specifik tjänst-konfiguration. Du kan välja toouse den här inställningen tooconfigure som en funktion i din molntjänst genom att läsa värdet hello hello inställningen och använda det här värdet toocontrol hello logik i koden. Du kan ändra konfigurationsvärdena tjänsten utan att behöva toorebuild ditt abonnemang eller när Molntjänsten körs. Koden kan söka efter meddelanden om ändringar i en inställning. Se [RoleEnvironment.Changing händelse](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Du kan lägga till, ta bort eller ändra anpassade inställningar för din tjänstkonfiguration. Du kanske vill olika värden för de här strängarna för olika konfigurationer.

Genom att använda ett annat värde för varje service-konfiguration kan du inte har toouse olika strängar i Molntjänsten eller ändra koden när du publicerar ditt moln tjänsten tooAzure. Du kan använda hello samma namn för hello sträng i koden och hello-värdet är olika, baserat på hello tjänstkonfiguration som du väljer när du skapar din molntjänst eller när du publicerar den.

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, expandera hello projektnoden. Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Välj hello **inställningar** fliken.

    ![Fliken Inställningar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. I hello **tjänstkonfiguration** listan, Välj hello tjänstkonfiguration som du vill tooupdate.

    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Välj tooadd en anpassad inställning **Lägg till inställning**.

    ![Lägg till anpassad inställning](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. När en ny inställning för hello har lagts toohello lista, uppdatera hello rad i hello lista hello nödvändig information.

    ![Ny anpassad inställning](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Namnet** -ange hello namn hello-inställning.
    - **Typen** – Välj **sträng** hello nedrullningsbara listan.
    - **Värdet** -ange hello hello inställningens värde. Du kan antingen ange hello värde direkt i hello **värdet** cell eller välj hello ellips (...) tooenter hello värde i hello **Redigera sträng** dialogrutan.  

1. toodelete en anpassad inställning Välj hello inställning och välj sedan **ta bort inställningen**.

1. Från hello Visual Studio i verktygsfältet väljer **spara**.

## <a name="programmatically-access-a-custom-settings-value"></a>Komma åt en inställningens värde
 
hello följande steg visar hur tooprogrammatically åt en anpassad inställning med C#.

1. Lägg till följande hello med direktiven tooa C#-filen där du ska toouse hello inställningen:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. hello följande kod visar ett exempel på hur tooaccess anpassade inställningen. Ersätt hello &lt;SettingName > med hello lämpligt värde. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Hantera lokal lagring för varje rollinstans
Du kan lägga till lokala system fillagring för varje instans av en roll. hello data lagras i denna lagring inte är tillgänglig i andra instanser av hello roll för vilka hello data lagras eller av andra roller.  

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, expandera hello projektnoden. Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Välj hello **lokal lagring** fliken.

    ![Fliken lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. I hello **tjänstkonfiguration** lista, se till att **alla konfigurationer av** är markerad som hello lokal Lagringsinställningar gäller tooall tjänstkonfiguration. Ett annat värde resulterar i alla hello inmatningsfält i hello sidan håller på att inaktiveras. 

    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. tooadd en lokal lagring-post, Välj **Lägg till lokal lagring**.

    ![Lägg till lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. När hello ny lokal lagring post har lagts till toohello lista, uppdatera hello rad i hello lista hello nödvändig information.

    ![Ny post för lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Namnet** -ange hello namn som du vill toouse för hello ny lokal lagring.
    - **Storlek (MB)** -ange hello storlek i MB som du behöver för hello ny lokal lagring.
    - **Rensa på rollen återvinning** -Markera det här alternativet tooremove hello data i hello ny lokal lagring när hello virtuell dator för hello rollen återanvänds.

1. toodelete lokal lagring-posten väljer hello-post och välj sedan **ta bort lokal lagring**.

1. Från hello Visual Studio i verktygsfältet väljer **spara**.

## <a name="programmatically-accessing-local-storage"></a>Att komma åt lokal lagring

Detta avsnitt visar hur tooprogrammatically åt lokal lagring med hjälp av C# genom att skriva en test-textfil `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-toolocal-storage"></a>Skriv en toolocal fillagring text

hello följande kod visar ett exempel på hur toowrite text fillagring toolocal. Ersätt hello &lt;LocalStorageName > med hello lämpligt värde. 

    ```csharp
    // Retrieve an object that points toohello local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define hello file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-toolocal-storage"></a>Hitta en fil skrivs toolocal lagring

tooview hello-filen som skapas av hello koden i hello föregående avsnitt, Följ dessa steg:
    
1.  I meddelandefältet i Windows hello, högerklicka hello Azure ikon och, hello snabbmenyn, Välj **visa Compute Emulator UI**. 

    ![Visa Azure-beräkningsemulatorn](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Välj hello webbroll.

    ![Azure-beräkningsemulatorn](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. På hello **Microsoft Azure Compute Emulator** väljer du **verktyg** > **öppna lokalt Arkiv**.

    ![Öppna lokalt Arkiv menyobjekt](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Ange när hello Windows Explorer öppnas ' MyLocalStorageTest.txt'' i hello **Sök** text och markera **RETUR** toostart hello sökning. 

## <a name="next-steps"></a>Nästa steg
Lär dig mer om Azure-projekt i Visual Studio genom att läsa [konfigurera ett Azure-projekt](vs-azure-tools-configuring-an-azure-project.md). Mer information om hello cloud service schemat genom att läsa [Schemareferens](https://msdn.microsoft.com/library/azure/dd179398).

