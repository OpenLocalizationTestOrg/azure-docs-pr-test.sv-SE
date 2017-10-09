


## <a name="tagging-a-virtual-machine-through-templates"></a>En virtuell dator via mallar-märkning
Först ska vi titta på Taggning via mallar. [Den här mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) placerar taggar på hello följande resurser: bearbetning (virtuell dator), lagring (Storage-konto) och nätverk (offentlig IP-adress, virtuella nätverk och gränssnitt). Den här mallen är för en virtuell Windows-dator, men kan anpassas för virtuella Linux-datorer.

Klicka på hello **distribuera tooAzure** knappen från hello [mall länk](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Detta ska gå toohello [Azure-portalen](https://portal.azure.com/) där du kan distribuera den här mallen.

![Enkel distribution med taggar](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Den här mallen innehåller hello följande taggar: *avdelning*, *programmet*, och *Skapad av*. Du kan lägga till eller redigera dessa taggar direkt i hello mallen om du vill ha olika taggnamn.

![Azure taggar i en mall](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Som du ser har hello taggar definierats som nyckel/värde-par, avgränsade med kolon (:). hello taggar måste definieras i det här formatet:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Spara filen med hello mallen när du har redigerat med hello taggar du väljer.

Nästa i hello **redigera parametrar** avsnittet kan du fylla i hello värden för taggarna.

![Redigera taggar i Azure-portalen](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klicka på **skapa** toodeploy den här mallen med taggvärden.

## <a name="tagging-through-hello-portal"></a>Taggning via hello Portal
Efter att dina resurser med taggar kan du visa, lägga till och ta bort taggar i hello portal.

Välj hello taggar ikonen tooview taggarna:

![Ikon för etiketter i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Lägg till en ny tagg hello-portalen genom att definiera en egen nyckel/värde-par och spara den.

![Lägg till ny tagg i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Din nya taggen bör nu visas i hello lista med taggar för din resurs.

![Ny tagg som sparats i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

