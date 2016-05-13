---
layout: post
title:  "使用array处理xml获取city"
date:   2016-05-13
categories: php
---



```
**array使用$xml['countries']形式**
**object和class使用$iter->regions形式**
<?php
$xml = "regions.xml";
if(file_exists($xml)){
	$xml = simplexml_load_file($xml);
	// print_r($xml);
	$xml = (array)$xml;
	// print_r($xml);
	// print_r($xml['countries']);die();
	foreach ($xml['countries'] as $iter) {
		// print_r($iter->regions);
		foreach ($iter->regions as $city) {
			echo $city->en."\n";
			}
		}
}else {
	exit('failed to open $xml');
}
?>
```

### result

```
Beijing
Tianjin
Hebei
Shanxi
Neimeng
Liaoning
Jilin
Heilongjiang
Shanghai
Jiangsu
Zhejiang
Anhui
Fujian
Jiangxi
Shandong
Henan
Hubei
Hunan
Guangdong
Guangxi
Hainan
Chongqing
Sichuan
Guizhou
Yunnan
Xizang
Shaanxi
Gansu
Qinghai
Ningxia
Xinjiang
Hong Kong
Taiwan
Macao
Canillo
Encamp
La Massana
Ordino
Sant Julia de Loria
Andorra la Vella
Escaldes-Engordany
Abu Dhabi
Ajman
Dubai
Fujairah
Ras Al Khaimah
Sharjah
Umm Al Quwain
Badakhshan
Badghis
Baghlan
Bamian
Farah
Faryab
Ghazni
Ghowr
Helmand
Herat
Kabol
Kapisa
Lowgar
Nangarhar
Nimruz
Kandahar
Kondoz
Takhar
Vardak
Zabol
Paktika
Balkh
Jowzjan
Samangan
Sar-e Pol
Konar
Laghman
Paktia
Khowst
Nurestan
Oruzgan
Parvan
Daykondi
Panjshir
Barbuda
Saint George
Saint John
Saint Mary
Saint Paul
Saint Peter
Saint Philip
Redonda
Berat
Diber
Durres
Elbasan
Fier
Gjirokaster
Korce
Kukes
Lezhe
Shkoder
Tirane
Vlore
Aragatsotn
Ararat
Armavir
Geghark'unik'
Kotayk'
Lorri
Shirak
Syunik'
Tavush
Vayots' Dzor
Yerevan
Benguela
Bie
Cabinda
Cuando Cubango
Cuanza Norte
Cuanza Sul
Cunene
Huambo
Huila
Malanje
Namibe
Moxico
Uige
Zaire
Lunda Norte
Lunda Sul
Bengo
Luanda
Buenos Aires
Catamarca
Chaco
Chubut
Cordoba
Corrientes
Distrito Federal
Entre Rios
Formosa
Jujuy
La Pampa
La Rioja
Mendoza
Misiones
Neuquen
Rio Negro
Salta
San Juan
San Luis
Santa Cruz
Santa Fe
Santiago del Estero
Tierra del Fuego
Tucuman
Burgenland
Karnten
Niederosterreich
Oberosterreich
Salzburg
Steiermark
Tirol
Vorarlberg
Wien
Australian Capital Territory
New South Wales
Northern Territory
Queensland
South Australia
Tasmania
Victoria
Western Australia
Abseron
Agcabadi
Agdam
Agdas
Agstafa
Agsu
Ali Bayramli
Astara
Baki
Balakan
Barda
Beylaqan
Bilasuvar
Cabrayil
Calilabad
Daskasan
Davaci
Fuzuli
Gadabay
Ganca
Goranboy
Goycay
Haciqabul
Imisli
Ismayilli
Kalbacar
Kurdamir
Lacin
Lankaran
Lankaran
Lerik
Masalli
Mingacevir
Naftalan
Naxcivan
Neftcala
Oguz
Qabala
Qax
Qazax
Qobustan
Quba
Qubadli
Qusar
Saatli
Sabirabad
Saki
Saki
Salyan
Samaxi
Samkir
Samux
Siyazan
Sumqayit
Susa
Susa
Tartar
Tovuz
Ucar
Xacmaz
Xankandi
Xanlar
Xizi
Xocali
Xocavand
Yardimli
Yevlax
Yevlax
Zangilan
Zaqatala
Zardab
Federation of Bosnia and Herzegovina
Republika Srpska
Christ Church
Saint Andrew
Saint George
Saint James
Saint John
Saint Joseph
Saint Lucy
Saint Michael
Saint Peter
Saint Philip
Saint Thomas
Dhaka
Khulna
Rajshahi
Chittagong
Barisal
Sylhet
Antwerpen
Hainaut
Liege
Limburg
Luxembourg
Namur
Oost-Vlaanderen
West-Vlaanderen
Brabant Wallon
Brussels Hoofdstedelijk Gewest
Vlaams-Brabant
Flanders
Wallonia
Bam
Boulkiemde
Ganzourgou
Gnagna
Kouritenga
Oudalan
Passore
Sanguie
Soum
Tapoa
Zoundweogo
Bale
Banwa
Bazega
Bougouriba
Boulgou
Gourma
Houet
Ioba
Kadiogo
Kenedougou
Komoe
Komondjari
Kompienga
Kossi
Koulpelogo
Kourweogo
Leraba
Loroum
Mouhoun
Namentenga
Naouri
Nayala
Noumbiel
Oubritenga
Poni
Sanmatenga
Seno
Sissili
Sourou
Tuy
Yagha
Yatenga
Ziro
Zondoma
Mikhaylovgrad
Blagoevgrad
Burgas
Dobrich
Gabrovo
Grad Sofiya
Khaskovo
Kurdzhali
Kyustendil
Lovech
Montana
Pazardzhik
Pernik
Pleven
Plovdiv
Razgrad
Ruse
Shumen
Silistra
Sliven
Smolyan
Sofiya
Stara Zagora
Turgovishte
Varna
Veliko Turnovo
Vidin
Vratsa
Yambol
Al Hadd
Al Manamah
Jidd Hafs
Sitrah
Al Mintaqah al Gharbiyah
Mintaqat Juzur Hawar
Al Mintaqah ash Shamaliyah
Al Mintaqah al Wusta
Madinat
Ar Rifa
Madinat Hamad
Al Muharraq
Al Asimah
Al Janubiyah
Ash Shamaliyah
Al Wusta
Bujumbura
Bubanza
Bururi
Cankuzo
Cibitoke
Gitega
Karuzi
Kayanza
Kirundo
Makamba
Muyinga
Ngozi
Rutana
Ruyigi
Muramvya
Mwaro
Alibori
Atakora
Atlanyique
Borgou
Collines
Kouffo
Donga
Littoral
Mono
Oueme
Plateau
Zou
Devonshire
Hamilton
Hamilton
Paget
Pembroke
Saint George
Saint George's
Sandys
Smiths
Southampton
Warwick
Alibori
Belait
Brunei and Muara
Temburong
Collines
Kouffo
Donga
Littoral
Tutong
Oueme
Plateau
Zou
Chuquisaca
Cochabamba
El Beni
La Paz
Oruro
Pando
Potosi
Santa Cruz
Tarija
Acre
Alagoas
Amapa
Amazonas
Bahia
Ceara
Distrito Federal
Espirito Santo
Mato Grosso do Sul
Maranhao
Mato Grosso
Minas Gerais
Para
Paraiba
Parana
Piaui
Rio de Janeiro
Rio Grande do Norte
Rio Grande do Sul
Rondonia
Roraima
Santa Catarina
Sao Paulo
Sergipe
Goias
Pernambuco
Tocantins
Bimini
Cat Island
Exuma
Inagua
Long Island
Mayaguana
Ragged Island
Harbour Island
New Providence
Acklins and Crooked Islands
Freeport
Fresh Creek
Governor's Harbour
Green Turtle Cay
High Rock
Kemps Bay
Marsh Harbour
Nichollstown and Berry Islands
Rock Sound
Sandy Point
San Salvador and Rum Cay
Bumthang
Chhukha
Chirang
Daga
Geylegphug
Ha
Lhuntshi
Mongar
Paro
Pemagatsel
Punakha
Samchi
Samdrup
Shemgang
Tashigang
Thimphu
Tongsa
Wangdi Phodrang
Central
Ghanzi
Kgalagadi
Kgatleng
Kweneng
North-East
South-East
Southern
North-West
Brestskaya Voblasts'
Homyel'skaya Voblasts'
Hrodzyenskaya Voblasts'
Minsk
Minskaya Voblasts'
Mahilyowskaya Voblasts'
Vitsyebskaya Voblasts'
Belize
Cayo
Corozal
Orange Walk
Stann Creek
Toledo
Alberta
British Columbia
Manitoba
New Brunswick
Newfoundland
Nova Scotia
Northwest Territories
Nunavut
Ontario
Prince Edward Island
Quebec
Saskatchewan
Yukon Territory
Bandundu
Equateur
Kasai-Oriental
Katanga
Kinshasa
Bas-Congo
Orientale
Maniema
Nord-Kivu
Sud-Kivu
Bamingui-Bangoran
Basse-Kotto
Haute-Kotto
Mambere-Kadei
Haut-Mbomou
Kemo
Lobaye
Mbomou
Nana-Mambere
Ouaka
Ouham
Ouham-Pende
Cuvette-Ouest
Nana-Grebizi
Sangha-Mbaere
Ombella-Mpoko
Bangui
Bouenza
Kouilou
Lekoumou
Likouala
Niari
Plateaux
Sangha
Pool
Brazzaville
Cuvette
Cuvette-Ouest
Aargau
Ausser-Rhoden
Basel-Landschaft
Basel-Stadt
Bern
Fribourg
Geneve
Glarus
Graubunden
Inner-Rhoden
Luzern
Neuchatel
Nidwalden
Obwalden
Sankt Gallen
Schaffhausen
Schwyz
Solothurn
Thurgau
Ticino
Uri
Valais
Vaud
Zug
Zurich
Jura
Agneby
Bafing
Bas-Sassandra
Denguele
Dix-Huit Montagnes
Fromager
Haut-Sassandra
Lacs
Lagunes
Marahoue
Moyen-Cavally
Moyen-Comoe
N'zi-Comoe
Savanes
Sud-Bandama
Sud-Comoe
Vallee du Bandama
Worodougou
Zanzan
Valparaiso
Aisen del General Carlos Ibanez del Campo
Antofagasta
Araucania
Atacama
Bio-Bio
Coquimbo
Libertador General Bernardo O'Higgins
Los Lagos
Magallanes y de la Antartica Chilena
Maule
Region Metropolitana
Tarapaca
Los Lagos
Tarapaca
Arica y Parinacota
Los Rios
Est
Littoral
Nord-Ouest
Ouest
Sud-Ouest
Adamaoua
Centre
Extreme-Nord
Nord
Sud
Amazonas
Antioquia
Arauca
Atlantico
Caqueta
Cauca
Cesar
Choco
Cordoba
Guaviare
Guainia
Huila
La Guajira
Meta
Narino
Norte de Santander
Putumayo
Quindio
Risaralda
San Andres y Providencia
Santander
Sucre
Tolima
Valle del Cauca
Vaupes
Vichada
Casanare
Cundinamarca
Distrito Especial
Bolivar
Boyaca
Caldas
Magdalena
Alajuela
Cartago
Guanacaste
Heredia
Limon
Puntarenas
San Jose
Pinar del Rio
Ciudad de la Habana
Matanzas
Isla de la Juventud
Camaguey
Ciego de Avila
Cienfuegos
Granma
Guantanamo
La Habana
Holguin
Las Tunas
Sancti Spiritus
Santiago de Cuba
Villa Clara
Boa Vista
Brava
Maio
Paul
Ribeira Grande
Sal
Sao Nicolau
Sao Vicente
Mosteiros
Praia
Santa Catarina
Santa Cruz
Sao Domingos
Sao Filipe
Sao Miguel
Tarrafal
Famagusta
Kyrenia
Larnaca
Nicosia
Limassol
Paphos
Hlavni mesto Praha
Jihomoravsky kraj
Jihocesky kraj
Vysocina
Karlovarsky kraj
Kralovehradecky kraj
Liberecky kraj
Olomoucky kraj
Moravskoslezsky kraj
Pardubicky kraj
Plzensky kraj
Stredocesky kraj
Ustecky kraj
Zlinsky kraj
Baden-Wurttemberg
Bayern
Bremen
Hamburg
Hessen
Niedersachsen
Nordrhein-Westfalen
Rheinland-Pfalz
Saarland
Schleswig-Holstein
Brandenburg
Mecklenburg-Vorpommern
Sachsen
Sachsen-Anhalt
Thuringen
Berlin
Ali Sabieh
Obock
Tadjoura
Dikhil
Djibouti
Arta
Hovedstaden
Midtjylland
Nordjylland
Sjelland
Syddanmark
Saint Andrew
Saint David
Saint George
Saint John
Saint Joseph
Saint Luke
Saint Mark
Saint Patrick
Saint Paul
Saint Peter
Azua
Baoruco
Barahona
Dajabon
Distrito Nacional
Duarte
Espaillat
Independencia
La Altagracia
Elias Pina
La Romana
Maria Trinidad Sanchez
Monte Cristi
Pedernales
Peravia
Puerto Plata
Salcedo
Samana
Sanchez Ramirez
San Juan
San Pedro De Macoris
Santiago
Santiago Rodriguez
Valverde
El Seibo
Hato Mayor
La Vega
Monsenor Nouel
Monte Plata
San Cristobal
Distrito Nacional
Peravia
San Jose de Ocoa
Santo Domingo
Alger
Batna
Constantine
Medea
Mostaganem
Oran
Saida
Setif
Tiaret
Tizi Ouzou
Tlemcen
Bejaia
Biskra
Blida
Bouira
Djelfa
Guelma
Jijel
Laghouat
Mascara
M'sila
Oum el Bouaghi
Sidi Bel Abbes
Skikda
Tebessa
Adrar
Ain Defla
Ain Temouchent
Annaba
Bechar
Bordj Bou Arreridj
Boumerdes
Chlef
El Bayadh
El Oued
El Tarf
Ghardaia
Illizi
Khenchela
Mila
Naama
Ouargla
Relizane
Souk Ahras
Tamanghasset
Tindouf
Tipaza
Tissemsilt
Galapagos
Azuay
Bolivar
Canar
Carchi
Chimborazo
Cotopaxi
El Oro
Esmeraldas
Guayas
Imbabura
Loja
Los Rios
Manabi
Morona-Santiago
Pastaza
Pichincha
Tungurahua
Zamora-Chinchipe
Sucumbios
Napo
Orellana
Harjumaa
Hiiumaa
Ida-Virumaa
Jarvamaa
Jogevamaa
Kohtla-Jarve
Laanemaa
Laane-Virumaa
Narva
Parnu
Parnumaa
Polvamaa
Raplamaa
Saaremaa
Sillamae
Tallinn
Tartu
Tartumaa
Valgamaa
Viljandimaa
Vorumaa
Ad Daqahliyah
Al Bahr al Ahmar
Al Buhayrah
Al Fayyum
Al Gharbiyah
Al Iskandariyah
Al Isma'iliyah
Al Jizah
Al Minufiyah
Al Minya
Al Qahirah
Al Qalyubiyah
Al Wadi al Jadid
Ash Sharqiyah
As Suways
Aswan
Asyut
Bani Suwayf
Bur Sa'id
Dumyat
Kafr ash Shaykh
Matruh
Qina
Suhaj
Janub Sina'
Shamal Sina'
Anseba
Debub
Debubawi K'eyih Bahri
Gash Barka
Ma'akel
Semenawi K'eyih Bahri
Islas Baleares
La Rioja
Madrid
Murcia
Navarra
Asturias
Cantabria
Andalucia
Aragon
Canarias
Castilla-La Mancha
Castilla y Leon
Catalonia
Extremadura
Galicia
Pais Vasco
Comunidad Valenciana
Adis Abeba
Afar
Amara
Binshangul Gumuz
Dire Dawa
Gambela Hizboch
Hareri Hizb
Oromiya
Sumale
Tigray
YeDebub Biheroch Bihereseboch na Hizboch
Aland
Lapland
Oulu
Southern Finland
Eastern Finland
Western Finland
Central
Eastern
Northern
Rotuma
Western
Kosrae
Pohnpei
Chuuk
Yap
Aquitaine
Auvergne
Basse-Normandie
Bourgogne
Bretagne
Centre
Champagne-Ardenne
Corse
Franche-Comte
Haute-Normandie
Ile-de-France
Languedoc-Roussillon
Limousin
Lorraine
Midi-Pyrenees
Nord-Pas-de-Calais
Pays de la Loire
Picardie
Poitou-Charentes
Provence-Alpes-Cote d'Azur
Rhone-Alpes
Alsace
Estuaire
Haut-Ogooue
Moyen-Ogooue
Ngounie
Nyanga
Ogooue-Ivindo
Ogooue-Lolo
Ogooue-Maritime
Woleu-Ntem
Barking and Dagenham
Barnet
Barnsley
Bath and North East Somerset
Bedfordshire
Bexley
Birmingham
Blackburn with Darwen
Blackpool
Bolton
Bournemouth
Bracknell Forest
Bradford
Brent
Brighton and Hove
Bristol
Bromley
Buckinghamshire
Bury
Calderdale
Cambridgeshire
Camden
Cheshire
Cornwall
Coventry
Croydon
Cumbria
Darlington
Derby
Derbyshire
Devon
Doncaster
Dorset
Dudley
Durham
Ealing
East Riding of Yorkshire
East Sussex
Enfield
Essex
Gateshead
Gloucestershire
Greenwich
Hackney
Halton
Hammersmith and Fulham
Hampshire
Haringey
Harrow
Hartlepool
Havering
Herefordshire
Hertford
Hillingdon
Hounslow
Isle of Wight
Islington
Kensington and Chelsea
Kent
Kingston upon Hull
Kingston upon Thames
Kirklees
Knowsley
Lambeth
Lancashire
Leeds
Leicester
Leicestershire
Lewisham
Lincolnshire
Liverpool
London
Luton
Manchester
Medway
Merton
Middlesbrough
Milton Keynes
Newcastle upon Tyne
Newham
Norfolk
Northamptonshire
North East Lincolnshire
North Lincolnshire
North Somerset
North Tyneside
Northumberland
North Yorkshire
Nottingham
Nottinghamshire
Oldham
Oxfordshire
Peterborough
Plymouth
Poole
Portsmouth
Reading
Redbridge
Redcar and Cleveland
Richmond upon Thames
Rochdale
Rotherham
Rutland
Salford
Shropshire
Sandwell
Sefton
Sheffield
Slough
Solihull
Somerset
Southampton
Southend-on-Sea
South Gloucestershire
South Tyneside
Southwark
Staffordshire
St. Helens
Stockport
Stockton-on-Tees
Stoke-on-Trent
Suffolk
Sunderland
Surrey
Sutton
Swindon
Tameside
Telford and Wrekin
Thurrock
Torbay
Tower Hamlets
Trafford
Wakefield
Walsall
Waltham Forest
Wandsworth
Warrington
Warwickshire
West Berkshire
Westminster
West Sussex
Wigan
Wiltshire
Windsor and Maidenhead
Wirral
Wokingham
Wolverhampton
Worcestershire
York
Antrim
Ards
Armagh
Ballymena
Ballymoney
Banbridge
Belfast
Carrickfergus
Castlereagh
Coleraine
Cookstown
Craigavon
Down
Dungannon
Fermanagh
Larne
Limavady
Lisburn
Derry
Magherafelt
Moyle
Newry and Mourne
Newtownabbey
North Down
Omagh
Strabane
Aberdeen City
Aberdeenshire
Angus
Argyll and Bute
Scottish Borders
Clackmannanshire
Dumfries and Galloway
Dundee City
East Ayrshire
East Dunbartonshire
East Lothian
East Renfrewshire
Edinburgh
Falkirk
Fife
Glasgow City
Highland
Inverclyde
Midlothian
Moray
North Ayrshire
North Lanarkshire
Orkney
Perth and Kinross
Renfrewshire
Shetland Islands
South Ayrshire
South Lanarkshire
Stirling
West Dunbartonshire
Eilean Siar
West Lothian
Isle of Anglesey
Blaenau Gwent
Bridgend
Caerphilly
Cardiff
Ceredigion
Carmarthenshire
Conwy
Denbighshire
Flintshire
Gwynedd
Merthyr Tydfil
Monmouthshire
Neath Port Talbot
Newport
Pembrokeshire
Powys
Rhondda Cynon Taff
Swansea
Torfaen
Vale of Glamorgan
Wrexham
Bedfordshire
Central Bedfordshire
Cheshire East
Cheshire West and Chester
Isles of Scilly
Saint Andrew
Saint David
Saint George
Saint John
Saint Mark
Saint Patrick
Abashis Raioni
Abkhazia
Adigenis Raioni
Ajaria
Akhalgoris Raioni
Akhalk'alak'is Raioni
Akhalts'ikhis Raioni
Akhmetis Raioni
Ambrolauris Raioni
Aspindzis Raioni
Baghdat'is Raioni
Bolnisis Raioni
Borjomis Raioni
Chiat'ura
Ch'khorotsqus Raioni
Ch'okhatauris Raioni
Dedop'listsqaros Raioni
Dmanisis Raioni
Dushet'is Raioni
Gardabanis Raioni
Gori
Goris Raioni
Gurjaanis Raioni
Javis Raioni
K'arelis Raioni
Kaspis Raioni
Kharagaulis Raioni
Khashuris Raioni
Khobis Raioni
Khonis Raioni
K'ut'aisi
Lagodekhis Raioni
Lanch'khut'is Raioni
Lentekhis Raioni
Marneulis Raioni
Martvilis Raioni
Mestiis Raioni
Mts'khet'is Raioni
Ninotsmindis Raioni
Onis Raioni
Ozurget'is Raioni
P'ot'i
Qazbegis Raioni
Qvarlis Raioni
Rust'avi
Sach'kheris Raioni
Sagarejos Raioni
Samtrediis Raioni
Senakis Raioni
Sighnaghis Raioni
T'bilisi
T'elavis Raioni
T'erjolis Raioni
T'et'ritsqaros Raioni
T'ianet'is Raioni
Tqibuli
Ts'ageris Raioni
Tsalenjikhis Raioni
Tsalkis Raioni
Tsqaltubo
Vanis Raioni
Zestap'onis Raioni
Zugdidi
Zugdidis Raioni
Greater Accra
Ashanti
Brong-Ahafo
Central
Eastern
Northern
Volta
Western
Upper East
Upper West
Nordgronland
Ostgronland
Vestgronland
Banjul
Lower River
Central River
Upper River
Western
North Bank
Beyla
Boffa
Boke
Conakry
Dabola
Dalaba
Dinguiraye
Faranah
Forecariah
Fria
Gaoual
Gueckedou
Kerouane
Kindia
Kissidougou
Koundara
Kouroussa
Macenta
Mali
Mamou
Pita
Telimele
Tougue
Yomou
Coyah
Dubreka
Kankan
Koubia
Labe
Lelouma
Lola
Mandiana
Nzerekore
Siguiri
Annobon
Bioko Norte
Bioko Sur
Centro Sur
Kie-Ntem
Litoral
Wele-Nzas
Evros
Rodhopi
Xanthi
Drama
Serrai
Kilkis
Pella
Florina
Kastoria
Grevena
Kozani
Imathia
Thessaloniki
Kavala
Khalkidhiki
Pieria
Ioannina
Thesprotia
Preveza
Arta
Larisa
Trikala
Kardhitsa
Magnisia
Kerkira
Levkas
Kefallinia
Zakinthos
Fthiotis
Evritania
Aitolia kai Akarnania
Fokis
Voiotia
Evvoia
Attiki
Argolis
Korinthia
Akhaia
Ilia
Messinia
Arkadhia
Lakonia
Khania
Rethimni
Iraklion
Lasithi
Dhodhekanisos
Samos
Kikladhes
Khios
Lesvos
Alta Verapaz
Baja Verapaz
Chimaltenango
Chiquimula
El Progreso
Escuintla
Guatemala
Huehuetenango
Izabal
Jalapa
Jutiapa
Peten
Quetzaltenango
Quiche
Retalhuleu
Sacatepequez
San Marcos
Santa Rosa
Solola
Suchitepequez
Totonicapan
Zacapa
Bafata
Quinara
Oio
Bolama
Cacheu
Tombali
Gabu
Bissau
Biombo
Barima-Waini
Cuyuni-Mazaruni
Demerara-Mahaica
East Berbice-Corentyne
Essequibo Islands-West Demerara
Mahaica-Berbice
Pomeroon-Supenaam
Potaro-Siparuni
Upper Demerara-Berbice
Upper Takutu-Upper Essequibo
Atlantida
Choluteca
Colon
Comayagua
Copan
Cortes
El Paraiso
Francisco Morazan
Gracias a Dios
Intibuca
Islas de la Bahia
La Paz
Lempira
Ocotepeque
Olancho
Santa Barbara
Valle
Yoro
Bjelovarsko-Bilogorska
Brodsko-Posavska
Dubrovacko-Neretvanska
Istarska
Karlovacka
Koprivnicko-Krizevacka
Krapinsko-Zagorska
Licko-Senjska
Medimurska
Osjecko-Baranjska
Pozesko-Slavonska
Primorsko-Goranska
Sibensko-Kninska
Sisacko-Moslavacka
Splitsko-Dalmatinska
Varazdinska
Viroviticko-Podravska
Vukovarsko-Srijemska
Zadarska
Zagrebacka
Grad Zagreb
Nord-Ouest
Artibonite
Centre
Nord
Nord-Est
Ouest
Sud
Sud-Est
Grand' Anse
Nippes
Bacs-Kiskun
Baranya
Bekes
Borsod-Abauj-Zemplen
Budapest
Csongrad
Debrecen
Fejer
Gyor-Moson-Sopron
Hajdu-Bihar
Heves
Komarom-Esztergom
Miskolc
Nograd
Pecs
Pest
Somogy
Szabolcs-Szatmar-Bereg
Szeged
Jasz-Nagykun-Szolnok
Tolna
Vas
Veszprem
Zala
Gyor
Bekescsaba
Dunaujvaros
Eger
Hodmezovasarhely
Kaposvar
Kecskemet
Nagykanizsa
Nyiregyhaza
Sopron
Szekesfehervar
Szolnok
Szombathely
Tatabanya
Veszprem
Zalaegerszeg
Salgotarjan
Szekszard
Erd
Aceh
Bali
Bengkulu
Jakarta Raya
Jambi
Jawa Tengah
Jawa Timur
Yogyakarta
Kalimantan Barat
Kalimantan Selatan
Kalimantan Tengah
Kalimantan Timur
Lampung
Nusa Tenggara Barat
Nusa Tenggara Timur
Sulawesi Tengah
Sulawesi Tenggara
Sumatera Barat
Sumatera Utara
Maluku
Maluku Utara
Jawa Barat
Sulawesi Utara
Sumatera Selatan
Banten
Gorontalo
Kepulauan Bangka Belitung
Papua
Riau
Sulawesi Selatan
Irian Jaya Barat
Kepulauan Riau
Sulawesi Barat
Carlow
Cavan
Clare
Cork
Donegal
Dublin
Galway
Kerry
Kildare
Kilkenny
Leitrim
Laois
Limerick
Longford
Louth
Mayo
Meath
Monaghan
Offaly
Roscommon
Sligo
Tipperary
Waterford
Westmeath
Wexford
Wicklow
HaDarom
HaMerkaz
HaZafon
Hefa
Tel Aviv
Yerushalayim
Andaman and Nicobar Islands
Andhra Pradesh
Assam
Chandigarh
Dadra and Nagar Haveli
Delhi
Gujarat
Haryana
Himachal Pradesh
Jammu and Kashmir
Kerala
Lakshadweep
Maharashtra
Manipur
Meghalaya
Karnataka
Nagaland
Orissa
Puducherry
Punjab
Rajasthan
Tamil Nadu
Tripura
West Bengal
Sikkim
Arunachal Pradesh
Mizoram
Daman and Diu
Goa
Bihar
Madhya Pradesh
Uttar Pradesh
Chhattisgarh
Jharkhand
Uttarakhand
Al Anbar
Al Basrah
Al Muthanna
Al Qadisiyah
As Sulaymaniyah
Babil
Baghdad
Dahuk
Dhi Qar
Diyala
Arbil
Karbala'
At Ta'mim
Maysan
Ninawa
Wasit
An Najaf
Salah ad Din
Azarbayjan-e Bakhtari
Chahar Mahall va Bakhtiari
Sistan va Baluchestan
Kohkiluyeh va Buyer Ahmadi
Fars
Gilan
Hamadan
Ilam
Hormozgan
Kerman
Bakhtaran
Khuzestan
Kordestan
Mazandaran
Semnan Province
Markazi
Zanjan
Bushehr
Lorestan
Markazi
Semnan
Tehran
Zanjan
Esfahan
Kerman
Khorasan
Yazd
Ardabil
East Azarbaijan
Markazi
Mazandaran
Zanjan
Golestan
Qazvin
Qom
Yazd
Khorasan-e Janubi
Khorasan-e Razavi
Khorasan-e Shemali
Alborz
Arnessysla
Austur-Hunavatnssysla
Austur-Skaftafellssysla
Borgarfjardarsysla
Eyjafjardarsysla
Gullbringusysla
Kjosarsysla
Myrasysla
Nordur-Mulasysla
Nordur-Tingeyjarsysla
Rangarvallasysla
Skagafjardarsysla
Snafellsnes- og Hnappadalssysla
Strandasysla
Sudur-Mulasysla
Sudur-Tingeyjarsysla
Vestur-Bardastrandarsysla
Vestur-Hunavatnssysla
Vestur-Isafjardarsysla
Vestur-Skaftafellssysla
Austurland
Hofuoborgarsvaoio
Norourland Eystra
Norourland Vestra
Suourland
Suournes
Vestfiroir
Vesturland
Abruzzi
Basilicata
Calabria
Campania
Emilia-Romagna
Friuli-Venezia Giulia
Lazio
Liguria
Lombardia
Marche
Molise
Piemonte
Puglia
Sardegna
Sicilia
Toscana
Trentino-Alto Adige
Umbria
Valle d'Aosta
Veneto
Clarendon
Hanover
Manchester
Portland
Saint Andrew
Saint Ann
Saint Catherine
Saint Elizabeth
Saint James
Saint Mary
Saint Thomas
Trelawny
Westmoreland
Kingston
Al Balqa'
Al Karak
At Tafilah
Al Mafraq
Amman
Az Zaraqa
Irbid
Ma'an
Ajlun
Al Aqabah
Jarash
Madaba
Aichi
Akita
Aomori
Chiba
Ehime
Fukui
Fukuoka
Fukushima
Gifu
Gumma
Hiroshima
Hokkaido
Hyogo
Ibaraki
Ishikawa
Iwate
Kagawa
Kagoshima
Kanagawa
Kochi
Kumamoto
Kyoto
Mie
Miyagi
Miyazaki
Nagano
Nagasaki
Nara
Niigata
Oita
Okayama
Osaka
Saga
Saitama
Shiga
Shimane
Shizuoka
Tochigi
Tokushima
Tokyo
Tottori
Toyama
Wakayama
Yamagata
Yamaguchi
Yamanashi
Okinawa
Central
Coast
Eastern
Nairobi Area
North-Eastern
Nyanza
Rift Valley
Western
Bishkek
Chuy
Jalal-Abad
Naryn
Osh
Talas
Ysyk-Kol
Osh
Batken
Batdambang
Kampong Cham
Kampong Chhnang
Kampong Speu
Kampong Thum
Kampot
Kandal
Koh Kong
Kracheh
Mondulkiri
Phnum Penh
Pursat
Preah Vihear
Prey Veng
Ratanakiri Kiri
Siem Reap
Stung Treng
Svay Rieng
Takeo
Banteay Meanchey
Batdambang
Pailin
Gilbert Islands
Line Islands
Phoenix Islands
Anjouan
Grande Comore
Moheli
Christ Church Nichola Town
Saint Anne Sandy Point
Saint George Basseterre
Saint George Gingerland
Saint James Windward
Saint John Capisterre
Saint John Figtree
Saint Mary Cayon
Saint Paul Capisterre
Saint Paul Charlestown
Saint Peter Basseterre
Saint Thomas Lowland
Saint Thomas Middle Island
Trinity Palmetto Point
Chagang-do
Hamgyong-namdo
Hwanghae-namdo
Hwanghae-bukto
Kaesong-si
Kangwon-do
P'yongan-bukto
P'yongyang-si
Yanggang-do
Namp'o-si
P'yongan-namdo
Hamgyong-bukto
Najin Sonbong-si
Cheju-do
Cholla-bukto
Ch'ungch'ong-bukto
Kangwon-do
Pusan-jikhalsi
Seoul-t'ukpyolsi
Inch'on-jikhalsi
Kyonggi-do
Kyongsang-bukto
Taegu-jikhalsi
Cholla-namdo
Ch'ungch'ong-namdo
Kwangju-jikhalsi
Taejon-jikhalsi
Kyongsang-namdo
Ulsan-gwangyoksi
Al Ahmadi
Al Kuwayt
Al Jahra
Al Farwaniyah
Hawalli
Mubarak al Kabir
Creek
Eastern
Midland
South Town
Spot Bay
Stake Bay
West End
Western
Almaty
Almaty City
Aqmola
Aqtobe
Astana
Atyrau
West Kazakhstan
Bayqonyr
Mangghystau
South Kazakhstan
Pavlodar
Qaraghandy
Qostanay
Qyzylorda
East Kazakhstan
North Kazakhstan
Zhambyl
Attapu
Champasak
Houaphan
Khammouan
Louang Namtha
Oudomxai
Phongsali
Saravan
Savannakhet
Vientiane
Xaignabouri
Xiangkhoang
Louangphrabang
Beqaa
Al Janub
Liban-Nord
Beyrouth
Mont-Liban
Liban-Sud
Nabatiye
Beqaa
Liban-Nord
Aakk
Baalbek-Hermel
Anse-la-Raye
Dauphin
Castries
Choiseul
Dennery
Gros-Islet
Laborie
Micoud
Soufriere
Vieux-Fort
Praslin
Balzers
Eschen
Gamprin
Mauren
Planken
Ruggell
Schaan
Schellenberg
Triesen
Triesenberg
Vaduz
Gbarpolu
River Gee
Central
North Central
North Western
Sabaragamuwa
Southern
Uva
Western
Eastern
Northern
Bong
Grand Cape Mount
Lofa
Maryland
Monrovia
Nimba
Sino
Grand Bassa
Grand Cape Mount
Maryland
Montserrado
Margibi
River Cess
Grand Gedeh
Lofa
Gbarpolu
River Gee
Berea
Butha-Buthe
Leribe
Mafeteng
Maseru
Mohales Hoek
Mokhotlong
Qachas Nek
Quthing
Thaba-Tseka
Alytaus Apskritis
Kauno Apskritis
Klaipedos Apskritis
Marijampoles Apskritis
Panevezio Apskritis
Siauliu Apskritis
Taurages Apskritis
Telsiu Apskritis
Utenos Apskritis
Vilniaus Apskritis
Diekirch
Grevenmacher
Luxembourg
Aizkraukles
Aluksnes
Balvu
Bauskas
Cesu
Daugavpils
Daugavpils
Dobeles
Gulbenes
Jekabpils
Jelgava
Jelgavas
Jurmala
Kraslavas
Kuldigas
Liepaja
Liepajas
Limbazu
Ludzas
Madonas
Ogres
Preilu
Rezekne
Rezeknes
Riga
Rigas
Saldus
Talsu
Tukuma
Valkas
Valmieras
Ventspils
Ventspils
Al Aziziyah
Al Jufrah
Al Kufrah
Ash Shati'
Murzuq
Sabha
Tarhunah
Tubruq
Zlitan
Ajdabiya
Al Fatih
Al Jabal al Akhdar
Al Khums
An Nuqat al Khams
Awbari
Az Zawiyah
Banghazi
Darnah
Ghadamis
Gharyan
Misratah
Sawfajjin
Surt
Tarabulus
Yafran
Grand Casablanca
Fes-Boulemane
Marrakech-Tensift-Al Haouz
Meknes-Tafilalet
Rabat-Sale-Zemmour-Zaer
Chaouia-Ouardigha
Doukkala-Abda
Gharb-Chrarda-Beni Hssen
Guelmim-Es Smara
Oriental
Souss-Massa-Dr
Tadla-Azilal
Tanger-Tetouan
Taza-Al Hoceima-Taounate
La
La Condamine
Monaco
Monte-Carlo
Gagauzia
Chisinau
Stinga Nistrului
Anenii Noi
Balti
Basarabeasca
Bender
Briceni
Cahul
Cantemir
Calarasi
Causeni
Cimislia
Criuleni
Donduseni
Drochia
Dubasari
Edinet
Falesti
Floresti
Glodeni
Hincesti
Ialoveni
Leova
Nisporeni
Ocnita
Orhei
Rezina
Riscani
Singerei
Soldanesti
Soroca
Stefan-Voda
Straseni
Taraclia
Telenesti
Ungheni
Antsiranana
Fianarantsoa
Mahajanga
Toamasina
Antananarivo
Toliara
Aracinovo
Bac
Belcista
Berovo
Bistrica
Bitola
Blatec
Bogdanci
Bogomila
Bogovinje
Bosilovo
Brvenica
Cair
Capari
Caska
Cegrane
Centar
Centar Zupa
Cesinovo
Cucer-Sandevo
Debar
Delcevo
Delogozdi
Demir Hisar
Demir Kapija
Dobrusevo
Dolna Banjica
Dolneni
Dorce Petrov
Drugovo
Dzepciste
Gazi Baba
Gevgelija
Gostivar
Gradsko
Ilinden
Izvor
Jegunovce
Kamenjane
Karbinci
Karpos
Kavadarci
Kicevo
Kisela Voda
Klecevce
Kocani
Konce
Kondovo
Konopiste
Kosel
Kratovo
Kriva Palanka
Krivogastani
Krusevo
Kuklis
Kukurecani
Kumanovo
Labunista
Lipkovo
Lozovo
Lukovo
Makedonska Kamenica
Makedonski Brod
Mavrovi Anovi
Meseista
Miravci
Mogila
Murtino
Negotino
Negotino-Polosko
Novaci
Novo Selo
Oblesevo
Ohrid
Orasac
Orizari
Oslomej
Pehcevo
Petrovec
Plasnica
Podares
Prilep
Probistip
Radovis
Rankovce
Resen
Rosoman
Rostusa
Samokov
Saraj
Sipkovica
Sopiste
Sopotnica
Srbinovo
Staravina
Star Dojran
Staro Nagoricane
Stip
Struga
Strumica
Studenicani
Suto Orizari
Sveti Nikole
Tearce
Tetovo
Topolcani
Valandovo
Vasilevo
Veles
Velesta
Vevcani
Vinica
Vitoliste
Vranestica
Vrapciste
Vratnica
Vrutok
Zajas
Zelenikovo
Zelino
Zitose
Zletovo
Zrnovci
Bamako
Kayes
Mopti
Segou
Sikasso
Koulikoro
Tombouctou
Gao
Kidal
Rakhine State
Chin State
Irrawaddy
Kachin State
Karan State
Kayah State
Magwe
Mandalay
Pegu
Sagaing
Shan State
Tenasserim
Mon State
Rangoon
Yangon
Arhangay
Bayanhongor
Bayan-Olgiy
Darhan
Dornod
Dornogovi
Dundgovi
Dzavhan
Govi-Altay
Hentiy
Hovd
Hovsgol
Omnogovi
Ovorhangay
Selenge
Suhbaatar
Tov
Uvs
Ulaanbaatar
Bulgan
Erdenet
Darhan-Uul
Govisumber
Orhon
Hodh Ech Chargui
Hodh El Gharbi
Assaba
Gorgol
Brakna
Trarza
Adrar
Dakhlet Nouadhibou
Tagant
Guidimaka
Tiris Zemmour
Inchiri
Saint Anthony
Saint Georges
Saint Peter
Black River
Flacq
Grand Port
Moka
Pamplemousses
Plaines Wilhems
Port Louis
Riviere du Rempart
Savanne
Agalega Islands
Cargados Carajos
Rodrigues
Seenu
Laamu
Alifu
Baa
Dhaalu
Faafu
Gaafu Alifu
Gaafu Dhaalu
Haa Alifu
Haa Dhaalu
Kaafu
Lhaviyani
Maale
Meemu
Gnaviyani
Noonu
Raa
Shaviyani
Thaa
Vaavu
Chikwawa
Chiradzulu
Chitipa
Thyolo
Dedza
Dowa
Karonga
Kasungu
Lilongwe
Mangochi
Mchinji
Mzimba
Ntcheu
Nkhata Bay
Nkhotakota
Nsanje
Ntchisi
Rumphi
Salima
Zomba
Blantyre
Mwanza
Balaka
Likoma
Machinga
Mulanje
Phalombe
Aguascalientes
Baja California
Baja California Sur
Campeche
Chiapas
Chihuahua
Coahuila de Zaragoza
Colima
Distrito Federal
Durango
Guanajuato
Guerrero
Hidalgo
Jalisco
Mexico
Michoacan de Ocampo
Morelos
Nayarit
Nuevo Leon
Oaxaca
Puebla
Queretaro de Arteaga
Quintana Roo
San Luis Potosi
Sinaloa
Sonora
Tabasco
Tamaulipas
Tlaxcala
Veracruz-Llave
Yucatan
Zacatecas
Johor
Kedah
Kelantan
Melaka
Negeri Sembilan
Pahang
Perak
Perlis
Pulau Pinang
Sarawak
Selangor
Terengganu
Kuala Lumpur
Labuan
Sabah
Putrajaya
Cabo Delgado
Gaza
Inhambane
Maputo
Sofala
Nampula
Niassa
Tete
Zambezia
Manica
Maputo
Bethanien
Caprivi Oos
Boesmanland
Gobabis
Grootfontein
Kaokoland
Karibib
Keetmanshoop
Luderitz
Maltahohe
Okahandja
Omaruru
Otjiwarongo
Outjo
Owambo
Rehoboth
Swakopmund
Tsumeb
Karasburg
Windhoek
Damaraland
Hereroland Oos
Hereroland Wes
Kavango
Mariental
Namaland
Caprivi
Erongo
Hardap
Karas
Kunene
Ohangwena
Okavango
Omaheke
Omusati
Oshana
Oshikoto
Otjozondjupa
Agadez
Diffa
Dosso
Maradi
Niamey
Tahoua
Zinder
Niamey
Lagos
Federal Capital Territory
Ogun
Akwa Ibom
Cross River
Kaduna
Katsina
Anambra
Benue
Borno
Imo
Kano
Kwara
Niger
Oyo
Adamawa
Delta
Edo
Jigawa
Kebbi
Kogi
Osun
Taraba
Yobe
Abia
Bauchi
Enugu
Ondo
Plateau
Rivers
Sokoto
Bayelsa
Ebonyi
Ekiti
Gombe
Nassarawa
Zamfara
Boaco
Carazo
Chinandega
Chontales
Esteli
Granada
Jinotega
Leon
Madriz
Managua
Masaya
Matagalpa
Nueva Segovia
Rio San Juan
Rivas
Zelaya
Autonoma Atlantico Norte
Region Autonoma Atlantico Sur
Drenthe
Friesland
Gelderland
Groningen
Limburg
Noord-Brabant
Noord-Holland
Utrecht
Zeeland
Zuid-Holland
Overijssel
Flevoland
Akershus
Aust-Agder
Buskerud
Finnmark
Hedmark
Hordaland
More og Romsdal
Nordland
Nord-Trondelag
Oppland
Oslo
Ostfold
Rogaland
Sogn og Fjordane
Sor-Trondelag
Telemark
Troms
Vest-Agder
Vestfold
Bagmati
Bheri
Dhawalagiri
Gandaki
Janakpur
Karnali
Kosi
Lumbini
Mahakali
Mechi
Narayani
Rapti
Sagarmatha
Seti
Aiwo
Anabar
Anetan
Anibare
Baiti
Boe
Buada
Denigomodu
Ewa
Ijuw
Meneng
Nibok
Uaboe
Yaren
Chatham Islands
Auckland
Bay of Plenty
Canterbury
Gisborne
Hawke's Bay
Manawatu-Wanganui
Marlborough
Nelson
Northland
Otago
Southland
Taranaki
Waikato
Wellington
West Coast
Ad Dakhiliyah
Al Batinah
Al Wusta
Ash Sharqiyah
Az Zahirah
Masqat
Musandam
Zufar
Bocas del Toro
Chiriqui
Cocle
Colon
Darien
Herrera
Los Santos
Panama
San Blas
Veraguas
Amazonas
Ancash
Apurimac
Arequipa
Ayacucho
Cajamarca
Callao
Cusco
Huancavelica
Huanuco
Ica
Junin
La Libertad
Lambayeque
Lima
Loreto
Madre de Dios
Moquegua
Pasco
Piura
Puno
San Martin
Tacna
Tumbes
Ucayali
Central
Gulf
Milne Bay
Northern
Southern Highlands
Western
North Solomons
Chimbu
Eastern Highlands
East New Britain
East Sepik
Madang
Manus
Morobe
New Ireland
Western Highlands
West New Britain
Sandaun
Enga
National Capital
Abra
Agusan del Norte
Agusan del Sur
Aklan
Albay
Antique
Bataan
Batanes
Batangas
Benguet
Bohol
Bukidnon
Bulacan
Cagayan
Camarines Norte
Camarines Sur
Camiguin
Capiz
Catanduanes
Cavite
Cebu
Basilan
Eastern Samar
Davao
Davao del Sur
Davao Oriental
Ifugao
Ilocos Norte
Ilocos Sur
Iloilo
Isabela
Kalinga-Apayao
Laguna
Lanao del Norte
Lanao del Sur
La Union
Leyte
Marinduque
Masbate
Mindoro Occidental
Mindoro Oriental
Misamis Occidental
Misamis Oriental
Mountain
Negros Occidental
Negros Oriental
Nueva Ecija
Nueva Vizcaya
Palawan
Pampanga
Pangasinan
Rizal
Romblon
Samar
Maguindanao
North Cotabato
Sorsogon
Southern Leyte
Sulu
Surigao del Norte
Surigao del Sur
Tarlac
Zambales
Zamboanga del Norte
Zamboanga del Sur
Northern Samar
Quirino
Siquijor
South Cotabato
Sultan Kudarat
Tawitawi
Angeles
Bacolod
Bago
Baguio
Bais
Basilan City
Batangas City
Butuan
Cabanatuan
Cadiz
Cagayan de Oro
Calbayog
Caloocan
Canlaon
Cavite City
Cebu City
Cotabato
Dagupan
Danao
Dapitan
Davao City
Dipolog
Dumaguete
General Santos
Gingoog
Iligan
Iloilo City
Iriga
La Carlota
Laoag
Lapu-Lapu
Legaspi
Lipa
Lucena
Mandaue
Manila
Marawi
Naga
Olongapo
Ormoc
Oroquieta
Ozamis
Pagadian
Palayan
Pasay
Puerto Princesa
Quezon City
Roxas
San Carlos
San Carlos
San Jose
San Pablo
Silay
Surigao
Tacloban
Tagaytay
Tagbilaran
Tangub
Toledo
Trece Martires
Zamboanga
Aurora
Quezon
Negros Occidental
Compostela Valley
Davao del Norte
Kalinga
Malaybalay
Zambales
San Jose del Monte
San Juan
Santiago
Sarangani
Sipalay
Surigao del Norte
Zamboanga
Federally Administered Tribal Areas
Balochistan
North-West Frontier
Punjab
Sindh
Azad Kashmir
Northern Areas
Islamabad
Dolnoslaskie
Kujawsko-Pomorskie
Lodzkie
Lubelskie
Lubuskie
Malopolskie
Mazowieckie
Opolskie
Podkarpackie
Podlaskie
Pomorskie
Slaskie
Swietokrzyskie
Warminsko-Mazurskie
Wielkopolskie
Zachodniopomorskie
Gaza
West Bank
Aveiro
Beja
Braga
Braganca
Castelo Branco
Coimbra
Evora
Faro
Madeira
Guarda
Leiria
Lisboa
Portalegre
Porto
Santarem
Setubal
Viana do Castelo
Vila Real
Viseu
Azores
Alto Parana
Amambay
Boqueron
Caaguazu
Caazapa
Central
Concepcion
Cordillera
Guaira
Itapua
Misiones
Neembucu
Paraguari
Presidente Hayes
San Pedro
Canindeyu
Chaco
Nueva Asuncion
Alto Paraguay
Ad Dawhah
Al Ghuwariyah
Al Jumaliyah
Al Khawr
Al Wakrah Municipality
Ar Rayyan
Madinat ach Shamal
Umm Salal
Al Wakrah
Jariyan al Batnah
Umm Sa'id
Alba
Arad
Arges
Bacau
Bihor
Bistrita-Nasaud
Botosani
Braila
Brasov
Bucuresti
Buzau
Caras-Severin
Cluj
Constanta
Covasna
Dambovita
Dolj
Galati
Gorj
Harghita
Hunedoara
Ialomita
Iasi
Maramures
Mehedinti
Mures
Neamt
Olt
Prahova
Salaj
Satu Mare
Sibiu
Suceava
Teleorman
Timis
Tulcea
Vaslui
Valcea
Vrancea
Calarasi
Giurgiu
Ilfov
Kosovo
Vojvodina
Adygeya
Aginsky Buryatsky AO
Gorno-Altay
Altaisky krai
Amur
Arkhangel'sk
Astrakhan'
Bashkortostan
Belgorod
Bryansk
Buryat
Chechnya
Chelyabinsk
Chita
Chukot
Chuvashia
Dagestan
Evenk
Ingush
Irkutsk
Ivanovo
Kabardin-Balkar
Kaliningrad
Kalmyk
Kaluga
Kamchatka
Karachay-Cherkess
Karelia
Kemerovo
Khabarovsk
Khakass
Khanty-Mansiy
Kirov
Komi
Komi-Permyak
Koryak
Kostroma
Krasnodar
Krasnoyarsk
Kurgan
Kursk
Leningrad
Lipetsk
Magadan
Mariy-El
Mordovia
Moskva
Moscow City
Murmansk
Nenets
Nizhegorod
Novgorod
Novosibirsk
Omsk
Orenburg
Orel
Penza
Perm'
Primor'ye
Pskov
Rostov
Ryazan'
Sakha
Sakhalin
Samara
Saint Petersburg City
Saratov
North Ossetia
Smolensk
Stavropol'
Sverdlovsk
Tambovskaya oblast
Tatarstan
Taymyr
Tomsk
Tula
Tver'
Tyumen'
Tuva
Udmurt
Ul'yanovsk
Ust-Orda Buryat
Vladimir
Volgograd
Vologda
Voronezh
Yamal-Nenets
Yaroslavl'
Yevrey
Permskiy Kray
Krasnoyarskiy Kray
Kamchatskiy Kray
Zabaykal'skiy Kray
Butare
Gitarama
Kibungo
Kigali
Est
Kigali
Nord
Ouest
Sud
Al Bahah
Al Madinah
Ash Sharqiyah
Al Qasim
Ar Riyad
Asir Province
Ha'il
Makkah
Al Hudud ash Shamaliyah
Najran
Jizan
Tabuk
Al Jawf
Malaita
Guadalcanal
Isabel
Makira
Temotu
Central
Western
Choiseul
Rennell and Bellona
Anse aux Pins
Anse Boileau
Anse Etoile
Anse Louis
Anse Royale
Baie Lazare
Baie Sainte Anne
Beau Vallon
Bel Air
Bel Ombre
Cascade
Glacis
Grand' Anse
Grand' Anse
La Digue
La Riviere Anglaise
Mont Buxton
Mont Fleuri
Plaisance
Pointe La Rue
Port Glaud
Saint Louis
Takamaka
Al Wusta
Al Istiwa'iyah
Al Khartum
Ash Shamaliyah
Ash Sharqiyah
Bahr al Ghazal
Darfur
Kurdufan
Upper Nile
Al Wahadah State
Central Equatoria State
Blekinge Lan
Gavleborgs Lan
Gotlands Lan
Hallands Lan
Jamtlands Lan
Jonkopings Lan
Kalmar Lan
Dalarnas Lan
Kronobergs Lan
Norrbottens Lan
Orebro Lan
Ostergotlands Lan
Sodermanlands Lan
Uppsala Lan
Varmlands Lan
Vasterbottens Lan
Vasternorrlands Lan
Vastmanlands Lan
Stockholms Lan
Skane Lan
Vastra Gotaland
Ascension
Saint Helena
Tristan da Cunha
Ajdovscina Commune
Beltinci Commune
Bled Commune
Bohinj Commune
Borovnica Commune
Bovec Commune
Brda Commune
Brezice Commune
Brezovica Commune
Celje Commune
Cerklje na Gorenjskem Commune
Cerknica Commune
Cerkno Commune
Crensovci Commune
Crna na Koroskem Commune
Crnomelj Commune
Divaca Commune
Dobrepolje Commune
Dol pri Ljubljani Commune
Dornava Commune
Dravograd Commune
Duplek Commune
Gorenja vas-Poljane Commune
Gorisnica Commune
Gornja Radgona Commune
Gornji Grad Commune
Gornji Petrovci Commune
Grosuplje Commune
Hrastnik Commune
Hrpelje-Kozina Commune
Idrija Commune
Ig Commune
Ilirska Bistrica Commune
Ivancna Gorica Commune
Izola-Isola Commune
Jursinci Commune
Kanal Commune
Kidricevo Commune
Kobarid Commune
Kobilje Commune
Komen Commune
Koper-Capodistria Urban Commune
Kozje Commune
Kranj Commune
Kranjska Gora Commune
Krsko Commune
Kungota Commune
Lasko Commune
Ljubljana Urban Commune
Ljubno Commune
Logatec Commune
Loski Potok Commune
Lukovica Commune
Medvode Commune
Menges Commune
Metlika Commune
Mezica Commune
Mislinja Commune
Moravce Commune
Moravske Toplice Commune
Mozirje Commune
Murska Sobota Urban Commune
Muta Commune
Naklo Commune
Nazarje Commune
Nova Gorica Urban Commune
Odranci Commune
Ormoz Commune
Osilnica Commune
Pesnica Commune
Pivka Commune
Podcetrtek Commune
Postojna Commune
Puconci Commune
Race-Fram Commune
Radece Commune
Radenci Commune
Radlje ob Dravi Commune
Radovljica Commune
Rogasovci Commune
Rogaska Slatina Commune
Rogatec Commune
Semic Commune
Sencur Commune
Sentilj Commune
Sentjernej Commune
Sevnica Commune
Sezana Commune
Skocjan Commune
Skofja Loka Commune
Skofljica Commune
Slovenj Gradec Urban Commune
Slovenske Konjice Commune
Smarje pri Jelsah Commune
Smartno ob Paki Commune
Sostanj Commune
Starse Commune
Store Commune
Sveti Jurij Commune
Tolmin Commune
Trbovlje Commune
Trebnje Commune
Trzic Commune
Turnisce Commune
Velenje Urban Commune
Velike Lasce Commune
Vipava Commune
Vitanje Commune
Vodice Commune
Vrhnika Commune
Vuzenica Commune
Zagorje ob Savi Commune
Zavrc Commune
Zelezniki Commune
Ziri Commune
Zrece Commune
Benedikt Commune
Bistrica ob Sotli Commune
Bloke Commune
Braslovce Commune
Cankova Commune
Cerkvenjak Commune
Destrnik Commune
Dobje Commune
Dobrna Commune
Dobrova-Horjul-Polhov Gradec Commune
Dobrovnik-Dobronak Commune
Dolenjske Toplice Commune
Domzale Commune
Grad Commune
Hajdina Commune
Hoce-Slivnica Commune
Hodos-Hodos Commune
Horjul Commune
Jesenice Commune
Jezersko Commune
Kamnik Commune
Kocevje Commune
Komenda Commune
Kostel Commune
Krizevci Commune
Kuzma Commune
Lenart Commune
Lendava-Lendva Commune
Litija Commune
Ljutomer Commune
Loska Dolina Commune
Lovrenc na Pohorju Commune
Luce Commune
Majsperk Commune
Maribor Commune
Markovci Commune
Miklavz na Dravskem polju Commune
Miren-Kostanjevica Commune
Mirna Pec Commune
Novo mesto Urban Commune
Oplotnica Commune
Piran-Pirano Commune
Podlehnik Commune
Podvelka Commune
Polzela Commune
Prebold Commune
Preddvor Commune
Prevalje Commune
Ptuj Urban Commune
Ravne na Koroskem Commune
Razkrizje Commune
Ribnica Commune
Ribnica na Pohorju Commune
Ruse Commune
Salovci Commune
Selnica ob Dravi Commune
Sempeter-Vrtojba Commune
Sentjur pri Celju Commune
Slovenska Bistrica Commune
Smartno pri Litiji Commune
Sodrazica Commune
Solcava Commune
Sveta Ana Commune
Sveti Andraz v Slovenskih goricah Commune
Tabor Commune
Tisina Commune
Trnovska vas Commune
Trzin Commune
Velika Polana Commune
Verzej Commune
Videm Commune
Vojnik Commune
Vransko Commune
Zalec Commune
Zetale Commune
Zirovnica Commune
Zuzemberk Commune
Apace Commune
Cirkulane Commune
Banska Bystrica
Bratislava
Kosice
Nitra
Presov
Trencin
Trnava
Zilina
Eastern
Northern
Southern
Western Area
Acquaviva
Chiesanuova
Domagnano
Faetano
Fiorentino
Borgo Maggiore
San Marino
Monte Giardino
Serravalle
Dakar
Diourbel
Tambacounda
Thies
Fatick
Kaolack
Kolda
Ziguinchor
Louga
Saint-Louis
Matam
Bakool
Banaadir
Bari
Bay
Galguduud
Gedo
Hiiraan
Jubbada Dhexe
Jubbada Hoose
Mudug
Nugaal
Sanaag
Shabeellaha Dhexe
Shabeellaha Hoose
Woqooyi Galbeed
Nugaal
Togdheer
Woqooyi Galbeed
Awdal
Sool
Brokopondo
Commewijne
Coronie
Marowijne
Nickerie
Para
Paramaribo
Saramacca
Sipaliwini
Wanica
Principe
Sao Tome
Ahuachapan
Cabanas
Chalatenango
Cuscatlan
La Libertad
La Paz
La Union
Morazan
San Miguel
San Salvador
Santa Ana
San Vicente
Sonsonate
Usulutan
Al Hasakah
Al Ladhiqiyah
Al Qunaytirah
Ar Raqqah
As Suwayda'
Dar
Dayr az Zawr
Rif Dimashq
Halab
Hamah
Hims
Idlib
Dimashq
Tartus
Hhohho
Lubombo
Manzini
Shiselweni
Praslin
Batha
Biltine
Borkou-Ennedi-Tibesti
Chari-Baguirmi
Guera
Kanem
Lac
Logone Occidental
Logone Oriental
Mayo-Kebbi
Moyen-Chari
Ouaddai
Salamat
Tandjile
Centrale
Kara
Maritime
Plateaux
Savanes
Mae Hong Son
Chiang Mai
Chiang Rai
Nan
Lamphun
Lampang
Phrae
Tak
Sukhothai
Uttaradit
Kamphaeng Phet
Phitsanulok
Phichit
Phetchabun
Uthai Thani
Nakhon Sawan
Nong Khai
Loei
Sakon Nakhon
Nakhon Phanom
Khon Kaen
Kalasin
Maha Sarakham
Roi Et
Chaiyaphum
Nakhon Ratchasima
Buriram
Surin
Sisaket
Narathiwat
Chai Nat
Sing Buri
Lop Buri
Ang Thong
Phra Nakhon Si Ayutthaya
Saraburi
Nonthaburi
Pathum Thani
Krung Thep
Phayao
Samut Prakan
Nakhon Nayok
Chachoengsao
Prachin Buri
Chon Buri
Rayong
Chanthaburi
Trat
Kanchanaburi
Suphan Buri
Ratchaburi
Nakhon Pathom
Samut Songkhram
Samut Sakhon
Phetchaburi
Prachuap Khiri Khan
Chumphon
Ranong
Surat Thani
Phangnga
Phuket
Krabi
Nakhon Si Thammarat
Trang
Phatthalung
Satun
Songkhla
Pattani
Yala
Ubon Ratchathani
Yasothon
Nakhon Phanom
Prachin Buri
Ubon Ratchathani
Udon Thani
Amnat Charoen
Mukdahan
Nong Bua Lamphu
Sa Kaeo
Kuhistoni Badakhshon
Khatlon
Sughd
Ahal
Balkan
Dashoguz
Lebap
Mary
Kasserine
Kairouan
Jendouba
Qafsah
El Kef
Al Mahdia
Al Munastir
Bajah
Bizerte
Nabeul
Siliana
Sousse
Ben Arous
Madanin
Gabes
Kebili
Sfax
Sidi Bou Zid
Tataouine
Tozeur
Tunis
Zaghouan
Aiana
Manouba
Ha
Tongatapu
Vava
Adiyaman
Afyonkarahisar
Agri
Amasya
Antalya
Artvin
Aydin
Balikesir
Bilecik
Bingol
Bitlis
Bolu
Burdur
Bursa
Canakkale
Corum
Denizli
Diyarbakir
Edirne
Elazig
Erzincan
Erzurum
Eskisehir
Giresun
Hatay
Mersin
Isparta
Istanbul
Izmir
Kastamonu
Kayseri
Kirklareli
Kirsehir
Kocaeli
Kutahya
Malatya
Manisa
Kahramanmaras
Mugla
Mus
Nevsehir
Ordu
Rize
Sakarya
Samsun
Sinop
Sivas
Tekirdag
Tokat
Trabzon
Tunceli
Sanliurfa
Usak
Van
Yozgat
Ankara
Gumushane
Hakkari
Konya
Mardin
Nigde
Siirt
Aksaray
Batman
Bayburt
Karaman
Kirikkale
Sirnak
Adana
Cankiri
Gaziantep
Kars
Zonguldak
Ardahan
Bartin
Igdir
Karabuk
Kilis
Osmaniye
Yalova
Duzce
Arima
Caroni
Mayaro
Nariva
Port-of-Spain
Saint Andrew
Saint David
Saint George
Saint Patrick
San Fernando
Tobago
Victoria
Pwani
Dodoma
Iringa
Kigoma
Kilimanjaro
Lindi
Mara
Mbeya
Morogoro
Mtwara
Mwanza
Pemba North
Ruvuma
Shinyanga
Singida
Tabora
Tanga
Kagera
Pemba South
Zanzibar Central
Zanzibar North
Dar es Salaam
Rukwa
Zanzibar Urban
Arusha
Manyara
Cherkas'ka Oblast'
Chernihivs'ka Oblast'
Chernivets'ka Oblast'
Dnipropetrovs'ka Oblast'
Donets'ka Oblast'
Ivano-Frankivs'ka Oblast'
Kharkivs'ka Oblast'
Khersons'ka Oblast'
Khmel'nyts'ka Oblast'
Kirovohrads'ka Oblast'
Krym
Kyyiv
Kyyivs'ka Oblast'
Luhans'ka Oblast'
L'vivs'ka Oblast'
Mykolayivs'ka Oblast'
Odes'ka Oblast'
Poltavs'ka Oblast'
Rivnens'ka Oblast'
Sevastopol'
Sums'ka Oblast'
Ternopil's'ka Oblast'
Vinnyts'ka Oblast'
Volyns'ka Oblast'
Zakarpats'ka Oblast'
Zaporiz'ka Oblast'
Zhytomyrs'ka Oblast'
Apac
Bundibugyo
Bushenyi
Gulu
Hoima
Jinja
Kalangala
Kampala
Kamuli
Kapchorwa
Kasese
Kibale
Kiboga
Kisoro
Kotido
Kumi
Lira
Masindi
Mbarara
Mubende
Nebbi
Ntungamo
Pallisa
Rakai
Adjumani
Bugiri
Busia
Katakwi
Luwero
Masaka
Moyo
Nakasongola
Sembabule
Tororo
Arua
Iganga
Kabarole
Kaberamaido
Kamwenge
Kanungu
Kayunga
Kitgum
Kyenjojo
Mayuge
Mbale
Moroto
Mpigi
Mukono
Nakapiripirit
Pader
Rukungiri
Sironko
Soroti
Wakiso
Yumbe
Armed Forces Americas
Armed Forces Europe
Alaska
Alabama
Armed Forces Pacific
Arkansas
American Samoa
Arizona
California
Colorado
Connecticut
District of Columbia
Delaware
Florida
Federated States of Micronesia
Georgia
Guam
Hawaii
Iowa
Idaho
Illinois
Indiana
Kansas
Kentucky
Louisiana
Massachusetts
Maryland
Maine
Marshall Islands
Michigan
Minnesota
Missouri
Northern Mariana Islands
Mississippi
Montana
North Carolina
North Dakota
Nebraska
New Hampshire
New Jersey
New Mexico
Nevada
New York
Ohio
Oklahoma
Oregon
Pennsylvania
Palau
Rhode Island
South Carolina
South Dakota
Tennessee
Texas
Utah
Virginia
Virgin Islands
Vermont
Washington
Wisconsin
West Virginia
Wyoming
Artigas
Canelones
Cerro Largo
Colonia
Durazno
Flores
Florida
Lavalleja
Maldonado
Montevideo
Paysandu
Rio Negro
Rivera
Rocha
Salto
San Jose
Soriano
Tacuarembo
Treinta y Tres
Andijon
Bukhoro
Farghona
Jizzakh
Khorazm
Namangan
Nawoiy
Qashqadaryo
Qoraqalpoghiston
Samarqand
Sirdaryo
Surkhondaryo
Toshkent
Toshkent
Charlotte
Saint Andrew
Saint David
Saint George
Saint Patrick
Grenadines
Amazonas
Anzoategui
Apure
Aragua
Barinas
Bolivar
Carabobo
Cojedes
Delta Amacuro
Falcon
Guarico
Lara
Merida
Miranda
Monagas
Nueva Esparta
Portuguesa
Sucre
Tachira
Trujillo
Yaracuy
Zulia
Dependencias Federales
Distrito Federal
Vargas
An Giang
Ben Tre
Cao Bang
Dong Thap
Hai Phong
Ho Chi Minh
Kien Giang
Lam Dong
Long An
Quang Ninh
Son La
Tay Ninh
Thanh Hoa
Thai Binh
Tien Giang
Lang Son
Dong Nai
Ha Noi
Ba Ria-Vung Tau
Binh Dinh
Binh Thuan
Gia Lai
Ha Giang
Ha Tinh
Hoa Binh
Khanh Hoa
Kon Tum
Nghe An
Ninh Binh
Ninh Thuan
Phu Yen
Quang Binh
Quang Ngai
Quang Tri
Soc Trang
Thua Thien-Hue
Tra Vinh
Tuyen Quang
Vinh Long
Yen Bai
Bac Giang
Bac Kan
Bac Lieu
Bac Ninh
Binh Duong
Binh Phuoc
Ca Mau
Da Nang
Hai Duong
Ha Nam
Hung Yen
Nam Dinh
Phu Tho
Quang Nam
Thai Nguyen
Vinh Phuc
Can Tho
Dac Lak
Lai Chau
Lao Cai
Dak Nong
Dien Bien
Hau Giang
Ambrym
Aoba
Torba
Efate
Epi
Malakula
Paama
Pentecote
Sanma
Shepherd
Tafea
Malampa
Penama
Shefa
Aiga-i-le-Tai
Atua
Fa
Gaga
Va
Gagaifomauga
Palauli
Satupa
Tuamasaga
Vaisigano
Abyan
Adan
Al Mahrah
Hadramawt
Shabwah
Lahij
Al Bayda'
Al Hudaydah
Al Jawf
Al Mahwit
Dhamar
Hajjah
Ibb
Ma'rib
Sa'dah
San'a'
Taizz
Ad Dali
Amran
Al Bayda'
Al Jawf
Hajjah
Ibb
Lahij
Taizz
North-Western Province
KwaZulu-Natal
Free State
Eastern Cape
Gauteng
Mpumalanga
Northern Cape
Limpopo
North-West
Western Cape
Western
Central
Eastern
Luapula
Northern
North-Western
Southern
Copperbelt
Lusaka
Manicaland
Midlands
Mashonaland Central
Mashonaland East
Mashonaland West
Matabeleland North
Matabeleland South
Masvingo
Bulawayo
Harare
Anguilla
Netherlands Antilles
Asia-Pacific region
Antarctica
American Samoa
Aruba
Åaland Island
Saint Barthélemy
Bouvet Island
Cocos (Keeling) Islands
Cook Islands
Christmas Island
Western Sahara
European Union
Falkland Islands
Faroe Islands
France, Metropolitan
French Guiana
Guernsey
Gibraltar
Guadeloupe
South Georgia and the South Sandwich Islands
Guam
Heard Island and McDonald Islands
Isle of Man
British Indian Ocean Territory
Jersey
Montenegro
Saint Martin (France)
Marshall islands
Northern Mariana Islands
Martinique
Malta
New Caledonia
Norfolk Island
Neutral Zone
Niue
French polynesia
Saint-Pierre and Miquelon
Pitcairn Islands
Puerto Rico
Palau
Réunion
Singapore
Svalbard
Turks & Caicos Islands
French Southern Territories
Tokelau
Timor-Leste (East Timor)
Tuvalu
United States Minor Outlying Islands
Vatican
British Virgin Islands
United States Virgin Islands
Wallis and Futuna
Mayotte
ARIN
AfriNIC
APNIC
CNNIC
LACNIC
RIPE

```