## <a name="how-toocreate-a-classic-vnet-in-hello-azure-portal"></a>Hur toocreate ett klassiskt virtuellt nätverk i hello Azure-portalen
toocreate ett klassiskt virtuellt nätverk baserat på hello scenariot ovan, följ hello stegen nedan.

1. Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.
2. Klicka på **ny** > **nätverk** > **för virtuella nätverk**, Lägg märke till att hello **Välj en distributionsmodell** lista redan visas **klassiska**, och klicka sedan på **skapa**enligt hello bilden nedan.
   
    ![Skapa VNet i Azure-portalen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. På hello **för virtuella nätverk** bladet, typen hello **namn** hello virtuella nätverk och klicka sedan på **adressutrymmet**. Konfigurera inställningar för hello VNet och det första undernätet din adressutrymme och klicka sedan på **OK**. hello bilden nedan visar hello CIDR block-inställningar för vårt scenario.
   
    ![Adressutrymme-bladet](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Klicka på **resursgruppen** och välj en resurs grupp tooadd hello VNet till, eller klicka på **Skapa ny resursgrupp** tooadd hello VNet tooa ny resursgrupp. hello bilden nedan visar hello resurs resursgruppsinställningarna för en ny resursgrupp som kallas **TestRG**. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
   
    ![Skapa blad för resursgrupp](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. Om det behövs ändrar hello **prenumeration** och **plats** inställningar för din VNet. 
6. Om du inte vill toosee hello VNet som en panel i hello **startsidan**, inaktivera **PIN-kod tooStartboard**. 
7. Klicka på **skapa** och meddelande hello ikonen som heter **skapar virtuellt nätverk** enligt hello bilden nedan.
   
    ![Skapa VNet i portalen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Vänta tills hello VNet toobe skapas och när du ser hello panelen nedan genom att klicka på tooadd flera undernät.
   
    ![Skapa VNet i portalen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Du bör se hello **Configuration** för din VNet som visas nedan. 
   
    ![Skapa VNet i portalen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Klicka på **undernät** > **Lägg till**, Skriv en **namn** och ange en **adressintervall (CIDR-block)** din undernät, och sedan Klicka på **OK**. hello bilden nedan visar hello inställningar för våra aktuella scenariot.
    
    ![Skapa VNet i Azure-portalen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

