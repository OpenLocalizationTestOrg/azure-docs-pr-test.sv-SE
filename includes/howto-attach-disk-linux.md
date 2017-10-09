
Mer information om diskar finns i [Om diskar och virtuella hårddiskar för Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a>Ansluta en tom disk
1. Öppna Azure CLI 1.0 och [ansluta tooyour Azure-prenumeration](../articles/xplat-cli-connect.md). Kontrollera att du är i läget för Azure-tjänsthantering (`azure config mode asm`).
2. Ange `azure vm disk attach-new` toocreate och koppla en ny disk som visas i följande exempel hello. Ersätt *myVM* med hello namn för din virtuella Linux-datorn och ange hello hello diskens storlek i GB, vilket är *100GB* i det här exemplet:

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. När hello datadisk skapas och bifogas, visas den i hello utdata från `azure vm disk list <virtual-machine-name>` som visas i följande exempel hello:
   
    ```azurecli
    azure vm disk list TestVM
    ```

    hello utdata är liknande toohello följande exempel:

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>Ansluta en befintlig disk
För att kunna ansluta en befintlig disk måste du ha en VHD-fil tillgänglig i ett lagringskonto.

1. Öppna Azure CLI 1.0 och [ansluta tooyour Azure-prenumeration](../articles/xplat-cli-connect.md). Kontrollera att du är i läget för Azure-tjänsthantering (`azure config mode asm`).
2. Kontrollera om hello VHD som du vill använda tooattach är redan överförts tooyour Azure-prenumeration:
   
    ```azurecli
    azure vm disk list
    ```

    hello utdata är liknande toohello följande exempel:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. Om du inte hittar hello disk som du vill toouse, du kan ladda upp en lokal VHD tooyour prenumeration med hjälp av `azure vm disk create` eller `azure vm disk upload`. Ett exempel på `disk create` skulle vara som i följande exempel hello:
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    hello utdata är liknande toohello följande exempel:

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   Du kan också använda `azure vm disk upload` tooupload ett VHD tooa specifika storage-konto. Läs mer om hello kommandon toomanage datadiskar ditt virtuella Azure-datorn [över här](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

4. Nu du bifoga önskad hello VHD tooyour virtuell dator:
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   Se till att tooreplace *myVM* med hello namnet på den virtuella datorn och *myVHD* med den önskade virtuella Hårddisken.

5. Du kan verifiera hello disk är anslutna toohello virtuell dator med `azure vm disk list <virtual-machine-name>`:
   
    ```azurecli
    azure vm disk list myVM
    ```

    hello utdata är liknande toohello följande exempel:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> När du lägger till en datadisk, du behöver toolog på toohello virtuella datorn och initiera hello disk så hello virtuell dator kan använda hello disk för lagring (se hello följande steg för mer information på hur toodo initierar hello disk).
> 
> 

