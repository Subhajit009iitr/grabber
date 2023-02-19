README
------------------------------------------------------------------------------------------
CONTRIBUTION BY SUBHAJIT BISWAS

Updated support for python3 which was originally only supported in python
------------------------------------------------------------------------------------------
INTRODUCTION
------------------------------------------------------------------------------------------
Grabber is a black box web application vulnerability scanner that looks for SQL Injection,
Blind SQL injection, XSS vulnerability and File include injection. 

The tool aims to be quite generic, and can work with any kind of web application
regardless of the server side programming language. The tool is designed to be 
a simple, efficient way to detect vulnerabilities in a small simple web applications.

Grabber is extended from an existing open source tool(https://github.com/neuroo/grabber).
The tool uses a modular and extensible design having individual module for each kind of 
vulnerability detection. You can also extend the attack list by adding them to the XML files
in specified format.

Grabber is written entirely in Python and requires additional python modules as dependencies
such as BeautifulSoup and PyXML for web scrapping and parsing XML.

---------------------------------------------------------------------------------------------
HOW IT WORKS
---------------------------------------------------------------------------------------------
Grabber is a simple command line tool that accepts several command line arguments as follows:
--url  <url>			- URL of the web base application
--spider <index>		- Depth of scanning performed by spider
--cookie <CookieString> - Cookie String to be passed along with request
--sql   				- look for SQL injection
--bsql					- look for blind SQL injection
--xss 					- look for Cross site scripting
--include 				- look for file inclusion attacks

Grabber first runs the spider to scan the given web application for all the pages. It parses 
the HTML content using Beautiful soup to identify links in href, form actions, javascript etc.
and builds an XML index file inside local/spidersite.xml.

Once it has scanned for all the links it makes a GET call to every link in the index to identify
all the variable parameters in GET queries, HTML form fields and generates a database of all the
the variable parameters found against the specific URL along with request methods available.

Based on the given attack parameters passed, Grabber then runs the investigation for each of the 
attacks by manipulating the parameter values using attack instances mentioned in the XML files.
It then performs GET or POST request on the URLs in the database. The returned HTML output from 
the server is scanned for Keywords that indicate injection was a successful or failure.

Grabber generates a live and interactive HTML report that displays the current status of the tool,
all the scanned URLs and vulnerabilities detected in an intuitive manner.

------------------------------------------------------------------------------------------------
ENHANCEMENTS / IMPROVEMENTS TO ORIGINAL TOOL (BY LAST TEAM)
------------------------------------------------------------------------------------------------
We have made following improvememnts in the original tool:

1. Added a Cookie support that allows passing Cookies that are sent with the requests to the web application.
This allows scanning for the pages which needs HTTP authentication thus improving the 
coverage of the tool.
For this we have introduced a new parameter --cookie that takes a string value and is sent with 
every request to the application. We had to make changes to all the request headers in multiple files such
as grabber.py, sql.py, xss.py, bsql.py and include.py.

2. Improvements and bug fixes to spider and web scraping: 
Spider.py was improved to generate more well formed urls, detect and form a valid absolute URLs from
relative paths. Improved spider can also handle recursive loops if same URL is present on multiple 
pages to prevent infinite loops. Original tool had many bugs that did not detect several kinds of HTML
input form elements such as text areas and select dropdowns, we added improvements to scan and correctly 
identify the form parameters.

3. Report Generation: A new module Report.py was added that generated a live and interactive report using 
HTML, CSS and Bootstrap. The report provides information about current status of the tool, URLs being 
processed, kind of vulnerability scanned. Once a vulnerability is detected, details about the vulnerability
are displayed including URL, request method, parameter used for injection, and value passed to perform the 
attack. Each module interacts with report generation by calling append_to_report method in report.py

4. Added more attacks to detect SQL injection vulnerability in sqlAttacks.xml
------------------------------------------------------------------------------------------------
HOW TO RUN
------------------------------------------------------------------------------------------------
Grabber requires Python installed along with the necessary dependencies.
You can start grabber by running following command inside the grabber folder:

python grabber.py --url <APPLICATION_URL> 
						[ --spider <index> 
						  --cookie <CookieString>
						  --sql 
						  --xss 
						  --bsql 
						  --include
						]
Only url is the required parameter, by default the tool searches for XSS vulnerability 
if no other parameeters are passed.

The code repository is available at (https://github.com/avaneeshd/grabber)

Original Author:
  author:  Romain Gaucher
  website: http://rgaucher.info/beta/grabber
  email:   r@rgaucher.info