# Creating your SQLShare config file
Brian High  
5/4/2015  

Introduction
------------

This short tutorial will guide you through the steps to create a small 
configuration file for use with SQLShare so that you can connect to the UW 
[SQLShare](https://sqlshare.escience.washington.edu/sqlshare/) service 
from your R (or Python, etc.) code, using the 
[REST API](http://escience.washington.edu/get-help-now/sql-share-rest-api).

Getting started
---------------

First, you will need to set up an account on [SQLShare](https://sqlshare.escience.washington.edu/sqlshare/).

Then , to help you set up your 
[SQLShare API Key](http://escience.washington.edu/get-help-now/accessing-sqlshare-r) file ("config"), run the following code blocks in R.
    

```r
config <- "~/.sqlshare/config"

# Create .sqlshare folder if not already present
dir.create(file.path("~/.sqlshare"), showWarnings=FALSE, recursive=TRUE)

body <- "[sqlshare]
host=rest.sqlshare.escience.washington.edu
user=your-netid@washington.edu
password=your-api-key"

# Create SQLShare config file if not already present
if (! file.exists(config)) {
    file.create(config)
    fileConn <- file(config)
    writeLines(body, fileConn)
    close(fileConn)
}
```

Edit the file
-------------
    
If you have not done so already, you will need to edit the `config` file to 
enter your own username (UW_NetID@washington.edu) and 
[your own API key](https://sqlshare.escience.washington.edu/sqlshare/#s=credentials).

The easiest way to edit it may be within RStudio, itself.


```r
file.edit('~/.sqlshare/config')
```

Please edit as needed and save your changes.

Alternate ways to edit the config file
--------------------------------------

Some operating systems make it hard to find and open files without a file
extension like "config" and within a folder starting with a "dot" (.) such 
as ".sqlshare". So, the previous method of opening in RStudio may be the
easiest, but here are some other options.

Windows users may edit the config file by hand with Notepad, launched 
from DOS as:

```
notepad "%HOMEDRIVE%%HOMEPATH%\My Documents\.sqlshare\config"
```

Mac users may want to edit the config file with TextEdit, launched from 
Terminal as:

```
open -a TextEdit "~/.sqlshare/config"
```

Linux users may wish to edit the config file using their favorite text editor, launched from their favorite shell as:
    
```
$EDITOR "~/.sqlshare/config"
```

... or just navigate to the file and edit with your favorite text editor.

Test Connection
---------------

Run this code chunk to test your connection ...


```r
if (! require(sqlshare)) { 
    install.packages("sqlshare")
    library(sqlshare)
}
```

```
## Loading required package: sqlshare
## Loading required package: RCurl
## Loading required package: bitops
```

```r
sql <- 'SELECT * FROM information_schema.tables'
head(fetch.data.frame(sql))
```

```
##   TABLE_CATALOG TABLE_SCHEMA                     TABLE_NAME TABLE_TYPE
## 1      sqlshare          dbo            single_column_stats BASE TABLE
## 2      sqlshare          dbo join_column_stats_experiment_v       VIEW
## 3      sqlshare          dbo          single_column_stats_v       VIEW
## 4      sqlshare          dbo                  indexed_reads BASE TABLE
## 5      sqlshare          dbo                      test_view       VIEW
## 6      sqlshare          dbo              join_column_stats BASE TABLE
```

```r
sql <- "SELECT [Provider City],[Average Covered Charges] 
        FROM [high@washington.edu].[table_IPPS.csv] 
        WHERE [Provider State] = 'WA' 
        AND [DRG Definition] = '039 - EXTRACRANIAL PROCEDURES W/O CC/MCC'"
head(fetch.data.frame(sql))
```

```
##   Provider.City Average.Covered.Charges
## 1       SEATTLE               $16989.61
## 2        YAKIMA               $32560.61
## 3       EVERETT               $34694.24
## 4     WENATCHEE               $13720.90
## 5       OLYMPIA               $35711.18
## 6       SEATTLE               $31339.68
```
