---
title: "PHP Data Table"
layout: single
summary: "Creating a WebForm like table control for PHP"
---
One nice thing about working in ASP.NET is the abundance of pre-made controls. They are convenient, powerful, and can be bound to numerous different data sources. Unfortunately, they are also bloated, slow, and unwieldy. The combined HTML and ViewState of a GridView (ASP.NET Web Control) is enough to drive a conscientious developer mad.

Here is a [GridView demo](http://quickstarts.asp.net/QuickStartv20/aspnet/samples/data/GridViewBoundFields_vb.aspx) on the official ASP.NET QuickStart site. I won't post it here, but the ViewState produced by the table is almost 5,000 characters long. If we include server-side sorting, the number jumps up a bit more and requires another call to the database. There are some benefits to the Microsoft, but in the end it feels like too much for too little.

The goal of this project was to create something that would satisfy the need of a GridView with a much smaller footprint. I am sure there is a PHP library somewhere bursting with similar functionality, but that wouldn't be much fun.

# Code

{% highlight php %}
$table = new Grid();

// Define the columns using "addColumn(dbColumnName, DisplayText)
$table->addColumn("id", "ID");
$table->addColumn("fName", "First Name");
$table->addColumn("lName", "Last Name");
$table->addColumn("sex", "Gender");
$table->addColumn("age", "Current Age");
$table->addColumn("email", "Contact Email");

// Format individual columns
$table->columns["age"]->textAlign = "center";
$table->columns["sex"]->replaceValue("m", "Male");
$table->columns["sex"]->replaceValue("f", "Female");

// Set the data source for the table and output the html
$table->setDataSource($MySqlQueryResult);
$table->render();
{% endhighlight %}

The first line assigns and instantiates a new Grid object. The next block defines the columns using the database column name and the display text that will appear in the table header.

Once columns have been created they can be configured individually. The first formatted column, "age", has its text-align style set to "center". I have not opened up every style attribute but it would be easy to do.

The next two lines allow for a table value to be replaced with a given string. In this case, we want to switch the gender indicator "m" with something more informative, like "Male". Anywhere "m" would appear in the "Gender" column, the table will use "Male" instead. Replacement values don't have to stop at text. For instance, checkboxes could be used in place of a boolean value.

The last lines of code tell the Grid what MySQL query to use and then to output the HTML. The result is clean, semantic markup.

# Grid Features

The Grid comes with it's own internal stylesheet but users can override it by changing the Grid object's css class name.

jQuery based column sorting is enabled by default and runs smoothly even on large data sets. In Safari 5 (Build 6533.16), a 6 column table with 1,000 rows sorts in 200ms.

If table columns are not explicitly defined, the grid will generate them from the MySQL query columns. In that instance, column configuration would not be available as the column array is defined at output.

# Future

I'm pleased with the Grid for the amount of time spent developing it. I was able to do some Object Oriented PHP work that helped illustrate some of the differences between it and other languages I've used.

I'm especially happy with the Javascript sorting technique. It provides just the right amount of functionality for this project. The overhead of [Tablesorter](http://tablesorter.com/docs/) has been an issue in past. Large tables often result in the "A script on this page is running slowly" dialog.

If I return to this project, I will add support for nested tables. They've been indispensable in several work-related projects and the functionality would be fun to replicate in PHP.