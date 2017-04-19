# Account Codes

Account Codes are arbitrary strings used in your internal financial reporting system; for example, QuickBooks Online uses 4-digit numbers to code accounts.

An Account Code can be attached to tickets and subscriptions, donations, retail-sale items, and nonticket purchases (e.g. exchange fees). The revenue reporting breaks out subtotals by account code.

Account Codes can be changed retroactively: if you change a particular ticket type's account code, future financial reports will honor the new account code.  In other words, account codes don't affect how revenue is tallied internally, but they are "carried around" and used for subtotaling to make your reporting easier.

There is always at least one Default Account Code, which cannot be deleted but can be changed.  That is, at least one Account Code must always exist.

There are two ways to access the *Add/Edit Account Codes* screen:

1. Click the *Donations* tab, and look for the *Add/Edit Account Codes* button next to the *Search* button.

2. While creating or editing any voucher type, one of the attributes of a voucher type is what account code should receive revenue from its sales.  Next to the dropdown menu for selecting the appropriate account code is a button to *Add/Edit Account Codes*.

# Online Donation to a Specific Fund

You can create a URL of the form `/donate_to_fund/XXXX` where `XXXX` is the Account Code string to which the donation should be credited.  If `XXXX` does not correspond to any existing Account Code, the default Account Code for donations (see [First-Time Setup](0. Introduction & First Time Setup)) will be used.

If this page is viewed in Admin mode, a cash or check donation can be recorded to the specific fund.  If viewed by a regular patron, the only way to donate is via credit card.
