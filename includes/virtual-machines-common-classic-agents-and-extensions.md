

Med hjälp av VM-tillägg kan du göra följande:

* Ändra säkerhets- och identitetsfunktioner, till exempel återställa kontovärden och använda program mot skadlig kod
* Starta, stoppa och konfigurera övervakning och diagnostik
* Återställa/installera anslutningsfunktioner, till exempel RDP och SSH
* Diagnostisera, övervaka och hantera virtuella datorer

Det finns många andra funktioner också. Nya funktioner för virtuella datortillägg släpps regelbundet. Den här artikeln beskriver hello Azure VM agenter för Windows- och Linux- och hur de stöder VM-tillägget funktioner. En lista över virtuella datortillägg efter funktionskategori finns i [Azure VM-tillägg och -funktioner](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="azure-vm-agents-for-windows-and-linux"></a>Azure VM-agenter för Windows- och Linux
hello Azure-agenten för virtuella datorer (VM-Agent) är en säker, inaktiverad process som installerar, konfigurerar och tar bort VM-tillägg på instanser av virtuella datorer i Azure. hello VM-agenten fungerar som hello säker lokal kontroll tjänst för Azure-VM. hello-tillägg som hello agent belastningar ange specifika funktioner tooincrease produktiviteten använder hello-instans.

Två Azure VM-agenter finns, en för virtuella datorer i Windows och en för virtuella Linux-datorer.

Om du vill en virtuell dator instans toouse en eller flera VM-tillägg, måste hello instans ha en installerad VM-Agent. En avbildning av virtuell dator som skapats med hjälp av hello Azure-portalen och en bild från hello **Marketplace** en VM-Agent installeras automatiskt i hello-processen. Om en virtuell datorinstans saknar en VM-Agent, kan du installera hello VM-agenten när hello virtuella instans skapas. Eller så kan du installera hello-agent på en anpassad VM-avbildning som du sedan överför.

> [!IMPORTANT]
> Dessa VM-agenter är mycket lätta tjänster som möjliggör säker administration av virtuella datorinstanser. Det kan finnas fall där du inte vill hello VM-agenten. I så fall vara säker på att toocreate VMs som inte har hello VM-agenten är installerad med hello Azure CLI eller PowerShell. Även om hello VM-agenten kan tas bort fysiskt har hello beteendet för VM-tillägg på hello-instansen inte definierats. Därför finns det inte stöd för att ta bort en installerad VM-agent.
>

hello VM-agenten är aktiverat i hello följande situationer:

* När du skapar en instans av en virtuell dator med hjälp av hello Azure-portalen och välja en bild från hello **Marketplace**,
* När du skapar en instans av en virtuell dator med hjälp av hello [ny AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) eller hello [ny AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx) cmdlet. Du kan skapa en virtuell dator utan en VM-agenten genom att lägga till hello **– DisableGuestAgent** parametern toohello [Lägg till AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) cmdlet,

* När du manuellt ladda ned och installera hello VM-agenten på en befintlig VM-instans och ange hello **ProvisionGuestAgent** värdet för**SANT**. Du kan använda denna teknik för Windows- och Linux-agenter med hjälp av ett PowerShell-kommando eller ett REST-anrop. (Om du inte anger hello **ProvisionGuestAgent** värdet när du har installerat hello hello tillägg av hello VM-agenten inte identifieras korrekt på VM-agenten manuellt.) hello följande exempel visas hur toodo detta med hjälp av PowerShell där hello `$svc` och `$name` argument har redan visats:

      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

* När du skapar en VM-avbildning som innehåller en installerad VM-agent. Du kan ladda upp det bild tooAzure när hello avbildningen med hello VM-agenten finns. En virtuell Windows-dator, kan du hämta hello [Windows VM-agenten MSI-filen](http://go.microsoft.com/fwlink/?LinkID=394789) och installera hello VM-agenten. För en Linux-VM installera hello VM-agenten från hello GitHub-lagringsplatsen finns på <https://github.com/Azure/WALinuxAgent>. Mer information om hur tooinstall hello VM-agenten på Linux finns hello [användarhandboken för Azure Linux VM-agenten](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> I PaaS, hello VM Agent kallas **WindowsAzureGuestAgent**, och alltid är tillgänglig på webben och Worker-rollen virtuella datorer. (Mer information finns i [Azure rollen arkitektur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).) hello VM-agenten för rollen virtuella datorer kan nu lägga till tillägg toohello Molntjänsten virtuella datorer i hello samma sätt som för permanenta virtuella datorer. hello största skillnaden mellan VM-tillägg för roll för virtuella datorer och permanenta virtuella datorer är om hello VM-tillägg som läggs till. Med rollen VMs läggs tillägg första toohello molnbaserad tjänst bör sedan toohello distributioner i Molntjänsten.
>
> Använd den [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet toolist alla tillgängliga roller VM-tillägg.
>
>

## <a name="find-add-update-and-remove-vm-extensions"></a>Hitta, lägga till, uppdatera och ta bort tillägg för virtuell dator
Mer information om dessa uppgifter finns i [Lägg till, hitta, uppdatera och ta bort Azure VM-tillägg](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
