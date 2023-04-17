# utl-creating-output-datasets-by-looping-over-input-datasets-and-variables
utl-creating-output-datasets-by-looping-over-input-datasets-and-variables
    %let pgm=utl-creating-output-datasets-by-looping-over-input-datasets-and-variables;

    Creating output datasets by looping over input datasets and variables

    StackOverflow
    https://tinyurl.com/647fdwa7
    https://stackoverflow.com/questions/76037389/sas-looping-through-macro-variables-on-an-if-statement-and-through-multiple-data

      Suppose you have two datasets associated variables

       Dataset          Variables      Output
       ================ ============  =============
       sashelp.classfit name age sex  work.classfit
       sashelp.cars     make model    work.cars

    /*                   _                    _              _       _
    (_)_ __  _ __  _   _| |_   _ __ ___   ___| |_ __ _    __| | __ _| |_ __ _
    | | `_ \| `_ \| | | | __| | `_ ` _ \ / _ \ __/ _` |  / _` |/ _` | __/ _` |
    | | | | | |_) | |_| | |_  | | | | | |  __/ || (_| | | (_| | (_| | || (_| |
    |_|_| |_| .__/ \__,_|\__| |_| |_| |_|\___|\__\__,_|  \__,_|\__,_|\__\__,_|
            |_|
    */

    data have;

     input lib$ inp$ out$ var & $40.;

    cards4;
    sashelp classfit classfit name age sex
    sashelp cars cars make model
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs from HAVE total obs=2 17APR2023:11:49:36                                                                  */
    /*                                                                                                                        */
    /* Obs      LIB      INP          OUT         VAR                                                                         */
    /*                                                                                                                        */
    /*  1     sashelp    classfit    class    name age sex                                                                    */
    /*  2     sashelp    cars        cars     make model                                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */
    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Up to 40 obs from LOG total obs=2 17APR2023:11:53:56                                                                   */
    /*                                                                                                                        */
    /* Obs      LIB      INP          OUT         VAR         RC      STATUS                                                  */
    /*                                                                                                                        */
    /*  1     sashelp    classfit    class    name age sex     0    Successful                                                */
    /*  2     sashelp    cars        cars     make model       0    Successful                                                */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  Up to 40 obs from WORK.CLASSFIT total obs=19 17APR2023:11:56:06                                                       */
    /*                                                                                                                        */
    /*  Obs    NAME       SEX    AGE                                                                                          */
    /*                                                                                                                        */
    /*    1    Joyce       F      11                                                                                          */
    /*    2    Louise      F      12                                                                                          */
    /*    3    Alice       F      13                                                                                          */
    /*    4    James       M      12                                                                                          */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  Up to 40 obs from WORK.CARS total obs=428 17APR2023:11:56:45                                                          */
    /*                                                                                                                        */
    /*  Obs    MAKE     MODEL                                                                                                 */
    /*                                                                                                                        */
    /*    1    Acura    MDX                                                                                                   */
    /*    2    Acura    RSX Type S 2dr                                                                                        */
    /*    3    Acura    TSX 4dr                                                                                               */
    /*    4    Acura    TL 4dr                                                                                                */
    /*    5    Acura    3.5 RL 4dr                                                                                            */
    /*    6    Acura    3.5 RL w/Navigation 4dr                                                                               */
    /*    7    Acura    NSX coupe 2dr manual S                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */
    data log;

     set have ;

     call symputx("_lib",lib);
     call symputx("_inp",inp);
     call symputx("_out",out);
     call symputx("_var",var);

     rc=dosubl('
        data &_out;
           set &_lib..&_inp(keep=&_var);
        run;quit;
        %let err=&syserr;
        ');

        if symgetn('err') = 0 then status="Successful";
        else status = 'Error';
    run;quit;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
