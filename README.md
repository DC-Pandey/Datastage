Datastage Project
DataStage is an ETL tool which is used to Extract the data from different data source, Transform the data as per the business requirement and Load into the target database. The data source can be of any type like Relational databases, files, external data sources, etc.

Important Clients Interface
Three components comprise the DataStage client: DataStage Administrator, DataStage Designer, DataStage Director.

![image](https://user-images.githubusercontent.com/111442914/189316206-d1681ee5-8070-4c2b-b4a8-060a53647afe.png)

RESPONSIBILITIES FOR Datastage Administrator:
Developing new tools and processes to ensure effective use of DataStage product. The position will also be responsible for administrating and maintaining a DataStage shared environment. This includes: Designing and sizing the environment.

RESPONSIBILITIES FOR Datastage Designer:
The DataStage Designer is the primary interface to the metadata repository and provides a graphical user interface that enables you to view, edit, and assemble DataStage objects from the repository needed to create an ETL job. An ETL job should include source and target stages.

RESPONSIBILITIES FOR Datastage Director:
DataStage Director has three view options: The Status view displays the status, date and time started, elapsed time, and other run information about each job in the selected repository category. The Schedule view displays job scheduling details. The Log view displays all of the events for a particular run of a job.

![image](https://user-images.githubusercontent.com/111442914/189316276-0b66c09a-8606-48cb-88c8-19c9e6a309ab.png)

Oracle connector
Oracle connector is used to access Oracle database systems and perform various read, write, and load functions. Setting required user privileges.

![image](https://user-images.githubusercontent.com/111442914/189316479-5cb70b88-e1c1-468d-b27f-4c5ad1c93209.png)

Parameter sets
Parameter sets enable you to expose different parameters to the user. And, to return different information based on the parameters specified by the user. You can only use one parameter set at a time.

![image](https://user-images.githubusercontent.com/111442914/189316522-25a30f96-e5a4-4323-8b18-774fb1a2553d.png)

########################################################################################

Project consists of 4 major jobs-
ORA - data from Oracle Database
DS - Dataset Files
FILE - Sequential Files
AGG - Aggregation
SHARED - Shared Containers
TRANSFORMER
MERGE
JOIN
LOOKUP
FUNNEL
SORT
FILTER
REMOVE_DUPLICATES
ORACLE_DATABASE
EXECUTE_COMMANDS

- ( 1 ) Requirement: SOURCE --> TARGET
![image](https://user-images.githubusercontent.com/111442914/189316799-57c2aee7-39be-432c-bc83-156a0d885aa9.png)

Implementation:
![image](https://user-images.githubusercontent.com/111442914/189316895-2debe607-160b-455e-a7fd-e3876e65d0f8.png)

- ( 2 ) Requirement: SOURCE --> TARGET
![image](https://user-images.githubusercontent.com/111442914/189316997-021ca609-9745-48ae-b5ab-463f058906b1.png)

Implementation:
alt text

alt text

- ( 3 ) Requirement: SOURCE --> TARGET
alt text

Implementation:
alt text

- ( 4 ) Requirement: SOURCE --> TARGET
alt text

Implementation:
alt text

SEQUENCIAL JOBS
Sequence job, that you use to specify a sequence of parallel jobs or server jobs to run. You specify the control information, such as the different courses of action to take depending on whether a job in the sequence succeeds or fails.

alt text

alt text

########################################################################################

Some more Jobs-

1] SRC :- EMP(ORA)
   TRG(FILE) :- EMPNO,ENAME,SAL,SEQ_NUM
   DATA :- ONLY FOR SAL >= 1000 & GENERATE THE SEQ_NUM.

2] SRC :- EMP(ORA), DEPT_(ORA)
   TRG(DS) :- RANK_NUM,EMPNO,ENAME,SAL,DEPTNO,DNAME,LOC
   RANK_NUM = SEQUENCE NUMBERS
   DATA :-TOP 3 SAL RECORDS

3] SRC :- EMP
   TRG(DS) :- EMPNO,ENAME,SAL,COMM,TSAL,DEPTNO,RANK_NUM
   RANK_NUM = SEQUENCE NUMBERS

   DATA :- TSAL = SAL + COMM[HANDLE NULL]
	   BOTTOM 5 TSAL RECORDS.

4] SRC :- EMP
   TRG[2 ORA TABLES] :- EMPNO,ENAME,SAL,TAX,SEQ_NUM
   SEQ_NUM = SEQUENCE NUMBERS

   DATA :- TAX = SAL * 0.10

	   DATA INTO THE TRGS MUST BE LOADED IN AN ALTERNATE WAY.


@PARTITIONNUM + ((@INROWNUM -1) * @NUMPARTITIONS) +1


-------------------------------------------------------------

1] SRC :- EMP(ORA),DEPT(ORA)
   TRG(2) :- DEPTNO,DNAME,LOC,SUM_SAL,TAX_SUM_SAL

	TRG1(ORA) :- 
	SUM(SAL) GROUP BY DEPTNO 
	TAX_SUM_SAL = SUM_SAL * 0.50 IF DEPTNO = 10
	TAX_SUM_SAL = SUM_SAL * 0.25 IF DEPTNO = 20
	TAX_SUM_SAL = SUM_SAL * 0.10 IF DEPTNO = 30
		
	DATA FOR TRG1(DS) :- (DEPTNO = 10 OR DEPTNO = 20) AND TAX_SUM_SAL >= 2000
		   
	TRG2(FILE) :- SUM_SAL BETWEEN 4000 & 15000 

2] SRC :- EMP(ORA)
   TRG(DS) :- EMPNO,ENAME,SAL,DEPTNO

	DEPTNO = P1(INTEGER) & SAL >= P2(INTEGER)

3] SRC :- EMP(ORA),DEPT(ORA)
   TRG() :- 
	CREATE A PARAMETER SET WITH REQUIRED PARAMETERS & USE IN THIS JOB :-

	TRG1(FILE) :- EMPNO,ENAME,SAL,COMM,TSAL,TAX,TRATE,NETSAL
	
	TSAL = SAL + COMM[HANDLE NULL]
	TRATE = P1(FLOAT/DOUBLE)
	TAX = TSAL * TRATE
	NETSAL = TSAL - TAX

	DATA ONLY FOR NETSAL >= P2(FLOAT/DOUBLE)

	TRG2(DS) :- SEQ_NUM,DEPTNO,DNAME,SUM_SAL,TAX_SUM_SAL,TAX_RATE
	TAX_RATE = P3(FLOAT/DOUBLE)
	SUM_SAL = SUM(SAL) GROUP BY DEPTNO 
	TAX_SUM_SAL = SUM_SAL * TAX_RATE

	SEQ_NUM = GENERATE SEQUENCE NUMBER


- ( 1 )
alt text

- ( 2 )
alt text

- ( 3 )
alt text

- ( 4 )
alt text

- ( 5 )
alt text

- ( 6 )
alt text

- ( 7 )
alt text
