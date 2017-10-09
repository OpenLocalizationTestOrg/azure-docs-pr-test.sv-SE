toocreate ett VNet i hello Resource Manager-distributionsmodellen med hjälp av hello Azure-portalen gör hello nedan. hello skärmbilderna tillhandahålls som exempel. Vara säker på att tooreplace hello värden med dina egna. Mer information om hur du arbetar med virtuella nätverk finns hello [översikt över virtuella nätverk](../articles/virtual-network/virtual-networks-overview.md).

1. Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och vid behov, logga in med ditt Azure-konto.
2. Klicka på **+**. I hello **Sök hello marketplace** skriver du ”virtuella nätverk”. Leta upp **virtuellt nätverk** från hello returnerade listan och klicka på tooopen hello **virtuellt nätverk** sidan.

  ![Sida för att leta upp virtuell nätverksresurs](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/newvnetportal700.png "Sida för att leta upp virtuell nätverksresurs")
3. Hello nedre delen av hello virtuellt nätverk sida från hello **Välj en distributionsmodell** väljer **Resource Manager**, och klicka sedan på **skapa**.

  ![Välj Resource Manager](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Välj Resource Manager")
4. På hello **skapa virtuellt nätverk** konfigurerar hello VNet-inställningarna. När du fyller i hello fält blir hello rött utropstecken en grön bock när hello tecken i hello fältet är giltiga. Det kan finnas värden som fylls i automatiskt. I så fall, ersätter du hello värden med dina egna. Hej **skapa virtuellt nätverk** sida visas liknande toohello följande exempel:

  ![Fältverifiering](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/createp2sgvnet.png "Fältverifiering")
5. **Namnet**: Ange hello namn för det virtuella nätverket.
6. **Adressutrymmet**: Ange hello-adressutrymme. Om du har flera adress blanksteg tooadd, lägger du till ditt första adressutrymme. Du kan lägga till extra adressutrymmen senare, när du har skapat hello virtuella nätverk.
7. **Undernätnamnet**: Lägg till hello namn och undernät adressintervall för gatewayundernät. Du kan lägga till ytterligare undernät senare, när du har skapat hello virtuella nätverk.
8. **Prenumerationen**: Verifiera att hello prenumeration i listan är hello korrekt. Du kan ändra prenumerationer med hjälp av hello i listrutan.
9. **Resursgrupp**: Välj en befintlig resursgrupp eller skapa en ny genom att skriva ett namn för en ny resursgrupp. Om du skapar en ny grupp planerade namnet hello resursgruppen enligt tooyour konfigurationsvärden. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
10. **Plats**: Välj hello plats för din VNet. hello platsen avgör var hello resurser som du distribuerar toothis VNet kommer att finnas.
11. Välj **PIN-kod toodashboard** om du vill toobe kan toofind ditt VNet enkelt på hello instrumentpanelen och klicka sedan på **skapa**.

 ![PIN-kod toodashboard](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "toodashboard PIN-kod")
12. När du klickar på **skapa**, visas en panel på instrumentpanelen som visar hello förloppet för ditt VNet. hello panelen ändringar som hello VNet skapas.

  ![Panelen Skapa virtuellt nätverk](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Panelen Skapa virtuellt nätverk")