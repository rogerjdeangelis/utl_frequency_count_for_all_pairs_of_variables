# utl_frequency_count_for_all_pairs_of_variables
Frequency counts for all pairs of variables. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Frequency counts for all pairs of variables

   See Data_null_ better solution below
   by datanull@gmail.com

    github
    https://github.com/rogerjdeangelis/utl_frequency_count_for_all_pairs_of_variables

    This uses macros  (I don't have all these macros set up for WPS, yet;

      do_over
      varrlist

    Both should be on my github site (or on internet)

    see
    https://communities.sas.com/t5/General-SAS-Programming/PROC-FREQ-with-many-variables/m-p/460031


    INPUT
    =====

        SASHELP.IRIS

                     Variables in Creation Order

        #    Variable       Type    Len    Label

        1    SPECIES        Char     10    Iris Species
        2    SEPALLENGTH    Num       8    Sepal Length (mm)
        3    SEPALWIDTH     Num       8    Sepal Width (mm)
        4    PETALLENGTH    Num       8    Petal Length (mm)
        5    PETALWIDTH     Num       8    Petal Width (mm)

        Should get n choose two crosstabs

        5!/(2! 3!) = 120 /12  = 10 combinations

       EXAMPLE WANT this code

        proc freq data=sashelp.iris;
         tables
            SEPALLENGTH * SPECIES
            SEPALLENGTH * SEPALWIDTH
            SEPALWIDTH  * SPECIES
            PETALLENGTH * SPECIES
            PETALLENGTH * SEPALLENGTH
            PETALLENGTH * SEPALWIDTH
            PETALLENGTH * PETALWIDTH
            PETALWIDTH  * SPECIES
            PETALWIDTH  * SEPALLENGTH
            PETALWIDTH  * SEPALWIDTH   / list;
        run;quit;
        
        SAS-L Data_null_ better solution;
        by datanull@gmail.com
        proc summary data=sashelp.iris chartype;
            class _all_ / mlf;
            ways 2;
            output out=_2way;  
         run;

    PROCESS
    =======

       * just to male sure the code below works;
       %symdel crx varsn vars1 vars2 vars3 vars4 vars5 /nowarn;

       %array(vars,values=%utl_varlist(sashelp.iris));  * creates macro array;

       data _null_;
         length crx $1000;
         retain crx;

         * variable names as data;
         array vars[&varsn] $32 (%do_over(vars,Phrase="?vars",between=comma));
         do i=1 to dim(vars);
            do j=1 to dim(vars);
              if vars[i] < vars[j] then do;
                crx=catx(' ',crx,vars[i],'*',vars[j]);
              end;
           end;
         end;
         call symputx('crx',crx);
         stop;

       run;quit;

       proc freq data=sashelp.iris;
        tables &crx. / list;
       run;quit;


    OUTPUT
    ======

    proc freq data=sashelp.iris;
     tables
        SEPALLENGTH * SPECIES
        SEPALLENGTH * SEPALWIDTH
        SEPALWIDTH  * SPECIES
        PETALLENGTH * SPECIES
        PETALLENGTH * SEPALLENGTH
        PETALLENGTH * SEPALWIDTH
        PETALLENGTH * PETALWIDTH
        PETALWIDTH  * SPECIES
        PETALWIDTH  * SEPALLENGTH
        PETALWIDTH  * SEPALWIDTH   / list;
    run;quit;


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

      only need sashelp.iris

    SAS (see above)


