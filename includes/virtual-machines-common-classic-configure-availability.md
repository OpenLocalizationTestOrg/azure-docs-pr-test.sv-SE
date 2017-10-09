


En tillgänglighetsuppsättning hjälper dig att hålla dina virtuella datorer som är tillgänglig under driftstopp, som under underhåll. Placera två eller flera liknande konfigurerad virtuella datorer i en tillgänglighetsuppsättning skapar hello redundans behövs toomaintain tillgängligheten för hello program eller tjänster som den virtuella datorn körs. Mer information om hur det fungerar finns [hantera hello tillgängligheten för virtuella datorer][Manage hello availability of virtual machines].

Det är en bästa praxis toouse både tillgänglighetsuppsättningar och belastningsutjämning slutpunkter toohelp Kontrollera att ditt program är alltid tillgänglig och fungerar effektivt. Mer information om belastningsutjämnade slutpunkter finns [belastningsutjämning för Azure infrastrukturtjänster][Load balancing for Azure infrastructure services].

Du kan lägga till den klassiska virtuella datorer i en tillgänglighetsuppsättning med hjälp av ett av två alternativ:

* [Alternativ 1: Skapa en virtuell dator och en tillgänglighetsuppsättning på hello samma tid][Option 1: Create a virtual machine and an availability set at hello same time]. Lägg sedan till den nya virtuella datorer toohello när du skapar de virtuella datorerna.
* [Alternativ 2: Lägg till en befintlig virtuell dator tooan tillgänglighetsuppsättning][Option 2: Add an existing virtual machine tooan availability set].

> [!NOTE]
> I klassiska modellen hello hello virtuella datorer som du vill använda tooput i samma tillgänglighetsuppsättning måste tillhöra toohello samma molntjänst.
> 
> 

## <a id="createset"></a>Alternativ 1: skapa en virtuell dator och en tillgänglighetsuppsättning på hello samma tid
Du kan använda antingen hello Azure-portalen eller Azure PowerShell kommandon toodo.

toouse hello Azure-portalen:

1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **+ ny**, och klicka sedan på **virtuella**.
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Välj hello Marketplace avbildning av virtuell dator du vill toouse. Du kan välja toocreate en Linux- eller Windows virtuell dator.
4. För hello valda virtuella datorn, kontrollera att hello distributionsmodell anges för**klassiska** och klicka sedan på **skapa**
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Ange ett virtuellt datornamn, användarnamn och lösenord (för Windows-datorer) eller offentlig SSH-nyckel (för Linux-datorer). 
6. Välj hello VM-storlek och klicka sedan på **Välj** toocontinue.
7. Välj **valfri konfiguration > tillgänglighetsuppsättning**, och välj hello tillgänglighetsuppsättning gärna tooadd hello virtuell dator.
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Granska inställningarna. När du är klar klickar du på **skapa**.
9. Medan Azure skapar den virtuella datorn, du kan följa förloppet för hello under **virtuella datorer** i hello hubbmenyn.

toouse Azure PowerShell-kommandon toocreate en virtuell Azure-dator och lägga till den tooa ny eller befintlig tillgänglighetsuppsättning, se [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"></a>Alternativ 2: Lägg till en befintlig virtuell dator tooan tillgänglighetsuppsättning
I hello Azure-portalen, kan du lägga till befintliga klassiska virtuella datorer tooan befintliga tillgänglighetsuppsättning eller skapa en ny för dem. (Kom ihåg att hello hello virtuella datorer i samma tillgänglighetsuppsättning måste tillhöra toohello samma molntjänst.) hello stegen är nästan hello samma. Du kan använda Azure PowerShell för att lägga till hello virtuella tooan befintlig tillgänglighetsuppsättning.

1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **virtuella datorer (klassisk)**.
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Hello lista över virtuella datorer, Välj hello namnet på hello virtuella dator som du vill tooadd toohello set.
4. Välj **tillgänglighetsuppsättning** från hello virtuella **inställningar**.
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Välj hello tillgänglighetsuppsättning gärna tooadd hello virtuell dator. hello virtuell dator måste tillhöra toohello samma molntjänst som hello tillgänglighetsuppsättning.
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klicka på **Spara**.

toouse Azure PowerShell-kommandon, öppna en administratörsnivå Azure PowerShell-session och kör följande kommando hello. Hello platshållare (exempelvis &lt;VmCloudServiceName&gt;), Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> hello virtuella datorn kanske startas om toobe toofinish lägger till den toohello tillgänglighetsuppsättning.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at hello same time]: #createset
[Option 2: Add an existing virtual machine tooan availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage hello availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

