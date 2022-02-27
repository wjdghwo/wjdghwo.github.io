# PROC FORMAT
- 변수에 대한 Informats과 Formats을 직접 정의할 수 있게 해주는 프로시저
- Informats은 Raw data 값을 읽고 저장, Formats은 산출 되는 변수 값들을 정의
- 형식
```
PROC FORMAT;
    VALUE format-name
      Data-value-1 = 'Label 1'
      Data-value-2 = 'Label 2';
    VALUE format-name-2
      Data-value-3 = 'Label 3'
      Data-value-4 = 'Label 4';
    .....;
RUN;                  
```
- 세미콜론의 위치
    - PROC FORMAT 뒤
    - 각 세트에 대한 마지막 레이블 할당 후
    - RUN 뒤
- 예시
    - 개별 값에 대한 Label 할당
    ```
    PROC FORMAT;
        VALUE LIKERT_SEVEN
        1   = "Strongly Disagree"
        2   = "Disagree"
        3   = "Slightly Disagree"
        4   = "Neither Agree nor Disagree"
        5   = "Slightly Agree"
        6   = "Agree"
        7   = "Strongly Agree";
    RUN;                   
    ```
    - 둘 이상의 데이터 값에 적용되는 Label 할당
    ```
    PROC FORMAT;
        VALUE LIKERT7_A
        1,2,3   = "Disagree"
        4       = "Neither Agree nor Disagree"
        5,6,7   = "Agree"
    RUN;           
    ```
    - 값의 범위를 사용해 Label 할당
    ```
    PROC FORMAT;
        VALUE LIKERT7_B
        1-3     = "Disagree"
        4       = "Neither Agree nor Disagree"
        5-7     = "Agree";
    RUN;              
    ```
    - LOW, HIGH, 부등호를 사용해 Label 할당
    ```
    PROC FORMAT;
        VALUE INCOME
        LOW   -< 20000 = "Low"
        20000 -< 60000 = "Middle"
        60000 - HIGH   = "High";
    RUN;           
    ```
    - OTHER를 사용해 Label 할당
    ```
    PROC FORMAT;
        VALUE RACE
        1     = "White"
        2     = "Black"
        OTHER = "Other";
    RUN;        
    ```
    - 문자 변수에 대해 Label 할당
    ```
    PROC FORMAT;
        VALUE $GENDERLABEL
        "M"   = "Male"
        "F"   = "Female";
    RUN;       
    ```
    - 결측치에 대해 FORMAT 할당
    '''
    PROC FORMAT;
        VALUE MYFMT .='N/A' other=[5.1];
    RUN;
    '''
    - 24시간 형식을 00:00:01–24:00:00으로 변경
    '''
    PROC FORMAT;
        PICTURE  HOURS (default=19)
            other='%Y-%0m-%0d %0H:%0M:%0S' (datatype=datetime_util);
    RUN;
    '''
    - 데이터 확인
    ```
    PROC FORMAT;
        VALUE FMTMARRIED
        0     = "NO"
        1     = "YES";
    RUN; 
    ```
    ```
    PROC PRING DATA = SURVEY;
        VAR SUBJECT MARRIED GENDER;
        FORMAT MARRIED FMTMARRIED.;
    RUN;    
    ```
    - 데이터 저장
    ```
    PROC FORMAT;
        VALUE GENDERCODE
        0 = 'Male'
        1 = 'Female';
        VALUE ATHLETECODE
        0 = 'Non-athlete'
        1 = 'Athlete';
        VALUE SMOKINGCODE
        0 = 'Nonsmoker'
        1 = 'Past smoker'
        2 = 'Current smoker';
    RUN;  
    ```
    ```
    DATA sample_formatted2;
        SET sample;
        FORMAT gender GENDERCODE. athlete ATHLETECODE. smoking SMOKINGCODE.;
    RUN;
    ```
    - GENDERCODE.에서 마침표가 오는 이유는 변수가 아닌 FORMAT이라는 표시이다.




# PROC REPORT
- PRINT, MEANS과 TABULATE procedure 기능들과 여러 리포트를 산출할 수 있는 단일 보고서작성 툴 안의 DATA step 의 기능을 합하는 프로시저. 
- 윈도우 환경/비윈도우 환경에서 사용 사능
- 형식
```
PROC REPORT<options>; /* 요약, 세부 보고서 생성 */
    BREAK location break-variable </ options> /* 그룹 또는 주문 변수의 값이 변경될 때 요약 생성 */
    BY variable-1 /* 각 BY 그룹에 대해 별도의 보고서 생성 */
        <<DESCENDING> variable-2 ...> <NOTSORTED>;
    COLUMNcolumn-specification(s); /* 두 개 이상의 열에 걸쳐 있는 모든 열과 제목의 배열을 설명 */
        COMPUTE location <target>
            </ STYLE=<style-override(s)> >;
                LINE specification(s); /* 맞춤형 요약 작성을 위한 PUT 문의 기능 하위 집합 제공 */
                . . . select SAS language elements . . .
                ENDCOMP;
        COMPUTE report-item </ type-specification>; /* PROC REPORT가 보고서를 작성할 때 실행하는 프로그래밍 문을 지정 */
                CALL DEFINE (column-id ', < ' attribute-name', value> /* 현재 행의 특정 열에 대한 속성 값 설정 */
                    | _ROW_, < 'attribute-name', value>);
                . . . select SAS language elements . . .
                ENDCOMP;
    DEFINE report-item / <options>; /* 보고서 항목 사용 및 표시방법 설명 */
    FREQ variable; /* 입력 데이터 세트에 여러 번 나타나는 것처럼 관측치 처리 */
    RBREAK location </ options>; /* 보고서의 시작이나 끝 또는 각 BY 그룹의 시작과 끝에 기본 요약을 생성 */
    WEIGHT variable; /* 통계 계산에서 분석 변수에 대한 가중치 지정 */
```



- 예시
- 변수 선택 및 요약 생성
```
libname proclib 'SAS-library';
data grocery;
    input Sector $ Manager $ Department $ Sales @@;
    datalines;
se 1 np1 50    se 1 p1 100   se 1 np2 120   se 1 p2 80
se 2 np1 40    se 2 p1 300   se 2 np2 220   se 2 p2 70
nw 3 np1 60    nw 3 p1 600   nw 3 np2 420   nw 3 p2 30
nw 4 np1 45    nw 4 p1 250   nw 4 np2 230   nw 4 p2 73
nw 9 np1 45    nw 9 p1 205   nw 9 np2 420   nw 9 p2 76
sw 5 np1 53    sw 5 p1 130   sw 5 np2 120   sw 5 p2 50
sw 6 np1 40    sw 6 p1 350   sw 6 np2 225   sw 6 p2 80
ne 7 np1 90    ne 7 p1 190   ne 7 np2 420   ne 7 p2 86
ne 8 np1 200   ne 8 p1 300   ne 8 np2 420   ne 8 p2 125
;
```
```
proc format library=proclib;
    value $sctrfmt 'se' = 'Southeast'
                    'ne' = 'Northeast'
                    'nw' = 'Northwest'
                    'sw' = 'Southwest';

    value $mgrfmt '1' = 'Smith'   '2' = 'Jones'
                    '3' = 'Reveiz'  '4' = 'Brown'
                    '5' = 'Taylor'  '6' = 'Adams'
                    '7' = 'Alomar'  '8' = 'Andrews'
                    '9' = 'Pelfrey';

    value $deptfmt 'np1' = 'Paper'
                    'np2' = 'Canned'
                    'p1'  = 'Meat/Dairy'
                    'p2'  = 'Produce';
run;
options fmtsearch=(proclib);
```
```
proc report data=grocery; /* 보고서 옵션을 지정 */
    column manager department sales; /* 보고서 열을 지정, 문자변수는 표시변수로 숫자 변수는 분석 변수로 사용 */
    rbreak after / summarize style=[font_weight=bold]; /* 보고서 요약 생성, 보고서 끝에 생성, 요약된 값의 글자는 굵게 */
    where sector='se'; /* 처리할 관측치 선택, se의 관측치만 선택 */
    format manager $mgrfmt. department $deptfmt. 
            sales dollar11.2; /* 보고서 열의 형식 지정 */
    title 'Sales for the Southeast Sector'; /* 제목 지정 */
    title2 "for &sysdate"; /* sysdate : 시작 날짜 반환 자동 매크로 변수 */
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex01.png?locale=en)

- 보고서 행 순서 지정
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery;
    column manager department sales;
    define manager / order order=formatted format=$mgrfmt.;
    define department / order order=internal format=$deptfmt.;
    /* 정렬 순서 변수 정의, 여기선 manager(형식이 지정됨), department(내부 값에 따라 정렬)의 순서로 정렬 */
    define sales / analysis sum format=dollar7.2;
    /* 분석 변수 정의, 여기선 합계 계산 */
    break after manager / summarize
                            style=[font_style=italic]; /* 보고서 요약 생성 */
    where sector='se';
    title 'Sales for the Southeast Sector';
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex02.png?locale=en)

- 별칭을 사용하여 동일한 변수에 대한 여러 통계 값 얻기
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery;
/* 열 및 별칭에 사용할 열 적용 */
column manager department sales
        sales=salesmin
        sales=salesmax;
        /* 정렬 순서 변수 정의 */
define manager / order
                order=formatted
                format=$mgrfmt.
                'Manager';
define department    / order
                order=internal
                format=$deptfmt.
                'Department';
define sales / analysis sum format=dollar7.2 'Sales';
/* 별칭 속성 입력 */
define salesmin / analysis min noprint;
define salesmax / analysis max noprint;
/* 맞춤형 요약 생성 */
compute after;
    line 'Departmental sales ranged from'
        salesmin dollar7.2  " " 'to' " " salesmax dollar7.2;         
endcomp;
where sector='se';
title 'Sales for the Southeast Sector';
title2 "for &sysdate";
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex03.png?locale=en)

- 하나의 변수에 대한 여러 통계값 표시
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery;
/* 구하고자하는 여러 통계값 입력 */
column sector manager (Sum Min Max Range Mean Std),sales;
   define manager / group format=$mgrfmt.;
   define sector / group format=$sctrfmt.;
   define sales / format=dollar11.2 ;
   title 'Sales Statistics for All Sectors';
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex04.png?locale=en)

- 여러 관찰값들을 보고서의 한 행으로 통합
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery;
   column sector manager sales;
   /* 순서대로 관찰값 그룹화 */
   define sector / group format=$sctrfmt.'Sector';
   define manager / group format=$mgrfmt.'Manager';
   define sales / analysis sum format=comma10.2 'Sales';
   break after sector / summarize
                        style=[font_style=italic]
                        suppress;
   /* 맞춤형 요약 지정 */                     
   compute after;
      line 'Combined sales for the northern sectors were '
            sales.sum dollar9.2 '.';
   endcomp;
   /* 요약행의 형식 지정 */
   compute sales;
      if _break_ ne ' ' then
      call define(_col_,"format","dollar11.2");
   endcomp;
   where sector contains 'n';
   title 'Sales Figures for Northern Sectors';
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex05.png?locale=en)

- 변수의 각 값에 대한 열 생성
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery split='*';
 /* 보고서에 사용할 행 지정 */
   column sector manager department,sales perish;
   define sector / group format=$sctrfmt. 'Sector' '';
   define manager / group format=$mgrfmt. 'Manager* ';
   /* 전체 변수 지정 */
   define department / across format=$deptfmt. 'Department';
   /* 계산 변수 지정 */
   define sales / analysis sum format=dollar11.2 ' ';
   /* 사용자 정의 변수 지정 */
   define perish / computed format=dollar11.2
                'Perishable*Total';
   /* 사용자 정의 변수 계산 방법 지정 */
   compute perish;
      perish=sum(_c3_, _c4_);
   endcomp;
   compute after;
            line  'Combined sales for meat and dairy : '
            _c3_ dollar11.2 '';
      line  'Combined sales for produce : '
            _c4_ dollar11.2 '';
      line  'Combined sales for all perishables: '
            _c5_ dollar11.2 '';
         endcomp;
   where sector contains 'n'
         and (department='p1' or department='p2');
   title 'Sales Figures for Perishables in Northern Sectors';
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex06.png?locale=en)


- 각 페이지에 사용자 정의 요약 작성
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery;
   title 'Sales for Individual Stores';
   column sector manager department sales Profit;
    /* 분석 변수 지정 */
    /* sector, manager print 제외 */
   define sector / group noprint;
   define manager / group noprint;
   /* profit은 식 지정, sales은 합 추출 */
   define profit / computed format=dollar11.2;
   define sales / analysis sum format=dollar11.2;
   /* department로 그룹핑 */
   define department / group format=$deptfmt.;
   /* 계산 변수 식 지정 */
   compute profit;
      if department='np1' or department='np2'
         then profit=0.4*sales.sum;
      else profit=0.25*sales.sum;
   endcomp;
    compute before _page_ / style={just=left};
      line sector $sctrfmt. ' Sector';
      line 'Store managed by ' manager $mgrfmt.;
         endcomp;
    /* 보고서 요약 생성(manager 기준) */
   break after manager / summarize style=[font_style=italic] page;
   compute after manager;
      length text $ 35;
      if sales.sum lt 500 then
         text='Sales are below the target region.';
      else if sales.sum ge 500 and sales.sum lt 1000 then
         text='Sales are in the target region.';
      else if sales.sum ge 1000 then
         text='Sales exceeded goal!';
      line text $35.;
   endcomp;
   run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex07.png?locale=en)

- 백분율 계산
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc report data=grocery;
   title;
   /* 행 지정 및 텍스트를 확장 제목 지정 */
   column ('Individual Store Sales as a Percent of All Sales'
            sector manager sales,(sum pctsum) comment);
   /* 그룹핑 */
   define manager / group
                    format=$mgrfmt.;
   define sector / group
                   format=$sctrfmt.;
    /* 분석 열 지정 */                  
   define sales / format=dollar11.2
                  '';
   define sum / format=dollar9.2
                'Total Sales';
   /* 백분율 및 계산된 열 지정 */             
   define pctsum / 'Percent of Sales' format=percent6.;
   define comment / computed style(column)=[cellwidth=2.5in];
   /* comment은 문자 데이터 */  
   compute comment / char length=40;
      /* pctsum가 0.15 초과 하면 문구 기입 */  
      if sales.pctsum gt .15 and _break_ = ' '
      then comment='Sales substantially above expectations.';
      else comment=' ';
   endcomp;
   rbreak after / summarize style=[font_style=italic];
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex08.png?locale=en)


- 결측치 처리
    - proc report에서 결측치가 존재할 때 기본 옵션은 결측치를 제외하고 계산 및 출력을 한다.
    - 이때 결측치를 포함하고 싶으면 (그룹핑과 같은 곳) ```proc report data=grocmiss missing;``` 처럼 `missing`옵션을 사용한다.

- 출력 데이터 세트 생성 및 계산된 변수 저장
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
/* 보고서 옵션 및 열을 지정 */ 
proc report data=grocery
            /* 출력 데이터 생성, 1000 초과 관측값만 추출*/ 
            out=profit( where=(sales gt 1000 and _break_='') );
   column manager sales manager_pct;
   define manager / group;
   define manager_pct / computed;
   compute before;
      total_sales = sales.sum;
   endcomp;
   compute manager_pct;
      manager_pct = sales.sum /total_sales;
   endcomp;
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex10out.png?locale=en)
```
/* 저장된 데이터 report로 추출 */ 
proc report data=profit;
   column manager sales manager_pct;
   define manager / group format=$mgrfmt.;
   define sales / analysis sum format=dollar11.2;
   define manager_pct /'Percent of Total Sales' format=percent8.2;
   title 'Managers with Daily Sales';
   title2 'of over';
   title3 'One Thousand Dollars';
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex10.png?locale=en)

- FORMAT을 사용하여 그룹 생성
```
libname proclib 'SAS-library';
options fmtsearch=(proclib);
proc format;
   value $perish 'p1','p2'='Perishable'
                'np1','np2'='Nonperishable';
run;
proc report data=grocery;

   column manager department,sales sales;
   define manager / group order=formatted
                    format=$mgrfmt.;
   define department / across order=formatted
                /* proc format에서 만든 format 입력 */
                 format=$perish. '';
   define sales / analysis sum
                  format=dollar9.2 style=[cellwidth=13];
   compute after / style=[font_style=italic];
      line 'Total sales for these stores were: '
            sales.sum dollar9.2;
   endcomp;
title 'Sales Summary for All Stores';
run;
```
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex11.png?locale=en)

- 다중 레이블 형식 사용
```
proc format;
  value agelfmt (multilabel notsorted)
     11='11'
     12='12'
     13='13'
     14='14'
     15='15'
     16='16'
     11-12='11 or 12'
     13-14='13 or 14'
     15-16='15 or 16'
     low-13='13 and below'
     14-high='14 and above' ;
run;
ods html file="example.html";
title "GROUP Variable with MLF Option";
proc report data=sashelp.class;

   col age ('Mean' height weight);
    
    /* 다중레이블 옵션 mlf 입력 후 format 기준 입력 */
   define age / group mlf format=agelfmt. 'Age Group' order=data preloadfmt; 
   define height / mean format=6.2 'Height (in.)';
   define weight / mean format=6.2 'Weight (lbs.)';
run;
```
- `proc format`으로 다중레이블 지정 후 `proc report` `define`의 `group`에서 다중레이블 지정
![](https://documentation.sas.com/api/docsets/proc/9.4/content/images/report_ex13.png?locale=en)
