# Next system preferences
Next system preferences

This is a repostitory for all of the system preferences used by the Next library consortium.

-----

Instructions:

Run report 3080 and save as a spreadsheet.

The SQL for this report is:

----------

```SQL
SELECT
  Concat(If(Length(REPLACE(systempreferences.value,'\r\n', CONCAT(Char(13),Char(10)))) > 32766, "X.", "R."), REPLACE(systempreferences.variable, ":", ".")) AS FILE_NAME,
  Concat(
    Concat("R.", systempreferences.variable), Char(13), Char(10), Char(13), Char(10),
    Concat("----------"), Char(13), Char(10), Char(13), Char(10),
    Concat("Name: ", systempreferences.variable), Char(13), Char(10), Char(13), Char(10),
    Concat("----------"), Char(13), Char(10), Char(13), Char(10),
    Concat("Options: ", Coalesce(systempreferences.options, " ")), Char(13), Char(10), Char(13), Char(10),
    Concat("----------"), Char(13), Char(10), Char(13), Char(10),
    Concat("Description: ", Coalesce(systempreferences.explanation, " ")), Char(13), Char(10), Char(13), Char(10),
    Concat("----------"), Char(13), Char(10), Char(13), Char(10),
    Concat("Type: ", Coalesce(systempreferences.type, " ")), Char(13), Char(10), Char(13), Char(10),
    Concat("----------"), Char(13), Char(10), Char(13), Char(10),
    Concat( IF(Length(REPLACE(systempreferences.value,'\r\n', CONCAT(Char(13), CHAR(10)))) > 32766, "Too large to process", REPLACE(systempreferences.value, '\r\n', Concat(Char(13), Char(10))) ) )
  ) AS CONTENT  
FROM
  systempreferences
```

----------

Make sure C:\\GIT\\ is empty.

Open the csv file and run the macro from the XLSX macro file.

The VBA for the macro is:

----------

```VBA
Sub WriteTotxtSQL()

Const forReading = 1, forAppending = 3, fsoForWriting = 2
Dim fs, objTextStream, sText As String
Dim lLastRow As Long, lRowLoop As Long, lLastCol As Long, lColLoop As Long

lLastRow = Cells(Rows.Count, 1).End(xlUp).Row

For lRowLoop = 1 To lLastRow

    Set fs = CreateObject("Scripting.FileSystemObject")
    Set objTextStream = fs.opentextfile("c:\GIT\" & Cells(lRowLoop, 1) & ".txt", fsoForWriting, True)

    sText = ""

    For lColLoop = 2 To 2
        sText = sText & Cells(lRowLoop, lColLoop) & Chr(10) & Chr(10)
    Next lColLoop

    objTextStream.writeline (Left(sText, Len(sText) - 1))


    objTextStream.Close
    Set objTextStream = Nothing
    Set fs = Nothing

Next lRowLoop

End Sub
```

----------

This should give you 1 text file for each row in the report.  Each text file represents 1 system preference from Koha.

Save all of these files into the appropriate github folder.

----------

Once all files are saved in the github folder, use Git or Atom to sync with the online repository.
