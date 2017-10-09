


Det här avsnittet beskrivs hur du:

* Mata in data i en Azure virtuell dator (VM) när den har etablerats.
* Hämta den för både Windows och Linux.
* Använd särskilda verktyg som finns på vissa system toodetect och hantera anpassade data automatiskt.

> [!NOTE]
> Den här artikeln beskriver hur anpassade data kan matas in med hjälp av en virtuell dator som skapats med hello Azure Service Management API. hur toouse hello Azure Resource Management API Se toosee [hello exempelmall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Placera anpassade data i ditt virtuella Azure-datorn
Den här funktionen stöds bara i hello [Azure-kommandoradsgränssnittet](https://github.com/Azure/azure-xplat-cli). Här kan vi skapa en `custom-data.txt` -fil som innehåller våra data sedan mata in som i toohello VM under etableringen. Även om du kan använda någon av hello alternativ för hello `azure vm create` kommandot hello följande visar ett grundläggande tillvägagångssätt:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-hello-virtual-machine"></a>Med hjälp av anpassade data i hello virtuell dator
* Om din Azure VM är en Windows-baserad virtuell dator och sedan hello anpassade data sparas för`%SYSTEMDRIVE%\AzureData\CustomData.bin`. Även om det var base64-kodad tootransfer från hello lokal dator toohello ny virtuell dator, är det automatiskt avkodas och som kan öppnas eller används omedelbart.
  
  > [!NOTE]
  > Om hello finns skrivs över. hello på hello directory säkerhetsnivån för**System: fullständig kontroll** och **administratörer: fullständig kontroll**.
  > 
  > 
* Om din Azure VM är en Linux-baserade Virtuella hello anpassade datafilen ska finnas i placerar något av följande hello beroende på din distro. hello data kan vara base64-kodade, så du kanske måste toodecode hello data först:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Molnet initiering på Azure
Om din Azure VM är från en Ubuntu eller virtuell CoreOS-avbildning, kan du använda CustomData toosend en moln-config toocloud initiering. Eller om anpassade datafilen är ett skript, sedan molnet init kan bara köra den.

### <a name="ubuntu-cloud-images"></a>Ubuntu molnet bilder
I de flesta Azure Linux-avbildningar, skulle du redigera ”/ etc/waagent.conf” tooconfigure hello tillfälliga disk och växla resursfil. Se [Azure Linux-agenten användarhandboken](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för mer information.

Dock på hello Ubuntu molnet bilder, måste du använda molnet init tooconfigure hello resurs disk (det vill säga hello ”tillfälliga” disk) och växla partition. Se hello följande sida på hello Ubuntu wiki för mer information: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Nästa steg: med molnet initiering
Mer information finns i hello [moln init-dokumentationen för Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Lägg till rollen Service Management REST API-referens](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure-kommandoradsgränssnittet](https://github.com/Azure/azure-xplat-cli)

