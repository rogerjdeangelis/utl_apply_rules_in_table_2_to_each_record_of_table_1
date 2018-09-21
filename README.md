# utl_apply_rules_in_table_2_to_each_record_of_table_1
Apply rules in table 2 to each record of table 1. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Apply rules in table 2 to each record of table 1

    github
    https://tinyurl.com/yb75f9q2
    https://github.com/rogerjdeangelis/utl_apply_rules_in_table_2_to_each_record_of_table_1

    SASA Forum
    https://tinyurl.com/yajt4ds4
    https://communities.sas.com/t5/SAS-Programming/Creating-and-resolving-macro-variables-as-a-validation-rule-in/m-p/497851

    INPUT
    =====

     WORK.HAV2ND total obs=3

      rule_no    operand1    rel_oper    operand2

        r1        Field1        >         0
        r2        Field3        =         UT
        r3        Field1        <=        Field2



     WORK.HAV1ST total obs=2

      Field1    Field2    Field3

         3         5       UT
         0        12       XX3


    EXAMPLE OUTPUT
    ==============

      WANT total obs=6

                cmp              Field1    Field2    Field3    rule_no    true

       true= Field1 > "0"          3         5        UT         r1         1   Apply rule 1 to each record in hav1st
       true= Field1 > "0"          0         12       XX3        r1         0

       true= Field3 = "UT"         3         5        UT         r2         1
       true= Field3 = "UT"         0         12       XX3        r2         0

       true= Field1 <= Field2      3         5        UT         r3         1
       true= Field1 <= Field2      0         12       XX3        r3         1


    PROCESS
    =======

     %symdel cmp rule /npwarn;

     data _null_;

       set hav2nd;

       cmp=catx(' ','true=',operand1,rel_oper,ifc(operand2 =: "Field",operand2,quote(strip(operand2))));

       call symputx("cmp",cmp);
       call symputx("rule",rule_no);

       put cmp=;

       rc=dosubl('
           data tmp;
             length cmp $64;
             set hav1st;
             rule_no="&rule";
             cmp=symget("cmp");
             &cmp;
           run;quit;
           proc contents data=tmp;
           run;quit;
           proc append data=tmp base=want;
           run;quit;
       ');

      run;quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data hav1st;
     input Field1$ Field2$ Field3$;
    cards;
     3 5 UT
     0 12 XX3
    ;;;;
    run;quit;


    data hav2nd;
     input rule_no$ operand1$ rel_oper$ operand2$;
    cards4;
     r1 Field1 > 0
     r2 Field3 = UT
     r3 Field1 <= Field2
    ;;;;
    run;quit;

