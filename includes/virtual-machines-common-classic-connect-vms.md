

![Virtuella datorer i en fristående molntjänst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Om du placerar dina virtuella datorer i ett virtuellt nätverk, kan du bestämma hur många molntjänster som du vill använda toouse för att läsa in belastningsutjämning och tillgänglighet. Dessutom kan du ordna hello virtuella datorer på undernät i hello samma sätt som ditt lokala nätverk och ansluta hello virtuellt nätverk tooyour lokalt nätverk. Här är ett exempel:

![Virtuella datorer i ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuella nätverken är hello rekommenderat sätt tooconnect virtuella datorer i Azure. Hej bästa praxis är tooconfigure varje nivå av ditt program i en separat molntjänst. Men du kanske måste toocombine vissa virtuella datorer från olika program nivåer i hello samma cloud service tooremain inom hello högst 200 molntjänster per prenumeration. tooreview detta och andra begränsningar, finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Ansluta virtuella datorer i ett virtuellt nätverk
tooconnect virtuella datorer i ett virtuellt nätverk:

1. Skapa hello virtuellt nätverk i hello [Azure-portalen](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) och ange 'klassisk distribution ”.
2. Skapa hello molntjänster för din distribution tooreflect din design för tillgänglighetsuppsättningar och belastningsutjämning. I hello Azure-portalen klickar du på **New > Compute > molntjänst** för varje tjänst i molnet.

  När du fyller i hello molnet tjänstinformation väljer hello samma _resursgruppen_ används med hello virtuellt nätverk.

3. toocreate varje nya virtuella datorn, klickar du på **New > Compute**, och sedan väljer hello lämpliga VM-avbildning från hello **aktuella appar**.

  I hello VM **grunderna** bladet välj hello samma _resursgruppen_ används med hello virtuellt nätverk.

  ![Grunderna i VM-bladet när du använder ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. När du fyller i hello VM **inställningar**, Välj rätt hello _Molntjänsten_ eller _virtuellt nätverk_ för hello VM.

  Azure markerar hello andra objekt baserat på ditt val.

  ![VM inställningsbladet när du använder ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Ansluta virtuella datorer i en fristående molntjänst
tooconnect virtuella datorer i en fristående molnbaserad tjänst:

1. Skapa hello Molntjänsten i hello [Azure-portalen](http://portal.azure.com). Klicka på **New > Compute > molntjänst**. Eller, du kan skapa hello Molntjänsten för din distribution när du skapar din första virtuella datorn.
2. När du skapar hello virtuella datorer väljer du hello samma resursgrupp som används med hello-Molntjänsten.

  ![Lägg till en virtuell dator tooan befintlig molntjänst](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  När du fyller i hello VM information Välj hello namnet på Molntjänsten som skapats i hello första steget.

  ![Att välja en tjänst i molnet för en virtuell dator](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
