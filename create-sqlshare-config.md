# Creating your SQLShare config file
Brian High  
5/4/2015  

Introduction
------------

This short tutorial will guide you through the steps to create a small  
configuration file for use with SQLShare so that you can connect to the 
UW [SQLShare](https://sqlshare.escience.washington.edu/sqlshare/) service 
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

Windows users may edit the config file by hand, e.g. from DOS as:

```
notepad "%HOMEDRIVE%%HOMEPATH%\My Documents\.sqlshare\config"
```

Mac users may may edit the config file by hand, e.g. from Terminal as:

```
open -a TextEdit "~/.sqlshare/config"
```

Linux users may may edit the config file by hand, e.g. from their favorite 
terminal emulator as:
    
```
$EDITOR "~/.sqlshare/config"
```