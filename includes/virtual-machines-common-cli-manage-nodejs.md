Innan du kan använda hello Azure CLI med Resource Manager-kommandon och mallar toodeploy Azure-resurser och arbetsbelastningar med hjälp av resursgrupper, du behöver ett konto med Azure. Om du inte har något konto kan du skaffa ett [kostnadsfritt Azure-utvärderingskonto här](https://azure.microsoft.com/pricing/free-trial/).

Om du inte redan har installerat hello Azure CLI och anslutna tooyour prenumeration finns [installera hello Azure CLI](../articles/cli-install-nodejs.md) ange hello-läge för`arm` med `azure config mode arm`, och ansluta tooAzure hello `azure login` kommando.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- Azure CLI 10 – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](../articles/virtual-machines/linux/cli-manage.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Grundläggande Azure Resource Manager-kommandon i Azure CLI
Den här artikeln täcker grundläggande kommandon som du vill toouse med Azure CLI toomanage och interagera med dina resurser (främst virtuella datorer) i din Azure-prenumeration.  Mer detaljerad hjälp med specifika kommandoradsväxlar och alternativ du kan använda hello online-kommandot Hjälp och alternativ genom att skriva `azure <command> <subcommand> --help` eller `azure help <command> <subcommand>`.

> [!NOTE]
> De här exemplen omfattar inte mallbaserade åtgärder som brukar rekommenderas för distribueringar av virtuella datorer i Resource Manager. Mer information finns i [Använd hello Azure CLI med Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md) och [distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

| Aktivitet | Resource Manager |
| --- | --- | --- |
| Skapa hello mest grundläggande VM |`azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Hämta hello `image-urn` från hello `azure vm image list` kommando. Fler exempel finns i [den här artikeln ](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) .) |
| Skapa en virtuell Linux-dator |`azure  vm create [options] <resource-group> <name> <location> -y "Linux"` |
| Skapa en virtuell Windows-dator |`azure  vm create [options] <resource-group> <name> <location> -y "Windows"` |
| Lista över virtuella datorer |`azure  vm list [options]` |
| Hämta information om en virtuell dator |`azure  vm show [options] <resource_group> <name>` |
| Starta en virtuell dator |`azure vm start [options] <resource_group> <name>` |
| Stoppa en virtuell dator |`azure vm stop [options] <resource_group> <name>` |
| Frigöra en virtuell dator |`azure vm deallocate [options] <resource-group> <name>` |
| Starta om en virtuell dator |`azure vm restart [options] <resource_group> <name>` |
| Ta bort en virtuell dator |`azure vm delete [options] <resource_group> <name>` |
| Avbilda en virtuell dator |`azure vm capture [options] <resource_group> <name>` |
| Skapa en virtuell dator av en användaravbildning |`azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>` |
| Skapa en virtuell dator från en särskild disk |`azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>` |
| Lägg till en data disk tooa VM |`azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]` |
| Ta bort en datadisk från en virtuell dator |`azure  vm disk detach [options] <resource-group> <vm-name> <lun>` |
| Lägg till en allmän tillägget tooa VM |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Lägg till åtkomst av VM-tillägget tooa VM |`azure vm reset-access [options] <resource-group> <name>` |
| Lägg till Docker-tillägget tooa VM |`azure  vm docker create [options] <resource-group> <name> <location> <os-type>` |
| Ta bort ett virtuellt datortillägg |`azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Få användning av virtuella datorresurser |`azure vm list-usage [options] <location>` |
| Hämta alla tillgängliga storlekar för virtuella datorer |`azure vm sizes [options]` |

## <a name="next-steps"></a>Nästa steg
* Ytterligare exempel på hello CLI-kommandona öka bortom grundläggande VM-hantering finns [Using hello Azure CLI med Azure Resource Manager](../articles/virtual-machines/azure-cli-arm-commands.md).
