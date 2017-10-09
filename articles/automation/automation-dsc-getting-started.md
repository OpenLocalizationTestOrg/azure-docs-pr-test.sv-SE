---
title: "aaaGetting igång med Azure Automation DSC | Microsoft Docs"
description: "Förklaring och exempel på hello vanligaste uppgifterna i Azure Automation önskad tillstånd Configuration (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 82910c96e928b9264c2e1b52a5c98dc47273dcc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a>Komma igång med Azure Automation DSC
Det här avsnittet beskrivs hur toodo hello vanligaste uppgifterna med Azure Automation önskad tillstånd Configuration (DSC), till exempel att skapa, importera, och kompilering konfigurationer, onboarding för att hantera datorer, och visa rapporter. En översikt över vilka Azure Automation DSC är finns [översikt över Azure Automation DSC](automation-dsc-overview.md). DSC-dokumentation finns [Windows PowerShell Desired Configuration översikt över](https://msdn.microsoft.com/PowerShell/dsc/overview).

Det här avsnittet innehåller en stegvis guide toousing Azure Automation DSC. Om du vill att en exempel-miljö som redan har konfigurerat utan hello stegen som beskrivs i det här avsnittet, kan du använda [hello följande ARM-mallen](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup). Den här mallen ställer in en slutförd Azure Automation DSC-miljö, inklusive en Azure-dator som hanteras av Azure Automation DSC.

## <a name="prerequisites"></a>Krav
toocomplete hello exemplen i det här avsnittet, hello följande krävs:

* Ett Azure Automation-konto. Instruktioner om hur du skapar ett Kör som-konto för Azure Automation finns i [Azure Kör som-konto](automation-sec-configure-azure-runas-account.md).
* En Azure Resource Manager VM (inte klassiskt) kör Windows Server 2008 R2 eller senare. Anvisningar om hur du skapar en virtuell dator finns [skapa din första Windows virtuell dator i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>Skapa en DSC-konfiguration
Vi skapar en enkel [DSC-konfigurationen](https://msdn.microsoft.com/powershell/dsc/configurations) som säkerställer hello förekomsten eller frånvaron av hello **Web Server** Windows funktionen (IIS), beroende på hur du tilldelar noder.

1. Starta hello Windows PowerShell ISE (eller valfri textredigerare).
2. Skriv hello följande text:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. Spara hello som `TestConfig.ps1`.

Den här konfigurationen anropar en resurs i varje nod i block hello [WindowsFeature resurs](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), som säkerställer hello förekomsten eller frånvaron av hello **Web Server** funktion.

## <a name="importing-a-configuration-into-azure-automation"></a>Importera en konfiguration i Azure Automation
Vi kommer därefter importera hello konfiguration till hello Automation-konto.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**.
4. På hello **DSC-konfigurationer** bladet, klickar du på **lägga till en konfiguration**.
5. På hello **importkonfigurationen** bladet, bläddra toohello `TestConfig.ps1` fil på din dator.
   
    ![Skärmbild av hello ** importera konfigurationen ** bladet](./media/automation-dsc-getting-started/AddConfig.png)
6. Klicka på **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Visa en konfiguration i Azure Automation
När du har importerat en konfiguration, kan du visa den i hello Azure-portalen.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**
4. På hello **DSC-konfigurationer** bladet, klickar du på **TestConfig** (detta är hello namn på hello-konfiguration som du har importerat i hello ovan).
5. På hello **TestConfig Configuration** bladet, klickar du på **visa konfigurationskälla**.
   
    ![Skärmbild av bladet för hello TestConfig konfiguration](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    En **TestConfig konfigurationskälla** öppnas bladet visar hello PowerShell-koden för hello konfiguration.

## <a name="compiling-a-configuration-in-azure-automation"></a>Kompilera en konfiguration i Azure Automation
Innan du kan tillämpa en tillstånd tooa nod, måste en DSC-konfiguration som definierar det aktuella tillståndet vara kompileras till ett eller flera nodkonfigurationer (MOF dokument) och placeras på hello Automation DSC Pull-Server. En mer detaljerad beskrivning av kompilering konfigurationer i Azure Automation DSC, se [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md). Mer information om kompilering konfigurationer finns [DSC-konfigurationer](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**
4. På hello **DSC-konfigurationer** bladet, klickar du på **TestConfig** (hello namnet på hello tidigare importerat konfigurationen).
5. På hello **TestConfig Configuration** bladet, klickar du på **Kompilera**, och klicka sedan på **Ja**. Detta startar ett jobb för kompilering.
   
    ![Skärmbild av hello TestConfig configuration bladet syntaxmarkering kompilera knappen](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> När du kompilerar en konfiguration i Azure Automation distribuerar automatiskt alla skapade nod MOF-filer toohello pull konfigurationsservern.
> 
> 

## <a name="viewing-a-compilation-job"></a>Visa en kompileringsjobbet
När du startar en sammanställning kan du visa den i hello **Kompileringsjobb** panelen i hello **Configuration** bladet. Hej **Kompileringsjobb** panelen visar för närvarande körs slutfört och misslyckade jobb. När du öppnar en sammanställning jobb-bladet visas information om det jobbet inklusive fel eller varningar uppstod, indataparametrar som används i hello konfiguration och kompilering loggar.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**.
4. På hello **DSC-konfigurationer** bladet, klickar du på **TestConfig** (hello namnet på hello tidigare importerat konfigurationen).
5. På hello **Kompileringsjobb** för hello **TestConfig Configuration** bladet, klickar du på någon av hello jobb visas. En **Kompileringsjobbet** blad öppnas med hello datum då hello kompileringsjobbet startades.
   
    ![Skärmbild av bladet för hello Kompileringsjobbet](./media/automation-dsc-getting-started/CompilationJob.png)
6. Klicka på panelen i hello **Kompileringsjobbet** bladet toosee ytterligare information om hello jobb.

## <a name="viewing-node-configurations"></a>Visa nodkonfigurationer
Slutförande av en kompileringsjobbet skapar en eller flera nya nodkonfigurationer. Konfiguration av noden är en MOF-dokument som är distribuerade toohello hämtningsservern och redo toobe hämtas och används av en eller flera noder. Du kan visa hello nodkonfigurationer i ditt Automation-konto i hello **DSC-Nodkonfigurationer** bladet. Konfiguration av noden har ett namn med formatet hello *ConfigurationName*. *Nodnamn*.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-Nodkonfigurationer**.
   
    ![Skärmbild av bladet för hello DSC-Nodkonfigurationer](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Ett Azure virtuella datorn för hantering med Azure Automation DSC
Du kan använda Azure Automation DSC toomanage virtuella Azure-datorer (både klassiska och hanteraren för filserverresurser), lokala virtuella datorer, Linux-datorer, AWS virtuella datorer och lokala fysiska datorer. I det här avsnittet beskriver vi hur tooonboard endast Azure Resource Manager virtuella datorer. Information om onboarding finns andra typer av datorer, [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md).

### <a name="tooonboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>tooonboard en Azure Resource Manager-VM för hantering av Azure Automation DSC
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.
4. I hello **DSC-noder** bladet, klickar du på **lägga till Azure VM**.
   
    ![Skärmbild av hello DSC-noder bladet syntaxmarkering hello lägga till Virtuella Azure-knappen](./media/automation-dsc-getting-started/OnboardVM.png)
5. I hello **lägga till virtuella Azure-datorer** bladet, klickar du på **Välj virtuella datorer tooonboard**.
6. I hello **Välj virtuella datorer** bladet välj hello VM som du vill tooonboard och på **OK**.
   
   > [!IMPORTANT]
   > Det här måste vara en Azure Resource Manager VM kör Windows Server 2008 R2 eller senare.
   > 
   > 
7. I hello **lägga till virtuella Azure-datorer** bladet, klickar du på **konfigurera registreringsdata**.
8. I hello **registrering** bladet ange hello namn hello nodkonfiguration som du vill tooapply toohello VM i hello **Nodkonfigurationsnamnet** rutan. Detta måste exakt matcha hello namnet på en nodkonfiguration i hello Automation-konto. Att tillhandahålla ett namn i det här läget är valfritt. Du kan ändra hello tilldelade nodkonfiguration efter onboarding hello nod.
   Kontrollera **starta om noden om det behövs**, och klicka sedan på **OK**.
   
    ![Skärmbild av bladet för hello-registrering](./media/automation-dsc-getting-started/RegisterVM.png)
   
    hello nodkonfiguration som du har angett kommer att tillämpas toohello VM med intervall som anges av hello **Konfigurationslägesfrekvens**, och hello VM ska söka efter uppdateringar toohello nodkonfiguration med intervall som anges av hello  **Uppdateringsfrekvens**. Läs mer om hur dessa värden används [konfigurera hello Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. I hello **lägga till virtuella Azure-datorer** bladet, klickar du på **skapa**.

Azure startar hello processen för onboarding hello VM. När den är klar hello VM visas i hello **DSC-noder** bladet i hello Automation-konto.

## <a name="viewing-hello-list-of-dsc-nodes"></a>Visa hello lista över DSC-noder
Du kan visa hello lista över alla datorer som har publicerats så för hantering i ditt Automation-konto i hello **DSC-noder** bladet.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.

## <a name="viewing-reports-for-dsc-nodes"></a>Visa rapporter för DSC-noder
Varje gång som Azure Automation DSC utför en konsekvenskontroll på en hanterad nod skickar hello nod en status tillbaka toohello pull-rapportserver. Du kan visa rapporterna på hello bladet för noden.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.
4. På hello **rapporter** panelen, klicka på någon av hello rapporter i hello-listan.
   
    ![Skärmbild av bladet för hello-rapport](./media/automation-dsc-getting-started/NodeReport.png)

På hello bladet för en enskild rapport visas hello efter information om status för hello motsvarande konsekvenskontroll kontrollera:

* Hej rapportera status – om hello nod är ”kompatibla”, hello configuration ”misslyckades” eller hello noden är ”inkompatibla” (när hello nod är i **applyandmonitor** läge och hello datorn inte är i hello önskad tillstånd).
* hello starttid för hello konsekvenskontroll.
* Kontrollera hello totala körtiden hello konsekvent.
* Kontrollera hello typ av konsekvenskontroll.
* Fel, inklusive hello kod och fel visas. 
* Alla DSC-resurser som används i hello konfiguration och hello tillståndet för varje resurs (om hello noden är hello önskad status för den här resursen) – du kan klicka på varje resurs tooget mer detaljerad information för den här resursen.
* hello namn, IP-adress och konfigurationsläge för hello-nod.

Du kan också klicka på **Visa rapport om oformaterat** toosee hello faktiska data som hello nod skickar toohello server. Mer information om hur du använder dessa data finns [med hjälp av en rapportserver för DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).

Det kan ta lite tid när en nod har publicerats så innan hello första rapporten är tillgänglig. Du kan behöva toowait in too30 minuter för hello första rapporten efter att du publicera en nod.

## <a name="reassigning-a-node-tooa-different-node-configuration"></a>Tilldela en annan nodkonfiguration av noden tooa
Du kan tilldela en nod toouse en annan nodkonfiguration än hello en som du ursprungligen har tilldelats.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.
4. På hello **DSC-noder** bladet, klickar du på namnet för hello hello nod som du vill tooreassign.
5. Klicka på hello bladet för noden **tilldela noden**.
   
    ![Skärmbild av hello nod bladet syntaxmarkering hello tilldela nod knappen](./media/automation-dsc-getting-started/AssignNode.png)
6. På hello **tilldela nodkonfiguration** bladet, Välj hello nod configuration toowhich du vill tooassign hello nod och klickar sedan på **OK**.
   
    ![Skärmbild av bladet för hello tilldela nodkonfiguration](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Avregistrerar en nod
Om du inte längre vill att en nod toobe som hanteras av Azure Automation DSC kan du avregistrera det.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.
3. På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.
4. På hello **DSC-noder** bladet, klickar du på namnet för hello hello nod som du vill toounregister.
5. Klicka på hello bladet för noden **avregistrera**.
   
    ![Skärmbild av hello nod bladet syntaxmarkering hello Unregister-knappen](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Relaterade artiklar
* [Översikt över Azure Automation DSC](automation-dsc-overview.md)
* [Onboarding-datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md)
* [Windows PowerShell Desired State Configuration-översikt](https://msdn.microsoft.com/powershell/dsc/overview)
* [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* [Priser för Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)

