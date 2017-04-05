

W przypadku aplikacji skalujących zasoby komputerowe w poziomie operacje skalowania są niejawnie równoważone w domenach błędów i aktualizacji. Aby zapoznać się z zagadnieniami dotyczącymi zestawów skalowania maszyn wirtualnych, zobacz najnowsze [Ogłoszenia na blogu platformy Azure](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Aby dowiedzieć się więcej o zestawach skalowania maszyn wirtualnych, obejrzyj te klipy wideo:

* [Mark Russinovich omawia zestawy skalowania na platformie Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Zestawy skalowania maszyn wirtualnych według Guya Bowermana](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Tworzenie zestawów skalowania maszyn wirtualnych i zarządzanie nimi
Zestawy skalowania maszyn wirtualnych można definiować i wdrażać za pomocą szablonów JSON oraz [interfejsów API REST](https://msdn.microsoft.com/library/mt589023.aspx) — podobnie jak poszczególne maszyny wirtualne w ramach usługi Azure Resource Manager. Można zatem użyć dowolnej standardowej metody wdrażania za pomocą usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat szablonów, zobacz [Tworzenie szablonów usługi Azure Resource Manager](../articles/resource-group-authoring-templates.md).

Zestaw przykładowych szablonów dla zestawów skalowania maszyn wirtualnych znajduje się w repozytorium szablonów z pakietu Azure Quickstart w witrynie GitHub (tutaj):

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) — poszukaj szablonów zawierających ciąg *vmss* w nazwie.

Na stronach szczegółów tych szablonów jest dostępny przycisk, który prowadzi do funkcji wdrażania w portalu. Aby wdrożyć zestaw skalowania maszyn wirtualnych, kliknij ten przycisk, a następnie wypełnij wymagane parametry w portalu. Jeśli nie wiesz, czy dany zasób obsługuje wielkie lub małe i wielkie litery, lepiej użyć wartości parametrów pisanych małymi literami. Dostępna jest również przydatna analiza wideo szablonu zestawu skalowania maszyn wirtualnych:

[Analiza szablonu zestawu skalowania maszyn wirtualnych](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Skalowanie w poziomie zestawu skalowania maszyny wirtualnej
Aby zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w zestawie skalowania maszyn wirtualnych, wystarczy zmienić właściwość *capacity* i ponownie wdrożyć szablon. Ta prosta metoda umożliwia utworzenie własnej niestandardowej warstwy skalowania w celu zdefiniowania niestandardowych zdarzeń skalowania, które nie są obsługiwane przez skalowanie automatyczne na platformie Azure.

W przypadku ponownego wdrażania szablonu w celu zmiany pojemności można zdefiniować dużo mniejszy szablon obejmujący jedynie jednostkę SKU i zaktualizowaną pojemność. Przykład takiego działania można znaleźć tutaj: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Aby poznać procedurę tworzenia zestawu skalowania, który jest skalowany automatycznie, zobacz [Automatyczne skalowanie maszyn w zestawie skalowania maszyn wirtualnych](../articles/virtual-machines/windows/vmss-powershell-creating.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="monitoring-your-vm-scale-set"></a>Monitorowanie zestawu skalowania maszyn wirtualnych
Obecnie zaleca się wyświetlanie zestawów skalowania maszyn wirtualnych za pomocą [Eksploratora zasobów Azure](https://resources.azure.com). Zestawy skalowania maszyn wirtualnych są zasobami dostępnymi w ramach dostawcy zasobów Microsoft.Compute. Z poziomu tej witryny można je wyświetlić, rozwijając następujące linki:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Scenariusze dotyczące zestawów skalowania maszyn wirtualnych
W tej sekcji przedstawiono niektóre typowe scenariusze dotyczące zestawów skalowania maszyn wirtualnych. Scenariusze te mają zastosowanie w przypadku niektórych usług platformy Azure wyższego poziomu (np. Batch, Service Fabric, Azure Container Service).

* **Połączenia RDP/SSH z wystąpieniami zestawów skalowania maszyn wirtualnych** — zestaw skalowania maszyn wirtualnych jest tworzony w sieci wirtualnej, a poszczególne maszyny wirtualne w zestawie nie uzyskują publicznych adresów IP. Jest to przydatne, ponieważ lepiej unikać wydatków i nakładów pracy związanych z przydzielaniem osobnych adresów IP wszystkim zasobom bezstanowym w sieci obliczeniowej. Z maszynami wirtualnymi można się łatwo połączyć z poziomu innych zasobów w sieci wirtualnej (także tych, które mają publiczne adresy IP, takich jak moduły równoważenia obciążenia lub autonomiczne maszyny wirtualne).
* **Łączenie się z maszynami wirtualnymi za pomocą reguł NAT** — można utworzyć publiczny adres IP, przypisać go do modułu równoważenia obciążenia i zdefiniować reguły NAT dla ruchu przychodzącego mapujące port w adresie IP na port na maszynie wirtualnej w zestawie skalowania maszyn wirtualnych. Na przykład
  
   Publiczny adres IP->Port 50000 -> vmss\_0->Port 22  Publiczny adres IP->Port 50001 -> vmss\_1->Port 22  Publiczny adres IP->Port 50002-> vmss\_2->Port 22
  
   Oto przykład tworzenia zestawu skalowania maszyn wirtualnych korzystającego z reguł NAT w celu udostępnienia połączenia SSH każdej maszynie wirtualnej w zestawie skalowania przy użyciu jednego publicznego adresu IP: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Tu natomiast pokazano, jak wykonać powyższe działania przy użyciu połączenia RDP i systemu Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Łączenie się z maszynami wirtualnymi za pomocą „rampy”** — jeśli utworzysz zestaw skalowania maszyn wirtualnych i autonomiczną maszynę wirtualną w tej samej sieci wirtualnej, autonomiczna maszyna wirtualna i maszyny wirtualne z zestawu skalowania mogą się łączyć ze sobą za pomocą wewnętrznych adresów IP zgodnie z definicją w sieci wirtualnej/podsieci. W przypadku utworzenia publicznego adresu IP i przypisania go do autonomicznej maszyny wirtualnej można nawiązać połączenie RDP lub SSH z autonomiczną maszyną wirtualną, a następnie z jej poziomu połączyć się z wystąpieniami danego zestawu skalowania maszyn wirtualnych. Można tutaj zauważyć, że prosty zestaw skalowania maszyn wirtualnych jest z natury bezpieczniejszy niż prosta autonomiczna maszyna wirtualna z publicznym adresem IP w konfiguracji domyślnej.
  
   Przykładem tego podejścia może być szablon tworzący prosty klaster Mesos obejmujący autonomiczną maszynę wirtualną Master, która zarządza klastrem maszyn wirtualnych bazującym na zestawie skalowania maszyn wirtualnych: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)
* **Działanie okrężne: równoważenie obciążenia wystąpień zestawów skalowania maszyn wirtualnych** — aby przekazywać zadania do klastra obliczeniowego maszyn wirtualnych w trybie okrężnym, można odpowiednio skonfigurować moduł równoważenia obciążenia platformy Azure za pomocą reguł równoważenia obciążenia. Definiując dodatkowo sondy do weryfikowania działania aplikacji, można wysyłać polecenia ping do portów przy użyciu określonego protokołu, interwału i ścieżki żądania.
  
   Oto przykład tworzenia zestawu skalowania maszyn wirtualnych z serwerem sieci Web usług IIS i równoważenia obciążenia poszczególnych maszyn wirtualnych za pomocą modułu równoważenia obciążenia. W przykładzie tym pokazano także, jak za pomocą protokołu HTTP wysyłać polecenia ping na określony adres URL na każdej maszynie wirtualnej: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) — spójrz na typ zasobu Microsoft.Network/loadBalancers oraz elementy networkProfile i extensionProfile w obszarze virtualMachineScaleSet.
* **Wdrażanie zestawu skalowania maszyn wirtualnych jako klastra obliczeniowego w menedżerze klastra w środowisku PaaS** — zestawy skalowania maszyn wirtualnych są czasami określane jako rola procesu roboczego następnej generacji. Jest to trafny opis, ale niesie ryzyko pomylenia funkcji zestawu skalowania z funkcjami roli procesu roboczego usługi PaaS w wersji 1. Zestawy skalowania maszyn wirtualnych pełnią rolę procesu roboczego lub stanowią zasób procesu roboczego w tym sensie, że są ogólnym zasobem obliczeniowym, który jest niezależny od platformy/środowiska wykonawczego, możliwy do dostosowania i zintegrowania z infrastrukturą IaaS usługi Azure Resource Manager.
  
   Rola procesu roboczego usługi PaaS w wersji 1 ma wprawdzie ograniczenia w zakresie obsługi platform/środowiska wykonawczego (obsługuje wyłącznie obrazy platformy Windows), ale obejmuje usługi, takie jak wymiana wirtualnego adresu IP, konfigurowane ustawienia uaktualniania i określone ustawienia wdrażania środowiska wykonawczego/aplikacji, które nie są *jeszcze* dostępne w zestawach skalowania maszyn wirtualnych lub zostaną zapewnione przez inne usługi PaaS wyższego poziomu, takie jak Service Fabric. Uwzględniając powyższe informacje, można postrzegać zestawy skalowania maszyn wirtualnych jako infrastrukturę obsługującą usługę PaaS. A zatem rozwiązania PaaS (np. Service Fabric) lub menedżery klastrów (np. Mesos) mogą bazować na zestawach skalowania maszyn wirtualnych jako skalowalna warstwa obliczeniowa.
  
   Oto przykład klastra Mesos, który wdraża zestaw skalowania maszyn wirtualnych w tej samej sieci wirtualnej co autonomiczna maszyna wirtualna. Autonomiczna maszyna wirtualna jest głównym elementem klastra Mesos, natomiast zestaw skalowania maszyn wirtualnych stanowi zestaw węzłów podrzędnych: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). W przyszłych wersjach usługi [Azure Container Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) zostaną wdrożone bardziej złożone/wzmocnione wersje tego scenariusza bazujące na zestawach skalowania maszyn wirtualnych.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Wytyczne dotyczące wydajności i skalowania zestawów skalowania maszyn wirtualnych
* Podczas korzystania z publicznej wersji zapoznawczej nie twórz jednocześnie więcej niż 500 maszyn wirtualnych w ramach wielu zestawów skalowania maszyn wirtualnych.
* Zaplanuj nie więcej niż 40 maszyn wirtualnych na jedno konto magazynu.
* Rozłóż jak najszerzej pierwsze litery nazw kont magazynu.  Odpowiednią procedurę można znaleźć, przeglądając przykładowe szablony VMSS w artykule [Szablony z pakietu Azure Quickstart](https://github.com/Azure/azure-quickstart-templates/).
* W przypadku niestandardowych maszyn wirtualnych w jednym zestawie skalowania maszyn wirtualnych nie powinno znaleźć się więcej niż 40 maszyn wirtualnych w ramach jednego konta magazynu.  Przed rozpoczęciem wdrażania zestawu skalowania maszyn wirtualnych trzeba wstępnie skopiować obraz do konta magazynu. Zapoznaj się z często zadawanymi pytaniami, aby uzyskać więcej informacji.
* W każdej sieci wirtualnej nie powinno się znajdować więcej niż 2048 maszyn wirtualnych.  Ten limit zostanie w przyszłości zwiększony.
* Liczba maszyn wirtualnych, które można utworzyć, jest ograniczona przez limit przydziału rdzeni obliczeniowych w każdym regionie. Nawet jeśli obecnie masz wysoki limit rdzeni na użytek usług w chmurze lub środowiska IaaS w wersji 1, może zaistnieć konieczność zwiększenia limitu przydziału obliczeniowego (za pośrednictwem działu pomocy technicznej). Aby wysłać zapytanie dotyczące przydziału, możesz uruchomić następujące polecenie w interfejsie wiersza polecenia platformy Azure: *azure vm list-usage* oraz następujące polecenie programu PowerShell: *Get-AzureRmVMUsage* (w przypadku programu PowerShell w wersji starszej niż 1.0 użyj polecenia *Get-AzureVMUsage*).

## <a name="vm-scale-set-frequently-asked-questions"></a>Często zadawane pytania dotyczące zestawu skalowania maszyn wirtualnych
**PYTANIE** Ile maszyn wirtualnych może być w zestawie skalowania maszyn wirtualnych?

**ODPOWIEDŹ** 100, jeśli używasz obrazów platformy, które mogą być rozproszone między wieloma kontami magazynu. W przypadku używania obrazów niestandardowych liczba maszyn wirtualnych wynosi najwyżej 40, ponieważ w wersji zapoznawczej obrazy niestandardowe są ograniczone do jednego konta magazynu.

**PYTANIE** Jakie inne ograniczenia dotyczące zasobów mają zastosowanie do zestawów skalowania maszyn wirtualnych?

**ODPOWIEDŹ** W wielu zestawach skalowania maszyn wirtualnych można utworzyć najwyżej 500 maszyn wirtualnych na region w okresie zapoznawczym. Mają także zastosowanie istniejące [ograniczenia dotyczące subskrypcji usług Azure](../articles/azure-subscription-service-limits.md).

**PYTANIE** Czy zestawy skalowania maszyn wirtualnych obsługują dyski danych?

**ODPOWIEDŹ** Nie w wersji początkowej. Dostępne opcje magazynowania danych obejmują:

* Pliki platformy Azure (dyski udostępnione za pośrednictwem protokołu SMB)
* Dysk systemu operacyjnego
* Dysk tymczasowy (lokalny, bez kopii zapasowej w usłudze Azure Storage)
* Zewnętrzna usługa danych (np. zdalna baza danych, tabele platformy Azure, obiekty blob platformy Azure)

**PYTANIE** Które regiony świadczenia usługi Azure obsługują zestawy skalowania maszyn wirtualnych?

**ODPOWIEDŹ** Zestawy skalowania maszyn wirtualnych są obsługiwane w każdym regionie, który obsługuje usługę Azure Resource Manager.

**PYTANIE** Jak utworzyć zestaw skalowania maszyn wirtualnych za pomocą obrazu niestandardowego?

**ODPOWIEDŹ** Pozostaw pustą właściwość vhdContainers, np.:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**PYTANIE** Jeśli zmniejszę pojemność zestawu skalowania maszyn wirtualnych z 20 do 15, które maszyny wirtualne zostaną usunięte?

**ODPOWIEDŹ** W celu zapewnienia maksymalnej dostępności maszyny wirtualne są usuwane z zestawu skalowania równomiernie w domenach błędów i domenach uaktualniania.

**PYTANIE** Co się stanie, jeśli następnie zwiększę pojemność z 15 do 18?

**ODPOWIEDŹ** Zwiększenie pojemności do 18 spowoduje utworzenie maszyn wirtualnych o indeksach 15, 16 i 17. W obu przypadkach maszyny wirtualne są równoważone w domenach błędów i domenach uaktualniania.

**PYTANIE** Czy mogę wymusić sekwencję wykonywania w zestawie skalowania maszyn wirtualnych w przypadku korzystania z wielu rozszerzeń?

**ODPOWIEDŹ** Nie można tego zrobić bezpośrednio, ale w przypadku rozszerzenia customScript skrypt może poczekać na ukończenie działania innego rozszerzenia (na przykład dzięki monitorowaniu dziennika rozszerzeń — zobacz [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**PYTANIE** Czy zestawy skalowania maszyn wirtualnych współdziałają z zestawami dostępności platformy Azure?

**ODPOWIEDŹ** Tak. Zestaw skalowania maszyn wirtualnych to niejawny zestaw dostępności z 3 domenami błędów i 5 domenami uaktualniania. W obszarze virtualMachineProfile nie trzeba nic konfigurować. W przyszłych wydaniach zestawy skalowania maszyn wirtualnych będą prawdopodobnie obejmować wiele dzierżaw, ale obecnie zestaw skalowania to pojedynczy zestaw dostępności.

