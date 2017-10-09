När du inte längre behöver en datadisk som är bifogade tooa virtuell dator (VM) kan du enkelt frånkopplas. När du kopplar från en disk från hello VM hello disken är inte bort från lagringsplatsen. Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.  

> [!NOTE]
> En virtuell dator i Azure använder olika typer av diskar – en operativsystemdisk, en lokal temporär disk och valfria datadiskar. Mer information finns i avsnittet om [diskar och virtuella hårddiskar för virtuella datorer](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Du kan inte koppla från en operativsystemdisk om du tar bort hello VM.

## <a name="find-hello-disk"></a>Hitta hello disk
Innan du kan koppla från en disk från en virtuell dator måste toofind ut hello LUN-nummer som är en identifierare för hello disk toobe oberoende. toodo som gör följande:

1. Öppna Azure CLI och [ansluta tooyour Azure-prenumeration](../articles/xplat-cli-connect.md). Kontrollera att du är i läget för Azure-tjänsthantering (`azure config mode asm`).
2. Ta reda på vilka diskar som är anslutna tooyour VM. hello följande exempel visar diskar för hello virtuella datorn med namnet `myVM`:

    ```azurecli
    azure vm disk list myVM
    ```

    hello utdata är liknande toohello följande exempel:

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Observera hello LUN eller hello **logiskt enhetsnummer** för hello disken som du vill toodetach.

## <a name="remove-operating-system-references-toohello-disk"></a>Ta bort referenser toohello operativsystemdisk
Innan du kopplar från hello disk från hello Linux Gäst, bör du se till att alla partitioner på disk hello inte används. Se till att operativsystemet hello inte försöker tooremount dem efter en omstart. De här stegen Ångra hello configuration skapade du antagligen när [kopplar](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disk.

1. Använd hello `lsscsi` kommandot toodiscover hello disk identifierare. `lsscsi` kan installeras med `yum install lsscsi` (Red Hat-baserade distributioner) eller med `apt-get install lsscsi` (Debian-baserade distributioner). Du kan hitta hello disk-ID som du söker efter med hello LUN-nummer. hello senaste antalet hello tuppel i varje rad är hello LUN. I följande exempel från hello `lsscsi`, LUN 0 mappar för  */dev/sdc*

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Använd `fdisk -l <disk>` toodiscover hello partitioner som är associerade med hello disk toobe oberoende. hello följande exempel visar hello utdata `/dev/sdc`:

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Demontera varje partition för hello disk. hello följande exempel demonterar `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

4. Använd hello `blkid` kommandot toodiscovery hello UUID: er för alla partitioner. hello utdata är liknande toohello följande exempel:

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Ta bort poster i hello **/etc/fstab** fil som är associerad med hello-sökvägar till enheter eller UUID: er för alla partitioner för hello disk toobe oberoende.  Poster i det här exemplet kan vara:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    eller
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-hello-disk"></a>Koppla bort hello disk
När du har hittat hello LUN antalet hello disk och borttagna hello operativsystemet referenser du är klar toodetach den:

1. Koppla från hello valda disken från hello virtuell dator genom att köra kommandot hello `azure vm disk detach
   <virtual-machine-name> <LUN>`. hello följande exempel tar bort LUN `0` från hello virtuella datorn med namnet `myVM`:
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. Du kan kontrollera om hello disk fick oberoende genom att köra `azure vm disk list` igen. följande exempel kontrollerar hello hello virtuella datorn med namnet `myVM`:
   
    ```azurecli
    azure vm disk list myVM
    ```

    hello utdata är liknande toohello följande exempel som visar hello datadisken är inte längre ansluten:

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

hello frånkopplade disken kvar i lagring men är inte längre kopplade tooa virtuell dator.

