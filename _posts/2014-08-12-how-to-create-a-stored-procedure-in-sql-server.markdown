---
layout: post
title: How to Create a Stored Procedure in SQL Server
date: 2014-08-12 00:11:00
categories: SQL
---

## Declaring and Setting variables ##
Example: Declare variable newID, set it to the top ID from table "tbl"

{% highlight sql %}
DECLARE @newID as int
SELECT TOP 1 @newID = id FROM tbl
{% endhighlight %}

## Create a comma delimited list with SELECT statement ##
Example: Create a list of users separated by a "," from the column username, table tblUsers
```
{% highlight sql %}
DECLARE @strList nvarchar(max)
SELECT @strList = COALESCE(@strList+',', '') + username FROM tblUsers
{% endhighlight %}

## While loop ##
{% highlight sql %}
WHILE Boolean_expression
BEGIN

END
{% endhighlight %}

## Getting Rows in a Random Order ##
{% highlight sql %}
SELECT * FROM table ORDER BY newid()
{% endhighlight %}

## Example of a Unique Case of ORDER BY ##
Select random rows from a table, but start with the id @ID
{% highlight sql %}
SELECT * FROM table ORDER BY CASE ID WHEN @ID THEN newid() END DESC
{% endhighlight %}

## Example of getting info from multiple databases ##
Get the story from database MSC_Admissions and the program name from MSC_POS
{% highlight sql %}
SELECT TOP 1 tblStory.*, tblPrograms.ProgramName FROM tblStory
    INNER JOIN MSC_POS.dbo.tblPrograms tblPrograms ON tblStory.ProgramID = tblPrograms.ProgramID
{% endhighlight %}

## Execute Stored Procedure in Stored Procedure ##
{% highlight sql %}
EXEC test2 @newId, @prod, @desc;
{% endhighlight %}

[SQL Server Functions](http://technet.microsoft.com/en-us/library/ms174318.aspx)

## Full Example
{% highlight sql %}
-- ================================================
-- Template generated from Template Explorer using:
-- Create Procedure (New Menu).SQL
--
-- Use the Specify Values for Template Parameters 
-- command (Ctrl-Shift-M) to fill in the parameter 
-- values below.
--
-- This block of comments will not be included in
-- the definition of the procedure.
-- ================================================
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:      Hank Hill
-- Create date: 10/11/2012
-- Description: Gets web requests based on ID or PeopleOrdersID
-- =============================================
CREATE PROCEDURE SelectWebRequests
    -- Add the parameters for the stored procedure here
    @ID as Integer = 0,
    @PeopleOrdersID as Integer = 0
AS
BEGIN
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;

    -- Insert statements for procedure here
    IF @ID <> 0
        SELECT ID, JobTitle, JobType, JobDate, DateNeeded, Notes, PeopleOrdersID
        FROM tblWebRequests 
        WHERE ID = @ID
    ELSE IF @PeopleOrdersID <> 0
        SELECT ID, JobTitle, JobType, JobDate, DateNeeded, Notes, PeopleOrdersID
        FROM tblWebRequests 
        WHERE PeopleOrdersID = @PeopleOrdersID
    ELSE
        SELECT ID, JobTitle, JobType, JobDate, DateNeeded, Notes, PeopleOrdersID
        FROM tblWebRequests 
END
GO
{% endhighlight %}
