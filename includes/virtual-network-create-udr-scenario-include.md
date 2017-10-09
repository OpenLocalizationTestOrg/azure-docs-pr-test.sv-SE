## <a name="scenario"></a>Scenario
toobetter visar hur toocreate udr: er, det här dokumentet använder hello scenariot nedan.

![BESKRIVNING AV AVBILDNING](./media/virtual-network-create-udr-scenario-include/figure1.png)

I det här scenariot skapar du en UDR för hello *Front end undernät* och en annan UDR för hello *tillbaka slutet undernät* , enligt beskrivningen nedan: 

* **UDR klientdel**. hello klientdelen UDR är tillämpade toohello *klientdel* undernät, och innehåller en väg:    
  * **RouteToBackend**. Den här vägen skickar all trafik toohello serverdel undernät toohello **FW1** virtuella datorn.
* **UDR BackEnd**. hello serverdel UDR kommer att använda toohello *BackEnd* undernät, och innehåller en väg:    
  * **RouteToFrontend**. Den här vägen skickar all trafik toohello klientdelen undernät toohello **FW1** virtuella datorn.

hello kombination av dessa vägar ska kontrollerar du att all trafik från ett undernät tooanother routade toohello **FW1** virtuell dator som används som en virtuell installation. Du måste också tooturn på IP-vidarebefordring för den virtuella datorn, det kan ta emot trafik tooensure avsedda tooother virtuella datorer.

