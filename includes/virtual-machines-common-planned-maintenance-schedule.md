

## <a name="multi-and-single-instance-vms"></a>Flera och instans virtuella datorer
Många kunder som körs på Azure antal kritiska kan schemalägga om deras virtuella datorer genomgår planerat underhåll på grund av toohello driftstopp--cirka 15 minuter--som uppstår under underhåll. Du kan använda tillgänglighet anger toohelp kontroll när etablerade virtuella datorer får planerat underhåll.

Det finns två möjliga konfigurationer för virtuella datorer som körs på Azure. Virtuella datorer är konfigurerade som flerinstans- eller eninstanskategori. Om virtuella datorer i en tillgänglighetsuppsättning, är de konfigurerade som flera instanser. Observera, även enstaka virtuella datorer kan distribueras i en tillgänglighetsuppsättning, så att de behandlas som flera instanser. Om virtuella datorer som inte är i en tillgänglighetsuppsättning är de konfigurerade som eninstanskategori.  Mer information om tillgänglighetsuppsättningar finns [hantera hello tillgänglighet för din virtuella Windows-datorer](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [hantera hello tillgänglighet för din virtuella Linux-datorer](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Planerat underhåll uppdaterar toosingle-instans och virtuella datorer med flera instanser sker separat. Du kan styra när deras virtuella datorer får hello planerat underhåll genom att konfigurera om din virtuella datorer toobe single instance (om de finns flera instanser) eller toobe flera instanser (om de är single instance). Se [planerat underhåll för virtuella Azure Linux-datorer](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [planerat underhåll för Windows Azure virtuella datorer](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) information om planerat underhåll för virtuella Azure-datorer.

## <a name="for-multi-instance-configuration"></a>För konfiguration av flera instanser
Du kan välja hello tid planerat underhåll påverkar dina virtuella datorer som distribueras i en Tillgänglighetsuppsättning konfiguration genom att ta bort dessa virtuella datorer från tillgänglighetsuppsättningar.

1. Ett e-postmeddelande skickas tooyou sju kalenderdagar innan hello planerat underhåll tooyour virtuella datorer i en konfiguration med flera instanser. hello prenumerations-ID: N och namnen på virtuella datorer med flera instanser av hello påverkas inkluderas i hello brödtext hello e-post.
2. Under dessa sju dagar du hello tid dina instanser uppdateras genom att ta bort dina virtuella datorer med flera instanser i den regionen från sina tillgänglighetsuppsättning. Den här ändringen i konfigurationen orsakar en omstart, som hello virtuella datorn flyttas från en fysisk värd, avsedda för underhåll, tooanother fysiska värden som inte är avsedda för underhåll.
3. Du kan ta bort hello VM från dess tillgänglighetsuppsättning i hello Azure-portalen.

   1. Välj hello VM tooremove hello Tillgänglighetsuppsättning i hello-portalen.  

   2. Under **inställningar**, klickar du på **tillgänglighetsuppsättning**.

      ![Val av Tillgänglighetsuppsättning](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. Ange listrutan i hello tillgänglighet, markerar ”inte en del av en tillgänglighetsuppsättning”.

      ![Ta bort från](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. Hello överkant, klickar du på **spara**. Klicka på **Ja** tooacknowledge som den här åtgärden startar om hello VM.

   >[!TIP]
   >Du kan konfigurera hello VM toomulti-instans senare genom att välja ett hello visas tillgänglighetsuppsättningar.

4. Virtuella datorer tas bort från tillgänglighetsuppsättningar är värdar för flyttade tooSingle-instans och uppdateras inte under hello planerat underhåll tooAvailability ange konfigurationer.
5. När hello uppdatering tooAvailability ange VMs är klar (enligt tooschedule som beskrivs i hello ursprungliga e-postmeddelandet) du bör lägga till hello virtuella datorer till deras tillgänglighetsuppsättningar. Bli en del av en tillgänglighetsuppsättning konfigurerar hello virtuella datorer som flera instanser och resulterar i en omstart. Vanligtvis när alla uppdateringar i flera instanser har slutförts över hello hela Azure-miljön, single instance Underhåll visas nedan.

Om du tar bort en virtuell dator från en tillgänglighetsuppsättning kan även uppnås med hjälp av Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>För Single-instance-konfiguration
Du kan välja hello tid planerat underhåll påverkar du virtuella datorer i en enda instans konfiguration genom att lägga till dessa virtuella datorer i tillgänglighetsuppsättningar.

Steg för steg

1. Ett e-postmeddelande skickas tooyou sju kalenderdagar innan hello planerat underhåll tooVMs i en enda instans-konfiguration. hello prenumerations-ID: N och namnen på hello påverkas Single-Instance virtuella datorer ingår i hello brödtext hello e-post.
2. Under dessa sju dagar du hello tid din instans omstarter genom att lägga till din Single-instance VMs tooan tillgänglighet som i den samma regionen. Den här ändringen i konfigurationen orsakar en omstart, som hello virtuella datorn flyttas från en fysisk värd, avsedda för underhåll, tooanother fysiska värden som inte är avsedda för underhåll.
3. Följ anvisningarna här tooadd befintliga virtuella datorer i tillgänglighetsuppsättningar med hello Azure-portalen och Azure PowerShell. (Se hello Azure PowerShell-exempel som följer de här stegen.)
4. När dessa virtuella datorer konfigureras som flera instanser, är de undantagna från hello planerat underhåll tooSingle instans virtuella datorer.
5. När hello single instance VM-uppdateringen är klar kan (enligt tooschedule i hello ursprungliga e-postmeddelandet), du returnera hello VMs toosingle-instans genom att ta bort hello virtuella datorer från sina tillgänglighetsuppsättningar.

Lägga till en VM-tooan kan tillgänglighetsuppsättning också uppnås med hjälp av Azure PowerShell:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
