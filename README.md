# utl_special_case_of_column_percentages_wps_proc_r
Special case of column percentages wps proc r.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Special case of column percentages wps proc r

    WPS/PROC R solution

    github
    https://tinyurl.com/y845sx4z
    https://github.com/rogerjdeangelis/utl_special_case_of_column_percentages_wps_proc_r

    This type of problem is best solved with a matrix like language.
    In this case WPS/Proc R, SAS/IML or SAS/IML/R


    HAVE  ( The 0s are just to checking result)
    ====

    SD1.HAVE total obs=10

      A    B    C    D    E    F    G    H    I    J    TOT

      9    0    0    0    0    0    0    0    0    0      1   ** note row sums are all 10
      0    8    0    0    0    0    0    0    0    0      2
      0    0    7    0    0    0    0    0    0    0      3
      0    0    0    6    0    0    0    0    0    0      4
      0    0    0    0    5    0    0    0    0    0      5
      0    0    0    0    0    4    0    0    0    0      6
      0    0    0    0    0    0    3    0    0    0      7
      0    0    0    0    0    0    0    2    0    0      8
      0    0    0    0    0    0    0    0    1    0      9
      0    0    0    0    0    0    0    0    0    0     10


    EXAMPLE WANT ( 1 row out)

       Consider variable A and B

       new  A = sum(of column A) / sum of (A + B + ... + TOT) = 9/10 = .9
       new  B = sum(of column B) / sum of (A + B + ... + TOT) = 8/10 = .8


         A      B      C   ...   J

        0.9    0.8    0.7  ...   0


    PROCESS  WORKING CODE  A ONE LINER   (justa ss easy in IML?)
    ============================================================

         colSums(have/rowSums(have)


    OUTPUT
    ======

     The WPS System

     WORK.WANT  total obs=1

       A      B      C      D      E      F      G      H      I     J

      0.9    0.8    0.7    0.6    0.5    0.4    0.3    0.2    0.1    0

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    *input surfm wskim Outdoorm dancem fishm snkrm tenpm Leisurem nine ten totadmin;
    input a b c d e f g h i j tot;
    cards4;
    9 0 0 0 0 0 0 0 0 0 1
    0 8 0 0 0 0 0 0 0 0 2
    0 0 7 0 0 0 0 0 0 0 3
    0 0 0 6 0 0 0 0 0 0 4
    0 0 0 0 5 0 0 0 0 0 5
    0 0 0 0 0 4 0 0 0 0 6
    0 0 0 0 0 0 3 0 0 0 7
    0 0 0 0 0 0 0 2 0 0 8
    0 0 0 0 0 0 0 0 1 0 9
    0 0 0 0 0 0 0 0 0 0 10
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.1";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.1/etc/Rprofile.site", echo=T);
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    head(have);
    colall<-as.data.frame(t(colSums(have/rowSums(have))));
    colnames(colall)<-colnames(have);
    class(colall);
    endsubmit;
    import r=colall     data=wrk.want;
    run;quit;
    proc print data=wrk.want;
    run;quit;
    ');


