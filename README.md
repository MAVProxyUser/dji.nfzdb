# dji.nfzdb
DJI No Fly Zone SQLite DB as extracted from DJI Go app. 

![Meet DJI's Brendan Schulman, also known as @dronelaws](https://raw.githubusercontent.com/MAVProxyUser/dji.nfzdb/master/brendan_dji_lord_of_war.png)

Variants:
.field private static final ENCRYPT_FLYFORBID_DB_NAME:Ljava/lang/String; = "dji.nfzdb.encrypt"
.field private static final FLYFORBID_DB_NAME:Ljava/lang/String; = "dji.nfzdb"

Plaintext - DJI GO--For products before P4
https://play.google.com/store/apps/details?id=dji.pilot&hl=en

To extract to .csv use SQLite

$ sqlite3 dji.nfzdb
SQLite version 3.16.2 2017-01-06 16:32:41
Enter ".help" for usage hints.
sqlite> .mode csv
sqlite> .headers on
sqlite> .once dji.nfzdb.csv
sqlite> select * from dji_midware_data_forbid_FlyForbidElement;

Encrypted - DJI GO 4--For drones since P4
https://play.google.com/store/apps/details?id=dji.go.v4&hl=en

This version is making use of SQLCipher, a technique will need to be created to extract / verify the encrypted DB vs the plaintext 
https://www.zetetic.net/sqlcipher/sqlcipher-api/

The password is currently unknown, but should be easy to extract. 

.method public static generateEncryptDb(Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;)V
...
    const-string/jumbo v1, "ATTACH DATABASE \'%s\' AS encrypted KEY \'%s\'"
...
    const-string/jumbo v1, "select sqlcipher_export(\'encrypted\')"
...
    const-string/jumbo v1, "DETACH DATABASE encrypted"


DJI No FLy Zones appear in several "levels":

WARNING, STRONG_WARNING, and CAN_NOT_UNLIMIT

You can view data points in each level as follows
select * from dji_midware_data_forbid_FlyForbidElement where level = 0;
select * from dji_midware_data_forbid_FlyForbidElement where level = 2;

The updated_at field is stored in Unix Time Stamp format... You can decode it at https://currentmillis.com
As an example below 1488046048 is Saturday, 25-Feb-17 18:07:28 UTC or when "NFZ 1 of Iraq" was first added. 

You should note that several War Zones have been added. 

$ sqlite3 dji.nfzdb "select * from dji_midware_data_forbid_FlyForbidElement where name like '%Iraq%' or name like '%Syria%';"
6867|1488186010|0|0|0|0||37.938878||10721|63157|760|NFZ 21 of Syria|0|1|36.294002|2
6868|1488186025|0|0|0|0||39.243141||10722|63157|760|NFZ 14 of Syria|0|1|36.230986|2
6869|1488186033|0|0|0|0||40.577947||10723|63157|760|NFZ 15 of Syria|0|1|36.17336|2
6870|1488190904|0|0|0|0||41.907051||10724|63157|368|NFZ 30 of Iraq|0|1|36.12908|2
6871|1488190974|0|0|0|0||43.083859||10725|63157|368|NFZ 31 of Iraq|0|1|35.884886|2
6872|1488191022|0|0|0|0||43.095727||10728|63157|368|NFZ 32 of Iraq|0|1|34.856721|2
6873|1488191032|0|0|0|0||43.510918||10729|63157|368|NFZ 33 of Iraq|0|1|33.861245|2
6874|1488191044|0|0|0|0||44.256448||10730|63157|368|NFZ 34 of Iraq|0|1|34.349456|2
6875|1488191052|0|0|0|0||42.336301||10731|63157|368|NFZ 35 of Iraq|0|1|34.198247|2
6876|1488191066|0|0|0|0||42.160025||10732|63157|368|NFZ 36 of Iraq|0|1|35.169207|2
6877|1488186040|0|0|0|0||41.104546||10733|63157|760|NFZ 16 of Syria|0|1|35.281657|2
6878|1488191071|0|0|0|0||41.319438||10734|63157|368|NFZ 37 of Iraq|0|1|34.429223|2
6879|1488186044|0|0|0|0||39.895059||10735|63157|760|NFZ 17 of Syria|0|1|35.407979|2
6880|1488186052|0|0|0|0||38.677426||10736|63157|760|NFZ 18 of Syria|0|1|35.478652|2
6881|1488186074|0|0|0|0||40.242728||10737|63157|760|NFZ 22 of Syria|0|1|34.569481|2
6882|1488186082|0|0|0|0||39.14013||10738|63157|760|NFZ 19 of Syria|0|1|34.682764|2
6883|1488186097|0|0|0|0||37.88221||10739|63157|760|NFZ 20 of Syria|0|1|34.777498|2
7769|1488191081|0|0|0|0||43.026516||32760|63157|368|NFZ 38 of Iraq|0|1|36.763832|2
7770|1488191105|0|0|0|0|Akre District|43.818691||32761|63157|368|NFZ 39 of Iraq|0|1|36.609051|2
7816|1488045538|0|0|0|0||41.247276||32826|35000|760|NFZ 12 of Syria|0|1|36.853028|2
7817|1488045601|0|0|0|0||42.030022||32827|30000|760|NFZ 13 of Syria|0|1|37.010715|2
7818|1488046048|0|0|0|0||44.631035||32828|30000|368|NFZ 1 of Iraq|0|1|36.418457|2
7819|1488045824|0|0|0|0||44.762871||32829|60000|368|NFZ 2 of Iraq|0|1|35.770409|2
7820|1488082629|0|0|0|0||44.70794||32830|65000|368|NFZ 3 of Iraq|0|1|35.168272|2
7821|1488082557|0|0|0|0||43.930156||32831|65000|368|NFZ 4 of Iraq|0|1|35.645516|2
7822|1488077753|0|0|0|0||45.733101||32832|30000|368|NFZ 5 of Iraq|0|1|35.510926|2
7823|1488078478|0|0|0|0||43.82148||32833|65000|368|NFZ 6 of Iraq|0|1|32.903738|2


