
# Cornelius

  A simple Cohort Visualization library

## Installation

Just include the `css` and `js` files.

## Example usage

    var data = [
      [1973,1000,750,300,400,70,20],
      [1268,549,336,221,122,115],
      [1892,1282,250,32,18],
      [1832,379,254,314],
      [1171,256,120],
      [2533,340]
    ],  
    initialDate = new Date(2012, 0),
    Cornelius.draw({
      initialDate: initialDate,
      container: document.getElementById('main-example'),
      cohort: data,
      title: 'Cornelius Demo'
    });

## Data Format
The cohort property must be an array of arrays whose values will be mapped to the cohort table cells in the same
order as provided. Given the distinctive triangle shape of the chart, each array should either have a value less 
than the previous one, or it should be filled with null, undefined, or false values.

#Options
Cornelius accepts any of these option values passed to the constructor:

##timeInterval
Cornelius accepts 4 time intervals that are used to compute the date labels: daily, weekly, monthly(Default), yearly.

##repeatLevels
It is possible to customize the colours of the cells depending on the percentage each of them have. You will have 
to specify the class names and the percentage ranges the library will use in order to decide which class will be assigned to each cell.

Below are the default css classes and ranges the library uses.

    var repeatLevels = {
      'low': [0, 10],
      'medium-low': [10, 20],
      'medium': [20, 30],
      'medium-high': [30, 40],
      'high': [40, 60],
      'hot': [60, 70],
      'extra-hot': [70, 100]
    }
    Cornelius.draw({
      repeatLevels: repeatLevels,
      initialDate: new Date(2012, 3),
      cohort: data,
      container: document.getElementById('container')
    });
In this example, Cornelius will assign the cornelius-low class to each cell with a value between 0% and 10%, 
cornelius-medium-low to each cell between 10% and 20%, and so on.

You can specify as many ranges and class names as you want, but don’t forget to add those classes to your stylesheets.

##drawEmptyCells
Setting this property to true will display a white empty cell for those cells that are “outside” of the main triangle.

##rawNumberOnHover
If rawNumberOnHover is set to true, it will display the raw number while leaving the mouse on top of the cell.

##displayAbsoluteValues
If displayAbsoluteValues is set to true, absolute values will be displayed instead of the percentage value.

##initialIntervalNumber
This property will allow you to specify the initial number that will be displayed in the first column header. 
Default: 1

##I18n
The library uses only 3 strings for the labels that can be specified in the options object:

    Cornelius.draw({
      labels: {
        time: 'Zeit', // Time
        people: 'Menschen', // People
        weekOf: 'Woche Vom' // Week Of
      },
      initialDate: new Date(2011, 6),
      cohort: data,
      container: document.getElementById('container')
    });
The long and short month names can also be customized:

    Cornelius.draw({
      monthNames: ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio', 'Julio',
                   'Agosto', 'Septiembre', 'Octubre', 'Noviembre', 'Diciembre'],
    
      shortMonthNames: ['Ene', 'Feb', 'Mar', 'Abr', 'Mar', 'Jun', 'Jul',
                        'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
    
      initialDate: initialDate,
      cohort: data,
      container: document.getElementById('container')
    });
        
Alternatively you can specify the functions that will be called to return the Strings for the header and date labels:

##formatHeaderLabel(index)
If setting the initialIntervalNumber and labels doesn’t fullfill your needs, you can format the header label using 
a lower level API. You can pass a function that will be called for every column header that is being drawn, and its 
return value will be used as the header label. The only parameter it receives is the index of the column that will 
be drawn (zero index).

Example

    Cornelius.draw({
      formatHeaderLabel: function(i) {
        return i * 2;
      },
    
      initialDate: initialDate,
      cohort: data,
      container: document.getElementById('format-header-example')
    });
    
#Formatting the dates
There are 4 optional parameters that Cornelius can use to format the date labels. Depending on the time interval you use,
 you can pass a formatDailyLabel, formatWeeklyLabel, formatMonthlyLabel and formatYearlyLabel function to format a daily, 
 weekly, monthly and yearly cohort chart respectively.
These functions takes 2 parameters: the date object corresponding to the initial date, and the column index.

Example:

    Cornelius.draw({
      formatMonthlyLabel: function(date, i) {
        date.setDate(date.getDate() + i); // update the date object to the corresponding month
        return "Month " + (i + 1) + " - " + this.monthNames[i];
      },
    
      initialDate: initialDate,
      cohort: data,
      container:document.getElementById('format-date-example')
    });
    
#Trimming the table
If your cohort data is too large and want only to display the first N columns or the last M rows, you can use the 
maxRows and maxColumns properties.

Example:    
        
        Cornelius.draw({
          maxRows: 4,
          maxColumns: 3,
        
          initialDate: initialDate,
          cohort: data,
          container: document.getElementById('trimming-example')
        });
        
#Chart Title
To display a title above the chart, use the title option:
    
    Cornelius.draw({
      title: 'My Chart Title',
    
      initialDate: new Date(2011, 6),
      cohort: data,
      container: document.getElementById('container')
    });
    
#Default options
One instance of Cornelius can be used to draw multiple charts using the same options. You can also set the global 
defaults that will be used in any Cornelius instance.

Example:

    Cornelius.getDefaults(); // { ..., initialIntervalNumber: 1, timeInterval: 'monthly', ... }
    
    Cornelius.setDefaults({initialIntervalNumber: 0, timeInterval: 'weekly'});
    
    Cornelius.getDefaults(); // { ..., initialIntervalNumber: 0, timeInterval: 'weekly', ... }
    
    Cornelius.resetDefaults();
    
    Cornelius.getDefaults(); // { ..., initialIntervalNumber: 1, timeInterval: 'monthly', ... }
    
#jQuery plugin
Cornelius doesn’t have jQuery as a dependency. But if you have it already, you can use the jQuery helper:

    $("#container").cornelius({
      initialDate: new Date(2013, 1),
      // cohort data
      cohort: [
        [100, 30, 23, 10],
        [399, 23, 10, 40]
      ],
      timeInterval: 'daily',
      /* ... other options ... */
    });
    
#MIT License
Copyright (c) 2013 Restorando

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.