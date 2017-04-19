# Overview

To set up a new season, you need Box Office Manager privilege or higher.  

A season has a start date (opening night of the season, usually), which
is set during the one-time setup of Admin Options.  Any references to
"the season" use that boundary.  So if the season start date is September 2,
then the "2014-2015 season" runs from September 2, 2014, to September 1,
2015 (even if there are no shows after, say, May).  If the start date is set to January 1 so that the season
coincides with the calendar year, Audience1st will refer simply to "the
2015 season".

A typical season setup consists of these steps:

0. A season consists of one or more **shows** or productions.  Each show has one or more **show dates** or performances.  Season setup begins with adding shows and then show dates for each show.

0. A **voucher type** represents a particular type of ticket: general admission, discount admission, comp ticket, and so on.  A given voucher type is only valid during a particular season, though you can easily "clone" a voucher type from a previous season in setting up this season.  So the next step in season setup is indicating which voucher types are in use this season.

0.  Finally, a **redemption** specifies which kinds of vouchers can be
sold for which performances.  In effect, a redemption "connects" a set
of performances to a set of voucher types.  For example, you might offer
a "Matinee Discount" voucher that is valid for matinees but not evening
shows.  The final step in season setup is associating voucher types with
show dates, and optionally, capacity controls (you may want to hold back
a certain number of house seats on opening night, or you may want to
limit the number of "special discount" tickets that can be sold for any
one performance). 

# Setting up the season's productions

To add a new show (production), click the Season tab and then "Add New
Show".  (It doesn't matter what season is currently shown when you click
"Add New Show"; a new show's season is automatically set based on its
opening and closing dates.)

Fill in the information about the new show; you can hover over the
question-mark icons for information about what each field represents.
Click "Create" when done, or "Don't Create" to cancel the operation.

Once a show has been created, it will appear in the list when you click
the Season tab.  (**Note:** Keep in mind that if it's currently 2016 and
you are adding shows with run dates in the 2017 season, in order to see
them in the list you will have to select 2017 from the "Listing shows
for season:" dropdown menu.)

Click on "[Link]" next to the name of an existing show to get a popup
window showing a link that takes the customer directly to a purchase
page pre-populated for that show.  You can copy and paste this link onto
your theater's website or into an announcement email.

You can click on an existing show's name to modify any of the
information you added.  Continue this process until all shows are added.

## Adding Show Dates (Performances) to Each Show

Click on the name of a show to see its details, including its list of
performance dates at the bottom of the page.  To add new performance
dates, click "Add a Performance" and fill in the recurring information;
for example, you might first add all the Friday and Saturday evening
performances, then click "Save & Add More Dates", then add all
the Sunday matinees.  Continue adding performances until all the
performances for this show have been added.  It doesn't matter in what
order you add them, as they'll always be listed in date order.

# Setting Up Voucher Types for the Season

A voucher type represents a particular type of ticket or other product
sold during a specific season, and can be in one of five categories:

* **revenue**, a normal single ticket for which you charge money

* **comp**, a single-ticket complimentary admission

* **subscriber**, a voucher only available as part of a package, like a
subscription that includes one ticket to each of 4 shows; subscriber
vouchers have no dollar value because they can only be purchased as part
of a bundle

* **bundle**, a collection of  vouchers that can be sold as a single product (like a subscription or two-fer)

* **nonticket** item, which lets you both sell products (wine, raffle
tickets, etc.) and collect fees (ticket-exchange fees, etc.)

Each voucher type is valid only for the specific season associated with
it, beginning on the season start date you specify in Admin > Options
and ending one year later.  Each new season, you need to create a new
set of voucher types for that season, even if many of the prices are the
same.  So, for example, a "General Admission" voucher created for the
2016 season cannot be associated with any 2017 shows; a new General
Admission voucher for 2017 must be created instead.

To setup vouchers, click the Vouchers tab to see a list of various types
of vouchers associated with a particular season.

## Creating a new voucher type from scratch

To create a new voucher type, click "New Voucher Type" at the bottom of
the list of voucher types.  The fields you fill in will vary depending
on the type of voucher.

When do you need to create a new voucher type, vs. just adding additional redemptions (see below) for an existing one?


0. If it has a different price point, it needs to be a separate voucher type.

0. If you want to be able to track its sales separately (even at the same price point), it needs to be a separate voucher type.  For example, Youth and Senior tickets may cost the same, but if you want to break down the revenue from each in reporting, they should be different voucher types.

0. If you want a ticket type that is only valid for specific productions or performances, it probably needs to be a separate voucher type.  

## Creating Bundles and Subscriptions

A special type of voucher is a _bundle_, which actually just collects together a bunch of other vouchers into a package.
Even though the bundle is itself a voucher, it's a voucher that cannot be redeemed for anything; think of it as a "container" for the rest of the vouchers in the bundle.  So when a patron buys (for example) a 4-show subscription, they are actually getting 5 vouchers: one representing the subscription itself, and one each for the 4 shows in the subscription.

Bundle vouchers have the special property that if a bundle is cancelled or transferred to another customer, all of its 
constituent vouchers travel with it.  If you cancel and refund a subscription, that automatically cancels the vouchers included in it; if you transfer a subscription to another patron (maybe it had been intended as a gift), all the included vouchers go with it.

To create a bundle, you must _first_ create all of the voucher types that will be part of the bundle, and they must have the category "Subscriber voucher" (even if the bundle isn't actually a subscription but just a package deal that doesn't make the buyer a subscriber).  Subscriber vouchers don't allow you to specify a price, because they will only be sold as part of a bundle.  Then you create a new Bundle type voucher and associate any number of bundled vouchers with it, and set the price for the entire bundle.

# Specifying Redemptions

A voucher type just specifies how much a ticket costs and who is allowed to buy it; it doesn't specify what performances it's valid for, or what the sales period is for that type of ticket for that performance. That information is carried in a **redemption**, which associates a particular voucher type with one or more performances for which that type of voucher can be used, and possibly specifies start/end sales dates, capacity controls, and promo-code-controlled discounts for that voucher type.

Click on the Season tab, click on a show name, and click Add Ticket Type to All Performances.  You can then choose a voucher type that can be redeemed for performances of that show and specify when sales of that voucher type begin and end.

You can also click on "New..." under a particular performance to add a voucher type to just selected performances.

You can also specify capacity controls, so that certain voucher types (e.g. discount tickets) are quantity-limited per performance, and "promo codes" that patrons can type in to reveal vouchers at special prices.

You can also associate specific voucher types with only a particular performance rather than all performances.  To do this, click on the Season tab, then the show name, and next to the desired performance click "Show Ticket Details".  This will reveal all voucher types that can be redeemed for that performance; you can add a new voucher type (click "Add New...") or make changes to the existing ones.  

You cannot delete a voucher type from a performance if some vouchers of that type have already been sold; however, you can prevent further sales of that voucher type by adjusting its "max sales" limit to the number already sold for that performance.

## Redemptions: An Example (Single Tickets)

Suppose we have a run of the play Hamlet, every Fri, Sat and Sun night from Jan 1-17, 2015 (nine performances), and we have a house capacity of 100.  Our general admission price is $40 adults, $25 youth, but we also want to do promotions to sell more seats at matinees.  We might set up tickets and validity records as follows:

| Voucher type and price | Validity |
|------------------------|----------|
| Adult General, $40     | All performances; unlimited sales; advance sales stop 3 hours before performance |
| Youth General, $25     | All performances; unlimited sales; advance sales stop 3 hours before performance |
| Matinee Special, $20   | Sunday shows only; sales limited to 30 per show; advance sales stop 2 days before performance |

This way, even if the matinee special is really popular, at most 30 such tickets can be sold for each matinee.

When a customer is on the "Buy Tickets" page and selects a particular performance, only the voucher types whose validity records allow that performance will be shown.  So, for example, if there were seats available for the Sunday Jan. 3 matinee but all 30 Matinee Special seats had been sold, the patron could still buy Adult General or Youth General seats for that performance, but not Matinee Special seats.  The same would happen if there were still Matinee Special seats left but the show was less than 2 days away, since the Matinee Special validity record says that sales of those tickets must stop 2 days before the performance.

## Redemptions: An Example (Subscriptions)

 The simplest subscription is "one ticket to each of our 3 shows this season" (let's say Hamlet, Othello, and King Lear).  To sell this subscription, you'd create a voucher type corresponding to a subscriber reservation for each of the shows.  The Hamlet subscriber voucher could be valid for any performance of Hamlet, but not for any other production; and so on.

You can also create bundles that are more restrictive.  For example, a "matinees only" subscription would require creating three more voucher types whose validity records would indicate they're only valid for matinees.  The validity records of the six vouchers in this scenario might then be:

| Voucher type | Validity |
|------------------------|----------|
| "Hamlet" - subscriber | All performances of Hamlet; unlimited sales |
| "Hamlet" - subscriber - Matinees only | Matinee performances of Hamlet; unlimited sales |
| "Othello" - subscriber | All performances of Hamlet; unlimited sales |
| "Othello" - subscriber - Matinees only | Matinee performances of Hamlet; unlimited sales |
| "King Lear" - subscriber | All performances of Hamlet; unlimited sales |
| "King Lear" - subscriber - Matinees only | Matinee performances of Hamlet; unlimited sales |
| Regular Subscription - $100 | Contains one each of "Hamlet - subscriber", "Othello - subscriber", "King Lear - subscriber" |
| Matinee-Only Subscription - $75 | Contains one each of "Hamlet - subscriber (Matinees only)", "Othello - subscriber (Matinees only)", "King Lear - subscriber (Matinees only)" |

If your subscription is more flexible--for example, "3 tickets to our regular productions, to use as you see fit"--you might create just a single "Subscriber voucher" type and declare it to be valid for any performance of any show.  You can also create "family pack" bundles that contain (for example) 2 "Adult subscriber" and 2 "Youth subscriber" tickets, and so on.
