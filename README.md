Search
======

##Extensions

- workspaces
- indexed_search
- crawler

##Crawler Extension Configuration

### Step one

Create user "_cli_crawler" without a group

### Step two

Create a sql file like

```sql
TRUNCATE index_fulltext;
TRUNCATE index_grlist;
TRUNCATE index_phash;
TRUNCATE index_rel;
TRUNCATE index_section;
TRUNCATE index_words;
TRUNCATE tx_crawler_queue;
```
### Step three

Create a bash script like

```bash
#! /bin/bash
mysql -udb******_*** -p*********** -hmysql5.******.de db******_*** --force < /kunden/ ... PATH ... /typo3-clear-index.sql;

php53 /kunden/ ... PATH ... /typo3/cli_dispatch.phpsh crawler_im 1 -d 99 -conf de -o queue
php53 /kunden/ ... PATH ... /typo3/cli_dispatch.phpsh crawler 1 -d 99 -conf de 
```

###Step four

Create Crawler configuration via List view

##TYPO3 Configuration

###Setup

```typoscript
#INDEXED SEARCH ENGINE - PLUGIN
config.index_enable = 1
#plugin.tx_indexedsearch.search.rootPidList = 1 #hier geht nur der Wert 1
##Suche begfrenzen ebene1 pid 174
plugin.tx_indexedsearch._DEFAULT_PI_VARS.sections=rl1_174
```

###Constants

```typoscript
#Nothing to do here
```

##indexed_search Extension Configuration

Check the following Option

```php
Disable Indexing in Frontend[disableFrontendIndexing]
By default pages are indexed during viewing of pages in the frontend. You can disable this features so indexing of pages is only initiated through the backend page crawler.
```

##Known Bug in TYPO3 4.7.x

```php
Fatal error: Call to undefined method t3lib_div::view_array() in /yourPath/typo3conf/ext/crawler/class.tx_crawler_lib.php on line 1723
```

Change in line 1723 (error message will give you the actual line)

```php
t3lib_div::view_array
```
to

```php
t3lib_utility_Debug::viewArray
```