Audience1st can import ticket sales from external vendors such as [Goldstar](goldstar.com), [TodayTix](todaytix.com), and so on. These vendors deliver sales reports in CSV, JSON, or XML format. The details provided in each vendor's report may vary.

Audience1st converts each sale transaction in such a report into an `Order` instance whose `purchasemethod` is set to `Purchasemethod::EXTERNAL` and `processed_by` is set to the special customer `Customer.boxoffice_daemon`.

# Overview

The `TicketSalesImport` ActiveRecord model stores only the original raw import data and which vendor it's for.  A single import is turned into zero or more `ImportableOrder` objects that are rendered for the user (box office worker); essentially an `ImportableOrder` has a component `Order` object whose `processed_by` is set to `Customer::BOXOFFICE_DAEMON`, `purchasemethod` is set to `Purchasemethod.get(:ext)`, and on which `add_tickets` has been called to add the appropriate tickets to the order, but `customer` is not yet set.  The `ImportableOrder` object has a list of candidate `Customer` instances to which the order could be assigned, since sometimes we cannot know with certainty if the customer in the imported order does or doesn't match someone we already know.

The work of parsing the raw data to produce a set of `ImportableOrder`s is done by an instance of a `TicketSalesImportParser::` service object.  `TicketSalesImportParser` itself is an abstract class that acts as an injected dependency.  The constructor just stores a copy of the `TicketSalesImport` model instance that is using this parser.  Its subclasses (one per vendor type) must implement the following methods:

* `valid?` - determine whether the raw data is valid import data for this vendor (by doing sanity checks on the format of the data).  ActiveRecord model's `valid?` delegates to this method.

* `parse` - parse the data and return a collection of `ImportableOrder` objects.

![](http://i63.tinypic.com/1kwv7.jpg)