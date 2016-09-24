Here are simple instructions for exporting and importing with SSMS. We may want to refer to this the first few times we export or import, but it will become familiar very quickly.

##Exporting databases with SSMS
1. Right-click the desired database in the Object Explorer. Select Tasks > Generate Scripts…
2. The Generate and Publish Scripts wizard will open.
3. Click Next until you are on the Choose Objects page.
 - Select the option called Select specific database objects
 - Check the box next to Tables
 - Click Next
4. On the Set Scripting Options page:
 - Click Advanced
  - In the General section, find the row Types of data to script
    - Set to Schema and data for an export of the complete database.
    - Set to Schema only for an export without data.
    - Set to Data only to only export the current data.
  - Click OK
  - Select Save to file
    - Set the File name and path to desired location using the `...` button.
  - Click Next
5. On the Summary page:
  - Ensure that all parameters are as you need them to be.
  - Click Next
6. On the Save or Publish Scripts page you'll see the progress of the export.
  - Click Finish when complete.

##Importing databases with SSMS
1. Open SSMS
2. Select File > Open > File and select your .sql file.
3. If the database does not already exist, add the following lines to the top of the script file

:>CREATE DATABASE [your_database_name]
:>GO

4. Save the file.
5. Click ! Execute.
6. erify that the database has been created and the schema and/or data imported.
