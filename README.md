# utl-importing-excel-string_of_32-thousand-characters-XLConnect
Importing excel string_of_32 thousand characters XLConnect 
    Importing excel string_of_32 thousand characters XLConnect                                                      
                                                                                                                    
    Last time a checked 32k was max length for a string in an excel cell.                                           
    Even though SAS can export a 32k string to excel. SAS cannot import the string.                                 
                                                                                                                    
    github                                                                                                          
    https://tinyurl.com/y2cw25x4                                                                                    
    https://github.com/rogerjdeangelis/utl-importing-excel-string_of_32-thousand-characters-XLConnect               
                                                                                                                    
    SAS Forum                                                                                                       
    https://tinyurl.com/y3yx5xzu                                                                                    
    https://communities.sas.com/t5/SAS-Programming/Troubleshooting-Proc-Import-Warning-messages/m-p/567751          
                                                                                                                    
    macros                                                                                                          
    https://tinyurl.com/y9nfugth                                                                                    
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                      
                                                                                                                    
    None of these sas interfaces will import a excel string of 32000 characters.                                    
                                                                                                                    
     Limited to 1024 characters                                                                                     
                                                                                                                    
      1. Proc import                                                                                                
      2. Linbame engine (SPSS engine bug)                                                                           
      3. SQL Passthru                                                                                               
                                                                                                                    
      SAS Warning                                                                                                   
      WARNING: During read: Data truncated for column NAME                                                          
                                                                                                                    
    I only know of thee ways to import excel strings of up to 32k.                                                  
    They all require R (or Python)                                                                                  
                                                                                                                    
         1. IML and proc R                                                                                          
                                                                                                                    
         2. This used to work?                                                                                      
            The free express version of WPS does not limit the number of                                            
            rows in the output SAS dataset,                                                                         
            however there appears to be a problem with WPS Express and Java                                         
                                                                                                                    
            Error occurred during initialization of VM                                                              
            Could not reserve enough space for object heap                                                          
            ERROR: Communication failure with R process (I/O error: The pipe has been ended)                        
                                                                                                                    
            The workaround is to use my R interface to read the the 32000 character                                 
            strings and create the R dataframe. Then use WPS to export the R dataframe to SAS                       
                                                                                                                    
         3. Use my R interface  and spss output                                                                     
                                                                                                                    
            SOAPBOX ON                                                                                              
            There is a bug in the superior libname engine for SPSS, maybe SAS introduced it                         
            to push 'proc import' and EG server sales. SAS race to the bottom with inferior                         
            products like 'proc import' and all those editors?                                                      
            SOAPBOX OFF                                                                                             
                                                                                                                    
    * SAS V5 Eport is limited to 200 byte character strings                                                         
                                                                                                                    
    *_                   _                                                                                          
    (_)_ __  _ __  _   _| |_                                                                                        
    | | '_ \| '_ \| | | | __|                                                                                       
    | | | | | |_) | |_| | |_                                                                                        
    |_|_| |_| .__/ \__,_|\__|                                                                                       
            |_|                                                                                                     
    ;                                                                                                               
                                                                                                                    
    %utlfkil(d:/xls/have.xlsx);                                                                                     
    libname xel "d:/xls/have.xlsx";                                                                                 
                                                                                                                    
    data xel.have;                                                                                                  
      length name $32000;                                                                                           
      set sashelp.class(keep=name age sex obs=5);                                                                   
      if mod(_n_,2)=0 then name=repeat("0123456789",31999);                                                         
    run;quit;                                                                                                       
                                                                                                                    
    libname xel clear;                                                                                              
                                                                                                                    
    d:/xls/have.xlsx (alo in github)                                                                                
                                                                                                                    
      +--------------------------------------------------+                                                          
      |     A (lenght 32,000)  |    B       |     C      |                                                          
      +--------------------------------------------------+                                                          
    1 |  NAME                  |   SEX      |    AGE     |                                                          
      +------------------------+------------+------------+                                                          
    2 | ALFRED                 |    M       |    14      |                                                          
      +------------------------+------------+------------+                                                          
    3 |012345678901234567890.. |    F       |    13      |                                                          
      +------------------------+------------+------------+                                                          
    4 | BARBARA                |    F       |    13      |                                                          
      +------------------------+------------+------------+                                                          
    5 |012345678901234567890.. |    F       |    14      |                                                          
      +------------------------+------------+------------+                                                          
    6 | Henry                  |    M       |    14      |                                                          
      +------------------------+------------+------------+                                                          
                                                                                                                    
    *            _               _                                                                                  
      ___  _   _| |_ _ __  _   _| |_                                                                                
     / _ \| | | | __| '_ \| | | | __|                                                                               
    | (_) | |_| | |_| |_) | |_| | |_                                                                                
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                               
                    |_|                                                                                             
    ;                                                                                                               
                                                                                                                    
    Up to 40 obs from WANT total obs=5                                                                              
                                                                                                                    
      Variables in Creation Order                                                                                   
                                                                                                                    
    #    Variable    Type      Len                                                                                  
                                                                                                                    
    1    NAME        Char    32000                                                                                  
    2    SEX         Char        1                                                                                  
    3    AGE         Num         8                                                                                  
                                                                                                                    
    Obs NAME (32000 chars)           SEX    AGE                                                                     
                                                                                                                    
     1  Alfred                        M      14                                                                     
     2  0123456789012345678906.....   F      13                                                                     
     3  Barbara                       F      13                                                                     
     4  0123456789012345678906....    F      14                                                                     
     5  Henry                         M      14                                                                     
                                                                                                                    
    *          _       _   _                                                                                        
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                                        
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|                                                                       
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                                       
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                                       
                                                                                                                    
     _      _           _       __                                                                                  
    / |    (_)_ __ ___ | |     / /  _ __                                                                            
    | |    | | '_ ` _ \| |    / /  | '__|                                                                           
    | |_   | | | | | | | |   / /   | |                                                                              
    |_(_)  |_|_| |_| |_|_|  /_/    |_|                                                                              
                                                                                                                    
    ;                                                                                                               
                                                                                                                    
    proc iml;                                                                                                       
       submit / R;                                                                                                  
        library(XLConnect);                                                                                         
        wb <- loadWorkbook("d:/xls/have.xlsx");                                                                     
        want<-readWorksheet(wb, "have");                                                                            
       endsubmit;                                                                                                   
       run importdatasetfromr("work.want","want");                                                                  
     run;quit;                                                                                                      
                                                                                                                    
    *____                               ___                                                                         
    |___ \     __      ___ __  ___     ( _ )      _ __                                                              
      __) |    \ \ /\ / / '_ \/ __|    / _ \/\   | '__|                                                             
     / __/ _    \ V  V /| |_) \__ \   | (_>  <   | |                                                                
    |_____(_)    \_/\_/ | .__/|___/    \___/\/   |_|                                                                
                        |_|                                                                                         
    ;                                                                                                               
                                                                                                                    
    * Bug with Java XLConnect will not work. I spent 3hrs checking clsspaths, libPaths, enviroment                  
      variables, reinstalling JAVA and chaging JAVA options ie heapsize. This is a WPS express issue?;              
                                                                                                                    
    %utl_submit_r64('                                                                                               
       library(haven);                                                                                              
       library(XLConnect);                                                                                          
       wb <- loadWorkbook("d:/xls/have.xlsx");                                                                      
       want<-readWorksheet(wb, "have");                                                                             
       save(want,file="d:/rda/want.rda")                                                                            
    ');                                                                                                             
                                                                                                                    
    %utl_submit_wps64('                                                                                             
       options set=R_HOME "C:/Program Files/R/R-3.5.1";                                                             
       libname wrk "%sysfunc(pathname(work))";                                                                      
       proc r;                                                                                                      
       submit;                                                                                                      
       load("d:/rda/want.rda");                                                                                     
       endsubmit;                                                                                                   
       import r=want data=wrk.want;                                                                                 
       run;quit;                                                                                                    
    ');                                                                                                             
                                                                                                                    
    proc print data=want;                                                                                           
    run;quit;                                                                                                       
                                                                                                                    
    *         ___                                                                                                   
     _ __    ( _ )     ___ _ __  ___ ___                                                                            
    | '__|   / _ \/\  / __| '_ \/ __/ __|                                                                           
    | |     | (_>  <  \__ \ |_) \__ \__ \                                                                           
    |_|      \___/\/  |___/ .__/|___/___/                                                                           
                          |_|                                                                                       
    ;                                                                                                               
                                                                                                                    
    %utl_submit_r64('                                                                                               
       library(haven);                                                                                              
       library(XLConnect);                                                                                          
       wb <- loadWorkbook("d:/xls/have.xlsx");                                                                      
       want<-readWorksheet(wb, "have");                                                                             
       write_sav(want, "d:/sav/want.sav")                                                                           
    ');                                                                                                             
                                                                                                                    
    proc import out=work.want                                                                                       
      datafile='d:/sav/want.sav'                                                                                    
      dbms=SAV replace;                                                                                             
    run;quit;                                                                                                       
                                                                                                                    
    /*                                                                                                              
                      Variables in Creation Order                                                                   
                                                                                                                    
    #    Variable    Type      Len    Format     Informat    Label                                                  
                                                                                                                    
    1    NAME        Char    32000    $32000.    $32000.     NAME                                                   
    2    SEX         Char        1    $1.        $1.         SEX                                                    
    3    AGE         Num         8    F8.2                   AGE                                                    
    */                                                                                                              
                                                                                                                    
