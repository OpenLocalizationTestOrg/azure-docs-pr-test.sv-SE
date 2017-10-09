
1. Logga in tooyour Azure-prenumeration med hjälp av stegen i hello [ansluta tooAzure från hello Azure CLI 1.0](../articles/xplat-cli-connect.md).

2. Kontrollera att du är i hello klassisk distribution läge på följande sätt:

    ```azurecli
    azure config mode asm
    ```

3. Ta reda på hello Linux bilden som du vill tooload från hello tillgängliga avbildningar på följande sätt:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    I ett kommandotolksfönster i Windows använder du **söka** i stället för grep.
   
4. Använd `azure vm create` toocreate en virtuell dator med hello Linux-avbildning från hello föregående lista. I det här steget skapas en molntjänst och ett lagringskonto. Du kan också ansluta den här Virtuella tooan befintlig molntjänst med en `-c` alternativet. Skapa en SSH-slutpunkten toolog i toohello virtuell Linux-dator med hello `-e` alternativet. hello följande exempel skapas en virtuell dator med namnet `myVM` med hello `Ubuntu-14_04_4-LTS` bilden i hello `West US` plats, och lägger till ett användarnamn `ops`:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    hello utdata är liknande toohello följande exempel:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > För en virtuell Linux-dator, måste du ange hello `-e` alternativet i `vm create`. Det är inte möjligt tooenable SSH efter hello virtuella datorn har skapats. Mer information om SSH läsa [hur tooUse SSH med Linux på Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Du kan verifiera hello attribut för hello VM med hjälp av hello `azure vm show` kommando. hello följande exempel visar information för hello virtuella datorn med namnet `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Starta den virtuella datorn med hello `azure vm start` kommandot på följande sätt:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Nästa steg
Mer information om alla Azure CLI 1.0 virtuella kommandona läsa hello [Using hello Azure CLI 1.0 med hello klassisk distribution API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

