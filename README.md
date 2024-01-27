# utl-removing-rows-and-columns-that-are-entirely-missing-or-any-constant
Removing rows and columns that are entirely missing or any constant
%let pgm=utl-removing-rows-and-columns-that-are-entirely-missing-or-any-constant;

Removing rows and columns that are entirely missing or any constant

github
http://tinyurl.com/228acfn2
https://github.com/rogerjdeangelis/utl-removing-rows-and-columns-that-are-entirely-missing-or-any-constant

If you have IML(proc r) you can run this code

Note this solution can easily hadle both row or columns

/**************************************************************************************************************************/
/*                                                                                                                        */
/*                                                                                                                        */
/*  INPUT                    PROCESS                         OUTPUT                                                       */
/*                                                                                                                        */
/*  SD1.HAVE                                                 SD1.WANT                                                     */
/*                                                                                                                        */
/*  x    y    z    w     want<-have[colSums(is.na(have)       X    Y                                                      */
/*                             | have == 0) != nrow(have)];                                                               */
/*  1    1    .    0                                          1    1                                                      */
/*  2    2    .    0                                          2    2                                                      */
/*  3    .    .    0                                          3    .                                                      */
/*  4    4    .    0                                          4    4                                                      */
/*  5    5    .    0                                          5    5                                                      */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*                   _
(_)_ __  _ __  _   _| |_
| | `_ \| `_ \| | | | __|
| | | | | |_) | |_| | |_
|_|_| |_| .__/ \__,_|\__|
        |_|
*/

data sd1.have;
 input x y z w;
cards4;
1 1 . 0
2 2 . 0
3 . . 0
4 4 . 0
5 5 . 0
;;;;
run;quit;

/**************************************************************************************************************************/
/*                                                                                                                        */
/*   SD1.HAVE                                                                                                             */
/*                                                                                                                        */
/*   x    y    z    w                                                                                                     */
/*                                                                                                                        */
/*   1    1    .    0                                                                                                     */
/*   2    2    .    0                                                                                                     */
/*   3    .    .    0                                                                                                     */
/*   4    4    .    0                                                                                                     */
/*   5    5    .    0                                                                                                     */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*
 _ __  _ __ ___   ___ ___  ___
| `_ \| `__/ _ \ / __/ _ \/ __|
| |_) | | | (_) | (_|  __/\__ \
| .__/|_|  \___/ \___\___||___/
|_|
*/

%utl_submit_wps64x('
 libname sd1 "d:/sd1";
 proc r;
 export data=sd1.have r=have;
 submit;
 want<-have[colSums(is.na(have) | have == 0) != nrow(have)];
 want;
 endsubmit;
 import data=sd1.want r=want;
');

proc print data=sd1.want;
run;quit;

 /**************************************************************************************************************************/
 /*                                                                                                                        */
 /* SD1.WANT                                                                                                               */
 /*                                                                                                                        */
 /* Obs    X    Y                                                                                                          */
 /*                                                                                                                        */
 /*  1     1    1                                                                                                          */
 /*  2     2    2                                                                                                          */
 /*  3     3    .                                                                                                          */
 /*  4     4    4                                                                                                          */
 /*  5     5    5                                                                                                          */
 /*                                                                                                                        */
 /**************************************************************************************************************************/

/*              _
  ___ _ __   __| |
 / _ \ `_ \ / _` |
|  __/ | | | (_| |
 \___|_| |_|\__,_|

*/
