# check_oracle_health Nagios Plugin

Fork from https://labs.consol.de/nagios/check_oracle_health/index.html

---

This plugin is used to monitor a variety of oracle database metrics. 

* For instructions on installing this plugin for use with Nagios,
  see below. In addition, generic instructions for the GNU toolchain
  can be found in the INSTALL file.

* For major changes between releases, read the CHANGES file.

* For information on detailed changes that have been made,
  read the Changelog file.

* This plugin is self documenting.  All plugins that comply with
  the basic guidelines for development will provide detailed help when
  invoked with the '-h' or '--help' options.

You can check for the latest plugin at:
  http://www.consol.com/opensource/nagios/check-oracle-health

The documentation in this README covers only the most common features.
To view the full documentation and examples, go to 
  http://www.consol.com/opensource/nagios/check-oracle-health or
  http://www.consol.de/opensource/nagios/check-oracle-health

Send mail to gerhard.lausser@consol.de for assistance.
Please include the OS type/version and the Perl DBI/DBD version
that you are using.
Also, run the plugin with the '-vvv' option and provide the resulting 
version information.  Of course, there may be additional diagnostic information
required as well.  Use good judgment.

For patch submissions and bug reports, please send me a mail. You can also find
me at http://www.nagios-portal.de




## How to "compile" the check_oracle_health script.


1) Run the configure script to initialize variables and create a Makefile, etc.

	./configure --prefix=BASEDIRECTORY --with-nagios-user=SOMEUSER --with-nagios-group=SOMEGROUP --with-perl=PATH_TO_PERL --with-statefiles-dir=STATE_PATH

   a) Replace BASEDIRECTORY with the path of the directory under which Nagios
      is installed (default is '/usr/local/nagios')
   b) Replace SOMEUSER with the name of a user on your system that will be
      assigned permissions to the installed plugins (default is 'nagios')
   c) Replace SOMEGRP with the name of a group on your system that will be
      assigned permissions to the installed plugins (default is 'nagios')
   d) Replace PATH_TO_PERL with the path where a perl binary can be found.
      Besides the system wide perl you might have installed a private perl
      just for the nagios plugins (default is the perl in your path).
   e) Replace STATE_PATH with the directory where you want the script to
      write state files which transport information from one run to the next.
      (default is /tmp)

   Simply running ./configure will be sufficient to create a check_oracle_health
   script which you can customize later.
      

2) "Compile" the plugin with the following command:

	make

    This will produce a "check_oracle_health" script. You will also find
    a "check_oracle_health.pl" which you better ignore. It is the base for
    the compilation filled with placeholders. These will be replaced during
    the make process.


3) Install the compiled plugin script with the following command:

	make install

   The installation procedure will attempt to place the plugin in a 
   'libexec/' subdirectory in the base directory you specified with
   the --prefix argument to the configure script.


4) Verify that your configuration files for Nagios contains
   the correct paths to the new plugin.


## Command line parameters

--connect=<the oracle connect string>
   This is what you would also use with tnsping and sqlplus.

--user=<username>
   This is the user which reads the system tables.

--password=<secret>
   This is the user's password.

--mode=<operation mode>
   This parameter tells the plugin what it should check.
   The list of known modes may grow frequently. Please look at 
   http://www.consol.com/opensource/nagios/check-oracle-health for a list
   of features.

--tablespace=<tablespace name>
  Tablespace-related modes check all tablespaces in one run by default.
  If only a single tablespace should be checked, use this parameter.

--warning=<warning threshold>
  If the metric is out of this range, the plugin returns a warning.

--critical=<critical threshold>
  If the metric is out of this range, the plugin returns a critical.

   


## How to prepare the database for monitoring

create user nagios identified by 'whatever';
grant create session to nagios; 
grant select any dictionary to nagios; 
grant select on V_$SYSSTAT to nagios; 
grant select on V_$INSTANCE to nagios; 
grant select on V_$LOG to nagios; 
grant select on SYS.DBA_DATA_FILES to nagios; 
grant select on SYS.DBA_FREE_SPACE to nagios;

on 8.x the user must be granted the SELECT_CATALOG_ROLE
grant select_catalog_role to nagios;
instead of "grant select any dictionary..."

If you monitor the oracle 7.x database at the local computer museum:
grant select on V_$SYSSTAT to nagios;
grant select on sys.dba_tablespaces to nagios;
grant select on sys.dba_free_space to nagios;
grant select on sys.dba_data_files to nagios;


---
That's it.  If you have any problems or questions, feel free to send mail
to gerhard.lausser@consol.de
