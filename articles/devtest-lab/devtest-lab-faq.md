---
title: "aaaAzure DevTest Labs vanliga frågor och svar | Microsoft Docs"
description: "Hitta svar toocommon Azure DevTest Labs frågor"
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
ms.openlocfilehash: 07d4c870eca21856750a472ed503de4a2734a438
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs vanliga frågor och svar
Den här artikeln besvarar några hello de flesta vanliga frågor om Azure DevTest Labs.

**Allmänt**
## <a name="what-if-my-question-isnt-answered-here"></a>Vad gör jag om min fråga inte besvaras här?
Om din fråga inte finns med här, meddela oss så att vi kan hjälpa dig att hitta ett svar.

* Skicka en fråga i hello [Disqus-tråden](#comments) hello slutet av dessa vanliga frågor och interagera med hello Azure Cache-teamet och andra gruppmedlemmar om den här artikeln.
* tooreach en bredare publik ställa en fråga på hello [Azure DevTest Labs MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), och interagera med hello Azure DevTest Labs team och andra medlemmar i hello-communityn.
* toomake en funktionsbegäran skicka din begäran och idéer toohello [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Varför ska jag använda Azure DevTest Labs?
Azure DevTest Labs kan spara ditt team tid och pengar. Utvecklare kan skapa sina egna miljöer med flera olika databaser och använda artefakter tooquickly distribuera och konfigurera program. Använda anpassade avbildningar och formler, kan virtuella datorer sparas som en mall och enkelt återges. Dessutom erbjuder labs flera konfigurerbara principer som gör att lab administratörer tooreduce avfallshantering och hantera en grupps miljöer. Dessa principer omfattar automatisk avstängning, kostnad tröskelvärdet maximala virtuella datorer per användare och maximala storlekar på VM. En mer detaljerad förklaring av Azure DevTest Labs läsa hello [översikt](devtest-lab-overview.md) eller titta på hello [inledande video](/documentation/videos/videos/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Vad ”problemfri, självbetjäning” betyder?
”Problemfri, självbetjäning” innebär att utvecklare och testare skapa sina egna miljöer efter behov och administratörer har hello säkerhet att veta att Azure DevTest Labs hjälper till att minimera avfallshantering och kontrollera kostnaden. Administratörer kan ange vilka VM-storlekar tillåts, hello maximalt antal virtuella datorer, och när virtuella datorer är igång och stänga av. Azure DevTest Labs gör det också enkelt toomonitor kostnader och ange aviseringar toostay tänka på hur resurserna i testmiljön hello används.

## <a name="how-can-i-use-azure-devtest-labs"></a>Hur kan jag använda Azure DevTest Labs?
Azure DevTest Labs är användbar när du kräver dev eller testmiljöer, och du vill tooreproduce dem snabbt och/eller hantera dem med kostnadsbesparingar principer.

Här följer några scenarier som våra kunder använder Azure DevTest Labs för:

* Hanterar utvecklings- och testmiljöer på en plats kan bygger använda principer för tooreduce kostnad och anpassade avbildningar tooshare över hello team.
* Utveckla ett program med hjälp av anpassade avbildningar toosave hello diskstatusen i hela hello development steg.
* För att spåra hello kostnaden i relationen tooprogress.
* Skapa drivrutinslista testmiljöer för quality assurance testning.
* Med artefakter och formler tooeasily konfigurera och återskapa ett program i olika miljöer.
* Distribuera virtuella datorer för hackathons (utvecklings- eller samarbete) och sedan enkelt avetablering dem när hello händelsen slutar.

## <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hur är debiteras för Azure DevTest Labs?
Azure DevTest Labs är en kostnadsfri, vilket innebär att skapa labs och konfigurera hello principer, mallar och artefakter är ledig. Du betalar bara för hello Azure-resurser används i din labs, till exempel virtuella datorer, lagringskonton och virtuella nätverk. Mer information om hello kostnaden lab resurser Läs mer om [priser för Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Säkerhet**
## <a name="what-are-hello-different-security-levels-in-azure-devtest-labs"></a>Vad är hello olika säkerhetsnivåer i Azure DevTest Labs?
Säkerhetsåtkomst bestäms av [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-built-in-roles.md). toounderstand hur åt fungerar, hjälper det toounderstand hello skillnaderna mellan en behörighet, en roll och ett omfång som definieras av RBAC.

* **Behörighet** -behörigheter är en definierad åtkomst tooa åtgärd. En behörighet kan till exempel vara läsbehörighet tooall virtuella datorer.
* **Rollen** -en roll är en uppsättning behörigheter som kan grupperas och tilldelad tooa användare. ”Prenumerationsägaren” har till exempel komma åt tooall resurser inom en prenumeration.
* **Scope** -scope är en nivå i hello hierarkin för Azure-resurs. Ett omfång kan exempelvis vara en resursgrupp eller en enda lab eller hello hela prenumerationen.

Inom hello omfång Azure DevTest Labs, det finns två typer av roller toodefine behörigheten: labbägaren och lab-användare.

* **Labbägaren** -en labbägaren har hello åtkomst tooany resurser i hello testmiljön. Därför kan de ändra principer, läsa och skriva alla virtuella datorer, ändra hello virtuella nätverk och så vidare.
* **Lab-användare** - lab-användare kan visa alla lab resurser, till exempel virtuella datorer, principer och virtuella nätverk, men de kan inte ändra principer eller alla virtuella datorer som skapats av andra användare. Det är också möjligt toocreate anpassade roller i Azure DevTest Labs och du kan lära dig hur toodo i hello artikeln [bevilja användarbehörighet toospecific principer för labbet](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Eftersom scope är hierarkiska, när en användare har behörighet för ett visst område kan beviljas de automatiskt dessa behörighet för definitionsområdet var lägre nivå finns. Till exempel om en användare har tilldelats rollen som toohello prenumerationsägaren, har sedan de åtkomst tooall resurser i en prenumeration. Dessa resurser inkluderar alla virtuella datorer, alla virtuella nätverk och alla övningar. Därför ärver en prenumerationsägaren automatiskt labbägaren hello roll. Dock gäller inte hello motsatt. En labbägaren har åtkomst tooa lab, vilket är en omfattning som är lägre än hello prenumerationsnivån. En labbägaren är därför inte kan toosee virtuella datorer eller virtuella nätverk eller resurser som ligger utanför hello labbet.

## <a name="how-do-i-create-a-role-tooallow-users-tooperform-a-specific-task"></a>Hur skapar jag en roll tooallow användare tooperform en viss uppgift?
En omfattande artikel om hur toocreate anpassade roller och tilldela behörigheter toothat roll finns här. Här är ett exempel på ett skript som skapar hello ”DevTest Labs avancerade användare”, som har behörigheten toostart och stoppa alla virtuella datorer i labbet hello:

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
Om du använder VSTS är en [Azure DevTest Labs uppgifter tillägget](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) som du kan tooautomate utgivningscykeln pipeline i Azure DevTest Labs. Några av hello använder det här tillägget är:

* Skapa och distribuera en virtuell dator automatiskt och konfigurera den med hello senaste versionen med Azure File Copy eller PowerShell VSTS uppgifter.
* Fånga automatiskt hello tillståndet för en virtuell dator när du testar tooreproduce ett programfel i hello samma virtuella dator för ytterligare undersökning.
* Om du tar bort hello VM hello slutet av hello släpp pipeline när den inte längre behövs.

hello ge följande blogginlägg vägledning och information om hur du använder hello VSTS tillägg:

* [Azure DevTest Labs – VSTS tillägg](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Distribuera en ny virtuell dator i en befintlig AzureDevTestLab från VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Med versionshantering i VSTS för kontinuerlig distribution tooAzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

För andra CI/CD toolchains alla hello tidigare nämnts scenarier som kan ske via hello VSTS uppgifter tillägg på samma sätt kan uppnås genom att distribuera [Azure Resource Manager-mallar](https://aka.ms/dtlquickstarttemplate) med [ Azure PowerShell-cmdlets](../azure-resource-manager/resource-group-template-deploy.md) och [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Du kan också använda [REST API: er för DevTest Labs](http://aka.ms/dtlrestapis) toointegrate med din toolchain.  


**Virtual Machines**
## <a name="why-cant-i-see-certain-vms-in-hello-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Varför kan jag inte se vissa virtuella datorer i hello Azure Virtual Machines bladet som visas i Azure DevTest Labs?
När en virtuell dator skapas i Azure DevTest Labs behörigheten är angivna tooaccess den virtuella datorn. Du är kan tooview den både i hello labs bladet och hello **virtuella datorer** bladet. Användare med hello DevTest Labs rollen kan se alla virtuella datorer som skapats i hello labbet via hello lab **alla virtuella datorer** bladet. Dock beviljas användare i hello DevTest Labs roll inte automatiskt läsbehörighet tooVM resurser som andra har skapat. Därför visas inte dessa virtuella datorer i hello **virtuella datorer** bladet.

## <a name="what-is-hello-difference-between-custom-images-and-formulas"></a>Vad är hello skillnaden mellan anpassade avbildningar och formler?
En anpassad avbildning är en virtuell Hårddisk (virtuell hårddisk), medan en formel är en avbildning som du kan konfigurera ytterligare inställningar som du kan spara och återskapa. En anpassad avbildning kan det vara bättre om du vill skapa tooquickly flera miljöer med hello samma grundläggande, ändras avbildning. En formel kan vara bättre om du vill tooreproduce hello konfigurationen av den virtuella datorn med hello senaste bits, ett virtuellt undernät eller en viss storlek. En mer ingående förklaring finns i hello artikeln [jämföra anpassade avbildningar och formler i DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-hello-same-template-at-once"></a>Hur skapar jag flera virtuella datorer från hello samma mall på samma gång?
Du kan använda hello [VSTS uppgifter tillägget](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) eller [Generera en Azure Resource Manager-mall](devtest-lab-add-vm.md#save-azure-resource-manager-template) när du skapar en virtuell dator och [distribuera hello Azure Resource Manager-mall från Windows PowerShell ](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Hur flytta mina befintliga virtuella Azure-datorer i min Azure DevTest Labs lab?
Följ hello stegen nedan toocopy din befintliga virtuella datorer tooAzure DevTest Labs:

1. Kopiera hello VHD-filen på den befintliga virtuella datorn med den här [Windows PowerShell-skript](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Skapa hello anpassad avbildning](devtest-lab-create-template.md) i Azure DevTest Labs-labb.
3. Skapa en virtuell dator i labbet hello från den anpassade avbildningen

## <a name="can-i-attach-multiple-disks-toomy-vms"></a>Kan jag ansluta flera diskar toomy virtuella datorer?
Bifoga flera diskar tooVMs stöds.  

## <a name="if-i-want-toouse-a-windows-os-image-for-my-testing-do-i-have-toopurchase-an-msdn-subscription"></a>Om jag vill toouse en operativsystemsavbildning för Windows för min testning finns toopurchase en MSDN-prenumeration?
Om du behöver toouse Windows klient-OS-avbildningar (Windows 7 eller senare) för din utveckling och testning i Azure Ja, måste du antingen:

- [Köp en prenumeration på MSDN](https://www.visualstudio.com/products/how-to-buy-vs).
- Om du har ett Enterprise-avtal kan du skapa en Azure-prenumeration med hello [utveckling och testning Enterprise erbjudande](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

Mer information om hello Azure-krediter för varje MSDN-erbjudande finns [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-hello-process-of-uploading-vhd-files-toocreate-custom-images"></a>Hur jag för att automatisera hello processen att överföra VHD-filer toocreate anpassade avbildningar?
Det finns två alternativ:

* [Azure AzCopy](../storage/common/storage-use-azcopy.md#blob-upload) kan använda toocopy eller överföra VHD-filer toohello storage-konto som är associerade med hello labbet.
* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som körs på Windows, OSX och Linux.   

toofind hello destinationslagringskontot som är associerade med ditt labb så här:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **resursgrupper** hello vänstra panelen.
3. Leta upp och välj hello resursgrupp som är associerade med ditt labb.
4. På hello **översikt** bladet Välj en av hello storage-konton.
5. Välj **Blobbar**.
6. Leta efter överföringar i hello-listan. Om inget finns returnera tooStep #4 och försök med ett annat lagringskonto.
7. Använd hello **URL** som mål i AzCopy-kommandot.

## <a name="how-can-i-automate-hello-process-of-deleting-all-hello-vms-in-my-lab"></a>Hur kan jag för att automatisera hello raderingen av alla hello virtuella datorer i min labbet?
Dessutom toodeleting virtuella datorer från ditt labb i hello Azure-portalen, du kan ta bort alla hello virtuella datorer i ditt labb använder ett PowerShell-skript. Följande exempel ändra hello parametervärden under hello i hello **värden toochange** kommentar. Du kan hämta hello `subscriptionId`, `labResourceGroup`, och `labName` värden från hello blad för labbet i hello Azure-portalen.

    # Delete all hello VMs in a lab

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login tooyour Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get hello lab that contains hello VMs toodelete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get hello VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete hello VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Artefakter**
## <a name="what-are-artifacts"></a>Vad är artefakter?
Artefakter är anpassningsbara element som kan använda toodeploy din senaste bitar eller din dev verktygen på en virtuell dator. De är anslutna tooyour VM under genereringen av med ett par enkla klick och när hello VM etableras hello artefakter distribuera och konfigurera den virtuella datorn. Det finns olika befintliga artefakter i vår [offentliga GitHub-lagringsplatsen](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), men du kan också enkelt [redigera egna artefakter](devtest-lab-artifact-author.md).


**Konfiguration av testlabb**
## <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hur skapar jag en lab från en Azure Resource Manager-mall
Vi har angett en [GitHub-lagringsplatsen för labbet Azure Resource Manager-mallar](https://aka.ms/dtlquickstarttemplate) som du kan distribuera som-är eller ändra toocreate anpassade mallar för din testlabb. Var och en av dessa mallar innehåller en länk som du kan klicka på toodeploy hello labb som-är under din egen Azure-prenumeration, eller så kan du anpassa mallen hello och [distribueras med hjälp av PowerShell eller Azure CLI](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Varför skapas Mina virtuella datorer i olika resursgrupper med godtyckliga namn? Kan jag byta namn på eller ändra de här resursgrupperna?
Det här sättet skapas resursgrupper för Azure DevTest Labs toomanage hello användarens behörighet och åtkomst toovirtual datorer. Medan du kan flytta hello VM tooanother resursgrupp med namnet på din önskade, rekommenderas göra så inte. Vi arbetar på att förbättra den här upplevelsen tooallow mer flexibilitet.   

## <a name="how-many-labs-can-i-create-under-hello-same-subscription"></a>Hur många labs kan jag skapa under hello samma prenumeration?
Det finns ingen specifik begränsning på hello övningar som kan skapas per prenumeration. Hello-resurser som används är dock begränsade per prenumeration. Du kan läsa om hello [gränser och kvoter som införts på Azure-prenumerationer](../azure-subscription-service-limits.md) och [hur tooincrease dessa gränser](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Hur många virtuella datorer kan skapa per labb?
Det finns ingen specifik gräns på hello antal virtuella datorer som kan skapas per labb. Hello-resurser som används är dock begränsad per prenumeration (t.ex. Virtuella kärnor, offentliga IP-adresser, etc.). Du kan läsa om hello [gränser och kvoter som införts på Azure-prenumerationer](../azure-subscription-service-limits.md) och [hur tooincrease dessa gränser](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-toomy-lab"></a>Hur gör jag för att dela en direktlänk toomy lab?
tooshare en direktlänk tooyour lab-användare, som du kan utföra hello nedan:

1. Bläddra toohello labb i hello Azure-portalen.
2. Kopiera hello lab URL i webbläsaren och dela den med lab-användare.

> [!NOTE]
> Om användarna labbet är externa användare med en [Microsoft-konto](#what-is-a-microsoft-account) och de inte tillhör tooyour företagets Active directory, de kan få ett felmeddelande när du navigerar toohello som länk. Om de får ett felmeddelande, instruerar du dem tooclick namnet i hello övre högra hörnet av hello Azure-portalen och väljer hello katalog där hello lab finns från hello **Directory** hello-menyn.
>
>

## <a name="what-is-a-microsoft-account"></a>Vad är ett Microsoft-konto?
Ett Microsoft-konto är det du använder för nästan allt du gör med Microsoft-enheter och tjänster. Det är ett e-postadress och lösenord som du använder toosign i tooSkype, Outlook.com, OneDrive, Windows Phone och Xbox LIVE – och det innebär att dina filer, foton, kontakter och inställningar kan följa tooany enhet.

> [!NOTE]
> Microsoft-konto används toobe som kallas ”Windows Live ID”.
>
>


**Felsökning**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Min artefakt misslyckades under skapande av virtuell dator. Hur felsöker jag det?
Se för[hur toodiagnose artefakt fel i DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md) toolearn hur tooobtain loggar om din misslyckade artefakt.

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Varför är min befintliga virtuella nätverk sparas korrekt?
Ett sätt är att din virtuella nätverksnamnet innehåller punkter. Om så försök att ta bort hello punkter eller ersätta dem med bindestreck, och sedan försöker hello Spara virtuella nätverk igen.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Varför visas ett ”överordnade resursen hittades inte” fel vid etablering av en virtuell dator från PowerShell?
När en resurs är en överordnad tooanother resurs, måste hello överordnade resursen finnas innan du skapar hello underordnade resursen. Om det inte finns, visas en **ParentResourceNotFound** fel. Om du inte anger ett beroende på hello överordnade resursen kan hello underordnade resursen distribueras innan hello överordnade.

Virtuella datorer som är underordnade resurser under ett labb i en resursgrupp. När du använder Azure Resource Manager mallar toodeploy via PowerShell ska hello resursgruppens namn anges i hello PowerShell-skript hello resursgruppens namn av hello labbet. Mer information finns i [Felsök vanliga fel i Azure-distribution ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Var hittar jag mer information om fel om inte en distribution av Virtuella datorer?
VM-distributionsfel fångas i hello aktivitetsloggar. Du kan hitta lab VMs aktivitetsloggar via hello **granskningsloggar** eller **diagnostik för virtuell dator** på hello resurs-menyn i hello lab VM bladet (hello bladet visas när du väljer hello VM från **Mina virtuella datorer** listan).

Ibland inträffar hello distributionsfel innan hello distribution av Virtuella datorer startar – till exempel när hello prenumeration för en resurs som skapats med hello VM överskreds. I det här fallet hello felinformation fångas i hello lab nivå **aktivitetsloggar** som kan vara Sök längst hello hello **konfiguration och principer** inställningar. Mer information om hur du använder aktiviteten loggar i Azure, finns [visa aktiviteten loggar tooaudit åtgärder på resurser](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
