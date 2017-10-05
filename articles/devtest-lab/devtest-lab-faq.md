---
title: "Azure DevTest Labs vanliga frågor och svar | Microsoft Docs"
description: "Få svar på vanliga frågor i Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: tarcher
ms.openlocfilehash: 8ee4a0fd714027cdc77247ec8dc259fe62442564
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs vanliga frågor och svar
Den här artikeln besvarar några av de vanligaste frågorna om Azure DevTest Labs.

**Allmänt**
## <a name="what-if-my-question-isnt-answered-here"></a>Vad gör jag om min fråga inte besvaras här?
Om din fråga inte finns med här, meddela oss så att vi kan hjälpa dig att hitta ett svar.

* Skicka en fråga i den [Disqus-tråden](#comments) i slutet av dessa vanliga frågor och genomföra med Azure Cache-teamet och andra gruppmedlemmar om den här artikeln.
* För att nå en bredare publik ställa en fråga på den [Azure DevTest Labs MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), och interagera med Azure DevTest Labs teamet och andra medlemmar i gruppen.
* Om du vill göra en funktionsbegäran skicka din begäran och idéer om den [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Varför ska jag använda Azure DevTest Labs?
Azure DevTest Labs kan spara ditt team tid och pengar. Utvecklare kan skapa sina egna miljöer med flera olika databaser och använda artefakter att snabbt distribuera och konfigurera program. Använda anpassade avbildningar och formler, kan virtuella datorer sparas som en mall och enkelt återges. Dessutom erbjuder labs flera konfigurerbara principer som gör att lab administratörer att minska skräp och hantera en grupps miljöer. Dessa principer omfattar automatisk avstängning, kostnad tröskelvärdet maximala virtuella datorer per användare och maximala storlekar på VM. En mer detaljerad förklaring av Azure DevTest Labs, läsa den [översikt](devtest-lab-overview.md) eller titta på den [inledande video](/documentation/videos/videos/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Vad ”problemfri, självbetjäning” betyder?
”Problemfri, självbetjäning” innebär att utvecklare och testare skapa sina egna miljöer efter behov och administratörer har säkerhet att veta att Azure DevTest Labs hjälper till att minimera avfallshantering och kontrollera kostnaden. Administratörer kan ange vilka VM-storlekar tillåts kan det maximala antalet virtuella datorer, och när virtuella datorer är igång och stänga av. Azure DevTest Labs också enkelt att övervaka kostnaderna och ställa in aviseringar ska vara medveten om hur resurserna i labbet används.

## <a name="how-can-i-use-azure-devtest-labs"></a>Hur kan jag använda Azure DevTest Labs?
Azure DevTest Labs är användbar när du kräver dev eller testmiljöer och du vill återskapa dem snabbt och/eller hantera dem med kostnadsbesparingar principer.

Här följer några scenarier som våra kunder använder Azure DevTest Labs för:

* Hanterar utvecklings- och testmiljöer på en plats kan bygger använda principer för att minska kostnaderna och anpassade avbildningar till delar i teamet.
* Utveckla ett program som använder anpassade avbildningar för att spara diskstatusen under utveckling steg.
* För att spåra kostnaden i förhållande till pågår.
* Skapa drivrutinslista testmiljöer för quality assurance testning.
* Använder artefakter och formler för att enkelt konfigurera och återskapa ett program i olika miljöer.
* Distribuera virtuella datorer för hackathons (utvecklings- eller samarbete) och sedan enkelt avetablering dem när händelsen slutar.

## <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hur är debiteras för Azure DevTest Labs?
Azure DevTest Labs är en kostnadsfri, vilket innebär att skapa labs och konfigurera principer, mallar och artefakter är ledig. Du betalar bara för Azure-resurser används i din labs, till exempel virtuella datorer, lagringskonton och virtuella nätverk. Mer information om kostnaderna för labbet resurser Läs mer om [priser för Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Säkerhet**
## <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Vilka är de olika säkerhetsnivåerna i Azure DevTest Labs?
Säkerhetsåtkomst bestäms av [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-built-in-roles.md). För att förstå hur åtkomst fungerar, hjälper det att du förstår skillnaderna mellan en behörighet, en roll och ett omfång som definieras av RBAC.

* **Behörighet** -behörigheter är en definierad åtkomst till en specifik åtgärd. En behörighet kan till exempel vara läsbehörighet till alla virtuella datorer.
* **Rollen** -en roll är en uppsättning behörigheter som kan grupperas och tilldelats till en användare. Till exempel har ”prenumerationsägaren” åtkomst till alla resurser inom en prenumeration.
* **Scope** -scope är en nivå i hierarkin för Azure-resurs. Ett omfång kan exempelvis vara en resursgrupp eller ett enda labb eller hela prenumerationen.

Det finns två typer av roller för att definiera behörigheter inom omfånget för Azure DevTest Labs: labbägaren och lab-användare.

* **Labbägaren** -en labbägaren har åtkomst till resurser i labbet. Därför kan de ändra principer, läsa och skriva alla virtuella datorer, ändra det virtuella nätverket och så vidare.
* **Lab-användare** - lab-användare kan visa alla lab resurser, till exempel virtuella datorer, principer och virtuella nätverk, men de kan inte ändra principer eller alla virtuella datorer som skapats av andra användare. Det är också möjligt att skapa anpassade roller i Azure DevTest Labs och du kan lära dig hur du gör i artikeln [bevilja användarbehörighet att principer för specifika labbet](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Eftersom scope är hierarkiska, när en användare har behörighet för ett visst område kan beviljas de automatiskt dessa behörighet för definitionsområdet var lägre nivå finns. Till exempel om en användare har tilldelats rollen prenumerationsägaren, har sedan de åtkomst till alla resurser i en prenumeration. Dessa resurser inkluderar alla virtuella datorer, alla virtuella nätverk och alla övningar. Därför ärver en prenumerationsägaren automatiskt labbägaren roll. Motsatsen är inte sant. En labbägaren har åtkomst till ett labb som är en omfattning som är lägre än prenumerationsnivån. En labbägaren är därför inte kan se virtuella datorer eller virtuella nätverk eller resurser som ligger utanför labbet.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hur skapar jag en roll om du vill tillåta användare att utföra en viss uppgift?
En omfattande artikel om hur du skapar anpassade roller och tilldela behörigheter till rollen hittar du här. Här är ett exempel på ett skript som skapar rollen ”DevTest Labs avancerade användare”, som har behörighet att starta och stoppa alla virtuella datorer i labbet:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advance User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  


**CI/CD-Integration och automatisering**
## <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Azure DevTest Labs kan integreras med min CI/CD toolchain?
Om du använder VSTS är en [Azure DevTest Labs uppgifter tillägget](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) som gör det möjligt att automatisera dina versionen pipeline i Azure DevTest Labs. Några av användning av det här tillägget är:

* Skapa och distribuera en virtuell dator automatiskt och konfigurera den med den senaste versionen med hjälp av Azure filkopiering eller PowerShell VSTS uppgifter.
* Samla in tillståndet för en virtuell dator efter tester för att återskapa ett programfel på samma virtuella dator för att vidare undersöka automatiskt.
* Ta bort den virtuella datorn i slutet av pipeline versionen när den inte längre behövs.

Följande blogginlägg ge vägledning och information om hur du använder tillägget VSTS:

* [Azure DevTest Labs – VSTS tillägg](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Distribuera en ny virtuell dator i en befintlig AzureDevTestLab från VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Med hjälp av VSTS versionshantering för kontinuerlig distributioner till AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

För andra CI/CD toolchains de tidigare nämnda scenarier som kan ske via tillägget VSTS uppgifter kan ske på liknande sätt via distribuera [Azure Resource Manager-mallar](https://aka.ms/dtlquickstarttemplate) med [Azure PowerShell-cmdlets](../azure-resource-manager/resource-group-template-deploy.md) och [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Du kan också använda [REST API: er för DevTest Labs](http://aka.ms/dtlrestapis) att integrera med din toolchain.  


**Virtual Machines**
## <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Varför kan jag inte se vissa virtuella datorer i Azure Virtual Machines bladet som visas i Azure DevTest Labs?
När en virtuell dator skapas i Azure DevTest Labs ges behörighet att komma åt den virtuella datorn. Du kan visa den både i bladet labs och **virtuella datorer** bladet. Användare i rollen DevTest Labs kan se alla virtuella datorer som skapats i labbet via testmiljön **alla virtuella datorer** bladet. Användare i rollen DevTest Labs är dock inte automatiskt beviljats läsbehörighet till VM resurser som andra har skapat. Därför visas inte dessa virtuella datorer i den **virtuella datorer** bladet.

## <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Vad är skillnaden mellan anpassade avbildningar och formler?
En anpassad avbildning är en virtuell Hårddisk (virtuell hårddisk), medan en formel är en avbildning som du kan konfigurera ytterligare inställningar som du kan spara och återskapa. En anpassad avbildning kan vara bäst om du snabbt vill skapa flera miljöer med samma grundläggande, ändras avbildningen. En formel kan vara bättre om du vill återskapa konfigurationen av den virtuella datorn med den senaste bits, ett virtuellt undernät eller en viss storlek. En mer ingående förklaring finns i artikeln [jämföra anpassade avbildningar och formler i DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hur gör jag skapa flera virtuella datorer från samma mall på samma gång?
Du kan använda den [VSTS uppgifter tillägget](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) eller [Generera en Azure Resource Manager-mall](devtest-lab-add-vm.md#save-azure-resource-manager-template) när du skapar en virtuell dator och [distribuera Azure Resource Manager-mall från Windows PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Hur flytta mina befintliga virtuella Azure-datorer i min Azure DevTest Labs lab?
Följ stegen nedan för att kopiera dina befintliga virtuella datorer till Azure DevTest Labs:

1. Kopiera VHD-filen på den befintliga virtuella datorn med den här [Windows PowerShell-skript](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Skapa den anpassade avbildningen](devtest-lab-create-template.md) i Azure DevTest Labs-labb.
3. Skapa en virtuell dator i labbet från den anpassade avbildningen

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kan jag ansluta flera diskar till Mina virtuella datorer?
Bifoga flera diskar till virtuella datorer stöds.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Om jag vill använda en Windows-operativsystemsavbildning för min testning måste jag köpa en MSDN-prenumeration?
Om du behöver använda Windows klient-OS-avbildningar (Windows 7 eller senare) för din utveckling och testning i Azure Ja, måste du antingen:

- [Köp en prenumeration på MSDN](https://www.visualstudio.com/products/how-to-buy-vs).
- Om du har ett Enterprise-avtal, skapa en Azure-prenumeration med den [utveckling och testning Enterprise erbjudande](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

Läs mer om Azure-krediter för varje MSDN-erbjudande [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hur jag för att automatisera processen att överföra VHD-filer för att skapa anpassade avbildningar?
Det finns två alternativ:

* [Azure AzCopy](../storage/common/storage-use-azcopy.md#blob-upload) kan användas för att kopiera eller överför VHD-filer till storage-konto som är kopplade till labbet.
* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som körs på Windows, OSX och Linux.   

Följ dessa steg för att hitta målet storage-konto som är associerade med ditt Labb:

1. Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **resursgrupper** från den vänstra panelen.
3. Leta upp och markera den resursgrupp som är associerade med ditt labb.
4. På den **översikt** bladet Välj en av storage-konton.
5. Välj **Blobbar**.
6. Leta efter överföringar i listan. Om inget finns tillbaka till steg #4 och försök med ett annat lagringskonto.
7. Använd den **URL** som mål i AzCopy-kommandot.

## <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hur automatisera processen med att ta bort alla virtuella datorer i min labbet?
Förutom att ta bort virtuella datorer från ditt labb i Azure-portalen kan du ta bort alla virtuella datorer i ditt labb använder ett PowerShell-skript. I följande exempel ändrar parametervärden under den **värden för att ändra** kommentar. Du kan hämta den `subscriptionId`, `labResourceGroup`, och `labName` värdena från bladet labb i Azure-portalen.

    # Delete all the VMs in a lab

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Artefakter**
## <a name="what-are-artifacts"></a>Vad är artefakter?
Artefakter är anpassningsbara element som kan användas för att distribuera din senaste bitar eller dev-verktyg på en virtuell dator. De är kopplade till den virtuella datorn under genereringen av med ett par enkla klick och när den virtuella datorn har etablerats artefakter distribuera och konfigurera den virtuella datorn. Det finns olika befintliga artefakter i vår [offentliga GitHub-lagringsplatsen](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), men du kan också enkelt [redigera egna artefakter](devtest-lab-artifact-author.md).


**Konfiguration av testlabb**
## <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hur skapar jag en lab från en Azure Resource Manager-mall
Vi har angett en [GitHub-lagringsplatsen för labbet Azure Resource Manager-mallar](https://aka.ms/dtlquickstarttemplate) som du kan distribuera som-är eller ändra för att skapa anpassade mallar för din testlabb. Var och en av dessa mallar innehåller en länk som du kan klicka på för att distribuera labbet som-är under din egen Azure-prenumeration, eller så kan du anpassa mallen och [distribueras med hjälp av PowerShell eller Azure CLI](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Varför skapas Mina virtuella datorer i olika resursgrupper med godtyckliga namn? Kan jag byta namn på eller ändra de här resursgrupperna?
Det här sättet skapas resursgrupper för Azure DevTest Labs att hantera behörigheter och åtkomst till virtuella datorer. Medan du kan flytta den virtuella datorn till en annan resursgrupp med namnet på din önskade, rekommenderas göra så inte. Vi arbetar på att förbättra upplevelsen för att tillåta mer flexibilitet.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hur många labs kan skapa under samma prenumeration?
Det finns ingen specifik gräns för antalet labs som kan skapas per prenumeration. De resurser som används är dock begränsade per prenumeration. Du kan läsa om den [gränser och kvoter som införts på Azure-prenumerationer](../azure-subscription-service-limits.md) och [öka gränserna](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Hur många virtuella datorer kan skapa per labb?
Det finns ingen specifik gräns för antalet virtuella datorer som kan skapas per labb. De resurser som används är dock begränsad per prenumeration (t.ex. Virtuella kärnor, offentliga IP-adresser, etc.). Du kan läsa om den [gränser och kvoter som införts på Azure-prenumerationer](../azure-subscription-service-limits.md) och [öka gränserna](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hur dela en direktlänk till min labbet?
Du kan utföra följande procedur om du vill dela en direktlänk till lab-användare:

1. Bläddra till labb i Azure-portalen.
2. Kopiera URL som lab från din webbläsare och dela den med lab-användare.

> [!NOTE]
> Om användarna labbet är externa användare med en [Microsoft-konto](#what-is-a-microsoft-account) och de inte tillhör ditt företags Active directory, de kan få ett felmeddelande när du navigerar till den angivna länken. Om de får ett felmeddelande, be dem att klicka på namnet i det övre högra hörnet i Azure-portalen och välj den katalog där labbet finns från den **Directory** på menyn.
>
>

## <a name="what-is-a-microsoft-account"></a>Vad är ett Microsoft-konto?
Ett Microsoft-konto är det du använder för nästan allt du gör med Microsoft-enheter och tjänster. Det är en e-postadress och lösenord som du använder för att logga in på Skype, Outlook.com, OneDrive, Windows Phone och Xbox LIVE – det innebär att dina filer, foton, kontakter och inställningar kan följa för alla enheter.

> [!NOTE]
> Microsoft-konto för att anropa ”Windows Live ID”.
>
>


**Felsökning**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Min artefakt misslyckades under skapande av virtuell dator. Hur felsöker jag det?
Referera till [felsökning artefakt fel i DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md) att lära dig hur du hämtar loggarna om din misslyckade artefakt.

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Varför är min befintliga virtuella nätverk sparas korrekt?
Ett sätt är att din virtuella nätverksnamnet innehåller punkter. I så fall, försök att ta bort perioderna eller ersätta dem med bindestreck och försök sedan spara det virtuella nätverket igen.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Varför visas ett ”överordnade resursen hittades inte” fel vid etablering av en virtuell dator från PowerShell?
När en resurs är en överordnad till en annan resurs, måste den överordnade resursen finnas innan du skapar den underordnade resursen. Om det inte finns, visas en **ParentResourceNotFound** fel. Om du inte anger ett beroende på den överordnade resursen kan underordnade resursen distribueras innan överordnat.

Virtuella datorer som är underordnade resurser under ett labb i en resursgrupp. När du använder Azure Resource Manager-mallar för distribution via PowerShell ska resursgruppens namn anges i PowerShell-skript resursgruppens namn labbet. Mer information finns i [Felsök vanliga fel i Azure-distribution ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Var hittar jag mer information om fel om inte en distribution av Virtuella datorer?
VM-distributionsfel fångas i aktivitetsloggarna. Du kan hitta lab aktivitetsloggar för virtuella datorer via den **granskningsloggar** eller **diagnostik för virtuell dator** på menyn resurs i den testmiljön VM bladet (bladet visas när du väljer den virtuella datorn från **min virtuella datorer** listan).

Ibland inträffar distribution felet innan VM-distributionen startar – till exempel när gränsen för en resurs som skapas med den virtuella datorn har överskridits. I det här fallet felinformationen avbildas på nivån lab **aktivitetsloggar** som kan vara Sök längst ned i den **konfiguration och principer** inställningar. Mer information om hur du använder aktiviteten loggar i Azure, finns [visa aktivitetsloggar granska åtgärder på resurser](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
