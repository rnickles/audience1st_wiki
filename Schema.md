# Vouchers, Donations, Shows

The primary models of interest are:

* `Item`: things patrons receive or pay for; subclasses `Voucher`, `Donation`, `RetailItem`, `CanceledItem`.  All stored in a single table `items` using Rails single-table inheritance.
* `Order`: a group of things purchased as part of a single payment
transaction.
* `Show` and `Showdate` (1-to-many relation): a production and a
performance respectively.
* `Vouchertype`: specific ticket names/types with price points and
season validity.
* `ValidVoucher`: captures the abstraction of "Up to N instances of vouchertype V may be sold for showdate S", including parameters such as capacity constraints on individual vouchertypes, different start/stop sales times, "promo codes" the patron must enter to reveal a particular offer, etc.  **Note:** This is a terrible name and was chosen for historical reasons.  It should really be called `Offer` or `TicketOffer` or something similar.

The relationship among these classes is shown in the figure below.

**(Figure TBD...)**

As is customary in Rails, a column whose name looks like
*something*`_id`  is a foreign key to the `somethings` table (note that
per Rails conventions, the table names are all plural but the
foreign key names are singular).

The most important model and table is `items`'.  The table's
`type` column indicates which subclass (voucher, donation, etc.) each row is an instance of.
Every item that costs money to purchase has an `amount` field showing
what was actually paid for that item.

## Items of subclass `Voucher` represent tickets

* The foreign key `vouchertype_id` (to the `vouchertypes` table) tells what type of
ticket this is.  A vouchertype typically represents a named price point
(ticket type) during a particular season.

* The foreign key `order_id` tells which order this ticket was part of.
An order consists of a single payment transaction, so details about the
payment (credit card confirmation code, etc.) are part of the `Order`
rather than of each `Item`.  The order also has three foreign keys to
the `customers` table: `customer_id` (the customer holding the item),
`purchaser_id` (the customer who paid for the item, which might be
different if e.g. it's a gift order), and `processed_by_id` (the person
who placed the order, which could be box office staff, etc. if not the
customer herself).  These keys are duplicated in the `items` table but
really shouldn't be.

* If the voucher is reserved for a particular performance, the `showdate_id`
foreign key tells which performance; otherwise it's `NULL`.  A
`showdate` has a (local timezone) date and time and some other
properties, and a foreign key to which `show` (production) it's related to.

## A Vouchertype is like a template

A `Vouchertype` is the
"template" for a particular ticket type--name by which it's listed,
price, who may purchase it (subscribers, box office only, anyone, etc.),
which season it's valid for, etc.  A `Showdate` models a single
performance, with a house capacity, start/end time, start/end sales
dates, and so on.  A `Showdate` and `Vouchertype` are tied together by
the model `ValidVoucher`, a join table that captures the idea of a
particular type of voucher being valid for a particular performance
(showdate), with optional capacity controls and promo codes for that
particular (showdate, vouchertype) pair.  Note that the `ValidVoucher`
model and join table is only used at sales time to determine who is
allowed to buy what and when; it is irrelevant to determining what
tickets _have been sold_.

## How subscriptions are handled

A subscription is a special case of a bundle--a group of vouchers sold
together.  A vouchertype whose `category` attribute is `bundle` is actually a container
for the individual vouchers in that bundle.  An individual subscriber
voucher, such as a ticket for a specific production that's part of a
season subscription, has the category `subscriber` and must have a price
of zero, because it's actually just part of a bundle (subscription) that
has a nonzero price.  

A bundle voucher doesn't have to be a subscription; the `subscription`
attribute on the vouchertype tells whether purchasing this vouchertype
makes the buyer a Subscriber.  (So in principle you can be a subscriber
without buying an actual subscription.)  This is relevant because the
concept of "being a subscriber" is deeply wired into Audience1st in
terms of setting up ticket sales.

## Items of subclass `Donation` are donations

The foreign key `account_code_id` tells which fund the donation went to;
the `order_id` ties it to the order (which also gives payment
information, date of payment, etc.)

## The Customers table

The table is pretty standard, modulo a few "special" customers such as
the Anonymous Customer (to whom all walkup sales are linked), the
Boxoffice Daemon (which automatically processes orders from third-party
vendors such as Goldstar), and a few others.  All users of the system,
even if they are not actually customers (eg administrators, box office
staff, etc), must appear in this table or they cannot login.

Email addresses in this table are used for login, and must be unique.
Case does not matter.

## Other tables

Most of the other tables handle ancillary work: `Options` tracks global
option (settings) values, `Purchasemethod` (which really should just be
some constants) are ways to pay for a purchase, and there's a few other
tables that are largely self-explanatory. 

