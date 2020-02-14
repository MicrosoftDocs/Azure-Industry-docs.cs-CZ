---
title: Analýza rizik v Gridech pomocí Azure Batch, Azure Data Lake
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Seznamte se s podnikovými požadavky na implementaci řešení rizikových sítí v bankovnictví v Azure.
ms.openlocfilehash: 746b93e545aa8ff61a8fab4a021b6c5caa1889bb
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053092"
---
# <a name="risk-grid-computing-in-banking-overview"></a>Přehled o rizikových tabulkách v bankovnictví

## <a name="introduction"></a>Úvod

Jedna z nejdůležitějších úloh v podnikovém financích a investičních bankách analyzuje riziko.

Aby bylo možné poskytnout ucelený přehled o rizikech spojených s investičním portfoliem, analytikům finančních rizik prověřit výzkum, monitorovat ekonomické a sociální podmínky, zůstat posuzovat předpisy a vytvářet počítačové modely investičního klimatu.

Analýza rizik napříč mnoha vektory, které ovlivňují portfolio, je dostatečně složitá, že je potřeba modelování počítačů. Většina analytiků stráví poměrně určitou dobu práce s modely počítačů, aby simulovala a předpovídá, jak se finanční podmínky změní. Při vyhodnocování rizik investic (tržní riziko, úvěrové riziko a provozní riziko) může být výpočetní zatížení zpracování predicativech modelů poměrně velké vzhledem k objemu a různorodým datům.

Cloud Computing nabízí významné výhody pro výpočetní nebo rizikové modelování v oblasti rizikových sítí, protože umožňuje analytikům přístup k obrovskému výpočetnímu prostředku na vyžádání, aniž by se musely účtovat kapitálové náklady nebo spravovat infrastrukturu. Tento článek prověřuje využití Microsoft Azure k rozšíření aktuálních výpočetních prostředků v oblasti rizikových sítí a optimalizaci nákladů a rychlosti výpočetních úloh v tabulce rizikových sítí. Mezi zahrnutá témata patří zabezpečené a spolehlivé připojení, dávkové zpracování a rozšíření výpočetních prostředků na základě poptávky, když jsou místní servery v kapacitě.

## <a name="grid-computing-services"></a>Služby grid computing

Analytici potřebují jednoduchý a spolehlivý způsob, jak poskytnout své modely do kanálu dávkového zpracování, který začíná přijímáním dat a toků prostřednictvím zpracování dat k analýze, kde je možné z výsledných dat odvozovat přehledy.

Vstupní data modelu rizik se nacházejí v několika formách, nejběžnějších souborů aplikace Excel nebo souborů. csv. Tyto soubory jsou často přestrukturované na formáty lépe vhodné pro zpracování modelu rizika v pozdějších fázích výpočetního kanálu rizik. Běžnou technikou analýzy a zpracování těchto souborů je dávkové zpracování s mřížkou virtuálních počítačů (virtuálních počítačů? WT. mc_id = gridbank-docs-dastarr) společně s cílem dosáhnout společného cíle.

[Azure Batch](/azure/batch/?WT.mc_id=gridbank-docs-dastarr) je služba Azure, která umožňuje souběžně běžet více pracovních virtuálních počítačů, jak je znázorněno níže. Zpracování datových souborů a odesílání výsledků do systémů strojového učení nebo úložišť dat jsou běžné úlohy pro pracovní uzly. Kód aplikace, který spouští pracovní uzly, je vytvořen zákazníkem, takže je v dávkové úloze možné provést téměř jakoukoli akci.

![Místní Batch](./assets/risk-grid-compute-assets/01-on-prem.png)

Azure poskytuje elegantní řešení pro výpočetní práci v oblasti rizikových sítí pomocí Azure Batch. Zákazníci můžou použít Azure Batch k rozšiřování stávající mřížky rizikových výpočtů nebo k nahrazení místních prostředků pomocí zcela cloudového řešení.

Připojení přímo a bezpečně ke cloudu Azure je plně podporované. Při připojování k datům, které jsou místně uložené v místní síti při připojování k Azure pomocí hybridní sítě, můžou uzly pracovních procesů mřížky služby Batch získat přístup k datům modelování. Zákazník může také nahrávat data do příslušného úložiště v rámci Azure a umožnit dávce přímý přístup k datům.

## <a name="secure-connectivity-to-azure"></a>Zabezpečené připojení k Azure

Při sestavování řešení pro práci s rizikovou sítí v Azure bude společnost často dál používat stávající místní aplikace, jako jsou obchodní systémy, řízení rizik v pobočce, analýza rizik atd. Azure se stal rozšířením těchto stávajících investic.

Při připojování ke cloudu je zabezpečení primárním aspektem. Účetnictví pro aktuální model zabezpečení je prvním krokem při přímém připojení k Azure. Pro zákazníky, kteří už používají službu Active Directory (AD? WT. mc_id = gridbank-docs-dastarr) v místním prostředí, které umožňuje připojení k Azure využít stávající prostředky identity. Účty služeb můžou být živé v místní službě AD.

### <a name="hybrid-network-solution"></a>Řešení hybridní sítě

Hybridní síť spojuje Azure přímo s místní sítí zákazníka&#39;. Azure nabízí dva modely pro bezpečné a spolehlivé připojení současných místních systémů k Azure, [Microsoft Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbank-docs-dastarr) a [VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbank-docs-dastarr). Obojí jsou řešení pro důvěryhodné připojení, i když existují rozdíly v implementaci, výkonu, nákladech a dalších atributech.

![Connectivty Azure](./assets/risk-grid-compute-assets/02-connectivity.png)

&quot;nárůstu na Cloud&quot; přesměruje výpočetní úlohy do cloudových počítačů, když se v nich nachází špička&#39;stávajících prostředků, rozšiřuje se o datový nebo privátní cloudové prostředky zákazníka s. Použití modelu hybridní sítě umožňuje snadnou zátěžový scénář v cloudu, protože cloudové výpočetní prostředí je jednoduché rozšíření stávající sítě.

Existuje několik konfigurací připojení k síti nad rámec těch v jednoduchém modelu, které jsou uvedeny v logické architektuře výše. Pomoc s rozhodováním a pokyny pro architekturu týkající se připojení sítě k Azure najdete v článku [_připojení místní sítě k Azure_](/azure/architecture/reference-architectures/hybrid-networking/).

### <a name="rest-api-solution-over-internet"></a>REST API řešení přes Internet

Alternativou k vytvoření hybridní sítě je nahrání dat do Azure Storage (soubory nebo úložiště objektů BLOB jsou nejspíš kandidáti? WT. mc_id = gridbank-docs-dastarr) a služba Batch si přečte datové soubory z úložiště. K tomu je možné využít zabezpečenou (SSL? WT. mc_id = gridbank-docs-dastarr) připojení pro připojení k Azure, ukládání dokumentů v Azure Storage a následnou správu výpočetních úloh z oblasti rizik prostřednictvím [služby Batch REST API](/rest/api/batchservice/?WT.mc_id=gridbank-docs-dastarr) nebo SDK s využitím aplikace s podporou pro konkrétní účely, orchestrace dávkového spuštění.

### <a name="azure-data-factory"></a>Azure Data Factory

Dalším řešením pro váš scénář může být použití [Azure Data Factory](/azure/data-factory/?WT.mc_id=gridbank-docs-dastarr)cloudová služba pro integraci dat, která umožňuje vytvářet rozsáhlé kanály úložiště, přesunu a zpracování. Data je možné nahrávat na vyžádání prostřednictvím kanálu Data Factory. Služba poskytuje vizuálního návrháře v Azure Portal pro sestavování extrakce, transformace a načítání (ETL? WT. mc_id = gridbank-docs-dastarr) řešení v Azure. Data Factory může přispět k ingestování dat do Azure pro další zpracování.

## <a name="matching-processing-needs-with-demand"></a>Vyhovující požadavky na zpracování s poptávkou

Při výpočtu rizika, ať už denně nebo u těžšího zatížení na konci měsíce, výpočty spotřebovávají významné výpočetní prostředky. Tyto výpočty nefungují nepřetržitě. V případě, že se výpočty rizik nespouštějí v místní mřížce, společnost bude mít cenné a nákladné servery, na kterých běží bez zatížení, ale s průběžnými náklady na výkon, chlazení a prostor Datacenter a také s dalšími pevnými náklady.

### <a name="augmenting-on-premises-grid-with-azure-batch"></a>Rozšíření místní mřížky pomocí Azure Batch

Pro minimalizaci nákladů se může stát, že se podnik rozhodne vlastnit a spravovat jen tolik pracovních uzlů, aby splňovaly požadavky, když je poptávka nízká. Na vysoce výkonné servery v Azure se pak můžou na vysoce výkonných serverech připínat náročné úlohy s vysokým výkonem v Azure, které umožňují Elastické škálování směrem nahoru a dolů s požadavky úloh

Model zpracování Azure Batch má několik výhod pro výpočetní řešení v oblasti rizikových sítí:

- Rozšiřuje stávající investice do různých místních systémů.
- Umožňuje stávající infrastruktuře obsluhovat potřeby analýzy rizik v případě nízké úrovně požadavků a naruší pracovní uzly založené na Azure.
- Pokud je poptávka vysoká, poskytuje dodatečnou kapacitu výpočetním mřížce rizik.
- Umožňuje porovnávací profily počítačů ke výpočetnímu výkonu vyžadovanému úlohou Batch, a to i v případě, že zátěž volá konfigurace pro prostředí [HPC (High Performance Computing)](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr) .

Běžným řešením je automatické přidávání pracovních uzlů v Azure, když se používají místní zaměstnanci. Hlavní uzel v mřížce rizika se jednoduše zeptá na další pracovní procesy. Tím se automaticky škáluje počet uzlů pracovních procesů v Azure a umožňuje řešení pro elastickou poptávku.

![Hybridní cloud](./assets/risk-grid-compute-assets/03-hybrid-cloud.png)

Kromě efektivního využívání prostředků toto uspořádání poskytuje další výhody. V případě nezávislých úloh přidávání dalších pracovníků umožňuje lineárně škálovat zatížení. Azure také poskytuje flexibilitu pro vyzkoušení velmi velké instance virtuálních počítačů nebo počítače s několika kartami GPU. Tato flexibilita umožňuje experimentování a inovace.

V případě, že potřebujete větší výpočetní kapacitu, jako je například čtvrtletní oceňování, může další kapacita také pocházet z Azure Batch automatické škálování. Automatické škálování zajišťuje pružnost pro vaše řešení Batch. Díky škálování prostředků tak, aby odpovídaly požadovanému zatížení, poskytuje Azure podstatně větší kapacitu s nižšími náklady než vlastnící hardware.

Většina komerčních produktů v Gridech podporuje určitou formu shlukování do cloudu a umožňuje snazší kontrolu konceptu pro zatížení analýzy rizik. [Sada Microsoft HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr) může například běžet v Azure, stejně jako produkty společnosti, jako jsou TIBCO, Univa a další. Mnohé z těchto nástrojů nebo systémů třetích stran jsou k dispozici prostřednictvím [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?WT.mc_id=gridbank-docs-dastarr).

### <a name="migrating-additional-resources-to-the-cloud"></a>Migrace dalších prostředků do cloudu

Díky nárůstu nebo místnímu výpadku infrastruktury Datacenter můžou organizace přesunout své celé dávkové zpracování do Azure v oblasti řízení rizikových sítí.

### <a name="growing-into-azure"></a>Rozšiřování do Azure

Když jsou místní počítače na konci životnosti, můžete pracovní uzly dále distribuovat do cloudu. Totéž může být pro hlavní uzel dávky true. Tím se obrátí vztah mezi místní sítí a Azure. To může být příležitost snížit náklady vyřazením jakýchkoli produktů do sítě, jako je Azure ExpressRoute, a všech zbývajících místních pracovních uzlů.

V rámci této změny můžou být data dostupná pro Azure s využitím různých technik pro vstupní soubory. Azure nabízí spoustu možností úložiště, ze kterých si můžete vybrat, včetně koncových bodů REST, aby bylo možné přímo nahrávat data. nemusíte přitom mít výpočetní úlohy v místní síti.

 ![Místní Batch](./assets/risk-grid-compute-assets/04-batch-process.png)

V rámci tohoto modelu můžou být v cloudu všechny aktivity výpočetních sítí v oblasti rizika. Datové soubory zpracovávané pracovními procesy mohou být uloženy v úložišti Azure, data lze dodávat přímo do Azure Data Lake a Azure HDInsight se může postarat o potřeby strojového učení. A konečně, Power BI a Azure Analytics jsou skvělé nástroje pro analýzu dat a můžou fungovat napříč všemi daty uloženými v Azure.

### <a name="data-security-considerations-for-risk-grid-computing"></a>Požadavky na zabezpečení dat pro výpočty v oblasti rizikových sítí

I když data výpočtů často neobsahují žádné identifikovatelné osobní údaje (PII)? WT. mc_id = gridbank-docs-dastarr), většina bank stále před umístěním jakékoli úlohy do cloudu vyhodnotí bezpečnostní riziko. Toto posouzení může vyžadovat vstup od Microsoftu a může mít za následek doporučení zabezpečení.

Významnou možností pro výpočetní práci v oblasti rizikových sítí je [spuštění dávkových procesů v rámci virtuální sítě Azure](/azure/batch/batch-virtual-network?WT.mc_id=gridbank-docs-dastarr). To umožňuje, aby výpočetní uzly fondu bezpečně komunikovaly s jinými výpočetními uzly nebo s místní sítí. Příslušné účty služeb a skupiny síťových služeb (NSG) by měly být vytvořeny a používány výpočetními uzly služby Batch. [Azure má také řešení](/azure/security/blueprints/financial-services-regulated-workloads?WT.mc_id=gridbank-docs-dastarr) pro šifrování dat při přenosu a v klidovém provozu v Azure Storage.

Mezi některé oblasti, které je potřeba zvážit, může být: služby Active Directory (AD) nebo výpočetní uzly nepřipojené k AD (pro uzly Windows serveru? WT. mc_id = gridbank-docs-dastarr), [šifrování disku virtuálního počítače](/azure/security/azure-security-disk-encryption?WT.mc_id=gridbank-docs-dastarr), zabezpečení vstupních vstupních a výstupních dat, která jsou v klidovém provozu a v přenosu, konfigurace sítě Azure, oprávnění a další. Ověřování se dá na úrovni REST API zpracovat taky prostřednictvím tajného klíče.

## <a name="getting-started"></a>Začínáme

Mnoho zákazníků má interní výpočetní tabulku, kterou už používají. Pokud vaše společnost vytvořila mřížku interně, zvažte Azure Batch, abyste mřížku rozšířili. Dobrým místem, kde začít s Azure Batch, je rozšířením aktuálního místního řešení tím, že se replikuje aktuální zpracování logiky aplikace a spustí se jako úloha služby Batch v Azure. To může vyžadovat síťové řešení pro připojení Azure Batch výpočetních uzlů do místní sítě v závislosti na funkcích aplikace&#39;.

Pokud chcete zmírnit problémy se zabezpečením, rychlostí a spolehlivostí připojení, zvažte připojení místní sítě k Azure pomocí Azure ExpressRoute nebo VPN Gateway. Odtud můžete mít místní hlavní uzel, který zřídí cluster pracovních uzlů založených na platformě Azure a rozpíná je podle potřeby.

Nakonec možná budete připraveni na úplnou migraci výpočetní infrastruktury rizik do Azure. Pokud se jedná o tento případ, [tady je článek](/azure/batch/?WT.mc_id=gridbank-docs-dastarr) , který vám umožní začít ještě dnes.

## <a name="technologies-presented"></a>Prezentované technologie

[Azure Batch](/azure/batch/?WT.mc_id=gridbank-docs-dastarr) umožňuje rozšířit místní rizikové pracovní uzly, aby dynamicky poskytovaly výpočetní prostředky na základě poptávky.

[Azure datalake](/azure/data-lake-store?WT.mc_id=gridbank-docs-dastarr) poskytuje úložiště, zpracování a analýzu napříč daty analýzy rizik.

[Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbank-docs-dastarr) rozšiřuje vaši místní síť do Azure prostřednictvím privátního připojení, které usnadňuje poskytovatel připojení.

[Azure HDInsight](/azure/hdinsight/?WT.mc_id=gridbank-docs-dastarr) je plně spravovaná Open Source analytická služba pro zpracování obrovských objemů dat, jako jsou data poskytovaná v rámci služby Batch na konci v měsících.

[Microsoft HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr) umožňuje zřídit vysoce výkonné výpočetní clustery pro dávkové zpracování.

[Power BI](/power-bi/?WT.mc_id=gridbank-docs-dastarr) je sada nástrojů pro obchodní analýzy, kterou analytiky rizik využívají k získání a sdílení přehledů.

[VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbank-docs-dastarr) rozšiřuje vaši místní síť do cloudu Azure prostřednictvím Internetu.

## <a name="conclusion"></a>Závěr

Řešení, která jsou popsaná v tomto článku, jsou přístupy k práci s rizikovou sítí v bankovnictví. Jiné architektury se dají použít pro nejrůznější funkce produktů a služeb Azure a různé stávající architektury klientských systémů. I tak služby Batch poskytují přiměřený model pro výpočetní prostředí pro rizikové sítě s ohledem na výhody stanovené v tomto článku.

[Rozšíření místní sítě do Azure](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbank-docs-dastarr) umožňuje snadný přístup k síťovým prostředkům a dalším systémům zpracování, které už v místní síti existují. Když se místní počítače blíží ke konci životnosti, může být vhodnější využít službu Batch COMPUTE výhradně v Azure místo podpory hybridního modelu.

Nahrávání souborů do Azure Storage před zahájením úlohy dávky je dalším způsobem, jak využít službu Batch bez nutnosti hybridní sítě. To může být provedeno přírůstkově nebo jako spouštěcí proces dávkového spuštění.

Po výběru strategie připojení je logické místo, kde začít s výpočetními riziky, umístění stávajících úloh do uzlů výpočetních pracovních procesů Azure a jejich spuštění v testovacím prostředí, abyste viděli, jestli je potřeba změnit nějaký kód. [Tento článek poskytuje výchozí bod](/azure/batch/batch-virtual-network?WT.mc_id=gridbank-docs-dastarr) pro zahájení práce s Azure Batch v jazyce nebo nástroji podle vašeho výběru.

[Průvodce pro řešení rizikové sítě v tabulce s bankovním řešením](/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=banking-docs-dastarr)
