# 365DataScience_Introduction_to_DAX

<img width="488" alt="image" src="https://github.com/SyakeerRahman/365DataScience_Introduction_to_DAX/assets/105381652/95558af5-f312-4d98-ba8c-d43bd6067bff">

## 1. Introduction_to_DAX

• Course intro
• Calculated Columns

``
Late Shipment = 
IF(factInternetSales[ShipDate] > factInternetSales[DueDate], "Late Shipment", "On Time")
``

• Calendar Table

``
Calendar Table = 
ADDCOLUMNS(
    CALENDARAUTO(), 
        "Year", YEAR([Date]),
        "Month", MONTH([Date]),
        "Month Name", FORMAT([Date], "MMMM"),
        "Month Year", FORMAT([Date], "MM/YYYY"))
``

• Building a Measure

``
Number of Sales = 
COUNT(factInternetSales[SalesOrderNumber])
``
• Measures Table

<img width="911" alt="image" src="https://github.com/SyakeerRahman/365DataScience_Introduction_to_DAX/assets/105381652/31647b4e-61bb-465c-8174-66693a452eca">

• CALCULATE


``
Total Sales Amount = SUM(factInternetSales[Sales Amount EUR])
``

``
Total Sales Amount CALCULATE = CALCULATE([Total Sales Amount], dimProduct[Color] = "Blue")
``

• FILTER vs KEEPFILTERS


• Iterators
• RELATED vs RELATEDTABLE
• SELECTEDVALUE
• DIVIDE
• Logical Operators
• Variables
• TREATAS
• SWITCH
• Text Functions
• CONCATENATEX
• Time Intelligence
• Expression Based Titles
• Role Level Security
• Calculation Groups
