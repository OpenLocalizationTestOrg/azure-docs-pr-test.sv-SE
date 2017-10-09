Om den virtuella datorn (VM) i Azure påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv. Ett vanligt exempel är en uppdatering av program med fel som förhindrar att hello VM Start har. Den här artikeln beskriver hur toouse Azure portal tooconnect din virtuella hårddisk tooanother VM toofix eventuella fel och skapa den ursprungliga virtuella datorn igen.

## <a name="recovery-process-overview"></a>Översikt över återställningsprocessen
hello felsökningsprocessen är följande:

1. Ta bort hello virtuell dator som det har uppstått problem, men behålla hello virtuella hårddiskar.
2. Anslut och montera hello virtuell hårddisk tooanother VM för felsökning.
3. Ansluta toohello felsökning VM. Redigera filer eller köra verktyg toofix fel på hello ursprungliga virtuella hårddisken.
4. Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.
5. Skapa en virtuell dator med hjälp av hello ursprungliga virtuella hårddisken.

## <a name="delete-hello-original-vm"></a>Ta bort hello ursprungliga VM
Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure. En virtuell hårddisk är där hello operativsystem, program och konfigurationer lagras. hello VM är bara metadata som definierar hello storlek eller plats och som refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC). Varje virtuell hårddisk hämtar ett lån som tilldelas när disken är anslutna tooa VM. Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort. hello lån fortsätter tooassociate hello OS disk tooa VM även om den virtuella datorn är i tillståndet stoppad och frigjord.

Hej första steg toorecovering den virtuella datorn är toodelete hello Virtuella datorresursen sig själv. Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto. Efter hello VM tas bort, kan du bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com). 
2. På menyn hello hello vänster **virtuella datorer (klassisk)**.
3. Klicka på Välj hello virtuell dator som har problem med hello, **diskar**, och sedan identifiera hello namn hello virtuell hårddisk. 
4. Välj hello OS virtuell hårddisk och kontrollera hello **plats** tooidentify hello storage-konto som innehåller den virtuella hårddisken. I följande exempel hello, hello sträng omedelbart före ”. blob.core.windows.net” är hello lagringskontonamn.

    ```
    https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd
    ```

    ![hello bilden om Virtuella datorns plats](./media/virtual-machines-classic-recovery-disks-portal/vm-location.png)

5. Högerklicka på hello VM och välj sedan **ta bort**. Kontrollera att hello diskarna inte är markerade när du tar bort hello VM.
6. Skapa en ny virtuell återställningsdator. Den här virtuella datorn måste vara i hello samma region och resurs grupp (Molntjänsten) som hello problemet VM.
7. Välj hello recovery VM och välj sedan **diskar** > **koppla befintliga**.
8. tooselect befintliga virtuella hårddisken, klickar du på **VHD-filen**:

    ![Bläddra till den befintliga virtuella hårddisken](./media/virtual-machines-classic-recovery-disks-portal/select-vhd-location.png)

9. Välj lagringskonto för hello > VHD-behållare > Hej virtuell hårddisk, klicka på hello **Välj** knappen tooconfirm valet.

    ![Välj din befintliga virtuella hårddisk](./media/virtual-machines-classic-recovery-disks-portal/select-vhd.png)

10. Med den virtuella Hårddisken nu markerat, Välj **OK** tooattach hello befintlig virtuell hårddisk.
11. Efter några sekunder hello **diskar** fönstret för den virtuella datorn visas en befintlig virtuell hårddisk ansluten som en datadisk:

    ![Befintlig virtuell hårddisk ansluten som en datadisk](./media/virtual-machines-classic-recovery-disks-portal/attached-disk.png)

## <a name="fix-issues-on-hello-original-virtual-hard-disk"></a>Åtgärda problem på hello ursprungliga virtuell hårddisk
Du kan nu utföra eventuella underhåll och felsökning efter behov när hello befintlig virtuell hårddisk är monterad. När du har åtgärdat hello problem, fortsätter du med hello följande steg.

## <a name="unmount-and-detach-hello-original-virtual-hard-disk"></a>Demontera och koppla från hello ursprungliga virtuella hårddisken
När eventuella fel har åtgärdats, demontera och koppla från hello befintlig virtuell hårddisk från den virtuella datorn med felsökning. Du kan inte använda din virtuella hårddisk tillsammans med andra Virtuella förrän hello lån som kopplar hello virtuell hårddisk toohello felsökning VM släpps.  

1. Logga in toohello [Azure-portalen](https://portal.azure.com). 
2. Välj på menyn hello hello vänster **virtuella datorer (klassisk)**.
3. Leta upp hello recovery VM. Välj diskar, högerklicka på hello disk, och välj sedan **Detach**.

## <a name="create-a-vm-from-hello-original-hard-disk"></a>Skapa en virtuell dator från hello ursprungliga hårddisken

toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [klassiska Azure-portalen](https://manage.windowsazure.com).

1. Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com).
2. Längst ned hello hello portal, Välj **ny** > **Compute** > **virtuella** > **från galleriet** .
3. I hello **Välj en bild** väljer **min diskar**, och sedan väljer hello ursprungliga virtuella hårddisken. Kontrollera hello platsinformation. Detta är hello region där hello VM måste distribueras. Välj hello nästa.
4. I hello **konfiguration av virtuell dator** avsnittet skriver hello VM namnet och välja en storlek för hello VM.
