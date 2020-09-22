<div align="center">

## Dynamic List Boxes


</div>

### Description

The code reads from two different tables whose data are related to each other and create two listboxes (using Javascript). When the user selects the first listbox, the second listbox will dynamically change to follow the related data in the first listbox.
 
### More Info
 
The code will create two example tables which consist of a list of countries in the first table and their cities in the second table. However, the user needs to supply the correct database parameters to enable the tables to be created.

Kindly read the comments..


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Asmadi Ahmad](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/asmadi-ahmad.md)
**Level**          |Intermediate
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Internet/ Browsers/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-browsers-html__8-9.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/asmadi-ahmad-dynamic-list-boxes__8-427/archive/master.zip)





### Source Code

```
<HTML>
<title>Dynamic List Boxes in PHP</title>
<BODY>
<?PHP
// NAME:
// DynLB.php
//
// VERSION:
// Version 1.0 - 2 Oct 2001
// initial release
//
// AUTHOR:
// Asmadi Ahmad (chloro@effitech.com)
// www.effitech.com
//
// DESCRIPTION:
// This script demonstrates the use of data from two related tables
// to create two dynamic listboxes via Javascript
// and shows example on how to retrieve the selected
// items from listboxes.
// The example tables will be automatically created for you, but
// you have to supply the correct mySQL database parameters.
//change the following database properties to suit your database
$db_Database = "mydbase";
$db_UserName = "me";
$db_Password = "password";
$db_Hostname = "localhost";
//connect to the database
mysql_connect($db_Hostname, $db_UserName, $db_Password) || UhOh("Can't Connect to Database: ".mysql_error());
mysql_select_db($db_Database);
$test="SELECT * FROM tblCountry";
if (mysql_query($test))
//if the tables already exist do nothing
{
}
else
//if the tables isn't there yet, create them and fill up the info
{
$query[]=" CREATE TABLE tblCountry (id int(10) DEFAULT '0' NOT NULL auto_increment, CountryName varchar(25), PRIMARY KEY(id), KEY id (id))";
$query[]=" CREATE TABLE tblCity (id int(10) DEFAULT '0' NOT NULL auto_increment, CityName varchar(25), Countryid int(10), PRIMARY KEY(id), KEY id (id))";
$query[] = "INSERT INTO tblCountry VALUES( 1, 'Malaysia')";
$query[] = "INSERT INTO tblCountry VALUES( 2, 'USA')";
$query[] = "INSERT INTO tblCountry VALUES( 3, 'UK')";
$query[] = "INSERT INTO tblCity VALUES( 1, 'Kuala Lumpur',1)";
$query[] = "INSERT INTO tblCity VALUES( 2, 'Penang',1)";
$query[] = "INSERT INTO tblCity VALUES( 3, 'Kulim',1)";
$query[] = "INSERT INTO tblCity VALUES( 4, 'New York',2)";
$query[] = "INSERT INTO tblCity VALUES( 5, 'Chicago',2)";
$query[] = "INSERT INTO tblCity VALUES( 6, 'London',3)";
$query[] = "INSERT INTO tblCity VALUES( 7, 'Liverpool',3)";
while ($each_query = each($query))
{
	$result = mysql_query($each_query[1]);
	if (!$result)
	{print("<b>WARNING! We've encountered an error. Please check manually. Error: ".mysql_error())."<p>";
	die();
	}
}
}
//declare the form
echo "<FORM name=f1 action='$PHP_SELF' method=post>";
//read the database
$result = mysql_query("SELECT tblCountry.CountryName,tblCity.Countryid,tblCity.CityName,tblCity.id FROM tblCity,tblCountry WHERE tblCity.Countryid=tblCountry.id");
//write the table
echo "<CENTER><BR><B>Dynamic List Boxes Demo (in PHP)</B><BR>";
echo "<TABLE font style='font-family: verdana; font-size: 12; font-weight:700' border=1>";
// write the country's listbox...
echo "<TR><TD valign=\"center\">Country</TD><TD><SELECT NAME=\"country\" SIZE=\"10\" ONCHANGE=\"countryselected(this);\" >\n";
// write the entry code for the javascript...
// \n is used to force a new line so the resultant code is more readable
$sJavaScript = "function countryselected(elem){\n for (var i = document.f1.city.options.length; i >= 0; i--){ \n document.f1.city.options[i] = null;\n";
// loop through the database..
$sLastCountry="";
while ( $row = mysql_fetch_array($result) )
 {
  // is this a new country?
  If ($sLastCountry!=$row["CountryName"]){
   // if yes, add the entry to the country's listbox
   $sLastCountry = $row["CountryName"];
   echo "\n<OPTION VALUE='".$row["Countryid"]."'>".$sLastCountry."</OPTION>";
  // and add a new section to the javascript...
   $sJavaScript = $sJavaScript."}\n"."if (elem.options[elem.selectedIndex].value==".$row["Countryid"]."){\n";
   }
  // and add a new city line to the javascript
  $sJavaScript = $sJavaScript."document.f1.city.options[document.f1.city.options.length] = new Option('".$row["CityName"]."','".$row["id"]."');\n";
 }
 // finish the country's listbox
 echo "</SELECT></TD>";
 // create the city listbox for no selection
 echo "\n<TD valign=\"center\">City</TD><TD><SELECT NAME=\"city\" SIZE=10>";
 echo "<OPTION>[no city selected]</OPTION>";
 echo "</SELECT></TD></TR>";
 echo "<TR><TD><font style='font-size=10'></TD><TD></TD><TD></TD><TD><INPUT TYPE=SUBMIT NAME='submitcity' VALUE='SUBMIT'></TD></TR>";
echo "</TABLE>";
 // finish the javascript and write out
 $sJavaScript = $sJavaScript."\n}\n}\n";
 echo "\n<SCRIPT LANGUAGE=\"JavaScript\">";
 echo "\n".$sJavaScript."\n</SCRIPT>\n";
//close the form
echo "</FORM></center>";
//code to test the submit button
//normally people would save the index in another table
//this example only display the indexes
if ("SUBMIT" == $submitcity)
{
echo "<center>Your Selected Country index= ".$country."<BR>";
echo "Your Selected City index= ".$city."<BR></center>";
}
 ?>
  </body>
  </html>
```

