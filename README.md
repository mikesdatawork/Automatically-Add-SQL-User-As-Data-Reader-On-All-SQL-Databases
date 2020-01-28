![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Automatically Add SQL User As Data Reader On All SQL Databases
**Post Date: May 7, 2014**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's a quick script you can use to automatically add a user across all databases with Read Only ( Data Reader ) rights. Additionally; this script will remove the user ( if it exists ) from any of the other roles just as a precaution.</p>      


## SQL-Logic
```SQL
use master;
set nocount on
declare @my_windows_user                varchar(50)
declare @change_access_to_read_only varchar(max)
set @my_windows_user        = 'MyDomain\MyUserName'
set @change_access_to_read_only = ''
select  @change_access_to_read_only = @change_access_to_read_only +
'use [' + name + '];'                                                                                               + char(10) + '
create user [' + @my_windows_user + '] for login [' + @my_windows_user + '];'                           + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''bulkadmin'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''dbcreator'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''diskadmin'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''processadmin'';'    + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''securityadmin'';'   + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''serveradmin'';' + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''setupadmin'';'      + char(10) + '
exec master..sp_dropsrvrolemember @loginame = '''   + @my_windows_user + ''',   @rolename   = ''sysadmin'';'    + char(10) + '
exec sp_addrolemember ''db_datareader'', '''        + @my_windows_user + ''';'                      + char(10) + '
exec sp_droprolemember ''db_owner'', '''        + @my_windows_user + ''';'                          + char(10) + char(10)                   
from
      sys.databases
where
      name not in ('master', 'model', 'msdb', 'tempdb')
 
exec  (@change_access_to_read_only) --for xml path(''), type
```




[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

