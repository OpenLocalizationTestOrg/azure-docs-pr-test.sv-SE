toocreate ett VNet i hello Resource Manager-distributionsmodellen med hjälp av hello Azure-portalen gör hello nedan. Använd hello [exempelvärden](#values) om du använder de här stegen som en vägledning. Om du inte genomför de här stegen som en vägledning är du säker på att tooreplace hello värden med dina egna. Mer information om hur du arbetar med virtuella nätverk finns hello [översikt över virtuella nätverk](../articles/virtual-network/virtual-networks-overview.md).

1. Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och logga in med ditt Azure-konto.
2. Klicka på **Ny**. I hello **Sök hello marketplace** skriver du ”virtuella nätverk”. Leta upp **virtuellt nätverk** från hello returnerade listan och klicka på tooopen hello **virtuellt nätverk** bladet.
3. Hello nedre delen av hello virtuellt nätverk-bladet, från hello **Välj en distributionsmodell** väljer **Resource Manager**, och klicka sedan på **skapa**. Då öppnas bladet för hello skapa virtuellt nätverk.

    ![Bladet Skapa virtuellt nätverk](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Bladet Skapa virtuellt nätverk")
4. På hello **skapa virtuellt nätverk** bladet konfigurera hello VNet-inställningarna. När du fyller i hello fält blir hello rött utropstecken en grön bock när hello tecken i hello fältet är giltiga.

  - **Namnet**: Ange hello namn för det virtuella nätverket. I det här exemplet använder vi TestVNet1.
  - **Adressutrymmet**: Ange hello-adressutrymme. Om du har flera adress blanksteg tooadd, lägger du till ditt första adressutrymme. Du kan lägga till extra adressutrymmen senare, när du har skapat hello virtuella nätverk. Se till att hello-adressutrymmet som du anger inte överlappa hello adressutrymmet för den lokala platsen.
  - **Undernätnamnet**: lägga till hello första undernätet namn och undernät adressintervall. Du kan lägga till ytterligare undernät och gateway-undernätet hello senare, när du har skapat detta virtuella nätverk. 
  - **Prenumerationen**: Verifiera att hello visad prenumeration är hello korrekt. Du kan ändra prenumerationer med hjälp av hello i listrutan.
  - **Resursgrupp**: Välj en befintlig resursgrupp eller skapa en ny genom att skriva ett namn för en ny resursgrupp. Om du skapar en ny grupp planerade namnet hello resursgruppen enligt tooyour konfigurationsvärden. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
  - **Plats**: Välj hello plats för din VNet. hello platsen avgör var hello resurser som du distribuerar toothis VNet kommer att finnas.

5. Välj **PIN-kod toodashboard** om du vill toobe kan toofind ditt VNet enkelt på hello instrumentpanelen och klicka sedan på **skapa**. När du klickar på **skapa**, visas en panel på instrumentpanelen som visar hello förloppet för ditt VNet. hello panelen ändringar som hello VNet skapas.
