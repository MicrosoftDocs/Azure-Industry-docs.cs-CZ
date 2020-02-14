---
title: Úvod do správy velkých objemů dat v maloobchodním průmyslu
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Maloobchodní prodejci mají velká úložiště dat nevyužitých dat, ze kterých můžou získat cenné poznatky. Tento článek popisuje, jak Microsoft Azure můžou efektivně využívat tato data.
ms.openlocfilehash: 198e0f609889eee86e005c5ee56090006ae2a413
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054112"
---
# <a name="data-management-in-retail-overview"></a>Přehled Správa dat v maloobchodě

## <a name="introduction"></a>Úvod

Data představují základ pro vývoj a poskytování lepších maloobchodních prostředí. Data se nacházejí v každé z aspektů maloobchodní organizace a dají se použít k extrakci přehledů v rámci tohoto řetězce hodnoty na provozní výkon a na chování zákazníků a také pro lepší využívání služeb. Z online režimu můžete přejít na sociální zapojení do nákupu v obchodě s daty abounds. Zachytávání dat je však pouze část správy dat. Spojování různorodých dat pro analýzu vyžaduje správné zpracování dat v celé organizaci. tím se zlepšuje schopnost maloobchodního prodeje dělat postižená rozhodnutí ohledně chodu jejich podnikání.

Například s růstem mobilního nákupu si zákazníci museli očekávat, že maloobchodní prodejci mají v zájmu zlepšení prostředí dostatečné množství údajů o svých nákupních zvyklostech. Příkladem případu použití je přizpůsobený produkt a nabídka propagace přímo do mobilního zařízení zákazníka při nákupu v určitém místě v rámci fyzického prodejního obchodu. Využití dat v tom, co, co, co, jak často a dalších vstupů, jako je třeba dostupnost produktu, vytváří příležitosti k posílání propagačních zpráv v reálném čase na zařízení zákazníka, když zákazník zakupuje v blízkosti cílené produktu. 

Efektivní využití dat může zákazníky aktivovat tak, že pomůže prodejci, který poskytuje relevantnější prostředí. maloobchodní prodejci například můžou zákazníkovi odeslat oznámení s kódem slevy na web elektronického obchodování maloobchodního prodejce.  Tato data dále provedou užitečné poznatky, ze kterých mohou vedoucí společnosti řídit své akce s rozhodováním na základě dat.

Akce pro nabídnutí povýšení je informována kombinací datových bodů a aktivovaná zákazníkem, který vstupuje do Storu. Schopnost vytvořit tato připojení a výsledné akce jsou založené na modelu správy dat, který vidíte níže.

![Tok procesu](./assets/retail-data-management-assets/process-flow.png)

Obrázek 1

Při načítání dat do Azure zvažte 3Ps zdrojů dat a jejich použitelnost do scénářů, které chce prodejce povolit. 3Psy zdrojů dat jsou koupené, veřejné a proprietární.

> **Zakoupená** data obvykle rozšiřují a zvyšují stávající data organizace s využitím trhu a demografických údajů, které doplňují dosah dat v rámci zachycení dat organizace. Maloobchodní prodejce si například může koupit další demografická data a rozšířit si hlavní záznam zákazníka a ujistit se, že je záznam přesný a úplný. 
>
> **Veřejná** data jsou volně dostupná a můžou se shromažďovat ze sociálních médií, prostředků státní správy (např. geografie) a dalších online zdrojů. Tato data můžou odvodit přehledy, jako jsou například klimatické vzorce, které korelují se vzory nákupu nebo sociální zapojení, které signalizují oblíbenou produkt mezi konkrétním geografickým. Veřejná data jsou často k dispozici prostřednictvím rozhraní API.
>
> **Vlastnická** data se nacházejí v organizaci. Může se jednat o místní systémy maloobchodního prodejce, aplikace SaaS nebo poskytovatele cloudu. Pro přístup k datům ve zprostředkovateli aplikace SaaS a dalších datech dodavatelů se rozhraní API obvykle používají ke komunikaci systému dodavatele. To zahrnuje data, jako jsou například protokoly webu elektronického obchodování, prodejní data POS a systémy správy inventáře.

Tyto různé datové typy se používají pro různé přehledy pocházející z kanálu správy dat.

## <a name="ingest"></a>Ingestování

Zpočátku se data načtou do Azure v nativním formátu a ukládají se odpovídajícím způsobem. Přijímání a Správa různorodých zdrojů dat může být těžké, ale Microsoft Azure nabízí služby pro rychlé a snadné načtení dat do cloudu a jejich zpřístupnění pro zpracování v kanálu správy dat. 

Azure má několik užitečných služeb pro migraci dat. Volba závisí na typu přenášených dat. [Migrace dat Azure](/azure/dms/dms-overview?WT.mc_id=retaildm-docs-dastarr) Služby pro SQL Server a [Služba Azure import/export](/azure/storage/common/storage-import-export-service?WT.mc_id=retaildm-docs-dastarr) jsou služby, které vám pomůžou získat data do Azure. Mezi další služby příchozího přenosu dat, které je potřeba zvážit, patří [Azure Data Factory](/azure/data-factory?WT.mc_id=retaildm-docs-dastarr) a [Azure Logic Apps](/azure/logic-apps/?WT.mc_id=retaildm-docs-dastarr) konektory. Každá z nich má své vlastní funkce a měla by se prozkoumat, aby se zjistilo, která technologie je pro danou situaci vhodná.

Přijímání dat není omezené na technologie společnosti Microsoft. Maloobchodníci můžou prostřednictvím [Azure Marketplace](https://azuremarketplace.microsoft.com?WT.mc_id=retaildm-docs-dastarr)nakonfigurovat mnoho různých databází dodavatelů v Azure, aby fungovaly se stávajícími místními systémy. 

V Azure není nutné uchovávat všechna data. Například data bodu prodeje (POS) se můžou uchovávat místně, aby výpadky Internetu neovlivnily prodejní transakce. Tato data je možné zařadit do fronty a odeslat je do Azure podle plánu (například noční nebo týdenní) pro použití při analýze, ale vždy považovat místní data za zdroj pravdy.

## <a name="prepare"></a>Příprava

Než začnete s analýzou, je potřeba připravit data. Tato tvarování dat je důležité pro zajištění kvality prediktivních modelů, vytváření sestav klíčových ukazatelů výkonu a relevanci dat.

Existují dva typy dat, které je potřeba adresovat při přípravě dat pro analýzu, strukturovaná a nestrukturovaná. Strukturovaná data je snazší řešit, protože jsou již vytvořená a formátovaná. Může vyžadovat jenom jednoduchou transformaci z strukturovaných dat ve zdrojovém formátu na strukturovaná data, která jsou připravená na úlohy analýzy. Nestrukturovaná data obvykle poskytují další výzvy. Nestrukturovaná data se ukládají ve formátu pevné délky záznamu. Mezi příklady patří dokumenty, kanály sociálních médií a digitální image a videa. Tato data se musí spravovat jinak než strukturovaná data a často vyžadují vyhrazený proces, aby se zajistilo, že tato data skončí ve správném úložišti dat, a to tak, že se budou řešit.

Datové tvarování probíhá během procesu extrakce-transformace-načítání (ETL) ve fázi přípravy. Data se extrahují z nezměněných zdrojů dat importovaných do Azure, vyčištěných nebo přeformátovaných podle potřeby a ukládají se do nového, více strukturovaného formátu. Běžná operace přípravy dat ETL slouží k transformaci souborů. csv nebo Excel do souborů Parquet, což je snazší pro systémy strojového učení, jako je například Apache Spark pro rychlé čtení a zpracování. Dalším běžným scénářem je vytvoření souborů XML nebo formátu JSON ze souborů. csv nebo jiných formátů. Výsledný formát je snazší použít s jinými analytickými moduly.

V Azure je k dispozici několik transformačních technologií jako služby ETL k přetvarování dat. Mezi možnosti patří [Azure Databricks](/azure/azure-databricks?WT.mc_id=retaildm-docs-dastarr), [Azure Functions](/azure/azure-functions/?WT.mc_id=retaildm-docs-dastarr) nebo Logic Apps. Datacihly jsou plně spravovaná instance Apache Spark a slouží k transformaci dat z jednoho formuláře do druhého. Azure Functions jsou funkce bez stavu (nebo bez serveru) s triggery, které je mají vyvolat a spouštějí kód. Logic Apps integruje služby.

## <a name="store"></a>Store

Uložení dat před provedením zpracování vyžaduje zvážení. Data mohou pocházet v strukturovaných nebo nestrukturovaných formátech a tvar dat často určuje jeho cíl úložiště. Například vysoce strukturovaná data můžou být vhodná pro Azure SQL. Méně strukturovaná data se můžou uchovávat v úložišti objektů blob, v úložišti souborů nebo v úložišti tabulek.

Data uložená v Azure mají skvělý výkon, který zálohuje služba SLA (Solid-level agreement). Služba Data Services usnadňuje správu řešení, vysoké dostupnosti, replikace napříč různými geografickými umístěními a – výše – Azure nabízí úložiště dat a služby potřebné k tomu, aby Machine Learning.

Strukturovaná i nestrukturovaná data mohou být uložena v [Azure Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=retaildm-docs-dastarr) a dotazována pomocí jazyka [U-SQL](/azure/data-lake-analytics/data-lake-analytics-u-sql-get-started?WT.mc_id=retaildm-docs-dastarr), dotazovacího jazyka specifického pro Azure Data Lake. Příklady dat, která mohou být zahrnuta v Data Lake, zahrnují následující, které jsou rozděleny do běžně strukturovaných a nestrukturovaných zdrojů dat.

### <a name="structured-data"></a>Strukturovaná data

- Data CRM a další obchodní aplikace
- Data transakcí POS
- Data snímače
- Relační data
- data transakce elektronického obchodování

### <a name="unstructured-data"></a>Nestrukturovaná data

- Informační kanály pro sociální sítě
- Video
- Digitální image
- Analýza navštívených webů

Existuje rostoucí počet případů použití, které podporují nestrukturovaná data, aby se generovala hodnota. To je poháněné přáním pro rozhodování na základě dat a technikou v technologii, jako je AI, aby bylo možné zachytávání a zpracování dat ve velkém měřítku. Data mohou například obsahovat fotografie nebo streamování videa. Například streamování videa se dá využít k detekci výběrů nákupu zákazníků pro bezproblémové rezervace. data katalogu produktů nebo můžete bez problémů sloučit s fotografií zákazníka jejich oblíbeného přípravku, aby bylo možné získat přehled o podobných nebo doporučených položkách. 

Mezi příklady strukturovaných dat patří datové kanály relační databáze, data senzorů, soubory Apache Parquet a data elektronického obchodování. Podkladová struktura těchto dat je vhodná pro Machine Learning kanál.

Služba Azure Data Lake také umožňuje provádět dávkové a interaktivní dotazy společně s analýzou v reálném čase pomocí [Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview?WT.mc_id=retaildm-docs-dastarr). Data Lake je také konkrétně vhodné pro velmi velké úlohy analýzy dat. Data v Data Lake jsou také trvalá a nemají žádný časový limit.

Další úložiště dat, jako jsou relační databáze, úložiště objektů blob, úložiště souborů Azure a Cosmos DB úložiště dokumentů, mohou také obsahovat čistá data připravená pro analýzu dat v kanálu správy dat. Neexistuje žádný požadavek, aby jeden používal Data Lake.

## <a name="analyze"></a>Analyzovat

V případě problémů, jako je snížení nákladů na inventář, můžou maloobchodníci využít analýzy prováděné procesem Machine Learning.

Analýza dat připravuje data pro zpracování prostřednictvím Machine Learningho stroje, aby bylo možné získat hlubší přehled o prostředí zákazníka. Tento proces vytvoří model, který je "znám" a může být použit pro budoucí data pro předpověď výsledků. Modely definují data, která se mají prozkoumat, a způsob, jakým se budou analyzovat data prostřednictvím různých algoritmů. Pomocí výstupních dat z analýzy s vizualizací dat můžete spustit vhled, jako je například nabídka kupónu v obchodě pro položku ze seznamu přání zákazníka v elektronického obchodování platformě maloobchodních prodejů.

Analýza dat probíhá dodávání ekosystémů s daty uloženými ke zpracování. Většinou se jedná o strojové učení, které provádí Hadoop, datacihly nebo samoobslužná instance Spark běžící na virtuálním počítači. To se dá udělat taky jednoduše dotazem na data. Vhled do klíčových ukazatelů výkonu je často možné najít v čistých datech, aniž byste museli procházet kanál strojového učení.

[Hadoop](/azure/hdinsight/hdinsight-hadoop-architecture?WT.mc_id=retaildm-docs-dastarr) je součástí plně spravované služby Azure [HDInsight](/azure/hdinsight/?WT.mc_id=retaildm-docs-dastarr). HDInsight je kolekce nástrojů pro učení dat, které se používají pro školení datových modelů, vkládání dat do datového skladu a provádění dotazů na Hadoop prostřednictvím dotazovacího jazyka pro podregistr. HDInsight dokáže analyzovat streamovaná nebo historická data.

V rámci školení a udržování datových modelů může být na data použita řada výukových algoritmů. Datový model explicitně určuje strukturu dat vytvořených pro analytiky.

Nejprve se data vyčistí a vytvoří správně. Pak je zpracován systémem strojového učení, jako je například HDInsight nebo Apache Spark. K tomu slouží existující data pro výuku modelu, který se zase používá k analýze dat. Školený model se pravidelně aktualizuje s novými známými dobrými daty, aby se zvýšila přesnost při analýze. Služby Machine Learning používají model k analýze zpracovávaných dat.

Po školení modelů a spuštění procesu analýzy dat můžete data odvozená z analýzy strojového učení ukládat do datového skladu nebo jako normalizované databáze úložiště pro analytická data. Microsoft poskytuje [Power BI](/power-bi/?WT.mc_id=retaildm-docs-dastarr)plně funkční nástroj pro analýzu dat pro rozsáhlou analýzu dat v datovém skladu.

## <a name="action"></a>Akce

Data v maloobchodě se přesunou nepřetržitě a systémy, které je zpracovávají, musí být tak včasné. Například elektronického obchodování data nakupujících musí být zpracovávána rychle. To znamená, že položky v košíku kupujícího lze použít k nabídnutí dalších služeb nebo doplňkových položek během procesu registrace. Tato forma zpracování a analýzy dat se musí nacházet téměř okamžitě a obvykle se provádí systémy vykonávajícími transakce "mikrodávkování". To znamená, že data se analyzují v systému, který má přístup k datům, která jsou už zpracovaná a běží přes model.

V pravidelných intervalech se můžou vyskytovat jiné operace Batch, ale nemusí se vyskytovat téměř v reálném čase. Když proběhne analýza dávky místně, tyto úlohy se často spouštějí v noci, na víkendech nebo v případě, že se prostředky nepoužívají. V případě Azure se může vyskytnout kdykoli i škálování velkých dávkových úloh a virtuálních počítačů potřebných k jejich podpoře.

Začněte tím, že použijete následující postup.

1. Vytvořte plán přijímání dat pro úložiště dat, která poskytují hodnotu pro provedení analýzy. Když zahájíte podrobnou synchronizaci dat nebo plán migrace, načtěte data do Azure v původním formátu. 

2. Určete, jaké užitečné poznatky potřebujete, a vyberte kanál zpracování dat, který bude vyhovovat činnostem zpracování dat. 

3. S těmito datovými funkcemi můžete vytvořit kanál zpracování dat pomocí vhodných algoritmů, abyste získali přehledy, které se snažíte získat.

4. Pokud je to možné, využijte pro výstup do datového skladu běžný datový model. To může vystavit nejzajímavější datové funkce. To obvykle znamená čtení dat v původních systémech úložiště Azure a zápis vyčištěné verze do jiného úložiště dat.

5. Zpracujte data prostřednictvím kanálů strojového učení, které poskytuje Spark nebo Hadoop. Pak výstup do datového skladu zapíšete do kanálu. Je k dispozici celá řada výchozích algoritmů pro zpracování dat nebo maloobchodní prodejci mohou implementovat své vlastní. Kromě scénářů ML načtěte data do standardního úložiště dat a vyvynuťte společný datový model a pak se podávají dotaz na data klíčového ukazatele výkonu. Data mohou být například uložená ve schématu hvězdičky nebo v jiném úložišti dat.

Díky tomu, že data jsou nyní připravena k používání analytikům dat, mohou být zjištěny užitečné poznatky a akce podniknuté pro zneužití tohoto nového vědomí. Například prodejní preference zákazníka mohou být načteny zpět do systémů maloobchodního prodejce a použity ke zlepšení několika zákaznických oslovení, například následujících.

- Zvyšte průměrnou transakci elektronického obchodování nebo POS díky sdružování produktů
- Historie nákupu v CRM pro podporu dotazů centra volání zákazníků
- Návrhy produktů přizpůsobené modulem doporučení pro elektronický obchod
- Cílené a relevantní reklamy na základě zákaznických dat
- Aktualizace dostupnosti inventáře na základě přesunu produktů v rámci dodavatelského řetězce

Další typ přehledu, který může nastat, jsou vzory, které se dřív nezpochybnily. Může se například zjistit, že se za hodinami 3:00 PM a 7:00 odp. dostává více ztrát inventáře. To může znamenat nutnost dalších dat k určení hlavní příčiny a postupu, jako je například vylepšení zabezpečení nebo standardní operační postupy.

## <a name="conclusion"></a>Závěr

Správa dat v maloobchodě je složitá. Nabízí ale cennou schopnost doručovat relevanci a vylepšené prostředí pro zákazníky. Pomocí technik v tomto článku můžete získat přehledy, které vám pomůžou zlepšit prostředí pro zákazníky, řídit ziskové obchodní výsledky a odhalit trendy, které můžou řídit provozní vylepšení.

Pokud chcete dál pochopit více možností Azure souvisejících s implementací kanálu správy dat, přečtěte si následující informace:

- Podívejte se, jak [Azure Data Factory](/azure/data-factory/?WT.mc_id=retaildm-docs-dastarr) může přispět k ingestování dat z místních úložišť dat do Azure.
- Přečtěte si další informace o tom, jak [Azure Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=retaildm-docs-dastarr) můžou sloužit jako úložiště pro všechna data, strukturovaná i nestrukturovaná.
- Podívejte se na téma skutečné maloobchodní sestavy znázorňující, jak [Power BI](https://powerbi.microsoft.com/en-us/industries/retail/?WT.mc_id=retaildm-docs-dastarr) může poskytnout hlubší přehled známých otázek, ale umožňuje analýzu trendů.
- Navštivte [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=retaildm-docs-dastarr) a najděte řešení kompatibilní s místně, která jsou už v místním prostředí.

_Tento článek vytvořily David Starr a Mariya Zorotovich._
