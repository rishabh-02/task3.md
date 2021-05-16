
<h2 align="center">Task-3</h2>

<details>
  <summary> Summary of Task </summary>
  <ul>
    <br>
    <li> Write a script in Shell.</li>
    <li> This script has been used to download 2 google sheets. </li>
    <li> Both of those Google sheets will have the formate csv file. </li>
    <li> Only the name, Average and Sum columns and their values should be printed. </li>
  </ul>
</details>

<details>
<summary> INDEX </summary>
  <ul>
    <br>
    <li> Test cases</li>
    <li> Implementation </li>
    <li> Script </li>
    <li> Configuration File </li>
    <li> Log file </li>
    <li> download files link  </li>
    <li> Conclusion </li>
  </ul>
  </details>

<details>
<details>
  <summary> Test Cases </summary>
  
|S.NO|Test Cases|Test Case Description|Expected Result|Test Status|Output|
|:----:|:-----:|:-----:|:-----:|:-----:|:----:|
|1|**Published Url** |First of all, i used publish to the web option to publish a spreadsheet link and select the .csv format |Url should be published|**PASS** |![webpubleshed](https://user-images.githubusercontent.com/82143335/117056330-d26c8300-ad39-11eb-8232-a9609f1167f8.PNG)|
|2|**The path of commands  is declared in Variable** |I declared the path of commands in variables in the configuration file which i used in my script file. |Path of command should be declare in the variable |**PASS** |![variables](https://user-images.githubusercontent.com/82143335/117057021-95ed5700-ad3a-11eb-9253-faa5df8ca109.PNG)|
|3|**Google spread sheet downloaded in CSV format** |I used wget with -q option with url of the google spread sheet to download in csv format -q option is used for silently downloaded <br/> I used this $WGET $WGETOPT1 $MYURL111 and $MYURL222 the value of these variable extracting from the configuration file |Google spreadsheet in csv format should be downloaded |**PASS** | ![csv formate](https://user-images.githubusercontent.com/82143335/117057486-24fa6f00-ad3b-11eb-8236-6796b3b96303.PNG) |
  
  </details>
  
  <details>
  <summary> Implementation </summary>
  
In this script, first of all I used publish to the web option to copy the link to the spreadsheet. into the csv format
After that  downloaded the link of the spreadsheet using the wget command and rename the download file with the mv command.
Then I got the required output from some Scripting.
  
  </details>
  
  <details>
  <summary> Script </summary>
  
! /bin/bash

#MKDIR=/usr/bin/mkdir   # Path of the commands used here are stored in variables and these are commented for future reference.
#RENAME=/usr/bin/mv     # These values are currently forked from conf file.
#ECHO=/usr/bin/echo
#SHOW=/usr/bin/cat
#DOWNLOAD=/usr/bin/wget
#MATCH=/usr/bin/grep
#TRAN=/usr/bin/tr
#COUNT=/usr/bin/wc
#SCAN=/usr/bin/awk
#date=`/usr/bin/date`
Pwd=`/usr/bin/pwd`          # pwd is Enabled to source the working directory of config file at the beginning
#REM=/usr/bin/rm
#TOUCH=/usr/bin/touch
source $Pwd/rishabh.conf     # used to source the config file to the script and $Pwd will redirect the exact path
$TOUCH $Pwd/scriptlogs  # log file has been created to the directory
#-----------------------------------------------------------------------------------------------------------------------------------------
func(){ if [[ $? -ne 0 ]]; then $ECHO "Failure at $1">> $Pwd/Scriptlogs.log 
        exit
    else 
	    $ECHO $date $Pwd $0 $2 $3 $4 
	    fi }                                   #function 'func()' will match the condition if commands execute or not and give the logs                                                       failure an exit if condition of execution fails.
                                                   #if function passes then it will generate output to logs folder from wherever it is called
						       
#----------------------------------------------------------------------------------------------------------------------------------------- 
$ECHO $date $Pwd  " INITIALISING $0">> $Pwd/Scriptlogs.log                                          #storing logs in specified >> filename 

$ECHO "Enter new Directory Name for Storing Data Files:" 
func $LINENO "[INITIAL]:Asking user to input directory name to store output datafile">> $Pwd/Scriptlogs.log
                                                          #LINENO will output the failure line no. to log folder with specified >> filename 
                                                          #Function func is called to store the respective values in $ assigned in func()

read DIR                                                 #input from user is stored to variable DIR with the help of read command.
func $LINENO "[SUCCESS]: Entered directory name is-" "$DIR" "">> $Pwd/Scriptlogs.log                   #storing logs in specified >> filename 

func $LINENO "[CHECK]:checking if directory name-" "$DIR" " already Exists or not">> $Pwd/Scriptlogs.log                                                                                                                                             #storing logs in specified >> filename
#-----------------------------------------------------------------------------------------------------------------------------------------

if test -d "$DIR"; then                            #Function will test if input directory name exists or not, if true then proceed.	
$ECHO "Already Exists! Try New"               
$ECHO $date $Pwd $0 "[FAIL]: Directory " "$DIR" " already Exists ">> Scriptlogs.log    
$0                                               #If Conditios is true, then it runs the script again to ask new directory name

func $LINENO "[REINITIATING SCRIPT "$0"] ">> Scriptlogs.log

else                                               #IF Condition is False, then it Enters under else for further processing.
#------------------------------------------------------------------------------------------------------------------------------------------

$MKDIR $DIR                                        #Creating a new Directory to store the downloaded and generated output files.
func $LINENO "[SUCCESS]: New Directory" "$DIR" " has been created at "$Pwd"">> $Pwd/Scriptlogs.log    #storing logs in specified >> filename 

cd $DIR                                            #to Enter inside the new created Directory.
func $LINENO "[SUCCESS]:Output Directory changed to "$Pwd"/"$DIR"" >> $Pwd/Scriptlogs.log              #storing logs in specified >> filename 

#------------------------------------------------------------------------------------------------------------------------------------------

$ECHO "Downloading Spreadsheets from url and Extracting data...."

#URL1='https://docs.google.com/spreadsheets/d/e/2PACX-1vTrNldUZStbLCL-Q9Le9ilWrWxR1XW5N4zOzpBbM4aBEsgp2wheS7ioOx0yQ8a_zZuxvw4fXkwYH-Mh/pub?output=csv'
#URL2='https://docs.google.com/spreadsheets/d/e/2PACX-1vRc3-RATBQ0U-XYPwb8uRHs0sMwJspqnspJxWFPXVz_pF0NA2QTFA-rkmPsRjMOlF_xPdpwBRjYOkhK/pub?output=csv'                                           #These links are already forked from config file and commented for future reference

$DOWNLOAD -q -O 'Evaluation_Sheet1.csv' $URL1                                 #To download the file Quietly using the published link of First spreadsheet.
func $LINENO "[SUCCESS] Spreadsheet1 downloaded from below url -" "$URL1" " ">> $Pwd/Scriptlogs.log    #storing logs in specified >> filename

$DOWNLOAD -q -O 'Evaluation_Sheet2.csv' $URL2                                 #To download the file Quietly using the published link of First spreadsheet.
func $LINENO "[SUCCESS] Spreadsheet2 downloaded from below url --" "$URL2" " ">> $Pwd/Scriptlogs.log    #storing logs in specified >> filename

#------------------------------------------------------------------------------------------------------------------------------------------

name1=$($SHOW Evaluation_Sheet1.csv|$MATCH -i name|$SCAN -F"$IntNm" '{print $1}'|$TRAN -cd , |$COUNT -c) add1=1 count1=$((name1+add1))
func $LINENO "[SUCCESS] Counted column no. of [_Intern Name_ in sheet1="$c1"]">> $Pwd/Scriptlogs.log
name2=$($SHOW Evaluation_Sheet2.csv|$MATCH -i name|$SCAN -F"Intern Name" '{print $1}'|$TRAN -cd , |$COUNT -c) count2=$((name2+add1))
func $LINENO "[SUCCESS] Counted column no. of [_Intern Name_ in sheet2="$c2"]">> $Pwd/Scriptlogs.log    #storing logs in specified >> filename


avg1=$($SHOW Evaluation_Sheet1.csv|$MATCH -i Average|$SCAN -F"$Avg" '{print $1}'|$TRAN -cd , |$COUNT -c) count3=$((avg1+add1))
func $LINENO "[SUCCESS] Counted column no. of [_Average_ in sheet1="$f1"]">> $Pwd/Scriptlogs.log
avg2=$($SHOW Evaluation_Sheet2.csv|$MATCH -i Average|$SCAN -F"Average" '{print $1}'|$TRAN -cd , |$COUNT -c) count4=$((avg2+add1))
func $LINENO "[SUCCESS] Counted column no. of [_Average_ in sheet2="$f2"]">> $Pwd/Scriptlogs.log        #storing logs in specified >> filename
  

The Above Function will match the name and average in the respective addressed sheet and count the number of commas before that               Particular found column and add +1 to get the exact column number in the sheet.Here name1,name2 and avg1,avg2 will count the                  commas with addition of +1 using variable add1. Further count1,count2 will store column no. of Intern name and count3,count4                  will store column no. of Average.   

#------------------------------------------------------------------------------------------------------------------------------------------

$SCAN -v "col1=$count1" -v "col2=$count3" -v "start=$Firstrow" -v "end=$Lastrow" -F"," 'BEGIN{printf "_____________________Evaluation Spreadsheet I (on Daily Basis)______________________\n"}NR>start && NR<end{print" " "NAME   : "$col1, "\n" , "SUM    : " $col2*8, "\n", "AVERAGE: " $col2, "\n","__________________"}' Evaluation_Sheet1.csv > Output_of_sheet.csv

func $LINENO  "[SUCCESS]: Extracted Name, Sum, Column from sheet1 and stored to "$Pwd"/"$DIR" as (Output_of_Sheet)">> $Pwd/Scriptlogs.log    #storing logs in specified >> filename 


$SCAN -v "col3=$count2" -v "col4=$count4" -v "start=$Firstrow" -v "end=$Lastrow" -F',' 'BEGIN{printf "_____________________Evaluation Spreadsheet II (on the basis of MD file)______________________\n"}NR>start && NR<end{print" " "NAME   : "$col3, "\n" , "SUM    : " $col4*8, "\n", "AVERAGE: " $col4, "\n","__________________"}' Evaluation_Sheet2.csv >> Output_of_sheet.csv
func $LINENO  "[SUCCESS]: Extracted Name, Sum, Column from sheet2 and stored to "$Pwd"/"$DIR" as (Output_of_Sheet)">> $Pwd/Scriptlogs.log    #storing logs in specified >> filename

 
AWK Command is used to Display the output with desired 2nd(Intern name) & 11th(Average) column & calculating the sum.                         -F is used here as field seperator(,) for each record and NR is used to Extract the data which is required i.e,                                  it's printing the no. of records between 4th to 24th or 26th to Extract and display the records lying within it.                              Further redirecting the output using > operator and append >> operator to the desired file name. 
#--------------------------------------------------------------------------------------------------------------------------------------------  
$SHOW Output_of_sheet.csv                                                     # Used to Display the output of sheets to the terminal
func $LINENO "[SUCCESS] Output generated to terminal ">> $Pwd/Scriptlogs.log     #storing logs in specified >> filename
func $LINENO "[EXITING "$0" ]">> $Pwd/Scriptlogs.log                             #storing logs in specified >> filename                        

fi                                                                          #End of if condition                                                                                                                                                                                                                                    
  </details>
  
 <details>
  <summary> Configuration file </summary>
  
#This is the Config File of script.sh 

#Name of the Script: script.sh
#Temporary location: /home/ubuntu/Desktop/myscript
#Operating System  : Ubuntu 20.04 LTS
#Editor used       : Vi
#package include   : script.conf, script.sh, scriplogs.log , $datafile (as per Entered directory name)

#Description of Script: Script is downloading the two google spreadsheet links mentioned below in csv format and Extracting the data in the
                       #form of Name:____ Sum:____ Average:_____ from both the Evaluation sheets.  

#Below are the list of path variables declared:
MKDIR=/usr/bin/mkdir           #Creating a Directory
RENAME=/usr/bin/mv             #Moving or Renaming file or Directory
ECHO=/usr/bin/echo             #To Print
SHOW=/usr/bin/cat              #To show content of Files 
DOWNLOAD=/usr/bin/wget         #Non-Interactive network downloader
MATCH=/usr/bin/grep            #To search or match the content
TRAN=/usr/bin/tr               #To traverse or delete
COUNT=/usr/bin/wc              #To Count
SCAN=/usr/bin/awk              #To Extract the data from files
date=`/usr/bin/date`           #To Print the Date and Time
Pwd=`/usr/bin/pwd`             #To print working directory
REM=/usr/bin/rm                #To remove the file
TOUCH=/usr/bin/touch
#URLs used to download both google spreadsheet files through public link exporting the data to csv:

URL1='https://docs.google.com/spreadsheets/d/e/2PACX-1vTrNldUZStbLCL-Q9Le9ilWrWxR1XW5N4zOzpBbM4aBEsgp2wheS7ioOx0yQ8a_zZuxvw4fXkwYH-Mh/pub?output=csv'

URL2='https://docs.google.com/spreadsheets/d/e/2PACX-1vRc3-RATBQ0U-XYPwb8uRHs0sMwJspqnspJxWFPXVz_pF0NA2QTFA-rkmPsRjMOlF_xPdpwBRjYOkhK/pub?output=csv'

#Please donot change the url as the script is designed to Extract values with similar spreadsheets to that of Url only.

#Script will automatically calculate the no. of column on which SEEK1=Intern name, SEEK2=Average using bactracking the count of commas from starting to respective column and adding +1 to grab the exact row no.

IntNm="Intern Name"
Avg="Average"

#Script is using awk function to extract the desired output.

#NR>4 && NR<24  No. of records in awk function(to select row ranges has been selected between 4 to 24 using this statement)
Firstrow=4
Lastrow=24

#Note = This config file is used to source the input to script file. Please do not delete and changes may lead to script failure. please check the logs for success and failure details.
  </details>

<details>
<summary> Log file </summary>
  
  Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script  INITIALISING ./rishabh.sh
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [INITIAL]:Asking user to input directory name to store output datafile
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS]: Entered directory name is- data
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [CHECK]:checking if directory name- data already Exists or not
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS]: New Directory data has been created at /home/rishabh/Downloads/Script
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS]:Output Directory changed to /home/rishabh/Downloads/Script/data
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Spreadsheet1 downloaded from below url - https://docs.google.com/spreadsheets/d/e/2PACX-1vTrNldUZStbLCL-Q9Le9ilWrWxR1XW5N4zOzpBbM4aBEsgp2wheS7ioOx0yQ8a_zZuxvw4fXkwYH-Mh/pub?output=csv
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Spreadsheet2 downloaded from below url -- https://docs.google.com/spreadsheets/d/e/2PACX-1vRc3-RATBQ0U-XYPwb8uRHs0sMwJspqnspJxWFPXVz_pF0NA2QTFA-rkmPsRjMOlF_xPdpwBRjYOkhK/pub?output=csv
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Counted column no. of [_Intern Name_ in sheet1=]
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Counted column no. of [_Intern Name_ in sheet2=]
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Counted column no. of [_Average_ in sheet1=]
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Counted column no. of [_Average_ in sheet2=]
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS]: Extracted Name, Sum, Column from sheet1 and stored to /home/rishabh/Downloads/Script/data as (Output_of_Sheet)
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS]: Extracted Name, Sum, Column from sheet2 and stored to /home/rishabh/Downloads/Script/data as (Output_of_Sheet)
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [SUCCESS] Output generated to terminal
Sunday 16 May 2021 11:53:16 AM IST /home/rishabh/Downloads/Script ./rishabh.sh [EXITING ./rishabh.sh ]

  </details>

<details>

<summary> Download files link</summary>

URL111='https://docs.google.com/spreadsheets/d/e/2PACX-1vTrNldUZStbLCL-Q9Le9ilWrWxR1XW5N4zOzpBbM4aBEsgp2wheS7ioOx0yQ8a_zZuxvw4fXkwYH-Mh/pub?output=csv'  

URL222='https://docs.google.com/spreadsheets/d/e/2PACX-1vRc3-RATBQ0U-XYPwb8uRHs0sMwJspqnspJxWFPXVz_pF0NA2QTFA-rkmPsRjMOlF_xPdpwBRjYOkhK/pub?output=csv'

</details>

<details>

<summary> Conclusion</summary>
  I want to share that I have learned alots of new things while working in this script which is working successfully.
</details>
  
  ```
     Thank You
```
