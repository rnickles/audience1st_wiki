A `Voucher` (really an `Item`) has a `seat` column which is blank for general-admission shows.  For reserved seating performances, this field contains a seat number.  General admission (GA) vs. reserved seating (RS) can be specified at the per-performance level, so some performances of a production can be GA while others are RS.

# Differences in reserved seating workflow

There are four workflows in which the user may have to choose seat(s) for an RS show:

1. Subscriber using a subscriber voucher to make a reservation ("My Tickets" screen)
2. Box office adding comps for a patron ("Add comps" screen)
3. Box office doing a walkup sale during front-of-house
4. Box office checking in a subscriber using subscriber vouchers during front-of-house

(The seatmap is also displayed during the "Add/Edit Seatmaps" dialog but we won't cover that here.)

In cases 1 and 2, selecting a show date from a dropdown menu causes a seatmap to appear if that showdate is RS, or disappear if it is GA.  (The file `components/_seatmap.html.haml` is an invisible `div` that is part of every page where this could occur and is made visible/invisible as needed.)  The JavaScript files whose name includes `seatmap` contain the code for interacting with the seatmap, though most of the work  is done by the external library [`jquery.seat-charts.js`](https://github.com/mateuszmarkowski/jQuery-Seat-Charts).  (BUG: the various `jquery.*` files should be vendored.)

As the user interacts with the seatmap, a designated "Seats selected" field on the page is filled in.  The JS code ensures that the correct number of seats is selected.  Upon form submission (confirm reservation, finalize sale, or whatever), the contents of that field are submitted as part of the form and saved for each associated voucher.  Model validations check that for GA shows, no seats are specified (field is blank), and for RS shows, the number of seats chosen matches the number of tickets in the order, the seat numbers are all valid for the show's seatmap, and the seats are not occupied.  (The JavaScript code won't allow choosing an occupied seat, but race conditions can happen.)

# Code flow for using reserved seating

Any flow that might use reserved seating needs to do the following:

1. Attach particular CSS selectors to various elements on the page, as described below.
1. Provide a _setup_ function that sets up various slots in `A1.seatmap` as described below, and causes the seatmap to be rendered for interaction with the user.  
1. Bind that setup function to the appropriate change event, such as the user selecting a date from a dropdown menu.  The function is invoked on **every** change, because if it detects that (e.g.) the user selects a  GA show, it might be necessary (e.g.) to re-hide the seatmap.
1. (Optional) Provide a _reset_ function that resets any state and visuals if the user clicks "Cancel" to exit the seat selection dialog without choosing seat(s).

## Setup function part 1: set up reserved-seating info

The setup function should fill in the following slots of the `A1.seatmap` data structure.  Non-required slots can be left empty.  Each of the following slots should be set to a jQuery selector expression, e.g. `$(.seat-display)`.  The expressions will be evaluated the first time the seatmap becomes visible on a given page.

* `max`: an integer representing the number of seats the user must select.

* `seatDisplayField`: where to display the seats chosen so far (e.g. "A1,A3").  Applying the class `.a1-passive-text-input` to a form field results in a read-only, undecorated, **but active** form field, which is what you need in order to ultimately submit the seat numbers as part of the form submission.

* `confirmSeatsButton`: the button that causes form submission.  The JS code will only enable the button when the correct number of seats has been chosen.

* `selectSeatsButton`: optional - the button that triggers the seatmap to appear, such as "Select Seats...".  If specified, the JS code will temporarily disable this button while seat selection is in progress.

* `resetAfterCancel`: optional - a function that will be called if the user cancels seat selection.  It can restore the page to look however it looked before the seatmap was triggered.  The JS code takes care of hiding the seatmap, resetting the "selected seats" field (`seatDisplayField` jQuery selector) to blank, etc., so this function just has to do any additional cleanup, such as re-enabling/disabling other buttons and fields.  For example, on the Subscriber Reservations flow, the dropdown menu of how many seats to reserve is temporarily frozen (disabled) while seat selection is in progress; cancelling (or completing) seat selection un-freezes (re-enables) this menu.

## Setup function part 2: render correct seatmap

Once the setup function has set the above slots, it must get its hands on the JSON representation of an A1 seatmap, as returned by the `Seatmap` model.  The easiest way to get this is to call the endpoint `/ajax/seatmap/:id`, which returns a JSON object representing the seatmap associated with showdate ID `:id`.  In particular, this object has the following slots, which you don't need to interpret or care about if you use the AJAX endpoint to fetch the object:

* `map`: the JSON representation of a seatmap used by the jQuery Seat Charts plugin
* `seats`: the JSON representation of seat classes (categories, price points) used by the jQuery Seat Charts plugin
* `unavailable`: an array of the seat numbers of unavailable seats
* `image_url`: the URL of an image to use as the background to display behind the seating chart (for example, to show where the stage is located)

(Note that some pages eagerly load this information for all showdates and keep it in a hidden form field, rather than doing an AJAX call.  That's probably uglier than needed.  Also, if you call the endpoint on a showdate ID that has no associated seatmap because it's reserved seating, you will get the object `{"map": null}`.)

Once the setup function has this JSON data (let's call it `jsSeats`), it should do the following:

```javascript
A1.seatmap.configureFrom(jsSeats);
A1.seatmap.seats = $('#seatmap').seatCharts(A1.seatmap.settings);  // this should be factored out as common code
A1.seatmap.setupMap();                                             // this should be factored out as common code
```

That's it.  If the user selects the right number of seats, the JS code will enable the "submit" button (`confirmSeatsButton`), the form will be submitted, and the controller action can check the "seats selected" element (`seatDisplayField`) for the actual seat numbers chosen.  If the user cancels without finishing selection, the seatmap will be hidden, selected-seat data will be cleared both internally and from the `seatDisplayField`, and the `resetAfterCancel` function will be called if one was provided.

