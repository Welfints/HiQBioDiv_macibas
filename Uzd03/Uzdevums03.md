Trešais uzdevums: rastra dati, rasterizēšana un kodējumi
================

## Termiņš

Līdz 2025-01-10, izmantojot
[fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
un [pull
request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)
uz zaru “Dalibnieki”, šī uzdevuma direktorijā pievienojot .Rmd vai .qmd
failu, kura nosaukums ir Uzd03\_\[JusuUzvards\], piemēram,
`Uzd03_Avotins.Rmd`, kas sagatavots izvadei github dokumentā (piemēram,
YAML galvenē norādot `output: rmarkdown::github_document`), un tā radīto
izvades failu.

## Premise

Uzdevumiem, kuru pamatā ir darbs ar atribūtdatiem, kas vienai ģemetrijai
var būt būt vienlaikus vaiirāki, tajā skaitā vairākām ģeometrijām
saistīti u.tml., piemērotākais datu veids ir vektordati, par kuriem ir
[Otrais uzdevums](./Uzd02/Uzdevums02.md). Asociatīvi, rastra dati ir ik
slānī, kas ir telpiska matrica, strukturēta informācija par ik
atribūtlauku vektordatos.

Zemes novērošanas jeb tālizpētes vai attālās izpētes dati primāri tiek
iegūti kā rastrs. Savukārt dažādu Valsts datubāzu informācija primāri
tiek glabāta kā vektordati, no kuriem analīžu veikšanai tiek izveidoti
rastri (notiek datu rasterizēšana). Ja vektordatu ģeometrijas ir zīmētas
mazā mērogā ar augstu precizitāti, tās ir precīzākas par rastrā
attēlotajām, jo ir iespējamas dabiskākas, nevis iespiestas regulāros
četrstūros. Tomēr darbs ar vektordatu ģeometrijām, atkarībā no to veida
un virstoņu daudzuma, var būt sevišķi izaicinošs skaitļošanā. Tādēļ un
lai risinātu dažādus ģeometriju savstarpējā novietojuma un neatbilstību
izaicinājumus, ģeodatu procesēšanā un analīzē bieži tiek izmantoti
rastri. Pārveidojot vektordatus uz rastru, ir jāpieņem lēmumi par
robežām - visbiežāk pēc noklusējuma rastra šūnā vērtība tiek piešķirta
atbilstoši tās centram vektordatos, tomēr ne vienmēr tā ir laba doma -
ir iespējams vērtības rastrā piešķirt visām šūnām, kuras pieskarās
vektordatu ģeometrijai
([piemērs](https://r.geocompx.org/raster-vector#rasterization)).

Tā kā rastriem, to ģeoprocesēšanas un analītiskās apstrādes ātruma un
iespēju dēļ, ir nozīmīga daļa šajā projektā, ir svarīgi, lai tie ir savā
starpā salīdzināmi un savietojami. Šajā projektā izmantojamie references
slāņi ir pieejami [šī projekta datu
repozitorijā](https://zenodo.org/communities/hiqbiodiv/records?q=&l=list&p=1&s=10&sort=newest):

- [references tīkli kā vektordati](https://zenodo.org/records/14277114);

- [references režģi kā rastra
  dati](https://zenodo.org/records/14497070).

Līdzīgi kā vektordatiem, arī rastra datiem ir pieejami dažādi failu
formāti. Viens no flagmaņiem darbā ar rastru ir
[GDAL](https://gdal.org/en/stable/index.html), kas ir pamatā vai ciešā
saistībā ar dažādām rastra apstrādes pakotnēm, tā
[mājaslapā](https://gdal.org/en/stable/drivers/raster/index.html) var
iepazīties ar dažādiem rastra failu veidiem un to īpašībām. Kompresijas
īpašību dēļ, populārs rastra failu formāts, kuru izmantosim arī mēs šajā
projektā, ir
[GeoTIFF](https://gdal.org/en/stable/drivers/raster/gtiff.html#raster-gtiff).

GeoTIFF var sastāvēt no vairākām joslām (spektrālās joslas), kuras pašas
par sevi var būt kā atsevišķi slāņi/faili, kas ir apvienoti. Katra josla
ir kodējama ar kāda veida **skaitliskām** vērtībām (Byte, UInt16, Int16,
UInt32, Int32, Float32, Float64, CInt16, CInt32, CFloat32 vai CFloat64),
kas ietekmē to, kādi skaitļi (cik vērtību, tikai veseli skaitļi, tikai
nenegatīvi skaitļi, cik decimālpozīciju, un tamlīdzīgi) var tikt
pierakstīti. Atkarībā no iespējotā un faktiski izmantotā skaitļu
pierakstīšanas veida mainās faila izmērs diskā, tā lasīšanas,
rakstīšanas un apstrādes ātrums. Līdz ar vērtību pierakstīšanas veidu,
mainās arī tas, kā tiek reģistrētas (vai - ir reģistrējamas) šūnas ar
iztrūkstošajām vērtībām (bez datiem).

R vidē šobrīd nozīmīgākās pokotnes rastra apstrādei ir {terra}, kas ir
tiešs pēctecis {raster} un {stars}. Atkarībā no mērķiem un uzdevumiem,
var būt atšķirīgas priekšrocības un resursu ietilpības uzdevumiem, kas
veikti ar {terra} un {stars}. Pakotne {raster} vairs netiek aktualizēta,
tomēr ar to ir saistītas pakotnes, kas var sniegt nozīmīgu pienesumu
skaitļošanas resursu plānošanā, piemēram, {fasterize} un
{exact_extractr}. Rastra apstrādē, jo sevišķi ar hidroloģiju un
topoloģiju saistītu uzdevumu veikšanai, jaudīgus risinājumus piedāvā
{whitebox}.

## Uzdevums

Lejupielādējiet references slāņus no projekta repozitorija (gan
vektordatus, gan rastu). Iepazīstieties ar
[WFS](https://docs.geoserver.org/main/en/user/services/wfs/reference.html)
un [tā nodrošināšanu
R](https://tutorials.inbo.be/tutorials/spatial_wfs_services/).

1.  Lejupielādējiet Lauku atbalsta dienesta datus par teritoriju, kuru
    aptver otrā uzdevuma Mežu valsts reģistra dati, no WFS, izmantojot
    šo saiti
    (<https://karte.lad.gov.lv/arcgis/services/lauki/MapServer/WFSServer>).

2.  Atbilstoši referencei (10m izšķirtspējā), rasterizējiet iepriekšējā
    punktā lejupielādētos vektordatus, sagatavojot GeoTIFF slāni ar
    informāciju par vietām, kurās ir lauku bloki (kodētas ar 1) vai tās
    atrodas Latvijā, bet tajās nav lauku bloki vai par tiem nav
    informācijas (kodētas ar 0). Vietām ārpus Latvijas saglabājiet šūnas
    bez vērtībām.

3.  Izmēģiniet {terra} funkcijas `resample()`, `aggregate()` un
    `project()`, lai no iepirkšējā punktā sagatavotā rastra izveidotu
    jaunu:

- ar 100m pikseļa malas garumu un atbilstību references slānim;

- informāciju par platības, kurā ir lauka bloki, īpatsvaru.

Kura funkcija vai to kombinācija piedāvā ātrāko risinājumu?

4.  Izmantojot iepriekšējā punktā radīto 100m šūnas izmēra slāni,
    sagatavojiet divus jaunus, tā, lai:

<!-- -->

1)  slānis satur informāciju par lauka bloku platību noapaļotu līdz
    procentam un izteiktu kā veselu skaitli, tāpat kā iepriekš
    saglabājot šūnas vietām ārpus Latvijas;

2)  slānis satur bināri kodētas vērtības: Latvijā esošā šūnā ir lauku
    bloki (vērtība 1) vai to nav (vērtība 0), tāpat kā iepriekš
    saglabājot šūnas vietām ārpus Latvijas.

Salīdziniet izveidotos slāņus, lai raksturotu:

- kā mainās faila izmērs uz cietā diska, atkarībā no rastra šūnas
  izmēra;

- kā mainās faila izmērs uz cietā diska, atkarībā no tajā ietvertās
  informācijas un tās kodējuma. Kāds kodējums (joslas tips) aizņem
  vismazāko apjomu uz cietā diska, saglabājot informāciju?

## Padomi

Šobrīd šķiet, ka nav nepieciešami. Pievienošu, ja saņemšu precizējošus
jautājumus.
