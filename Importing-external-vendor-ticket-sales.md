Audience1st can import ticket sales from external vendors such as [Goldstar](goldstar.com), [TodayTix](todaytix.com), and so on. These vendors deliver sales reports in CSV, JSON, or XML format. The details provided in each vendor's report may vary.

Audience1st converts each sale transaction in such a report into an `Order` instance whose `purchasemethod` is set to `Purchasemethod::EXTERNAL` and `processed_by` is set to the special customer `Customer.boxoffice_daemon`.

# Class architecture

The `TicketSalesImport` model stores only the original raw import data and which vendor it's for. The vendor names are generated dynamically by looking at all the class names within the `TicketSalesImportParser::` namespace, in the subdirectory `lib/ticket_sales_import_parser/`.

`TicketSalesImportParser` is an abstract class.