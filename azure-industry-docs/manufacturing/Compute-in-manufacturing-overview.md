---
title: Vysoce výkonné služby COMPUTE Azure pro výrobu na vyžádání
author: ercenk
ms.author: ercenk
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Přehled vysokého výkonu pro výpočetní výkon ve výrobním odvětví.
ms.openlocfilehash: fe5200a726b2a65efaed2bc7a8de01e97766d425
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052531"
---
# <a name="on-demand-scalable-high-power-compute"></a>Škálovatelné, vysoce výkonné výpočetní prostředky na vyžádání

## <a name="introduction"></a>Úvod

Zákazníci mají náročné produkty, které mají tyto atributy: odlehčené, silné, bezpečné, udržitelné a přizpůsobené. V důsledku toho se fáze návrhu stane stále složitou. V této fázi se počítače používají k vizualizaci, analýze, simulaci a optimalizaci. A tyto úlohy budou rozšiřovat propracovanější a výpočetní náročné. Přidejte k tomuto faktu, že produkty jsou stále stále propojeny a generují velké objemy dat, které je třeba zpracovat a analyzovat.

To vše přináší až jednu potřebu: velké výpočetní prostředky na vyžádání.

V tomto článku se seznámíme s některými známými oblastmi v technikách a výrobou, které vyžadují velký výpočetní výkon, a prozkoumejte, jak vám může pomoci platforma Microsoft Azure.

## <a name="cloud-design-workstations"></a>Pracovní stanice pro návrh cloudu

Návrháři produktů používají různé softwarové nástroje během fází návrhu a plánování životního cyklu vývoje produktu. Nástroje CAD vyžadují silné grafické funkce na pracovní stanici návrháře a náklady na tyto pracovní stanice jsou vysoké. Tyto přidržené pracovní stanice jsou obvykle uvnitř kanceláří návrháře a jejich fyzické připojení k místu je fyzicky k dispozici.

Vzhledem k zajištění většího počtu cloudových řešení a k dispozici jsou nové funkce, které byly u cloudových pracovních stanic zahájené k dosažení větší životaschopnosti. Použití pracovní stanice hostované v cloudu umožňuje návrhářům přístup k nim z libovolného místa. A umožňuje organizaci změnit nákladový model z investičních nákladů na provozní výdaje.

### <a name="remote-desktop-protocol"></a>protokol RDP (Remote Desktop Protocol)

Microsoft protokol RDP (Remote Desktop Protocol) (RDP) podporuje protokol TCP pouze po dlouhou dobu. TCP přináší větší režii než UDP. Od protokolu RDP 8,0 je k dispozici protokol UDP pro servery, na kterých běží Vzdálená plocha Microsoft Services. Aby bylo možné použít, musí mít virtuální počítač k dispozici dostatek hardwarových prostředků, jmenovitě procesor, paměť a – nejdůležitějším způsobem – grafický procesor (GPU). (GPU je pravděpodobně nejdůležitější součástí vysoce výkonné cloudové pracovní stanice.) Windows Server 2016 poskytuje několik možností pro přístup k základním grafickým funkcím. Výchozím řešením vzdáleného GPU, označovaného také jako pokročilá rastrová platforma systému Windows (pokřivení), je vhodné řešení pro scénáře znalostní stanice, ale poskytuje nedostatečné prostředky pro scénáře cloudových pracovních stanic. RemoteFX vGPU je funkce RemoteFX, která byla představena pro vzdálená připojení, která poskytuje scénáře s vyššími hustotami uživatelů na server a umožňuje vysoké využití procesoru. Pokud je však čas nutný k použití síly GPU, je nutné diskrétní přiřazení zařízení (DDA) využít k plnému využití energie GPU.

Virtuální počítače řady NV jsou dostupné s jedním nebo několika NVDIA GPU jako součást nabídky Azure N Series. Tyto virtuální počítače jsou optimalizované pro vzdálené vizualizace a scénáře VDI pomocí platforem, jako je OpenGL a DirectX. Až 4 GPU je možné zřídit pracovní stanice, které plně využívají GPU prostřednictvím DDA v Azure.

Velmi důležitým bodem, který představuje zmínku, je programovatelnost platformy Azure. Nabízí několik možností pro virtuální počítač. Můžete například zřídit pracovní stanici na vyžádání. Stav vzdáleného počítače můžete také zachovat na místních souborech prostřednictvím disků Azure na Premium Storage nebo souborů Azure. Tyto možnosti poskytují možnost řídit náklady. Partnerství Microsoftu s Citrix pro svá XenDesktop a XenApp řešení také nabízí další alternativu k řešení virtualizace plochy.

## <a name="analysis-and-simulation"></a>Analýza a simulace

Analýza a simulace fyzických systémů v počítačích byly delší dobu. FEA (konečný element Analysis) je číselná metoda, která se používá pro řešení mnoha problémů s analýzou. FEA vyžaduje spoustu výpočetního výkonu pro provádění velkých maticových výpočtů. Počet matic zapojených do řešení modelu FEA se exponenciálně rozbalí, protože procházíme od 2D po 3D a přidáváme do sítě FEA členitost. To vyžaduje výpočetní výkon nasazený na vyžádání.
Je důležité, aby kód řešení problému mohl běžet paralelně, aby bylo možné využívat škálovatelnost prostředků.

Řešení problémů s simulací vyžaduje rozsáhlé výpočetní prostředky. Výpočty s vysokým výkonem (HPC) jsou třídy rozsáhlých výpočetních prostředků. HPC vyžaduje nízkou latenci sítě s back-endu s funkcemi vzdáleného přímého přístupu do paměti (RDMA) pro rychlé paralelní výpočty. Platforma Azure nabízí virtuální počítače vytvořené pro vysoce výkonné výpočetní prostředí. Specializované procesory funkcí jsou spárované s DDR4 pamětí a umožňují efektivní spouštění řešení náročných na systémy Linux a Windows. A jsou k dispozici v různých velikostech. Podívejte se na [vysoce výkonné výpočetní velikosti virtuálních počítačů](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json?WT.mc_id=computeinmanufacturing-docs-ercenk).
Pokud chcete zjistit, jak Azure podporuje HPC jiným způsobem, přečtěte si téma Big Compute: [HPC & Batch](https://azure.microsoft.com/solutions/big-compute/?WT.mc_id=computeinmanufacturing-docs-ercenk).

Platforma Azure umožňuje navýšení a zmenšení kapacity řešení. Jedním z běžně známých softwarových balíčků pro simulaci je STARá CCM +, z CD-adapco. [Publikovaná studie](https://azure.microsoft.com/blog/availability-of-star-ccm-on-microsoft-azure/?WT.mc_id=computeinmanufacturing-docs-ercenk) ukazující Star-ccm + Running "Le Mans 100 000 000 buňka" (CFD), která nabízí nakoukněte škálovatelnost platformy. Následující graf znázorňuje zjištěnou škálovatelnost, protože při spuštění simulace se přidávají další jádra:

![https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/34f1873b-4db5-4c62-b963-8bdf3966cf60.png](assets/bigcompute-assets/starccm.png)

Dalším oblíbeným softwarovým balíčkem pro analýzu je ANSYS CFD. Umožňuje inženýrům provádět analýzu více fyzika, včetně kapalinových sil, tepelných účinků, strukturální integrity a elektromagnetického záření. [Publikovaná studie](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/?WT.mc_id=computeinmanufacturing-docs-ercenk) ukazuje škálovatelnost řešení v Azure, která zobrazuje podobné výsledky.

![https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/77129585-f25c-4c29-b22b-80c627d03daa.png](assets/bigcompute-assets/fluent.png)

Namísto investování do místního výpočetního clusteru je možné nasadit softwarový balíček, který vyžaduje paralelní spuštění, na virtuální počítače Azure nebo [Virtual Machine Scale Sets (VMSS)](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) pomocí rodin [virtuálních počítačů HPC a GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json?WT.mc_id=computeinmanufacturing-docs-ercenk) pro řešení pro všechny cloudy.

### <a name="burst-to-azure"></a>Nárůst do Azure

Pokud je k dispozici místní cluster, je další možností rozšíření do Azure a tím také přesměrovat zatížení ve špičce (podobně jako při roztržení do Azure). K tomu je potřeba použít jedno z místních správců úloh, které podporují Azure (např. [alces let COMPUTE](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview?WT.mc_id=computeinmanufacturing-docs-ercenk), [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/?WT.mc_id=computeinmanufacturing-docs-ercenk), [jasný Správce clusteru](http://www.brightcomputing.com/technology-partners/microsoft), [IBM spektrum Symphony a Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/?WT.mc_id=computeinmanufacturing-docs-ercenk), [PBS pro](http://pbspro.org/), [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx?WT.mc_id=computeinmanufacturing-docs-ercenk)).

Další možností je Azure Batch, což je služba, která umožňuje efektivně spouštět rozsáhlé paralelní úlohy a dávkové úlohy HPC. Azure Batch umožňuje úlohy, které používají rozhraní API rozhraní API pro předávání zpráv (MPI). Batch podporuje [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx?WT.mc_id=computeinmanufacturing-docs-ercenk) a Intel MPI s RODINAMI virtuálních počítačů [HPC](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc?WT.mc_id=computeinmanufacturing-docs-ercenk) a [GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu?WT.mc_id=computeinmanufacturing-docs-ercenk) . Společnost Microsoft taky získala [cyklická výpočetní](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/?WT.mc_id=computeinmanufacturing-docs-ercenk)prostředí, které nabízí řešení, které nabízí vyšší úroveň abstrakce pro spouštění clusterů v Azure. Další možností je spustit [Cray počítače](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) v Azure s bezproblémovým přístupem k doplňkovým službám Azure, jako jsou [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage?WT.mc_id=computeinmanufacturing-docs-ercenk) a [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview?WT.mc_id=computeinmanufacturing-docs-ercenk).

## <a name="generative-design"></a>Regenerační návrh

Proces návrhu je vždy iterativním příkladem. Návrhář začíná sadou omezení a parametrů pro cílový návrh a provede iteraci nad několika alternativami návrhu a nakonec se doplní k jednomu, který splňuje tato omezení.
Pokud je však výpočetní výkon prakticky nekonečný, jeden z nich může vyhodnotit *tisíce* nebo dokonce *miliony* alternativ k návrhu místo několika málo. Tato cesta začala s modely ukazatelů a jejich použití v nástrojích CAD. Díky velkému množství výpočetních prostředků na cloudových platformách se v oboru chystá další krok. Regenerační návrh je termín, který popisuje proces návrhu poskytování parametrů a omezení softwaru. Nástroj pak vygeneruje alternativy návrhu a vytvoří několik permutací řešení.
Existuje několik přístupů na regenerační návrh: optimalizace topologie, optimalizace mřížky, optimalizace povrchu a syntéza formulářů. Podrobnosti těchto přístupů jsou z rozsahu tohoto článku. Společný vzor v rámci těchto přístupů je ale potřebný k přístupu k prostředím náročným na výpočetní výkon.

Počátečním bodem regeneračního návrhu je definovat parametry návrhu, přes které algoritmus musí iterovat, a přiměřené přírůstky a rozsahy hodnot. Algoritmus poté vytvoří alternativní návrh pro každou platnou kombinaci těchto parametrů. Výsledkem je velký počet alternativ k návrhu. Vytváření těchto alternativ vyžaduje spoustu výpočetních prostředků.
Pro každou alternativu návrhu musíte také spustit všechny simulace a úlohy analýzy. Výsledkem netto je, že budete potřebovat obrovský výpočetní prostředí.

V Azure je více možností škálování na vyžádání pro potřeby výpočtů, a to prostřednictvím [Azure Batch](https://docs.microsoft.com/azure/batch/batch-technical-overview?WT.mc_id=computeinmanufacturing-docs-ercenk)a [VMSS](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) představují pro tyto úlohy přirozené cíle.

## <a name="machine-learning-ml"></a>Machine Learning (ML)

Na velmi zjednodušený úrovni můžeme zobecnit navýšit systémy ML, jako tedy: s ohledem na datový bod nebo sadu datových bodů, systém vrátí korelační výsledek. Tímto způsobem se používají systémy ML k řešení otázek, jako například:

- S ohledem na ceny za předchozí dům a vlastnosti na pracovišti, jaká je předpokládaná cena za danou dům, která přichází na trh?

- S ohledem na čtení z různých senzorů a minulých případů selhání počítače, jaká je pravděpodobnost, že tento počítač selže v následujícím období?

- Máte-li sadu imagí, což je domácí Cat?

- Vzhledem k tomu, že video kanál oleje obsahuje, je poškozená jeho část s podstatným odsazením?

Přidání pokročilé analytické funkce s využitím umělých inteligentních funkcí (AI) a strojového učení (ML) začíná vývojem modelu s podobným procesem.

![](assets/bigcompute-assets/aipipeline.png)

[Výběr algoritmu](https://docs.microsoft.com/azure/machine-learning/studio/algorithm-choice?WT.mc_id=computeinmanufacturing-docs-ercenk) závisí na velikosti, kvalitě a povaze dat a na typu očekávané odpovědi. V závislosti na velikosti vstupu a vybraném algoritmu a výpočetním prostředí Tento krok obvykle vyžaduje velké výpočetní prostředky náročné na výpočetní výkon a jejich dokončení může trvat různou dobu. Následující graf je [technickým článkem](https://blogs.technet.microsoft.com/machinelearning/2017/07/25/lessons-learned-benchmarking-fast-machine-learning-algorithms/?WT.mc_id=computeinmanufacturing-docs-ercenk) pro srovnávací testy pro školení algoritmů ml. zobrazuje čas dokončení školicího cyklu s různými algoritmy, velikostmi sady dat a cíli výpočtů (GPU nebo CPU).

![](assets/bigcompute-assets/vmsizes.png)

Hlavním ovladačem pro rozhodnutí je obchodní problém. Pokud problém vyžaduje zpracování rozsáhlých datových sad s vhodným algoritmem, kritická faktor je cloudové výpočetní prostředky pro školení algoritmu.
[Azure Batch AI](https://azure.microsoft.com/services/batch-ai/?WT.mc_id=computeinmanufacturing-docs-ercenk) je služba, která navlakuje modely AI paralelně a ve velkém měřítku.

Pomocí Azure Batch AI může odborník na data vyvíjet řešení na pracovní stanici pomocí [azure Data Science Virtual Machine (DSVM)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) nebo [virtuálního počítače Azure s hloubkovým učením (DLVM)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/deep-learning-dsvm-overview?WT.mc_id=computeinmanufacturing-docs-ercenk) a vložit školení do clusteru. DSVM a DLVM jsou speciálně nakonfigurované image virtuálních počítačů s bohatou sadou předinstalované sady nástrojů a ukázek.

![](assets/bigcompute-assets/azurebatchai.png)

## <a name="conclusion"></a>Závěr

Výrobní odvětví vyžaduje velký počet matematických výpočtů, a to pomocí přístupu k špičkovým hardwarovým součástem, včetně grafických procesorů (GPU). Zásadní je škálovatelnost a pružnost platformy, která je hostitelem prostředků. Aby bylo možné řídit náklady, musí být k dispozici na vyžádání a současně poskytovat optimální rychlost.

Platforma Microsoft Azure poskytuje nejrůznější možnosti pro splnění těchto potřeb. Můžete začít od začátku, řídit každý prostředek a jeho aspekt pro sestavení vlastního řešení. Nebo můžete najít partnera Microsoftu, který vám urychlí vytváření řešení. Naši partneři znají, jak využít sílu Azure.

## <a name="next-steps"></a>Další kroky

- Nastavení cloudové pracovní stanice nasazením [virtuálního počítače řady NV](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu?WT.mc_id=computeinmanufacturing-docs-ercenk)

- Projděte si [Možnosti](https://docs.microsoft.com/azure/virtual-machines/linux/high-performance-computing?WT.mc_id=computeinmanufacturing-docs-ercenk) nasazení nástroje pro návrh, aby bylo možné využít výhody funkcí prostředí Azure HPC.

- Seznamte se s možnostmi [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/?WT.mc_id=computeinmanufacturing-docs-ercenk)
