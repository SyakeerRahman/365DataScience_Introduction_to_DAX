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
``
Total Sales Amount FILTER = CALCULATE (
    [Total Sales Amount],
    FILTER ( dimProduct, dimProduct[Color] = "Blue" )
)
``

``
Total Sales Amount KEEPFILTERS = CALCULATE (
    [Total Sales Amount],
    KEEPFILTERS ( dimProduct[Color] = "Blue" )
)
``

• Iterators

``
Iterator SUMX = SUMX (
    FILTER (
        factInternetSales,
        RELATED ( dimCustomer[Age Buckets] ) IN VALUES ( dimCustomer[Age Buckets] )
    ),
    factInternetSales[Sales Amount EUR]
)
``

• RELATED vs RELATEDTABLE

<img width="610" alt="image" src="https://github.com/SyakeerRahman/365DataScience_Introduction_to_DAX/assets/105381652/b4efd3c5-106d-4ab0-97f0-11b96ac7084c">

``
Number of Sales = COUNTX(RELATEDTABLE(factInternetSales), factInternetSales[SalesOrderNumber])
``

• SELECTEDVALUE

<img width="608" alt="image" src="https://github.com/SyakeerRahman/365DataScience_Introduction_to_DAX/assets/105381652/7eb61676-eb5a-4ee5-a0d5-02f16153c6d4">

• DIVIDE

``
% Sales Amount Selected Age Bucket = 
DIVIDE (
    [Iterator SUMX],
    CALCULATE ( [Total Sales Amount], ALL ( dimCustomer[Age Buckets] ) )
)
``
• Logical Operators

``
Currency Table Filtered = IF (
    ISFILTERED ( dimCurrency ),
    SELECTEDVALUE ( dimCurrency[CurrencyName] ),
    "No Currency Selected"
)
``


• Variables

``
% Sales YoY Growth = 
-- First we calculate the Sales Amount for the current/latest year
VAR salesAmtCurrentYear =
    (
        [Total Sales Amount]
            - CALCULATE (
                [Total Sales Amount],
                PARALLELPERIOD ( 'Calendar Table'[Date], -12, MONTH )
            )
    ) -- Then we calculate the Total Sales Amount for all time until beginning of latest year
VAR salesAmtAllTimeMinusCurrentYear =
    CALCULATE (
        [Total Sales Amount],
        PARALLELPERIOD ( 'Calendar Table'[Date], -12, MONTH )
    )
RETURN
    DIVIDE ( salesAmtCurrentYear, salesAmtAllTimeMinusCurrentYear )
``

• TREATAS

``
Currency Conversion = 
-- getting the total sales amount in base currency EUR
VAR salesEUR =
    SUM ( factInternetSales[Sales Amount EUR] )
RETURN
    IF (
        ISFILTERED ( dimCurrency[CurrencyAlternateKey] ),
        salesEUR
            * CALCULATE (
                MAX ( dimCurrency[Exchange Rate] ),
                TREATAS (
                    VALUES ( dimCurrency[CurrencyAlternateKey] ),
                    factInternetSales[Currency Code]
                )
            ),
        salesEUR
    )
``
• SWITCH

``
Switched = 
SWITCH(
    SELECTEDVALUE('Data Type'[Type]),
    "Percentage", [% Currency Conversion], "Number", [Currency Conversion])
``

• Text Functions

``
% Indicator YoY Sales Growth = 
SWITCH (
    TRUE (),
    [% Sales YoY Growth] < 0.1, UNICHAR ( 9660 ),
    [% Sales YoY Growth] > 0.1, UNICHAR ( 9650 ),
    BLANK ()
)
    & FORMAT ( [% Sales YoY Growth], "percent" )
``

• CONCATENATEX

``
Order Numbers = CONCATENATEX(
    RELATEDTABLE(factInternetSales), 
    factInternetSales[SalesOrderNumber], ", ", [Sales Amount EUR], ASC
    )
``

• Time Intelligence

``
Accumulative Sales = 
CALCULATE ( [Currency Conversion], DATESYTD ( 'Calendar Table'[Date] ) )
``

• Expression Based Titles

``
LY vs TY Difference = 
VAR ty =
    CALCULATE (
        [Currency Conversion],
        'Calendar Table'[Year] = SELECTEDVALUE ( 'Calendar Table'[Year] )
    )
RETURN
    ty - '#Measures'[Selected Year Minus One]
``

• Role Level Security

Tabular Editor 2.x is a free, open-source, tool that lets you easily manipulate and manage measures, calculated columns, display folders, perspectives and translations in Analysis Services Tabular and Power BI XMLA Models (from Compatibility Level 1200 and onwards). The tool is written entirely in .NET WinForms (C#).

https://github.com/TabularEditor/TabularEditor/tree/master

![image](https://github.com/SyakeerRahman/365DataScience_Introduction_to_DAX/assets/105381652/17e812c4-a11e-47c9-89e5-8fcd69e0716c)


• Calculation Groups

![Uploading image.png…]()

