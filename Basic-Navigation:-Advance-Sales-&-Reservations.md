# Looking Up and Acting On Behalf of a Customer

If you're logged in as any kind of admin (i.e. Staff or higher privilege), under the main navigation "tabs" is a yellow ribbon with an autocompletion field to search for a customer.  You can also use List All or List Dups (finds possible duplicates for merging, see below).  Once the customer is selected, they become the **current customer** and you are acting on their behalf if you click Buy Tickets, My Tickets, Add Comps, etc.

When (and only when) a current customer is selected, the buttons in the secondary navbar appear:

* Add Comps - add complimentary tickets to the customer's account, either open (unreserved) or reserved for a particular performance
* Orders - list this customer's order history
* Transfer - transfer items from this customer to another customer
* Donations - list this customer's donation history (actually an alias for the main Donations screen, but with "restrict to customer" selected)
* Transactions - database transaction log, semi-structured and variably useful
* New Donation - record a new donation for this customer

In addition, clicking the main navbar's "Buy Tickets" will enter the regular tickets sales flow with this customer selected as the purchaser and recipient.  This purchase flow is the same one the customer would see if they self-purchase, except that their only payment option is credit card whereas you can record a cash or check sale as well.

Click "Billing/Contact" to see or modify the customer's contact information (mailing address, email, etc.)  You can make changes and click Save to update the customer's contact info.  The settings  "opt out of US mail" and "opt out of email" affect customer report generation and MailChimp integration if enabled.

A framed set of fields labeled "Administrator Preferences" displays customer information that only admins can see and change, including  Privilege level, Comments visible to staff only, and Labels, which lets you attach one or more labels of your own choosing ("Potential donor", "Advisory board member", "Community leader") that can be used later in reporting.

The "Do not email customer a confirmation of these changes" box is checked by default.  If you un-check it, then when you click "Save" to save changes to this customer, a summary email will be sent to them (if they have a valid email address) indicating the changes.  (Changes to admin-only-visible fields, such as Staff Comments or customer privilege level, will not be indicated in this confirmation email.)

# Merging duplicate customers

Some customers will inadvertently create multiple accounts for themselves, and become confused when they login to account #2 and don't see the tickets they purchased under account #1.  In this case, the best thing to do is merge the accounts.

To merge accounts, both customer names must be visible in the summary listing.  For example, if you suspect customer Al Johns has duplicate entries, use the Search Any Field box in the customer navbar to search for "Al Johns", and you'll see a summary list of all records matching that search.  Presumably, two of the matching records will be the two duplicate entries for Al Johns, possibly shown alongside Albert Jones, Alice Johnson, and so on.  The "List Dups" button in the secondary navbar also tries to find all possibly-duplicate records.

To merge two customer accounts shown in the summary list, check  the checkbox to the left of each customer's name and choose either Manual Merge (lets you select which field is retained from each record) or Auto Merge (automatically keeps information belonging to the account that was accessed most recently).  All purchases, donations, and other activity from both accounts will be preserved in the merged account.  Whichever email address you choose to retain in manual merge, the corresponding password is retained too.  You may  need to communicate with the customer to remind them that only this single email address can now be used to login, although once they login they can always change their email or password to something else, as long as the email address is unique in the database.

# Deleting customers

If you check one or more customers in the listing and click Forget, their transactions are transferred to the "Anonymous Customer", a special fictional customer, for relational integrity; and the customer's name and contact info are permanently, irretrievably deleted. 