#!/usr/bin/perl

my %r1;

while (<>) {
    chop;
    my ($code, $cntry, $rgn) = split("\t");
   
    my $C = uc $code;
    $r1{$rgn}{$C} = $cntry;
}

for $region( sort keys %r1 ) { 
    print "# $region\n\n"; 
    my @arr;
    for $c( sort keys %{ $r1{$region}} ) {
       push(@arr,$c);
       my $cntry = $r1{$region}{$c};
       print "header   RELAYCOUNTRY_$c X-Relay-Countries =~ /$c/\n";
       print "describe RELAYCOUNTRY_$c Relayed through $cntry\n";
       print "score    RELAYCOUNTRY_$c 0.01\n\n";
       print "\n";
    }

    my $str = join('|', @arr);
    my $rulename = "RELAYCOUNTRY_" . uc($region);
    $rulename =~ s/ /_/g;    
    print "header   $rulename X-Relay-Countries =~ /($str)/\n";
    print "describe $rulename Relayed through $region\n";
    print "score    $rulename 0.01\n\n";

    print "\n"; 
}



# cat countries.txt 
AF	Afghanistan	Southern Asia
AX	Aland Islands	Northern Europe
AL	Albania	Southern Europe
DZ	Algeria	Northern Africa
AS	American Samoa	Polynesia
AD	Andorra	Southern Europe
AO	Angola	Middle Africa
AI	Anguilla	Caribbean
AG	Antigua and Barbuda	Caribbean
AR	Argentina	South America
AM	Armenia	Western Asia
AW	Aruba	Caribbean
AU	Australia	Australia and New Zealand
AT	Austria	Western Europe
AZ	Azerbaijan	Western Asia
BH	Bahrain	Western Asia
BD	Bangladesh	Southern Asia
BB	Barbados	Caribbean
BY	Belarus	Eastern Europe
BE	Belgium	Western Europe
BZ	Belize	Central America
BJ	Benin	Western Africa
BM	Bermuda	Nothern America
BT	Bhutan	Southern Asia
BO	Bolivia	South America
BQ	Bonaire, Sint Eustatius and Saba	Caribbean
BA	Bosnia and Herzegovina	Southern Europe
BW	Botswana	Southern Africa
BV	Bouvet Island	South America
BR	Brazil	South America
BN	Brunei	South_Eastern Asia
BG	Bulgaria	Eastern Europe
BF	Burkina Faso	Western Africa
BI	Burundi	Eastern Africa
CV	Cabo Verde	Western Africa
KH	Cambodia	South_Eastern Asia
CM	Cameroon	Middle Africa
CA	Canada	Nothern America
TD	Chad	Middle Africa
CL	Chile	South America
TW	China	Eastern Asia
CX	Christmas Island	Australia and New Zealand
CC	Cocos (Keeling) Islands	Australia and New Zealand
CO	Colombia	South America
KM	Comoros	Eastern Africa
CG	Congo	Middle Africa
CR	Costa Rica	Central America
CI	Cote d'Ivoire	Western Africa
HR	Croatia	Southern Europe
CU	Cuba	Caribbean
CW	Curacao	Caribbean
CY	Cyprus	Western Asia
CD	Democratic Republic of the Congo	Middle Africa
DK	Denmark	Northern Europe
DJ	Djibouti	Eastern Africa
DM	Dominica	Caribbean
DO	Dominican Republic	Caribbean
EC	Ecuador	South America
EG	Egypt	Northern Africa
SV	El Salvador	Central America
GQ	Equatorial Guinea	Middle Africa
ER	Eritrea	Eastern Africa
EE	Estonia	Northern Europe
SZ	Eswatini	Southern Africa
ET	Ethiopia	Eastern Africa
FJ	Fiji	Melanesia
FI	Finland	Northern Europe
FR	France	Western Europe
PF	French Polynesia	Polynesia
GA	Gabon	Middle Africa
GE	Georgia	Western Asia
DE	Germany	Western Europe
GH	Ghana	Western Africa
GI	Gibraltar	Southern Europe
GL	Greenland	Nothern America
GD	Grenada	Caribbean
GP	Guadeloupe	Caribbean
GU	Guam	Micronesia
GT	Guatemala	Central America
GG	Guernsey	Northern Europe
GN	Guinea	Western Africa
GW	Guinea-Bissau	Western Africa
GY	Guyana	South America
GF	Guyane	South America
HT	Haiti	Caribbean
HM	Heard Island and McDonald Islands	Australia and New Zealand
HN	Honduras	Central America
HK	Hong Kong	Eastern Asia
HU	Hungary	Eastern Europe
IS	Iceland	Northern Europe
IN	India	Southern Asia
ID	Indonesia	South_Eastern Asia
IR	Iran	Southern Asia
IQ	Iraq	Western Asia
IE	Ireland	Northern Europe
IL	Israel	Western Asia
IT	Italy	Southern Europe
JM	Jamaica	Caribbean
JP	Japan	Eastern Asia
JE	Jersey	Northern Europe
JO	Jordan	Western Asia
KZ	Kazakhstan	Central Asia
KE	Kenya	Eastern Africa
KI	Kiribati	Micronesia
KR	South Korea	Eastern Asia
KG	Kryzgstan	Central Asia
KW	Kuwait	Western Asia
LA	Laos	South_Eastern Asia
LV	Latvia	Northern Europe
LB	Lebanon	Western Asia
LS	Lesotho	Southern Africa
LR	Liberia	Western Africa
LY	Libya	Northern Africa
LI	Liechtenstein	Western Europe
LT	Lithuania	Northern Europe
LU	Luxembourg	Western Europe
MG	Madagascar	Eastern Africa
MW	Malawi	Eastern Africa
MY	Malaysia	South_Eastern Asia
MV	Maldives	Southern Asia
ML	Mali	Western Africa
MT	Malta	Southern Europe
MQ	Martinique	Caribbean
MR	Mauritania	Western Africa
MU	Mauritius	Eastern Africa
YT	Mayotte	Eastern Africa
MX	Mexico	Central America
FM	Micronesia	Micronesia
MD	Moldova	Eastern Europe
MC	Monaco	Western Europe
MN	Mongolia	Eastern Asia
ME	Montenegro	Southern Europe
MS	Montserrat	Caribbean
MA	Morocco	Northern Africa
MZ	Mozambique	Eastern Africa
MM	Myanmar	South_Eastern Asia
NA	Namibia	Southern Africa
NR	Nauru	Micronesia
NP	Nepal	Southern Asia
NL	Netherlands	Western Europe
NC	New Caledonia	Melanesia
NZ	New Zealand	Australia and New Zealand
NI	Nicaragua	Central America
NG	Nigeria	Western Africa
NU	Niue	Polynesia
NF	Norfolk Island	Australia and New Zealand
MK	North Macedonia	Southern Europe
MP	Northern Mariana Islands	Micronesia
NO	Norway	Northern Europe
OM	Oman	Western Asia
PK	Pakistan	Southern Asia
PW	Palau	Micronesia
PS	Palestine	Western Asia
PA	Panama	Central America
PG	Papua New Guinea	Melanesia
PY	Paraguay	South America
PE	Peru	South America
PH	Philippines	South_Eastern Asia
PL	Poland	Eastern Europe
PT	Portugal	Southern Europe
PR	Puerto Rico	Caribbean
QA	Qatar	Western Asia
RE	Reunion	Eastern Africa
RO	Romania	Eastern Europe
RU	Russia	Eastern Europe
RW	Rwanda	Eastern Africa
SH	Saint Helena, Ascension and Tristan da Cunha	Western Africa
KN	Saint Kitts and Nevis	Caribbean
LC	Saint Lucia	Caribbean
VC	Saint Vincent and the Grenadines	Caribbean
BL	Saint-Barthelemy	Caribbean
MF	Saint-Martin	Caribbean
PM	Saint-Pierre and Miquelon	Nothern America
WS	Samoa	Polynesia
SM	San Marino	Southern Europe
ST	Sao Tome and Principe	Middle Africa
SA	Saudi Arabia	Western Asia
SN	Senegal	Western Africa
RS	Serbia	Southern Europe
SC	Seychelles	Eastern Africa
SL	Sierra Leone	Western Africa
SG	Singapore	South_Eastern Asia
SX	Sint Maarten	Caribbean
SI	Slovenia	Southern Europe
SO	Somalia	Eastern Africa
ZA	South Africa	Southern Africa
GS	South Georgia and the South Sandwich Islands	South America
AQ	South of the 60th Parallel	Antartica
SS	South Sudan	Eastern Africa
ES	Spain	Southern Europe
LK	Sri Lanka	Southern Asia
SD	Sudan	Northern Africa
SR	Suriname	South America
SJ	Svalbard and Jan Mayen	Northern Europe
SE	Sweden	Northern Europe
CH	Swiss Confederation	Western Europe
SY	Syria	Western Asia
TJ	Tajikistan	Central Asia
TZ	Tanzania	Eastern Africa
TH	Thailand	South_Eastern Asia
BS	Bahamas	Caribbean
IO	The British Indian Ocean Territory	Eastern Africa
KY	Cayman Islands	Caribbean
CF	The Central African Republic	Middle Africa
CK	Cook Islands	Polynesia
CZ	Czech Republic	Eastern Europe
KP	The Democratic People's Republic of Korea	Eastern Asia
FK	Falkland Islands	South America
FO	Faroe Islands	Northern Europe
TF	The French Southern and Antarctic Lands	Eastern Africa
GM	Gambia	Western Africa
IM	Isle of Man	Northern Europe
MO	The Macao Special Administrative Region of China	Eastern Asia
MH	Marshall Islands	Micronesia
NE	Nigeria	Western Africa
CN	The People's Republic of China	Eastern Asia
PN	The Pitcairn, Henderson, Ducie and Oeno Islands	Polynesia
EH	The Sahrawi Arab Democratic Republic	Western Africa
SK	The Slovak Republic	Eastern Europe
SB	Solomon Islands	Melanesia
TC	Turks and Caicos Islands	Caribbean
VG	British Virgin Islands	Caribbean
VI	US Virgin Islands	Caribbean
TL	Timor-Leste	South_Eastern Asia
TG	Togo	Western Africa
TK	Tokelau	Polynesia
TO	Tonga	Polynesia
TT	Trinidad and Tobago	Caribbean
TN	Tunisia	Northern Africa
TR	Turkey	Western Asia
TM	Turkmenistan	Central Asia
TV	Tuvalu	Polynesia
UG	Uganda	Eastern Africa
UA	Ukraine	Eastern Europe
AE	United Arab Emirates	Western Asia
GB	United Kingdom	Northern Europe
US	United States	Nothern America
UY	Uruguay	South America
UM	US Minor Outlying Islands	Micronesia
UZ	Uzbekistan	Central Asia
VU	Vanuatu	Melanesia
VA	Vatican City	Southern Europe
VE	Venezuela	South America
VN	Viet Nam	South_Eastern Asia
WF	Wallis and Futuna Islands	Polynesia
YE	Yemen	Western Asia
ZM	Zambia	Eastern Africa
ZW	Zimbabwe	Eastern Africa
GR	Greece	Southern Europe

