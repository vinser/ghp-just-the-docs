---
layout: default
title: Advanced User Guide
nav_order: 3
has_children: true
---

## Advanced usage
From command line run `./flibgolite -help` to see run options
```
Usage: flibgolite [OPTION] [data directory]

With no OPTION program will run in console mode (Ctrl+C to exit)
Caution: Only one OPTION can be used at a time

OPTION should be one of:
  -service [action]     control FLibGoLite system service
          where action is one of: install, start, stop, restart, uninstall, status 
  -reindex              empty book stock index and then scan book stock directory to add books to index (database)
  -config               create default config file in ./config folder for customization and exit
  -help                 display this help and exit
  -version              output version information and exit

data directory is optional (current directory by default)
```

Examples:

```bash
./flibgolite                          Run FLibGoLite in console mode

sudo ./flibgolite -service install    Install FLibGoLite as a system service
sudo ./flibgolite -service start      Start FLibGoLite service	
```

## Setup and fine tuning

### 1. Main configuration file

For advanced sutup you can edit `config/config.yml` selfexplanatory configuration file.  
This file by default is located in `config` subfolder of program file location.

### _2. Locations of folders setup_

To change location of a folder just edit corresponding line in `config.yml`

For example, if you need to setup separate folder for new aquired books uncomment line  

```yml
NEW: "books/new"
``` 

and change `books/new` to the appropriate folder path.

### _3. OPDS tuning_

You can change OPDS default 8085 http port to yours 
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
Here you can set OPDS-server preferred name
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
You can change the number of books your bookreader will load at a time when you page (pulldown/update the screen)

```yml
# OPDS feeds entries page size
PAGE_SIZE: 30
```
Do not set this value more than default. With lower values it updates faster.

### _4. Localization tips_

There are some easy features that may help to tune your language experience

4.1. By default new books processing is limited to English, Russian and Ukrainian books. You can add [others](https://en.wikipedia.org/wiki/IETF_language_tag) like `"de"`, `"fr"`, `"it"` and so on.  

```yml
# Accept only these languages publications. Add others if needed please.
ACCEPTED: "en, ru, uk"
```  

4.2. By default bookreader will show menues and comments in English `"en"` If you are Rusiian or Ukranian you can change this setting to `"ru"` or `"uk`" 

```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

4.3. If your native language is other then three mentioned above for your convinience you can make language file and put it in `config/locales` folder  

```yml
# Locales folder. You can add your own locale file there like en.yml, ru.yml, uk.yml
DIR: "config/locales"
```

For example, for German, copy `en.yml` to `de.yml` and translate the phrases into German to the right of the colon separator. Leave `%d` format symbols untouchced. Something like this:  

```yml
Found authors - %d: Autoren gefunden - %d
```

Don't forget to replace alphabet string `ABC` to German. This will ensure that German names and titles are displayed and sorted correctly.  

4.4. Genres tree selection language adaptation can be done by editing the file `genres.xml` in `config` folder

```yml
  TREE_FILE: "config/genres.xml"
```

This can be done by adding language specific lines in `genres.xml` file

```xml
<genre-descr lang="en" title="Alternative history"/>
<genre-descr lang="ru" title="Альтернативная история"/>
<genre-descr lang="uk" title="Альтернативна історія"/>
<genre-descr lang="de" title="Alternative Geschichte"/>
```

### _5. Default config.yml_

Default configuration file `config.yml` with folder tree is created at the first programm run. You can edit it and your edits will not be canceled the next time you run the program. Thus, you can distribute the files used by the program into the necessary folders. With reasonable care, you can edit or add any configuration file located by default in the `config` folder and it will not be deleted or overwriten.

```yml
library:
  # Selfexplained folders
  STOCK: "books/stock" # Book stock
  TRASH: "books/trash" # Error and duplicate files and archives wil be moved to this folder 
  # NEW: "books/new" # Uncomment the line to have separate folder for new acquired books

genres:
  TREE_FILE: "config/genres.xml"
  # Alternative genres tree can be used (Russian only, sorry) 
  # TREE_FILE: "config/alt_genres.xml"
  
database:
  DSN: "dbdata/books.db"
  # Delay before start each new acquisitions processing
  POLL_DELAY: 30 
  # Maximum simultaneous new aquisitios processing threads
  MAX_SCAN_THREADS: 30

logs:
  # Logs are here
  # To redirect the log output to console (stdout) just comment out the appropriate line OPDS or SCAN
  OPDS: "logs/opds.log"
  SCAN: "logs/scan.log"
   # Logging levels: D - debug, I - info, W - warnings (default), E - errors
  LEVEL: "W" 

opds:
  # OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
  PORT: 8085
  # OPDS-server title that is displayed in a book reader
  TITLE: "FLib Go Go Go!!!"
  # OPDS feeds entries page size
  PAGE_SIZE: 30

locales:
  # Locales folder. You can add your own locale file there like en.yml, ru.yml, uk.yml
  DIR: "config/locales"
  # Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
  # "uk" for Ukrainian, 
  # "ru" for Russian 
  DEFAULT: "en"
  # Accept only these languages publications. Add others if needed please.
  ACCEPTED: "en, ru, uk"
```

### _6. Book index database_

Book index is stored in SQLite database file located in `dbdata` folder. It is created at the first program run and __is not intended for manual editing__. 

```yml
DSN: "dbdata/books.db"
```

### _7. Logging_

While running program writes `opds.log` and `scan.log` located in `logs` folder.

```yml
OPDS: "logs/opds.log"
SCAN: "logs/scan.log"
```
`opds.log` contains records about bookreaders requests.  
`scan.log` contains records about new books and archive indexing.  
To redirect the log output to console (stdout) just comment out the appropriate line OPDS or SCAN.

You don't need to delete logs to free up disk space, as logs are rotated (overwrite) after 7 days.

You can setup logging level (verbosity) to one of: `D` - debug, `I` - info, `W` - warnings (default), `E` - errors
```yml
LEVEL: "W" 
```

### _8. Run in Docker container_

As an option you may run program in [docker container](README.docker.md)

### _9. Build from sources_

If you have any security doubts about builded executables or there is no suitable one you may easily build it yourself.    
To build an executable install [Golang](https://go.dev/dl/), [Git](https://git-scm.com/downloads) clone [FLibGoLite repositiry](https://github.com/vinser/flibgolite) and run
```
go build ./cmd/flibgolite
```  
It's better to build it on the host the service will run. You will get executable right for the host OS and hardware.  
For crosscompile install GNU `make` and run it with Makefile


-------------------------------
___*Suggestions, bug reports and comments are welcome [here](https://github.com/vinser/flibgolite/issues)*___

   

