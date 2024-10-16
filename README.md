# utl-sql-column-dictionary-in-sas-r-and-python-multi-language
SQL column dictionary in sas r and python multi language 
    %let pgm=utl-sql-column-dictionary-in-sas-r-and-python-multi-language;

    SQL column dictionary in sas r and python multi language

    %stop_submission;

    Remove column names the end= '_lb'

        SOLUTIONS (COLUMN META DATA)

              1 sas meta data
              2 r meta data
              3 python meta data

    github
    https://tinyurl.com/yeymr2x2
    https://github.com/rogerjdeangelis/utl-sql-column-dictionary-in-sas-r-and-python-multi-language

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                 INPUT             |           PROCESS                      |              OUTPUT                       */
    /*                 =====             |           =======                      |              ======                       */
    /*                                   |                                        | SAS                                       */
    /*          FLOUR  FLOUR  RICE  RICE | proc sql;                              |                                           */
    /*     BID    KG     LB    KG    LB  |  create                                |  NAME     TYPE LENGTH VARNUM     LABEL    */
    /*                                   |      table cols as                     |                                           */
    /*      a     4      5      2     2  |  select                                |  BID      char    1      1  Primary Key   */
    /*      b     3      6      4     3  |      name                              |  FLOUR_KG num     8      2  Flour kilos   */
    /*      c     2      4      1     2  |     ,type                              |  FLOUR_LB num     8      3  Flour Pounds  */
    /*      d     2      3      1     1  |     ,length                            |  RICE_KG  num     8      4  Rice kilos    */
    /*      e     5      1      3     2  |     ,varnum                            |  RICE_LB  num     8      5  RicePounds    */
    /*                                   |     ,label                             |                                           */
    /*      options validvarname=upcase; |    from                                |                                           */
    /*      libname sd1 "d:/sd1";        |        dictionary.columns              |                                           */
    /*      data sd1.have;               |    where                               |                                           */
    /*       label                       |            libname = 'sd1'             |                                           */
    /*         bid      = 'Primary Key ' |       and  memname = 'have'            |                                           */
    /*         flour_kg = 'Flour kilos ' |                                        |                                           */
    /*         flour_lb = 'Flour Pounds' |  ;quit;                                |                                           */
    /*         rice_kg  = 'Rice kilos  ' |                                        |                                           */
    /*         rice_lb  = 'RicePounds  ' |------------------------------------------------------------------------------------*/
    /*        ;                          |                                        |                                           */
    /*       input bid $1.               | R                                      |  R                                        */
    /*             flour_kg flour_lb     | ==                                     |                       NOT         DFLT_   */
    /*             rice_kg  rice_lb;     |                                        |  CID  NAME      TYPE  NULL VALUE  PK      */
    /*      cards4;                      | %utl_rbeginx;                          |                                           */
    /*      a 4 5 2 2                    | parmcards4;                            |   0   BID       TEXT  0      .     0      */
    /*      b 3 6 4 3                    | library(sqldf)                         |   1   FLOUR_KG  REAL  0      .     0      */
    /*      c 2 4 1 2                    | library(haven)                         |   2   FLOUR_LB  REAL  0      .     0      */
    /*      d 2 3 1 1                    | source("c:/oto/fn_tosas9x.r")          |   3   RICE_KG   REAL  0      .     0      */
    /*      e 5 1 3 2                    | have<-                                 |   4   RICE_LB   REAL  0      .     0      */
    /*      ;;;;                         |  read_sas("d:/sd1/have.sas7bdat")      |                                           */
    /*      run;quit;                    | want <- sqldf("                        |                                           */
    /*                                   |   select                               |                                           */
    /*                                   |      *                                 |                                           */
    /*                                   |   from                                 |                                           */
    /*                                   |     pragma_table_info('have')          |                                           */
    /*                                   |   ")                                   |                                           */
    /*                                   |  fn_tosas9x(                           |                                           */
    /*                                   |       inp    = want                    |                                           */
    /*                                   |      ,outlib ="d:/sd1/"                |                                           */
    /*                                   |      ,outdsn ="rwant"                  |                                           */
    /*                                   |      )                                 |                                           */
    /*                                   | ;;;;                                   |                                           */
    /*                                   | %utl_rendx;                            |                                           */
    /*                                   |                                        |                                           */
    /*                                   | proc print data=sd1.rwant;             |                                           */
    /*                                   | run;quit;                              |                                           */
    /*                                   |                                        |                                           */
    /*                                   |------------------------------------------------------------------------------------*/
    /*                                   |                                        |                                           */
    /*                                   | PYTHON                                 | PYTHON                                    */
    /*                                   | ======                                 |                                           */
    /*                                   |                                        |                                           */
    /*                                   | %utl_pybeginx;                         |  PRAGMA table_info:  not                  */
    /*                                   | parmcards4;                            |  cid      name  type null dflt_value  pk  */
    /*                                   | import pyreadstat as ps                |    0       BID  TEXT    0       None   0  */
    /*                                   | import pandas as pd                    |    1  FLOUR_KG  REAL    0       None   0  */
    /*                                   | import sqlite3                         |    2  FLOUR_LB  REAL    0       None   0  */
    /*                                   | df,meta =  \                           |    3   RICE_KG  REAL    0       None   0  */
    /*                                   |  ps.read_sas7bdat( \                   |    4   RICE_LB  REAL    0       None   0  */
    /*                                   |  'd:/sd1/have.sas7bdat');              |                                           */
    /*                                   |                                        |                                           */
    /*                                   | # Create temporary in memorySQLite     |                                           */
    /*                                   | conn = sqlite3.connect(':memory:')     |                                           */
    /*                                   |                                        |                                           */
    /*                                   | # Write DataFrame to a SQLite table    |                                           */
    /*                                   | df.to_sql('my_table',conn,index=False) |                                           */
    /*                                   |                                        |                                           */
    /*                                   | # pd.read_sql PRAGMA table command     |                                           */
    /*                                   | pragma_df =  \                         |                                           */
    /*                                   |  pd.read_sql( \                        |                                           */
    /*                                   |   "PRAGMA table_info(my_table)" \      |                                           */
    /*                                   |  ,conn)                                |                                           */
    /*                                   |                                        |                                           */
    /*                                   | # Display the PRAGMA information       |                                           */
    /*                                   | print("PRAGMA table_info:")            |                                           */
    /*                                   | print(pragma_df)                       |                                           */
    /*                                   |                                        |                                           */
    /*                                   | # Close the connection                 |                                           */
    /*                                   | conn.close()                           |                                           */
    /*                                   | ;;;;                                   |                                           */
    /*                                   | %utl_pyendx;                           |                                           */  |                                        |                                           */
    /*                                   |                                        |                                           */
    /**************************************************************************************************************************/


    /*                   _
    (_)_ __  _ __      _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     label
       bid      = 'Primary Key '
       flour_kg = 'Flour kilos '
       flour_lb = 'Flour Pounds'
       rice_kg  = 'Rice kilos  '
       rice_lb  = 'RicePounds  '
      ;
     input bid $1.
           flour_kg flour_lb
           rice_kg  rice_lb;
    cards4;
    a 4 5 2 2
    b 3 6 4 3
    c 2 4 1 2
    d 2 3 1 1
    e 5 1 3 2
    ;;;;
    run;quit;

    proc print data=sd1.have split='_';
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  SD1.HAVE total obs=5                                            Variables in Creation Order                           */
    /*                                                                                                                        */
    /*   BID    FLOUR_KG    FLOUR_LB    RICE_KG    RICE_LB          Variable    Type    Len    Label                          */
    /*                                                                                                                        */
    /*    a         4           5          2          2             BID         Char      1    Primary Key                    */
    /*    b         3           6          4          3             FLOUR_KG    Num       8    Flour kilos                    */
    /*    c         2           4          1          2             FLOUR_LB    Num       8    Flour Pounds                   */
    /*    d         2           3          1          1             RICE_KG     Num       8    Rice kilos                     */
    /*    e         5           1          3          2             RICE_LB     Num       8    RicePounds                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                  _              _       _
    / |  ___  __ _ ___   _ __ ___   ___| |_ __ _    __| | __ _| |_ __ _
    | | / __|/ _` / __| | `_ ` _ \ / _ \ __/ _` |  / _` |/ _` | __/ _` |
    | | \__ \ (_| \__ \ | | | | | |  __/ || (_| | | (_| | (_| | || (_| |
    |_| |___/\__,_|___/ |_| |_| |_|\___|\__\__,_|  \__,_|\__,_|\__\__,_|

    */
    proc sql;
      create
          table cols as
      select
          name
         ,type
         ,length
         ,varnum
         ,label
      from
          dictionary.columns
      where
              libname = 'sd1'
         and  memname = 'have'

    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  WORK.COLS total obs=5                                                                                                 */
    /*                                                                                                                        */
    /*   NAME        TYPE    LENGTH    VARNUM       LABEL                                                                     */
    /*                                                                                                                        */
    /*   BID         char       1         1      Primary Key                                                                  */
    /*   FLOUR_KG    num        8         2      Flour kilos                                                                  */
    /*   FLOUR_LB    num        8         3      Flour Pounds                                                                 */
    /*   RICE_KG     num        8         4      Rice kilos                                                                   */
    /*   RICE_LB     num        8         5      RicePounds                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___
    |___ \   _ __
      __) | | `__|
     / __/  | |
    |_____| |_|

    */

    %utl_rbeginx;
    parmcards4;
    library(sqldf)
    library(haven)
    source("c:/oto/fn_tosas9x.r")
    have<-
     read_sas("d:/sd1/have.sas7bdat")
    want <- sqldf("
      select
         *
      from
        pragma_table_info('have')
      ")
     want
     fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rwant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R                                                 SAS                                                                 */
    /*                                                                                                                        */
    /*  > want                                            SD1.rwant  total obs=5                                              */
    /*                                                                                                      DFLT_             */
    /*    cid     name type notnull dflt_value pk         ROWNAMES    CID    NAME        TYPE    NOTNULL    VALUE    PK       */
    /*                                                                                                                        */
    /*  1   0      BID TEXT       0         NA  0             1        0     BID         TEXT       0         .       0       */
    /*  2   1 FLOUR_KG REAL       0         NA  0             2        1     FLOUR_KG    REAL       0         .       0       */
    /*  3   2 FLOUR_LB REAL       0         NA  0             3        2     FLOUR_LB    REAL       0         .       0       */
    /*  4   3  RICE_KG REAL       0         NA  0             4        3     RICE_KG     REAL       0         .       0       */
    /*  5   4  RICE_LB REAL       0         NA  0             5        4     RICE_LB     REAL       0         .       0       */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____               _   _                                 _              _       _
    |___ /   _ __  _   _| |_| |__   ___  _ __   _ __ ___   ___| |_ __ _    __| | __ _| |_ __ _
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \ | `_ ` _ \ / _ \ __/ _` |  / _` |/ _` | __/ _` |
     ___) | | |_) | |_| | |_| | | | (_) | | | || | | | | |  __/ || (_| | | (_| | (_| | || (_| |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_||_| |_| |_|\___|\__\__,_|  \__,_|\__,_|\__\__,_|
            |_|    |___/
    */

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    import sqlite3

    df,meta =  \
     ps.read_sas7bdat( \
     'd:/sd1/have.sas7bdat');

    # Create temporary in memorySQLite
    conn = sqlite3.connect(':memory:')

    # Write DataFrame to a SQLite table
    df.to_sql('my_table',conn,index=False)

    # pd.read_sql PRAGMA table command
    pragma_df =  \
     pd.read_sql( \
      "PRAGMA table_info(my_table)" \
     ,conn)

    # Close the connection
    conn.close()
    print(pragma_df);
    fn_tosas9x(pragma_df,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* PYTHON                                             SAS                                                                 */
    /*                                                                                          DFLT_                         */
    /*     cid      name  type  notnull dflt_value  pk    CID    NAME        TYPE    NOTNULL    VALUE    PK                   */
    /*                                                                                                                        */
    /*  0    0       BID  TEXT        0       None   0     0     BID         TEXT       0                 0                   */
    /*  1    1  FLOUR_KG  REAL        0       None   0     1     FLOUR_KG    REAL       0                 0                   */
    /*  2    2  FLOUR_LB  REAL        0       None   0     2     FLOUR_LB    REAL       0                 0                   */
    /*  3    3   RICE_KG  REAL        0       None   0     3     RICE_KG     REAL       0                 0                   */
    /*  4    4   RICE_LB  REAL        0       None   0     4     RICE_LB     REAL       0                 0                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
