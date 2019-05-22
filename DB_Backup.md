## MSSQL Express 백업스크립트

### backup.sql
declare @dir nvarchar(100)
set @dir = N'D:\backup\[데이터베이스명]-' + convert(nvarchar(20), getDate(), 112) +N'.bak'
BACKUP DATABASE [데이터베이스명] TO DISK = @dir WITH NOFORMAT, NOINIT, NAME = N'welfare7-Full', SKIP, NOREWIND, NOUNLOAD, STATS = 10
GO

### backup.bat
@echo off

rem 지난 7일자 날짜
echo wscript.echo ^(Date^(^)- 7^)>delday.vbs
for /f %%a in ('cscript //nologo delday.vbs') do set delday=%%a
del delday.vbs
echo delday was %delday%
set delyear=%delday:~-10,4%
set delmons=%delday:~-5,2%
set delday=%delday:~-2,2%

del D:\BACKUP\[데이터베이스명]-%delyear%%delmons%%delday%.bak

sqlcmd -S localhost -E -i "C:\backup\backup.sql"

backup.bat 를 스케쥴러에 등록.
