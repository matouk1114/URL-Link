# URL-Link
Am I creating this correctly? Please commit any changes that need to be made
<%
Function InsertHyperlinks(inText)
Dim objRegExp, strBuf
Dim objMatches, objMatch
Dim Value, ReplaceValue, iStart, iEnd

strBuf = ""
iStart = 1
iEnd = 1
Set objRegExp = New RegExp

objRegExp.Pattern = "(www|http|S+@)S+" ' Match URLs and emails
objRegExp.IgnoreCase = True ' Set case insensitivity.
objRegExp.Global = True ' Set global applicability.
Set objMatches = objRegExp.Execute(inText)
For Each objMatch In objMatches
iEnd = objMatch.FirstIndex
strBuf = strBuf & Mid(inText, iStart, iEnd-iStart+1)
If InStr(1, objMatch.Value, "@") Then
strBuf = strBuf & GetHref(objMatch.Value, "EMAIL", "_BLANK")
Else
strBuf = strBuf & GetHref(objMatch.Value, "WEB", "_BLANK")
End If
iStart = iEnd+objMatch.Length+1
Next
strBuf = strBuf & Mid(inText, iStart)
InsertHyperlinks = strBuf
End Function


Function GetHref(url, urlType, Target)
Dim strBuf

strBuf = "<a href="""
If UCase(urlType) = "WEB" Then
If LCase(Left(url, 3)) = "www" Then
strBuf = "<a href=""http://" & url & """ Target=""" & _
Target & """>" & url & "</a>"
Else
strBuf = "<a href=""" & url & """ Target=""" & _
Target & """>" & url & "</a>"
End If
ElseIf UCase(urlType) = "EMAIL" Then
strBuf = "<a href=""mailto:" & url & """ Target=""" & _
Target & """>" & url & "</a>"
End If

GetHref = strBuf

End Function
%>
