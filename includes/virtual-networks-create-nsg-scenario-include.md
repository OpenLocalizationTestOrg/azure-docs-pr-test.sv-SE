## <a name="scenario"></a>Scenario
toobetter visar hur toocreate NSG: er, det här dokumentet använder hello scenariot nedan.

![VNet-scenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

I det här scenariot skapar du en NSG för varje undernät i hello **TestVNet** virtuella nätverk som beskrivs nedan: 

* **NSG-klientdel**. hello klientdelen NSG är tillämpade toohello *klientdel* undernät, och innehåller två regler:    
  * **RDP-regel**. Den här regeln tillåter RDP-trafik toohello *klientdel* undernät.
  * **regel för Web**. Den här regeln ska tillåta HTTP-trafik toohello *klientdel* undernät.
* **NSG-BackEnd**. hello serverdel NSG kommer att tillämpas toohello *BackEnd* undernät, och innehåller två regler:    
  * **SQL-regel**. Den här regeln kan SQL-trafik från hello *klientdel* undernät.
  * **regel för Web**. Den här regeln nekar alla internet-bunden trafik från hello *BackEnd* undernät.

hello kombination av reglerna skapar du ett DMZ-liknande scenario, där hello serverdel undernät kan endast ta emot inkommande trafik för SQL från hello klientdelen undernät och har ingen åtkomst toohello Internet, medan hello klientdelens undernät kan kommunicera med hello Internet, och får endast inkommande HTTP-begäranden.

