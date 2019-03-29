# Bubble map creator

*    Title: Bubble map creator     
*    Author: Guillem Alomar
*    Initial release: March 29th, 2019                     
*    Code version: 0.1                         
*    Availability: Public     

**Index**
* [Requirements](#requirements)
* [Documentation](#documentation)
    * [Application Architecture](#application-architecture)
* [Using the application](#using-the-application)
    * [First of all](#first-of-all)
    * [Executing](#executing)
    * [Additional Parameters](#additional-parameters)
* [Understanding the output](#understanding-the-output)
* [Comments](#comments)
* [Acknowledgements](#acknowledgements)

## Requirements

- Python +3.5
- basemap 1.2.0
- geopy 1.18.1
- matplotlib 3.0.2
- mysql-connector-python 8.0.14
- mysql-connector-python-rf 2.2.2
- numpy 1.16.0
- pandas 0.24.0

## Documentation

### Explanation

This project is about obtaining values assigned to locations from a local csv file or a MySQL database, and plotting them on a world map. It connects to an external geolocation service to obtain the coordinates from the names of the locations.

The application idea is to obtain data from a MySQL database being feed from another source (this can be a crawler obtaining information about many products that can be found on the internet such as flights, items from ebay or amazon, tweets, facebook posts... It has infinite possibilities).

It can also be used to generate a map without connecting to a database, but in this case you will have to already provide a CSV containing the coordinates and values to be shown.

### Application Architecture

![alt text][logo]

[logo]: Documentation/Diagram.png "Application Architecture"

## Using the application

### First of all

#### MySQL Configuration

You need to have a running MySQL instance accessible from your machine, and put the credentials for it in a new file _src/settings/creds.py_, with the same structure as the _creds_dummy.py_ file from that same folder.

The data in MySQL has to follow the next structure:

|      location      |   keyword   | parserID | total |
|:------------------:|:-----------:|:--------:|:-----:|
|    123 Fake St.    |     Lisa    |     2    |   1   |
|    123 Fake St.    | Bart, Homer |     5    |   2   |
| Springfield School |   Student   |    103   |  300  |

Where **location, keyword, parserID** is the composite primary key.

#### Python libraries installation

- I recommend creating a virtualenv for this project. After creating it, you should run the following command:
```
~/RedditCrawler$ pip install -r requirements.frozen
```

To use Basemap is a little more complex than other python libraries.
I recommend using Conda to do it (I used version 4.3.16).

#### Image parameters

The parameters of the images that will be generated can be modified from the file _src/settings/\_\_init\_\_.py_

### Executing

This is done by typing the following command:
```
~/RedditCrawler$ python3 bubble.py
```

### Additional Parameters

Both a parser and a keyword can be given as parameter to obtain more specific results.
The global map can be disabled, which can be useful to obtain the specific results faster.

```
usage: bubble.py [-h] [-p PARSER] [-k KEYWORD] [-ng] [-f FILE]

Jobs map creator

optional arguments:
  -h, --help            show this help message and exit
  -p PARSER, --parser PARSER
                        The parser id that you want to check.
  -k KEYWORD, --keyword KEYWORD
                        The keyword that you want to check.
  -ng, --noglobal       Deactivate the global map.
  -f FILE, --file FILE  CSV input file. If not specified it will generate it from the MYSQL data.
 ```
 
 If both the parser and keywords argument are given, the result will be an image with the locations where both conditions are found.
 
 ## Understanding the output
 
Once the execution has finished, the resulting images will be stored in _data/images_. These images should look similar to this:
 
![alt text][logo2]

[logo2]: Documentation/ImageExample.png "Application Architecture"

The names of the images follow the next syntax:

```
$date[_[$keyword/$parser]].png
```

The resulting CSVs used to create the images can also be found in _data/csvs_. These csvs should look similar to this:

```
homelon;homelat;homecontinent;n
80.2838331;13.0801721;chennai, tamil nadu;3000
2.1774322;41.3828939;Barcelona;2250
-6.2602732;53.3497645;Dublin, Co. Dublin;2700
-4.2501672;55.8611389;Glasgow, Scotland;3600
-2.991665;53.407154;Liverpool, England;3150
-0.1276474;51.5073219;London, England;2250
-3.7035825;40.4167047;Madrid;1350
-2.2451148;53.4794892;Manchester, England;1800
5.09480471718312;52.2652457;ankeveen;1125
4.78130379920329;52.2336018;De Kwakel;6600
4.26968022053645;52.07494555;Den Haag;2850
5.7668453548675;50.81024155;eckelrade;375
5.478633;51.4392648;Eindhoven;1620
5.8870568;50.9189062;gemeente nuth;750
5.53460738161551;52.2379891;Nederland;3900
...
```

The names of the csvs follow the next syntax:

```
$date[_[$keyword/$parser]].csv
```

Logs can be found in _logs/bubble.log_ (if the folder and file don't exist, will be autogenerated at the beginning of the execution).

## Comments

- I have tested this application with input files with sizes in the order of MBs, with successful results. I'm not sure how would it perform with files of sizes in the order of GBs.

## Acknowledgements

- Some of the code was inspired by this great [post](https://python-graph-gallery.com/315-a-world-map-of-surf-tweets/) from the Python Graph Gallery website. Check it out!
- The geolocation service used is from [GeoPy](https://geopy.readthedocs.io/en/stable/), and it works great.