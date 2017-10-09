Nu finns stöd för två felsökningsfunktioner i Azure: konsolutdata och skärmbild för Azure Virtual Machines Resource Manager-distributionsmodellen. 

När en egen avbildning tooAzure eller även starta en av hello plattform bilder, kan det finnas många orsaker till varför en virtuell dator hämtar till tillståndet ej startbar. Dessa funktioner aktivera du tooeasily diagnostisera och återställa dina virtuella datorer från startfel.

För Linux virtuella datorer kan du lätt visa hello utdata från konsolen loggen från hello Portal:

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Men för både Windows och Linux virtuella datorer kan Azure också toosee en skärmbild av hello VM från hello hypervisor-programmet:

![Fel](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

De här båda funktionerna finns för Azure Virtual Machines i alla regioner. Obs skärmbilder och utdata kan ta upp too10 minuter tooappear i ditt lagringskonto.

## <a name="common-boot-errors"></a>Vanliga startfel

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Inget operativsystem hittades](https://support.microsoft.com/help/4010142)
- [Startfel eller INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Aktivera diagnostik på en ny virtuell dator
1. När du skapar en ny virtuell dator från hello Preview Portal väljer hello **Azure Resource Manager** hello distribution modellen listrutan:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Konfigurera hello övervakning alternativet tooselect hello storage-konto där du vill ha tooplace filerna diagnostik.
 
    ![Skapa en virtuell dator](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Om du distribuerar en Azure Resource Manager-mall, navigera tooyour virtuella datorresurser och Lägg till hello diagnostik profil avsnitt. Kom ihåg toouse hello ”2015-06-15” API version-huvud.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Hej diagnostikprofilen kan tooselect hello storage-konto där du vill att tooput dessa loggar.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

toodeploy ett exempel på en virtuell dator med startdiagnostikinställningar aktiverad checka ut våra lagringsplatsen här.

## <a name="update-an-existing-virtual-machine"></a>Uppdatera en befintlig virtuell dator ##

tooenable startdiagnostikinställningar via hello Portal, kan du även uppdatera en befintlig virtuell dator via hello Portal. Välj hello Startdiagnostikinställningar alternativet och spara. Starta om hello VM tootake effekt.

![Uppdatera befintlig virtuell dator](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

