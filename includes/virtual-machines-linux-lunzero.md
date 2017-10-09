När du lägger till data diskar tooa Linux VM kan fel inträffa om en disk inte finns på LUN 0. Om du lägger till en disk manuellt med hjälp av hello `azure vm disk attach-new` kommando och anger ett LUN (`--lun`) i stället för att tillåta hello Azure-plattformen toodetermine Hej lämpliga LUN, ta hand som en disk redan finns / kommer att finnas på LUN 0. 

Överväg följande exempel visar en fragment på hello utdata från hello `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

hello två hårddiskar finns på LUN 0 och 1 för LUN (hello första kolumnen i hello `lsscsi` utdata information `[host:channel:target:lun]`). Båda diskarna måste vara accessbile från inom hello VM. Om du manuellt hade angetts hello första disk toobe läggs till på LUN 1 och hello andra disken på LUN 2 syns inte hello diskar från rätt inom den virtuella datorn.

> [!NOTE]
> hello Azure `host` värdet är 5 i dessa exempel, men detta kan variera beroende på hello typ av lagring som du väljer.
> 
> 

Det här beteendet för disken är inte ett problem med Azure utan hello sätt i vilka hello Linux kernel efter hello SCSI-specifikationer. En enhet måste finnas på LUN 0 för hello system toocontinue vid sökning efter ytterligare enheter när hello Linux kernel-sökningar hello SCSI-bussen för anslutna enheter. Som exempel:

* Granska hello utdata från `lsscsi` när du lägger till en tooverify för data-disk som du har en disk på LUN 0.
* Om disken inte visas korrekt i den virtuella datorn, kontrollerar du att det finns en disk på LUN 0.

