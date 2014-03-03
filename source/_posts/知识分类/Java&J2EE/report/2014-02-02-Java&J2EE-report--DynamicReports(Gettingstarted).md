---
layout: post
title: "DynamicReports(Getting started)"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - report
--- 

# DynamicReports(Getting started)

## Getting started

**Table of contents**

1. [Overview](http://www.dynamicreports.org/getting-started#overview)
1. [Simple report](http://www.dynamicreports.org/getting-started#simplereport)

* [Step 1 : Start](http://www.dynamicreports.org/getting-started#step1)
* [Step 2 : Styles](http://www.dynamicreports.org/getting-started#step2)
* [Step 3 : Additional columns](http://www.dynamicreports.org/getting-started#step3)
* [Step 4 : Group](http://www.dynamicreports.org/getting-started#step4)
* [Step 5 : Subtotals](http://www.dynamicreports.org/getting-started#step5)
* [Step 6 : Charts](http://www.dynamicreports.org/getting-started#step6)
* [Step 7 : Column grid & Containers](http://www.dynamicreports.org/getting-started#step7)
* [Step 8 : Hide subtotal](http://www.dynamicreports.org/getting-started#step8)
* [Step 9 : Title](http://www.dynamicreports.org/getting-started#step9)
* [Step 10 : Currency data type](http://www.dynamicreports.org/getting-started#step10)
* [Step 11 : Detail row highlighters](http://www.dynamicreports.org/getting-started#step11)
* [Step 12 : Conditional styles](http://www.dynamicreports.org/getting-started#step12)

[]()

### Overview

Creating a report with **DynamicReports** is very easy, take a look at the example below
01
 
import

static

net.sf.dynamicreports.report.builder.DynamicReports.*;

02
 
  

public

class

Report {
03
 
  
 

04
 
    

private

void

build() {
05
 
      

try

{

06
 
        

report()

//create new report design
07
 
          

.columns(...)

//adds columns

08
 
          

.groupBy(...)

//adds groups
09
 
          

.subtotalsAtSummary(...)

//adds subtotals

10
 
          

...
11
 
          

//set datasource

12
 
          

.setDataSource(...)
13
 
          

//export report

14
 
          

.toPdf(...)

//export report to pdf
15
 
          

.toXls(...)

//export report to excel

16
 
          

...
17
 
          

//other outputs

18
 
          

.toJasperPrint()

//creates jasperprint object
19
 
          

.show()

//shows report

20
 
          

.print()

//prints report
21
 
          

...

22
 
      

}

catch

(DRException e) {
23
 
        

e.printStackTrace(); 

24
 
      

}
25
 
    

} 

26
 
    

... 
27
 
  

}

See [ReportBuilder](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/ReportBuilder.html) class for all available features and [JasperReportBuilder](http://apidocs.dynamicreports.org/net/sf/dynamicreports/jasper/builder/JasperReportBuilder.html) class for all available exports and outputs

 

[]()

### Simple report

Please note that this is a tutorial example and that this tutorial doesn't describe all features!

You will learn in this section how to create a simple report step by step.
First we need to create an empty report desing, configure it and set a datasource. Afterwards the report will be ready to export into any format.
It's important to remember the [DynamicReports](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/DynamicReports.html) class, through which you will have available most of features. The class provides methods for building a particular piece of a report.
Let's start!

[]()

### Step 1 : Start

First define columns through [DynamicReports.col](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/column/ColumnBuilders.html).column(title, field name, datatype) (the field name must match with the name of a field contained in the datasource) and pass them into the report as follows:
1
 
.columns(

//add columns

2
 
  

//             title,     field name     data type
3
 
  

col.column(

"Item"

,      

"item"

,      type.stringType()),

4
 
  

col.column(

"Quantity"

,  

"quantity"

,  type.integerType()),
5
 
  

col.column(

"Unit price"

,

"unitprice"

, type.bigDecimalType()))

now define some text at the title and number of pages at the page footer as follows:

1
 
.title(cmp.text(

"Getting started"

))

//shows report title

2
 
.pageFooter(cmp.pageXofY())

//shows number of page at page footer

[DynamicReports.cmp](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/component/ComponentBuilders.html).text(some text) - creates a component that shows a text
[DynamicReports.cmp](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/component/ComponentBuilders.html).pageXofY() - creates a component that shows page X of Y
Methods **title(...)** and **pageFooter(...)** allows to customize a particular report band by adding components to it

 
[![SimpleReport_Step01]()](http://www.dynamicreports.org/examples/simplereport_step01.png "SimpleReport_Step01") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step01.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step01.png) [SimpleReport_Step01](http://www.dynamicreports.org/examples/simplereport_step01 "SimpleReport_Step01")

[]()

### Step 2 : Styles

Each style can have a parent style from which it will inherit its properties. Empty style can be created by [DynamicReports.stl](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/style/StyleBuilders.html).style()
01
 
StyleBuilder boldStyle         = stl.style().bold(); 

02
 
StyleBuilder boldCenteredStyle = stl.style(boldStyle)
03
 
                                    

.setHorizontalAlignment(HorizontalAlignment.CENTER);

04
 
StyleBuilder columnTitleStyle  = stl.style(boldCenteredStyle)
05
 
                                    

.setBorder(stl.pen1Point())

06
 
                                    

.setBackgroundColor(Color.LIGHT_GRAY);
07
 
report()

08
 
  

.setColumnTitleStyle(columnTitleStyle) 
09
 
  

.highlightDetailEvenRows() 

10
 
  

.title(cmp.text(

"Getting started"

).setStyle(boldCenteredStyle))
11
 
  

.pageFooter(cmp.pageXofY().setStyle(boldCenteredStyle)) [![SimpleReport_Step02]()](http://www.dynamicreports.org/examples/simplereport_step02.png "SimpleReport_Step02") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step02.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step02.png) [SimpleReport_Step02](http://www.dynamicreports.org/examples/simplereport_step02 "SimpleReport_Step02")

[]()

### Step 3 : Additional columns

You can very easy multiply, divide, add or subtract column of numbers by another column of numbers or by a number value
1
 
//price = unitPrice * quantity 

2
 
TextColumnBuilder<BigDecimal> priceColumn = unitPriceColumn.multiply(quantityColumn)
3
 
                                                           

.setTitle(

"Price"

);

Adding percentage of any column of numbers is simple [DynamicReports.col](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/column/ColumnBuilders.html).percentageColumn(title, column)

1
 
PercentageColumnBuilder pricePercColumn = col.percentageColumn(

"Price %"

, priceColumn);

[DynamicReports.col](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/column/ColumnBuilders.html).reportRowNumberColumn(title) creates a column that shows row number

1
 
TextColumnBuilder<Integer> rowNumberColumn =

2
 
    

col.reportRowNumberColumn(

"No."

) 
3
 
    

//sets the fixed width of a column, width = 2 * character width 

4
 
    

.setFixedColumns(

2

) 
5
 
    

.setHorizontalAlignment(HorizontalAlignment.CENTER);  [![SimpleReport_Step03]()](http://www.dynamicreports.org/examples/simplereport_step03.png "SimpleReport_Step03") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step03.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step03.png) [SimpleReport_Step03](http://www.dynamicreports.org/examples/simplereport_step03 "SimpleReport_Step03")

[]()

### Step 4 : Group

We continue by adding a group as shown below
1
 
.groupBy(itemColumn) [![SimpleReport_Step04]()](http://www.dynamicreports.org/examples/simplereport_step04.png "SimpleReport_Step04") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step04.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step04.png) [SimpleReport_Step04](http://www.dynamicreports.org/examples/simplereport_step04 "SimpleReport_Step04")

[]()

### Step 5 : Subtotals

Now we can try to sum a column of numbers. Subtotal of sum is created through [DynamicReports.sbt](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/subtotal/SubtotalBuilders.html).sum(column)
1
 
.subtotalsAtSummary( 

2
 
      

sbt.sum(unitPriceColumn), sbt.sum(priceColumn)) 
3
 
.subtotalsAtFirstGroupFooter( 

4
 
      

sbt.sum(unitPriceColumn), sbt.sum(priceColumn)) 

Method **subtotalsAtSummary(...)** allows to add subtotals to the summary band
Method **subtotalsAtFirstGroupFooter(...)** will find first defined group and add subtotals to the group footer band

[![SimpleReport_Step05]()](http://www.dynamicreports.org/examples/simplereport_step05.png "SimpleReport_Step05") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step05.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step05.png) [SimpleReport_Step05](http://www.dynamicreports.org/examples/simplereport_step05 "SimpleReport_Step05")

[]()

### Step 6 : Charts

[DynamicReports.cht](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/chart/ChartBuilders.html) provide methods for building charts. Category and series are required.
01
 
Bar3DChartBuilder itemChart = cht.bar3DChart()

02
 
                                 

.setTitle(

"<a href="

http:

//www.dynamicreports.org/examples/sales" title="Sales">Sales</a> by item")
03
 
                                 

.setCategory(itemColumn)

04
 
                                 

.addSerie(
05
 
                                    

cht.serie(unitPriceColumn), cht.serie(priceColumn));

06
 
Bar3DChartBuilder itemChart2 = cht.bar3DChart()
07
 
                                 

.setTitle(

"<a href="

http:

//www.dynamicreports.org/examples/sales" title="Sales">Sales</a> by item")

08
 
                                 

.setCategory(itemColumn)
09
 
                                 

.setUseSeriesAsCategory(

true

)

10
 
                                 

.addSerie(
11
 
                                   

cht.serie(unitPriceColumn), cht.serie(priceColumn));

Chart is a component and can be placed into any report band.

1
 
.summary(itemChart, itemChart2) [![SimpleReport_Step06]()](http://www.dynamicreports.org/examples/simplereport_step06.png "SimpleReport_Step06") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step06.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step06.png) [SimpleReport_Step06](http://www.dynamicreports.org/examples/simplereport_step06 "SimpleReport_Step06")

[]()

### Step 7 : Column grid & Containers

Components inserted into the bands are arranged vertically, each component is below the previously added component. To arrange components horizontally it is needed to wrap these components with a horizontal container. Container is a component as well and therefore it can be added to any of the report bands.
1
 
.summary( 

2
 
    

cmp.horizontalList(itemChart, itemChart2))

Columns layout can be modified by a column grid. The layout is applied to the columns title, details and subtotals.

1
 
.columnGrid( 

2
 
    

rowNumberColumn, quantityColumn, unitPriceColumn,
3
 
    

grid.verticalColumnGridList(priceColumn, pricePercColumn)) [![SimpleReport_Step07]()](http://www.dynamicreports.org/examples/simplereport_step07.png "SimpleReport_Step07") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step07.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step07.png) [SimpleReport_Step07](http://www.dynamicreports.org/examples/simplereport_step07 "SimpleReport_Step07")

[]()

### Step 8 : Hide subtotal

Subtotal for the group notebook is unnecessary because contains only one row.
We need to change the group declaration and set the boolean expression condition on which depends whether subtotal is printed.
[DynamicReports.exp](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/expression/ExpressionBuilders.html).printWhenGroupHasMoreThanOneRow(itemGroup) creates a boolean condition which returns true when itemGroup has more than one row.
1
 
ColumnGroupBuilder itemGroup = grp.group(itemColumn); 

2
 
itemGroup.setPrintSubtotalsWhenExpression(
3
 
            

exp.printWhenGroupHasMoreThanOneRow(itemGroup));

1
 
.groupBy(itemGroup)
[![SimpleReport_Step08]()](http://www.dynamicreports.org/examples/simplereport_step08.png "SimpleReport_Step08") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step08.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step08.png) [SimpleReport_Step08](http://www.dynamicreports.org/examples/simplereport_step08 "SimpleReport_Step08")

[]()

### Step 9 : Title

First define a title style.
1
 
StyleBuilder titleStyle = stl.style(boldCenteredStyle)

2
 
                             

.setVerticalAlignment(VerticalAlignment.MIDDLE)
3
 
                             

.setFontSize(

15

);

[DynamicReports.cmp](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/component/ComponentBuilders.html).image() creates an image component
[DynamicReports.cmp](http://apidocs.dynamicreports.org/net/sf/dynamicreports/report/builder/component/ComponentBuilders.html).filler() creates an empty component

1
 
.title(

//shows report title

2
 
     

cmp.horizontalList()
3
 
  

.add(

4
 
    

cmp.image(getClass().getResourceAsStream(

"../images/dynamicreports.png"

)).setFixedDimension(

80

,

80

),
5
 
    

cmp.text(

"DynamicReports"

).setStyle(titleStyle).setHorizontalAlignment(HorizontalAlignment.LEFT),

6
 
    

cmp.text(

"Getting started"

).setStyle(titleStyle).setHorizontalAlignment(HorizontalAlignment.RIGHT))
7
 
  

.newRow()

8
 
  

.add(cmp.filler().setStyle(stl.style().setTopBorder(stl.pen2Point())).setFixedHeight(

10

)))

The defined filler creates an additional blank space between the title and the column header.
Setting top border of a filler draws the line at the bottom of the title.
Horizontal list, as previously mentioned, arranges components horizontally in one row but by calling the method row() a new horizontal list is created which is located at the bottom of the previous horizontal list.

[![SimpleReport_Step09]()](http://www.dynamicreports.org/examples/simplereport_step09.png "SimpleReport_Step09") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step09.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step09.png) [SimpleReport_Step09](http://www.dynamicreports.org/examples/simplereport_step09 "SimpleReport_Step09")

[]()

### Step 10 : Currency data type

Unit price and price column are currency types.
Showing currency is possible by setting pattern to both columns (via method setPattern()), but the best practice is to create a custom data type and apply it to the columns. The custom data type then can be used in other reports.
01
 
CurrencyType currencyType =

new

CurrencyType();

02
 
TextColumnBuilder<BigDecimal> unitPriceColumn = col.column(

"Unit price"

,

"unitprice"

, currencyType);
03
 
//price = unitPrice * quantity

04
 
TextColumnBuilder<BigDecimal> priceColumn     = unitPriceColumn.multiply(quantityColumn).setTitle(

"Price"

)
05
 
                                                               

.setDataType(currencyType);

06
 
private

class

CurrencyType

extends

BigDecimalType {
07
 
  

private

static

final

long

serialVersionUID = 1L;

08
 
            
 
09
 
  

@Override

10
 
  

public

String getPattern() {
11
 
    

return

"$ #,###.00"

;

12
 
  

}
13
 
} [![SimpleReport_Step10]()](http://www.dynamicreports.org/examples/simplereport_step10.png "SimpleReport_Step10") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step10.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step10.png) [SimpleReport_Step10](http://www.dynamicreports.org/examples/simplereport_step10 "SimpleReport_Step10")

[]()

### Step 11 : Detail row highlighters

1
 
ConditionalStyleBuilder condition1 = stl.conditionalStyle(cnd.greater(priceColumn,

150

))

2
 
                                        

.setBackgroundColor(

new

Color(

210

,

255

,

210

));
3
 
ConditionalStyleBuilder condition2 = stl.conditionalStyle(cnd.smaller(priceColumn,

30

))

4
 
                                        

.setBackgroundColor(

new

Color(

255

,

210

,

210

));

1
 
.detailRowHighlighters(

2
 
  

condition1, condition2)

Condition1 is applied only if price is greater than 150 and sets background color of a row to green.
Condition2 is applied only if price is smaller than 30 and sets background color of a row to red.
[![SimpleReport_Step11]()](http://www.dynamicreports.org/examples/simplereport_step11.png "SimpleReport_Step11") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step11.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step11.png) [SimpleReport_Step11](http://www.dynamicreports.org/examples/simplereport_step11 "SimpleReport_Step11")

[]()

### Step 12 : Conditional styles

1
 
ConditionalStyleBuilder condition3 = stl.conditionalStyle(cnd.greater(priceColumn,

200

))

2
 
                                        

.setBackgroundColor(

new

Color(

0

,

190

,

0

))
3
 
                                        

.bold();

4
 
ConditionalStyleBuilder condition4 = stl.conditionalStyle(cnd.smaller(priceColumn,

20

))
5
 
                                        

.setBackgroundColor(

new

Color(

190

,

0

,

0

))

6
 
                                        

.bold();
7
 
StyleBuilder priceStyle = stl.style()

8
 
                             

.conditionalStyles(
9
 
                               

condition3, condition4);

1
 
priceColumn = unitPriceColumn.multiply(quantityColumn).setTitle(

"Price"

)

2
 
                       

.setDataType(currencyType)
3
 
                       

.setStyle(priceStyle);

Condition3 is applied only if price is greater than 200 and sets background color of a price to green.
Condition4 is applied only if price is smaller than 20 and sets background color of a price to red.
[![SimpleReport_Step12]()](http://www.dynamicreports.org/examples/simplereport_step12.png "SimpleReport_Step12") [![pdf]()](http://www.dynamicreports.org/examples/simplereport_step12.pdf "pdf preview")[![preview]()](http://www.dynamicreports.org/examples/simplereport_step12.png) [SimpleReport_Step1](http://www.dynamicreports.org/examples/simplereport_step12 "SimpleReport_Step12")

来源： <[http://www.dynamicreports.org/getting-started](http://www.dynamicreports.org/getting-started)>
 
