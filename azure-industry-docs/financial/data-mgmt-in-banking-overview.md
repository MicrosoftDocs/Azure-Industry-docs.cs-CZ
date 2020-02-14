---
title: Přehled – Správa Estates dat pro banky se službami Azure
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Popisuje techniky správy dat ve spravovaném bankovním prostředí pomocí Microsoft Azure.
ms.openlocfilehash: cfafd5242b6da994def2fe1470db9fd86b2675e7
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052922"
---
# <a name="data-management-in-banking-overview"></a>Přehled Správa dat v bankovnictví

## <a name="introduction"></a>Úvod

V rámci svých bran firewall, kteří mají v současnosti odpovědnost za zabezpečení a ukládání obrovských objemů cenných informací, jak na jejich zákazníky, tak i o přesunu finančních finančních prostředí. V mnoha případech se tyto informace nepoužívají, protože nejsou snadno dostupné nebo prohledávatelné, i když použití dat může zlepšit rozhodování napříč více bankovními aktivitami.

Pomocí těchto dat můžou banky najít informace rychleji o tom, kdo je ve výchozím nastavení na půjčku, a rozhodnout se, které úpravy ocenění portfolia na trhu potřebujete. V bankách může být také jasné, jak jsou jejich data uložena a spravována, aby splňovala zákonné požadavky, aby bylo možné data využít, uchovávat, archivovat nebo odstranit, aby splňovala předpisy.

S tisíci rozhodnutími, Velká a malá, která vyžaduje splnění všech každodenních požadavků na funkce, budou data stále důležitější. Kromě toho, že tyto přísné zákonné požadavky a povinnosti finančních trestných činů, vyžadují, aby banky byly schopné auditovat výsledky jakéhokoli procesu analýzy dat, a to až do počátečních informací, které se budou do úložiště dat nakládat. Sledovatelnost vyžaduje průhlednost z příjmu až po vytvoření dat, která umožňují zpracování.

Aby bylo možné spravovat spoustu účtů nebo podniků, které spravují banky, potřebují, aby měli smysl všechna tato data rychle a nákladově efektivně. Vzhledem k tomu, že se jedná o digitálně vyspělé banky, množství dat a příležitosti k jejich využití novými způsoby se exponenciálně zvětšují a umožňují bankám vydávat nové obchodní modely a oblasti příležitostí orientovaných na zákazníky.

Vhodnými strategiemi úložiště dat je klíč k provozní efektivitě, dobrý výkon aplikací a dodržování předpisů. Strategie úložiště dat je také počáteční lynchpin při získávání dat do formátů, kde je lze použít pro business intelligence a užitečné poznatky.

Následuje společný vzor správy dat:

![Tok správy dat](./assets/data-mgmt-in-banking-overview/data-mgmt-00.png)

V tomto modelu "Data Services" popisuje jakoukoli transformaci, připojení k datům nebo jiné jiné operace než archivace dat. Jedná se o klíčovou aktivitu potřebnou k využití dat, která vám pomůžou dělat podrobnější rozhodnutí.

Všechny banky a finanční instituce ingestují, přesouvá a ukládají data. Tento článek se zaměřuje na přenesení dat do Azure a přesunutí z tradičních místních úložišť dat, zpracování, archivace a odstranění. Po přesunu dat do Azure mohou banky a finanční instituce využít základní výhody, včetně těchto:

- Pomocí výpočetních prostředků a kapacity dat jenom tehdy, když a tam, kde je to potřeba, se řízení nákladů provádí efektivně bez omezení globálního škálování.

- Snížení nákladů na kapitál a náklady na správu prostřednictvím vyřazení fyzických serverů v místním prostředí.

- Integrované zálohování a zotavení po havárii, což snižuje náklady a složitost ochrany dat.

- Automatizovaná archivace studených dat do úložiště s nízkými náklady a zároveň zajišťuje splnění potřeb dodržování předpisů.

- Přístup k pokročilým a integrovaným datovým službám pro zpracování dat pro učení, předpovědi, transformaci nebo jiné potřeby.

Tento článek poskytuje doporučené postupy pro zajištění efektivního příchozího přenosu dat do Azure a základních techniků správy dat, které se použijí, když jsou v cloudu.

## <a name="data-ingest"></a>Přijímání dat

Finanční instituce budou mít data, která již byla shromážděna a používána aktuálními aplikacemi. Tato data můžete do Azure přesunout několika způsoby. V mnoha případech se stávající aplikace mohou připojit k datům v Azure, jako kdyby byly v místním prostředí, s minimálními změnami těchto existujících aplikací. To platí hlavně při použití Microsoft [Azure SQL Database](/azure/sql-database/?WT.mc_id=bankdm-docs-dastarr), ale prostřednictvím [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/databases?WT.mc_id=bankdm-docs-dastarr) řešení najdete pro Oracle, TeraData MongoDB a další.

Existují různé strategie migrace dat pro přesun dat z místního prostředí do Azure a mají různé stupně latence. Všechny techniky, na které se odkazuje níže, poskytují transparentnost a spolehlivé zabezpečení dat.

### <a name="virtual-network-vnet-service-endpoints"></a>Koncové body služby Virtual Network (VNet)

Zabezpečení je zásadním problémem při obchodování s finančními informacemi zákazníků.
Zabezpečení prostředků (například databáze) v rámci Azure často závisí na nastavení síťové infrastruktury v rámci samotného Azure a následném přístupu k této síti přes konkrétní koncový bod.

Před přenosem dat do Azure je vhodné zvážit síťovou topologii zabezpečení prostředků Azure a připojení k nim z místního prostředí.
[Virtual Network (virtuální síť? WT. mc_id = bankdm-docs-dastarr) koncové body služby](/azure/virtual-network/virtual-network-service-endpoints-overview?WT.mc_id=bankdm-docs-dastarr) poskytují zabezpečené přímé připojení k virtuální síti definované v Azure.

Virtuální sítě jsou v Azure definované tak, aby obsahovaly prostředky Azure v rámci vázané virtuální sítě. Koncový bod pro tuto virtuální síť pak umožňuje zabezpečený přístup k důležitým prostředkům služby Azure a jenom pro ty, které jsou v definované virtuální síti.

## <a name="database-lift-and-shift"></a>Přezvednutí databáze a posunutí

Jeden z nejběžnějších scénářů použití Azure SQL Database je modelem "výtah a Shift" migrace databáze. Výtah a posun jednoduše znamená přebírání stávajících místních databází a jejich přesun přímo do cloudu. K tomu patří následující důvody:

- Pohyb z aktuálního datového centra, kde jsou ceny vyšší nebo jiné provozní důvody
- Aktuální místní SQL Server databázovému hardwaru vyprší platnost nebo se blíží ukončení životního cyklu.
- Podpora Obecné strategie "přesun ke cloudu" pro společnost
- Využijte možnosti dostupnosti a zotavení po havárii SQL Azure.

V případě menších databází se první krok příjmu dat obvykle vytváří úložiště dat a struktury (například tabulky) potřebné prostřednictvím webu Azure Portal, Azure CLI nebo Azure SDK. U těchto menších úložišť dat může být další postup proveden vlastní aplikací vytvořenou ke zkopírování správných dat do příslušného úložiště dat Azure. Pro větší migrace dat obnovení záloh v Azure je obvykle nejrychlejší trasa.

Existuje mnoho způsobů, jak bezpečně přenášet data a rychle je do Azure. [V tomto článku najdete](/azure/architecture/data-guide/scenarios/data-transfer?WT.mc_id=bankdm-docs-dastarr) některé standardní techniky s výhodami a nevýhodami každého z nich.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

Při zdvihání a přesunu SQL Server databází [Microsoft Azure Database Migration Service](/azure/dms/dms-overview?WT.mc_id=bankdm-docs-dastarr) možné použít k přesunu databází do Azure. Služba používá [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?WT.mc_id=bankdm-docs-dastarr) , aby se zajistilo, že vaše místní databáze bude kompatibilní s funkcemi nabízenými v Azure SQL. Všechny změny, které vyžaduje migrace databáze, jsou až na vás. Dále používání služby vyžaduje internetové připojení typu Site-to-site mezi místní sítí a Azure.

### <a name="bulk-copy-program-for-sql-server"></a>Program hromadného kopírování pro SQL Server

Pokud je SQL Server ještě dnes a cílem je přejít na SQL Azure, další skvělou technikou je použití SQL Server Management Studio [a nástroje BCP k přesunu dat](https://azure.microsoft.com/blog/bcp-and-sql-azure/?WT.mc_id=bankdm-docs-dastarr) do SQL Azure. Po skriptování a vytváření databází SQL Azure z původního místního serveru je možné pomocí BCP rychle přenášet data do SQL Azure.

### <a name="blob-and-file-storage"></a>BLOB a úložiště souborů

Jednotlivé pobočky banky mají často své vlastní úložiště souborů na místních serverech. To může způsobit problémy se sdílením souborů mezi větvemi a mít za následek, že pro daný soubor není k dispozici žádný samostatný zdroj pravdy. I horší, tato instituce může mít "oficiální" úložiště souborů, které se větví přidělí, ale má přerušované připojení nebo jiné problémy s přístupem ke sdílené složce souborů.

Azure má služby, které pomáhají tyto problémy zmírnit. Přesunutí těchto dat do Azure poskytuje jeden zdroj pravdy pro všechna data a univerzální přístupnost úložiště s centralizovanými oprávněními a řízení přístupu.

Různá řešení pro ukládání dat můžou být vhodnější pro konkrétní formáty dat.
Například data uložená místně v SQL Server jsou nejspíš nejlépe vhodná pro Azure SQL. Data uložená ve formátu CSV nebo excelové soubory jsou nejspíš nejlépe vhodná pro úložiště [objektů blob Azure](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=bankdm-docs-dastarr) nebo [Azure Files](/azure/storage/files/storage-files-introduction?WT.mc_id=bankdm-docs-dastarr) , které je implementované na BLOB Service.

Téměř všechna data, která se přenášejí do Azure, procházejí prostřednictvím služby Blob Storage jako některé součásti přesunu dat. Úložiště objektů BLOB má následující pilíře.

- Trvalé & k dispozici
- Zabezpečený & kompatibilní
- Spravovatelná & nákladová efektivita
- Škálovatelná & výkonná
- Otevření & interoperabilní

Propojení všech větví se stejnou sdílenou složkou v Azure se často provádí prostřednictvím stávajícího datacentra banky, jak je znázorněno na obrázku 1. Podnikové datové centrum se připojuje k souborům úložiště prostřednictvím připojení SMB (Server Message Block).
Logicky a z hlediska sítě lokality může být sdílená složka v podnikovém datovém centru a může být připojena jako jakákoli jiná síťová sdílená složka.
Při použití této techniky se data zašifrují v klidovém stavu a během přenosu mezi datovým centrem a Azure.

![Sdílení logických souborů](./assets/data-mgmt-in-banking-overview/data-mgmt-01.png)

Obrázek 1

Podniky často používají úložiště souborů pro konsolidaci a zabezpečení velkých objemů souborů. To umožňuje vyřazení starých souborových serverů nebo opětovného purposingí hardwaru.
Další výhodou přesunu do úložiště souborů je Centralizace služeb správy dat a obnovení.

### <a name="azure-data-box"></a>Azure Data Box

Banky budou často mít terabajty, pokud nejsou petabajty, informace, které se mají převést do Azure. Úložiště dat donovanovo v Azure jsou velmi elastická a vysoce škálovatelná.

Služba, která se zaměřuje na migraci velmi rozsáhlých objemů dat do Azure, je [Azure Data box](https://azure.microsoft.com/services/storage/databox/?WT.mc_id=bankdm-docs-dastarr). Tato služba je určená k migraci dat bez přenosu dat nebo záloh přes připojení Azure. Vhodný pro terabajty dat, Azure Data Box je zařízení, které je možné objednat na webu Azure Portal. Je dodávána do vašeho umístění, kde může být připojena k vaší síti a načtena s daty prostřednictvím standardních protokolů NAS a zabezpečena prostřednictvím šifrování standard256-AES. Jakmile se data na zařízení nacházejí, doplní se do datového centra Azure, kde se data v Azure dehydratované.
Zařízení se pak bezpečně vymaže.

## <a name="azure-information-protection"></a>Azure Information Protection

Azure Information Protection (AIP) je cloudové řešení, které pomáhá organizacím klasifikovat, označovat a chránit dokumenty a e-maily. Můžete to udělat automaticky správci, kteří definují pravidla a podmínky, ručně podle uživatelů nebo kombinaci, kde se uživatelům přidávají doporučení.

## <a name="data-services"></a>Datové služby

Banka bojovat s hlavními Správa daty, která je v konfliktu s různými základními bankovními systémy, a data pocházející ze systémů pro nakládání, systémů zprovoznění, systémů pro správu, systémů CRM a dalších zdrojů. Azure obsahuje nástroje, které pomáhají zmírnit tyto a často vyskytující se problémy s daty.

K dispozici je mnoho organizací finančních služeb, které je potřeba provést na svých datech. Při zápisu dat do úložišť dat Azure může být potřeba tato data transformovat nebo je připojit k dalším datům, která rozšiřují, co se ingestuje.

### <a name="azure-data-factory"></a>Azure Data Factory

[Microsoft Azure Data Factory](/azure/data-factory/introduction?WT.mc_id=bankdm-docs-dastarr) je plně spravovaná služba, která vám umožní při příchozím, zpracování a monitorování přesunu dat do kanálu Data Factory. Data Factory aktivity tvoří strukturu kanálu správy dat.

Data Factory umožňuje transformaci nebo rozšíření dat při jejich toku do Azure a mezi ostatními službami Azure. Data Factory je spravovaná cloudová služba, která je sestavená pro komplexní hybridní projekty extrakce, transformace a načítání (ETL), extrakce, načítání a transformace (ELT) a integrace dat.

Data mohou být například předávány do analytických kanálů nebo nástrojů, jejichž výsledkem jsou užitečné poznatky. Data se můžou přesměrovat do řešení strojového učení nebo je transformovat na jiný formát pro pozdější zpracování dat. Příkladem je převod souborů. csv na soubory Parquet, které jsou vhodnější pro systémy strojového učení, a uložení těchto Parquet souborů do úložiště blogu.

Data můžete také odeslat do výpočetních služeb, jako je například [Azure HDInsight](/azure/hdinsight/hadoop/apache-hadoop-introduction?WT.mc_id=bankdm-docs-dastarr), [Spark](/azure/hdinsight/spark/apache-spark-overview?WT.mc_id=bankdm-docs-dastarr), [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview?WT.mc_id=bankdm-docs-dastarr)a [Azure Machine Learning](/azure/machine-learning/?WT.mc_id=bankdm-docs-dastarr). To umožňuje přímo dodávat systémy, které vedou k analýzám a inteligentním sestavám. Jeden společný model pro příchozí datové přenosy je znázorněný na obrázku 2 níže. Data se uchovávají ve společném [Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=bankdm-docs-dastarr) , které budou používat služby pro podřízenou analýzu.

![Data Lake model ingestování](./assets/data-mgmt-in-banking-overview/data-mgmt-02.png)

Obrázek 2

Kanály Data Factory se skládají z aktivit, které přebírají a výstupují datové sady. Aktivity lze sestavovat do kanálu, který definuje, kam chcete data získat, jak je chcete zpracovat a kam chcete výsledky uložit. Vytváření kanálů s aktivitami je srdcem Data Factory a sestavování vizuálního pracovního postupu přímo na webu Azure Portal usnadňuje vytváření kanálů. [Úplný seznam aktivit najdete tady](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=bankdm-docs-dastarr) .

### <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/?WT.mc_id=bankdm-docs-dastarr) je spravovaná analytická platforma založená na Apache Spark v Azure. Je vysoce škálovatelné a Sparkové úlohy se v případě potřeby spouštějí v clusterech počítačů. Datacihly fungují na poznámkovém bloku, který poskytuje jediné místo spolupráce mezi odborníky přes data, datovými inženýry a obchodními analytiky.

Datacihly jsou logické zpracování kanálu, když je potřeba transformace nebo analýza dat. Dá se doplňovat přímo Data Factory pro scénáře strojového učení, kde je čas na přehled důležitý nebo pro jednoduché transformace souborů.

![Databricks](./assets/data-mgmt-in-banking-overview/data-mgmt-03.png)

## <a name="archiving-data"></a>Archivace dat

Když už data v aktivním úložišti dat nepotřebujete, je možné je archivovat kvůli dodržování předpisů nebo záznamům pro audit v souladu se stavem a místními předpisy pro bankovnictví. Azure má dostupné možnosti pro ukládání zřídka používaných dat. Často dochází k problémům s ochranou osobních údajů, které vyžadují uchovávání dat v úložišti pro roky.

Náklady na ukládání dat můžou být vysoké, zejména při ukládání do místních databází. K těmto databázím se někdy používá nečasto a jenom zápis nových archivovaných dat nebo identifikátorů RID databáze dat, která už v archivu nepotřebujete.
Nečastý přístup k místním počítačům znamená vyšší celkové náklady na vlastnictví hardwaru.

### <a name="azure-archive-storage"></a>Archivní úložiště Azure

Pro nestrukturovaná data, jako jsou soubory nebo Image, Azure nabízí [několik úrovní úložiště](/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=bankdm-docs-dastarr) pro Blob Storage, včetně horké, studené a archivní. Úroveň Hot Access je určena pro data, která jsou aktivní a očekává se, že budou být nejvíce prováděná a použitá v aplikacích. Studená úroveň přístupu je určena pro krátkodobé zálohování a datové sady pro zotavení po havárii a také pro data dostupná pro aplikaci, ale je zřídka přístupná. Archivní úroveň má nejnižší náklady a je určena pro data, která jsou v režimu offline.

Data archivní vrstvy lze znovu použít do studené nebo horké vrstvy, ale dokončení této akce může trvat několik hodin. Archivní úložiště může být vhodné, pokud k datům nebudete mít k dispozici minimálně 180 dní. Když je objekt BLOB v archivním úložišti, nedá se číst, ale můžou se provádět jiné stávající operace, jako je například seznam, odstranění a načtení metadat. Datová vrstva archivu má nejméně nákladnou datovou vrstvu pro úložiště objektů BLOB.

### <a name="azure-sql-database-long-term-retention"></a>Azure SQL Database dlouhodobé uchovávání

Při používání Azure SQL existuje [dlouhodobá služba pro uchovávání](/azure/sql-database/sql-database-long-term-retention?WT.mc_id=bankdm-docs-dastarr) záloh pro ukládání záloh do deseti let. Uživatelé můžou naplánovat uchovávání záloh pro dlouhodobé ukládání, takže záloha bude zachovaná pro týdny, měsíce nebo dokonce i roky.

Chcete-li obnovit databázi z dlouhodobého úložiště, vyberte konkrétní zálohu na základě jejího časového razítka. Databázi můžete obnovit na existující server v rámci stejného předplatného, jako má původní databáze.

## <a name="deleting-unwanted-data"></a>Odstraňování nežádoucích dat

Aby zůstala v souladu s bankovními předpisy nebo zásadami týkajícími se uchovávání dat, je potřeba data často odstranit, pokud už nechcete. Před implementací technického řešení pro tato nepotřebná data je důležité mít plán vyprázdnění tak, aby byl schválený na základě zásad. Data je možné v Azure kdykoli odstranit z archivu nebo jiných úložišť dat.

Efektivní strategií pro odstraňování nežádoucích dat je udělat v intervalu, nočním nebo týdenním nejběžnějším. K provedení této úlohy je možné zapsat [čas aktivovaný službou Azure Functions](/azure/azure-functions/functions-bindings-timer?WT.mc_id=bankdm-docs-dastarr) . Pokud odstraníte všechna data, Microsoft Azure data odstraní, včetně všech uložených v mezipaměti nebo záložních kopií.

## <a name="getting-started"></a>Začínáme

Existuje mnoho způsobů, jak začít na základě aktuálního využití a zralosti datových modelů, které se v současnosti používají. Ve všech případech je to perfektní čas ke kontrole úložiště dat, zpracování a modelu uchování potřebného pro každé úložiště dat. To je důležité při sestavování systémů správy dat ve scénářích dodržování předpisů.
Cloud nabízí nové příležitosti, které nejsou aktuálně k dispozici v místním prostředí. To může znamenat aktualizace existujících datových modelů, které můžete mít.

Jakmile budete spokojeni s novým datovým modelem, určete strategii přijímání dat. Jaké zdroje dat zde existují? Kde budou data živá v Azure? Jak a kdy bude přesunuta do Azure? K dispozici je mnoho dostupných prostředků, které vám pomůžou s migrací na základě typu obsahu, velikosti a dalších. Služba Azure Data Migration je takový příklad.

Jakmile jsou vaše data hostována v Azure, vytvořte plán vyprázdnit data pro data, která mají živý užitečnost nebo životnost. I když dlouhodobé (studené) úložiště je vždy skvělou možností pro archivaci, vyčištění dat s vypršenou platností snižuje nároky na kapacitu a celkové náklady na úložiště. [Architektura řešení](https://azure.microsoft.com/solutions/architecture/?solution=backup-archive?WT.mc_id=bankdm-docs-dastarr) pro zálohování a archivaci je dobrým prostředkem, který vám umožní naplánovat celkovou strategii.

## <a name="relevant-technologies"></a>Relevantní technologie

- [Azure Functions](/azure/azure-functions/functions-bindings-timer?WT.mc_id=bankdm-docs-dastarr) jsou skripty bez serveru a malé programy, které mohou běžet v reakci na událost systému nebo časovač.

- Nástroje pro [Azure Storage klienta](/azure/storage/common/storage-explorers?WT.mc_id=bankdm-docs-dastarr) jsou nástroje pro přístup k úložištím dat a zahrnují mnohem více než Azure Portal.

- [Úložiště objektů BLOB](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=bankdm-docs-dastarr) je vhodné k ukládání souborů, jako jsou text nebo obrázky, a dalších typů nestrukturovaných dat.

- [Datacihly](/azure/azure-databricks/?WT.mc_id=bankdm-docs-dastarr) jsou plně spravovaná služba, která nabízí snadnou implementaci clusteru Spark.

- [Data Factory](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=bankdm-docs-dastarr) je cloudová služba pro integraci dat, která se používá k vytváření služeb pro ukládání, průjezd a zpracování dat do automatizovaných datových kanálů.

## <a name="conclusion"></a>Závěr

Díky rychlé změně digitální krajiny pro bankovní a finanční obor se zákazníkům stále častěji hledají řešení a partnery, kteří ho můžou hned využít, a to bez zpomalení provozu. Jelikož se příjem dat zvyšuje exponenciálně, banky potřebují rychlé, inovativní a bezpečné způsoby ukládání, analýzy a používání důležitých dat.

Azure může pomoci při přijímání, zpracovávání, archivaci a odstraňování požadavků pomocí několika technologií a strategií. Ingestování dat do Azure je jednoduché a k dispozici jsou různá úložiště dat, která slouží k ukládání dat v závislosti na typu, struktuře atd. Data Solutions jsou k dispozici nad rámec SQL Server a SQL Azure, aby zahrnovala databáze třetích stran.

Provozování těchto dat a práce s nimi může být jednoduché pomocí služeb Azure, jako jsou datacihly a Data Factory. Archivace úložiště je k dispozici pro dlouhodobé ukládání zřídka se používaných dat a v případě potřeby je můžete v cyklických cyklech odstranit.

Přejděte do knihovny řešení Azure pro [zálohování a archivaci úložiště](https://azure.microsoft.com/solutions/architecture/?solution=backup-archive?WT.mc_id=bankdm-docs-dastarr) , kde můžete začít navrhovat plán správy dat.

**Článek o**: Howard Bush a David Starr
