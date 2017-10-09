## <a name="scenario"></a>Scenario
En virtuell dator med en enda nätverkskort är virtuella nätverk som skapats och anslutna tooa. hello VM kräver tre olika *privata* IP-adresser och två *offentliga* IP-adresser. hello IP-adresser tilldelas toohello efter IP-konfigurationer:

* **IPConfig-1:** tilldelar en *statiska* privat IP-adress och en *statiska* offentlig IP-adress.
* **IPConfig-2:** tilldelar en *statiska* privat IP-adress och en *statiska* offentlig IP-adress.
* **IPConfig-3:** tilldelar en *statiska* privat IP-adress och ingen offentlig IP-adress.
  
    ![Flera IP-adresser](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

hello IP-konfigurationer är associerade toohello NIC när hello NIC skapas och hello NIC bifogade toohello VM när hello VM skapas. hello typer av IP-adresser som används för hello scenariot är för bilden. Du kan tilldela IP-adress och tilldelning av typer som du behöver.

> [!NOTE]
> Även om hello stegen i den här artikeln tilldelar alla IP-konfigurationer tooa nätverkskort, kan du också tilldela flera IP-konfigurationer tooany NIC på en virtuell dator flera nätverkskort. toolearn hur toocreate en virtuell dator med flera nätverkskort, läsa hello [skapa en virtuell dator med flera nätverkskort](../articles/virtual-network/virtual-network-deploy-multinic-arm-ps.md) artikel.
