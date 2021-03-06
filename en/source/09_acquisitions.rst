.. include:: images.rst

Acquisitions
============

The Koha Acquisitions module provides a way for the library to record
orders placed with vendors and manage purchase budgets.

-  *Get there:* More > Acquisitions

`Setup <#acqsetup>`__
---------------------

Before using the Acquisitions Module you will want to make sure that you
have completed all of the set up.

First, set your `Acquisitions System Preferences <#acqprefs>`__ and
`Acquisitions Administration <#acqadmin>`__ to match your library's
workflow. Before setting your `EDI Accounts <#ediaccounts>`__ and
`Library EANs <#libraryeans>`__ you will need to have `entered your
vendors <#addacqvendor>`__.

On the main acquisitions page you will see your library's funds listed.

Acquisitions Funds Summary
|image781|

    **Note**

    If the total line is confusing for the funds you have set up you can
    hide it by adding

    ::

        #funds_total {display:none;}

    to the `IntranetUserCSS <#IntranetUserCSS>`__ preference.

To see all active funds you can click the checkbox next to 'Show active
and inactive' above the funds table.

To see a history of all orders in a fund you can click on the linked
amount and it will run a search for you.

Breakdown of orders against the FIC Fund
|image782|

Learn more in the `Budget/Fund Tracking <#fundtracking>`__ section of
this manual.

`Vendors <#acqvendors>`__
-------------------------

Before any orders can be places you must first enter at least one
vendor.

`Add a Vendor <#addacqvendor>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add a vendor click the 'New Vendor' button on the Acquisitions page

New Vendor Button on Acquisitions
|image783|

The vendor add form is broken into three pieces

-  The first section is for basic information about the Vendor

   Basic Vendor Information
   |image784|

   -  Of these fields, only the Vendor name is required, the rest of the
      information should be added to help with generating claim letters
      and invoices

-  The second section is for information regarding your contact at the
   Vendor's office

   Vendor Contact Details
   |image785|

   -  None of these fields are required, they should only be entered if
      you want to keep track of your contact's information within Koha

-  The final section is for billing information

   Vendor Ordering/Billing Information
   |image786|

   -  To be able to order from a vendor you must make them 'Active'

   -  For List Prices and Invoice Prices choose the currency

      -  Currencies are assigned in the `Currencies & Exchange
         Rates <#currexchangeadmin>`__ admin area

   -  If your library is charged tax mark your Tax Number as registered

   -  Note if you list prices and/or invoice prices include tax

   -  If the vendor offers a consistent blank discount, enter that in
      the 'Discount' field

      -  You can enter item specific discounts when placing an order

   -  Enter your tax rate if your library is charged taxes on orders

   -  If you know about how long it usually takes orders to arrive from
      this vendor you can enter a delivery time. This will allow Koha to
      estimate when orders will arrive at your library on the late
      orders report.

   -  Notes are for internal use

`View/Edit a Vendor <#editacqvendor>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To view a vendor's information page you must search for the vendor from
the Acquisitions home page. Your search can be for any part of the
Vendor's name:

Vendor Search Results
|image787|

From the results, click on the name of the vendor you want to view or
edit

Vendor Information Page
|image788|

To make changes to the vendor, simply click the 'Edit vendor' button.

If the vendor has no baskets attached to it then a 'Delete vendor'
button will also be visible and the vendor can be deleted. Otherwise you
will see a 'Receive shipment' button.

Receive shipment button
|image789|

`Vendor Contracts <#vendorcontracts>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can define contracts (with a start and end date) and attach them to
a vendor. This is used so that at the end of the year you can see how
much you spent on a specific contract with a vendor. In some places,
contracts are set up with a minimum and maximum yearly amount.

`Add a Contract <#addvendorcontract>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

At the top of a Vendor Information Page, you will see a 'New Contract'
button.

New Contract Button
|image790|

The contract form will ask for some very basic information about the
contract

New Contract Form
|image791|

    **Important**

    You cannot enter a contract retrospectively. The end date must not
    be before today's date.

Once the contract is saved it will appear below the vendor information.

Vendor with contracts
|image792|

It will also be an option when creating a basket

Contract Pull Down on New Basket Form
|image793|

`Managing Suggestions <#managesuggest>`__
-----------------------------------------

Purchase suggestions can be generated in one of two ways. You can create
suggestions via the staff client either for the library or `on the
patron's behalf <#patronsuggestions>`__ from their record. Depending on
your settings in the `suggestion <#suggestionspref>`__ system
preference, patrons may also be able to make purchase suggestions via
the OPAC. When a suggestion is waiting for library review, it will
appear on the Acquisitions home page under the vendor search.

Pending suggestions on Acquisitions
|image794|

It will also appear on the main staff dashboard under the module labels:

Pending suggestions on main page
|image795|

Clicking 'Manage suggestions' will take you to the suggestion management
tool. If there are no pending suggestions you can access the suggestion
management tool by clicking the 'Manage suggestions' link on the menu on
the left of the Acquisitions page.

Suggestion Management
|image796|

Your suggestions will be sorted into several tabs: Accepted, Pending,
Checked, Ordered and/or Rejected. Each accepted or rejected suggestion
will show the name of the librarian who managed the suggestion and the
reason they gave for accepting or rejecting it (found under 'Status').

An 'Accepted' suggestion is one that you have marked as 'Accepted' using
the form below the suggestions. A 'Pending' suggestion is one that is
awaiting action from the library. A 'Checked' suggestion is one that has
been marked as 'Checked' using the form before the suggestions. An
'Ordered' suggestion is on that has been ordered using the '`From a
purchase suggestion <#orderfromsuggestion>`__' link in your basket. A
'Rejected' suggestion is one that you have marked at 'Rejected' using
the form below the list of suggestions.

For libraries with lots of suggestions, there are filters on the left
hand side of the Manage Suggestions page to assist in limiting the
number of titles displayed on the screen.

Suggestion Filtering
|image797|

Clicking on the blue headings will expand the filtering options and
clicking '[clear]' will clear all filters and show all suggestions.

    **Note**

    The suggestions page will automatically be limited to suggestions
    for your library. To see information for all (or any other)
    libraries click on the 'Acquisition information' filter and change
    the library.

    Branch filter
    |image798|

When reviewing 'Pending' suggestions you can choose to check the box
next to the item(s) you want to approve/reject and then choose the
status and reason for your selection. You can also choose to completely
delete the suggestion by checking the 'Delete selected' box.

Pending Suggestions
|image799|

Another option for libraries with long lists of suggestions is to
approve or reject suggestions one by one by clicking on the title of the
suggestion to open a summary of the suggestion, including information if
the item was purchased.

Suggestion Information
|image800|

Clicking 'edit' to the right of the suggested title or at the to pof the
suggestion detail page will open a suggestion editing page.

Edit Purchase Suggestion
|image801|

From this form you can make edits to the suggestion (adding more details
or updating incorrect information provided by the patron). You can also
choose to accept or reject the suggestion on an individual basis.

-  Choosing to mark a request as 'Pending' will move the request back to
   the 'Pending' tab.

Reasons for accepting and rejecting suggestions are defined by the
`SUGGEST <#suggestauthorized>`__ authorized value.

Reasons for approving or rejecting suggestions
|image802|

If you choose 'Others...' as your reason you will be prompted to enter
your reason in a text box. Clicking 'Cancel' to the right of the box
will bring back the pull down menu with authorized reasons.

Enter reason for 'Others...'
|image803|

You can also assign this suggestion to a fund. Edit suggestion fund

This edit can trigger a notice (defined in the `Notices &
Slips <#notices>`__ tool with the `TO\_PROCESS <#toprocessnotice>`__
notice) to the fund owner that there is a suggestion ready for them to
manage if you have turned on the `cron job to generate these
notices <#emailsuggestfund>`__.

Once you have clicked 'Submit' the suggestion will be moved to the
matching tab. The status will also be updated on the patron's account in
the OPAC and an `email notice <#notices>`__ will be sent to the patron
using the template that matches the status you have chosen.

Purchase suggestions in the OPAC
|image804|

`Placing Orders <#placingacqorder>`__
-------------------------------------

To place an order you must first search for the vendor or bookseller you
want to send the order to.

    **Important**

    If you are planning on using EDIFACT to submit your order you will
    need to first set up your library's `EDI Accounts <#ediaccounts>`__
    and `EANs <#libraryeans>`__.

`Create a basket <#createacqbasket>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    **Note**

    If you're using EDI for ordering you will want to download your
    order record from your vendor before starting the process in Koha.

To create a basket you must first search for the vendor you're ordering
from:

New Basket / Add Basket Options
|image805|

And click the 'New basket' button to the right of the vendor name.

    **Note**

    You can also add to an existing basket by clicking the 'Add to
    basket' link to the far right of each basket's information in the
    results table.

After clicking 'New basket' you will be asked to enter some information
about the order:

Add Basket Form
|image806|

-  When adding a basket you want to give it a name that will help you
   identify it later

-  Enter in the Billing Place and Delivery Place (this will default the
   library you're logged in at)

-  If you would like to change the vendor you're ordering from you can
   use the Vendor pull down menu

-  The notes fields are optional and can contain any type of information

-  If you're ordering standing items (items which arrive regularly) then
   you will want to check the 'Orders are standing' box for this basket

If you have `added contracts <#addvendorcontract>`__ to the vendor
you're ordering from, you will also have an option to choose which
contract you're ordering these items under.

Basket with contract options
|image807|

When finished, click 'Save'

New Empty Basket
|image808|

Once your basket is created you are presented with several options for
adding items to the order.

-  If you are ordering another copy of an existing item, you can simply
   search for the record in your system.

   Search for existing records
   |image809|

   -  From the results, simply click 'Order' to be brought to the order
      form.

      Order form
      |image810|

      -  All of the details associated with the item will already be
         listed under 'Catalog details.'

-  If you allow patrons to make purchase suggestions (learn more in the
   `Managing Suggestions <#managesuggest>`__ section of this manual),
   then you can place orders from those suggestions. In order to keep
   track of suggestions that have been ordered and received you must
   place the order using this link.

   Approved Suggestions to Order From
   |image811|

   -  From the results, click 'Order' next to the item you want to order
      and you will be presented with the order form including a link to
      the suggestion

      Order from a Suggestion
      |image812|

      -  From this form you can make changes to the Catalog Details if
         necessary.

      -  When the item appears in your basket it will include a link to
         the suggestion.

         Suggestion Link in basket
         |image813|

   -  Orders added to the basket in this way will notify the patron via
      email that their suggestion has been ordered and will update the
      patron's '`My purchase suggestions <#opacmysuggestions>`__' page
      in the OPAC.

-  If you're using the `Serials <#serials>`__ module you can link your
   subscription order information to acquisitions by choosing to order
   'From a subscription'

   -  After clicking the order link you will be brought to a search page
      that will help you find your subscription

      Subscription order search
      |image814|

   -  Your results will appear to the right of the form and each
      subscription will have an 'Order' link to the right

      Subscription results
      |image815|

   -  Clicking 'Order' will bring the subscription info in to the order
      form without an 'Add item' section since you are just ordering a
      subscription and an item isn't needed

      Order from subscription
      |image816|

-  To order from a record that can't be found anywhere else, choose the
   'From a new (empty) record.'

   Order a new record
   |image817|

   -  You will be presented with an empty form to fill in all of the
      necessary details about the item you are ordering.

-  If you want to search other libraries for an item to purchase, you
   can use the 'From an external source' option that will allow you to
   order from a MARC record found via a Z39.50 search.

   Search for record to add
   |image818|

   -  From the results, click the Order link next to the item you want
      to purchase.

      Search Results to Order From
      |image819|

   -  If the item you're ordering from an external source looks like it
      might be a duplicate, Koha will warn you and give you options on
      how to proceed.

      Duplicate order warning
      |image820|

      -  From the warning, you can choose to order another copy on the
         existing bib record, create a new bib record, or cancel your
         order of this item.

   -  In the order form that pops up, you will not be able to edit the
      catalog details.

      New order from Z39.50 Search
      |image821|

-  The next option for ordering is to order from a staged record (`learn
   more about staging records <#stagemarc>`__).

       **Note**

       This is the option you will choose if you have an order file from
       your vendor.

Order from a staged file
~~~~~~~~~~~~~~~~~~~~~~~~

   Staged Files to Order From
   |image822|

   -  From the list of files you are presented with, choose the 'Add
      orders' link to add the records in the staged file to your order.
      Staged records

   -  Next to each title is a checkbox, check the items you would like
      to order, or choose 'Check all' at the top. Depending on your
      settings in the `MarcFieldsToOrder <#MarcFieldsToOrder>`__
      preference Koha will populate the next screen with with the
      relevant Quantity, Price, Fund, Statistic 1, and Statistic 2 found
      within the staged file.Add orders from staged file

   -  In the 'Item Information' tab you can enter information that will
      be added to every ordered item such as item type, collection code
      and not for loan status.Item information

   -  If no information is imported from the MARC record regarding fund
      information the 'Default accounting details' tab can be used to
      apply values related to the accounting.Accounting details

-  The final option for ordering is to order from a list of titles with
   the highest hold ratios

   -  This option will take you to the Holds Ratio report where you can
      find items with a high hold ratio and order additional copies.
      Next to each title will be a link with the number of items to
      order, click that and it will add the item to your basket.Holds
      Ratio Order

With any of the above ordering options you're presented with an option
to notify patrons of the new item when it's received. The contents of
that notification can be edited in the `Notices & Slips <#notices>`__
tool and will have the code of ACQ\_NOTIF\_ON\_RECEIV. In the 'Patrons'
section you will see an option to 'Add user'. Click that button to add
patrons who will be notified of the new issue.

Patron notification search

-  In the window that pops up search for the patrons you'd like to
   notify and click 'Select'

-  Once you're done you can close the window and you'll see the list of
   patrons under the 'Patrons' sectionPatrons

After bringing in the bib information (for all import methods except for
the staged file), if your `AcqCreateItem <#AcqCreateItem>`__ system
preference is set to add an item when ordering you will enter the item
info next. You need to fill out at least one item record and then click
the 'Add' button at the bottom left of the item form.

Item order
|image823|

After clicking the 'Add item' button below the item record the item will
appear above the form and then you can enter your next item the same way
(if ordering more than one item).

Item ordered
|image824|

Once you have entered the info about the item, you need to enter the
Accounting information.

Accounting Details
|image825|

-  Quantity is populated by the number of items you've added to the
   order above.

   -  **Important**

          You cannot edit the quantity manually, you must click 'Add'
          below the item form to add as many items as you're ordering.

-  The list of funds is populated by the `funds <#funds>`__ you have
   assigned in the Acquisitions Administration area.

-  The currency pull down will have the
   `currencies <#currexchangeadmin>`__ you set up in the `Acquisitions
   Administration <#acqadmin>`__ area.

-  The vendor price is the price before any taxes or discounts are
   applied.

-  If the price is uncertain, check the uncertain price box.

   -  A basket with at least one uncertain price can't be closed.

-  If you are charged sales tax, choose that from the gstrate field

-  Enter the percentage discount you're receiving on this order, once
   you enter this, hit tab and Koha will populate the rest of the cost
   fields below.

-  If you added Planning Values when `creating the
   Fund <#addbudgetfund>`__, those values will appear in the two
   Planning Value fields.

Once you have filled in all of the fields click 'Save' to add the item
to your basket. If your price goes over the amount availalbe in the fund
you will be presented with a confirmation.

Fund warning
|image826|

The confirmation warning will allow you order past your fund amount if
you so choose.

After an item is added to the basket you will be presented with a basket
summary.

Basket with item info
|image827|

If you would like to see more details you can check the 'Show all
details' checkbox

Show all details
|image828|

From here, you can edit or remove the items that you have added.

-  Choosing to 'Delete the order' will delete the order line but leave
   the record in the catalog.

-  Choosing to 'Delete order and catalog record' removes both the order
   line and the record in the catalog.

   -  The catalog record cannot always be deleted. You might see notes
      explaining why.

      Can't delete order line
      |image829|

On the summary page, you also have the option to edit the information
that you entered about the basket by clicking the 'Edit basket header
information' button, to delete the basket altogether by clicking the
'Delete this basket' button, or to export your basket as a CSV file by
clicking the 'Export this basket as CSV' button.

Basket Buttons
|image830|

If you're using EDI for your order you can click the 'Create EDIFACT
order' button when you're done to send the file to the vendor and close
the basket.EDIFACT Order

Once you're sure your basket is complete, you can click 'Close this
basket' button to indicate that this basket is complete and has been
sent to the vendor.

    **Important**

    You must close the basket to be able to `receive
    items <#receiveacqorder>`__ when they arrive. Only items in closed
    baskets will show as ready to receive.

If you have your `BasketConfirmations <#BasketConfirmations>`__
preference set to show a confirmation, you will be asked if you are sure
about closing the basket.

Basket Closure Confirmation
|image831|

When closing the basket you can choose to add the basket to a group for
easy printing and retrieval. If you check the box to 'Attach this basket
to a new basket group' you will be brought to the group list where you
can print a PDF of the order.

Closed Baskets
|image832|

    **Important**

    A basket with at least one item marked as 'uncertain price' will not
    be able to be closed.

    A basket with items where the price is uncertain
    |image833|

Clicking the 'Uncertain Prices' button will call up a list of items with
uncertain prices to quick editing. From that list, you can quickly edit
the items by entering new prices and quantities.

Uncertain Prices
|image834|

    **Important**

    The Uncertain Prices page is independent of the basket. It is linked
    to the vendor so you will see all items on order with uncertain
    prices for that vendor.

Once your order is entered you can search for it through acquisitions or
view the information on the biblio detail page in the staff client (if
the `AcquisitionDetails <#AcquisitionDetails>`__ preference is set to
'Display).Acquisitions details

`Create a basket group <#acqbasketgroup>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A basket group is simply a group of baskets. In some libraries, you have
several staff members that create baskets, and, at the end of a period
of time, someone then groups them together to send to the vendor in
bulk. That said, it is possible to have one basket in a basket group if
that's the workflow used in your library.

`Printing baskets <#printacqbasket>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When you are finished adding items to your basket, click 'Close this
Basket.'

Close Basket
|image835|

You will be asked if you want to 'Attach this basket to a new basket
group with the same name'. A basket group is necessary if you want to be
able to print PDFs of your orders.

Create Purchase Order
|image836|

Your completed order will be listed on the Basket Grouping page for
printing or further modification.

Basket Grouping
|image837|

If you closed the basket before generating the EDIFACT order you can do
so from the basket grouping page.

Basket Grouping EDIFACT

Clicking the 'Print' button next to your order will generate a PDF for
printing, which will have all of your library information followed by
the items in your order.

Order found on PDF
|image838|

`Receiving Orders <#receiveacqorder>`__
---------------------------------------

    **Important**

    You must close the basket to be able to `receive
    items <#receiveacqorder>`__ when they arrive. Only items in closed
    baskets will show as ready to receive.

Orders can be received from the vendor information page

Receive from Vendor Information
|image839|

or the vendor search results page

Vendor Search Results
|image840|

After clicking 'Receive shipment' you will be asked to enter a vendor
invoice number, a shipment received date, a shipping cost and a budget
to subtract that shipping amount from.

Receive Shipment
|image841|

The receive page will list all items still on order with the vendor
regardless of the basket the item is from.

Receipt Summary
|image842|

To receive a specific item, click the 'Receive' link to the right of the
item.

Receive Item Form
|image843|

From this form you can alter the cost information. You can also choose
to mark only part of the order as received if the vendor didn't send
your entire order by checking only the boxes next to the items on the
left that you want to receive. The values you enter in the 'Replacement
cost' and 'Actual cost' will automatically populate the item record by
filling in subfield v (Cost, replacement price) and subfield g (Cost,
normal purchase price) on the item record after saving.

Item record after receipt
|image844|

You can also make edits to the item record from this form by clicking
the 'Edit' link next to each item. This will allow you to enter in
accurate call numbers and barcodes if you'd like to do that at the point
of receipt. Once you have made any changes necessary (to the order
and/or items, click 'Save' to mark the item(s) as received.

    **Note**

    If you have your
    `AcqItemSetSubfieldsWhenReceived <#AcqItemSetSubfieldsWhenReceived>`__
    preference set to add or change values on received items those
    changes will take place after you hit 'Save'.

Already Received Items
|image845|

If the item is no longer available from this vendor you can transfer the
order to another vendor's basket by clicking the 'Transfer' link to the
right of the title. This will pop up a vendor search box.

Transfer search
|image846|

From the results you can click 'Choose' to the right of the vendor you
would like to reorder this item from.

Transfer vendor
|image847|

You will then be presented with the open baskets for that vendor to
choose from. To move the item simply click 'Choose' to the right of the
basket you would like to add the item to.

Basket choice
|image848|

Once you have chosen you will be presented with a confirmation message.

Confirm transfer
|image849|

When you're finished receiving items you can navigate away from this
page or click the 'Finish receiving' button at the bottom of the screen.

If the item cannot be found anywhere you can cancel the order by
clicking 'Delete order' to the far right. This will prompt you to enter
your reason and confirm cancellation.Cancel order

You will also see that the item is received and/or cancelled if you view
the basket.

One item marked (rcvd) in basket
|image850|

`Invoices <#acqinvoices>`__
---------------------------

When orders are received invoices are generated. Invoices can be
searched by clicking on 'Invoices' in the left of the Acquisitions page.

Invoices page
|image851|

After searching, your results will appear to the right of the search
options.

Invoice search results
|image852|

From the results you can click the 'Details' link to see the full
invoice or 'Close' to note that the invoice is closed/paid for.

Invoice details
|image853|

If you're allowing the uploading of acquisitions files with the
`AcqEnableFiles <#AcqEnableFiles>`__ preference you will see the option
to manage invoice files next to the link to 'Go to receipt
page'AcqEnableFiles

To see or attach new files click the 'Manage invoice files' link

No invoice files
|image854|

From here you can find a file to upload and/or see the files you have
already attached.

Invoice files
|image855|

From the invoice search results you can also merge together two invoices
should you need to. Simply click the checkbox to the left of the
invoices you would like to merge and click the 'Merge selected invoices'
button at the bottom of the page. You will be presented with a
confirmation screen:

Merge invoices
|image856|

Click on the row of the invoice number you would like to keep and it
will be highlighted in yellow. Enter any different billing information
in the fields provided and click 'Merge'. The two invoices will become
one.

`Claims & Late Orders <#acqclaims>`__
-------------------------------------

If you have entered in an email address for the vendors in your system
you can send them claim emails when an order is late. Before you can
send claims you will need to set up an `acquisitions claim
notice <#ACQCLAIM>`__.

Upon clicking on the link to 'Late Orders' from the Acquisitions page
you will be presented with a series of filter options on the left hand
side. These filters will be applied only closed baskets.

Acquisitions Late Order Filters
|image857|

    **Note**

    The vendor pull down only shows vendors with closed baskets that are
    late.

Once you filter your orders to show you the things you consider to be
late you will be presented with a list of these items.

Late Orders
|image858|

To the right of each late title you will be see a checkbox. Check off
the ones you want a claim letter sent to and click 'Claim Order' at the
bottom right of the list. This will automatically send an email to the
vendor at the email address you have on file.

    **Note**

    The Estimated Delivery Date is based on the Delivery time value
    entered on the vendor record.

If you would rather use a different acquisition claim letter (other than
the default) you can `create that in the notices module <#addnotices>`__
and choose it from the menu above the list of late items.

Choose a Claim Letter
|image859|

`Acquisition Searches <#acqsearch>`__
-------------------------------------

At the top of the various Acquisition pages there is a quick search box
where you can perform either a Vendor Search or an Order Search.

Acquisition Searches
|image860|

In the Vendor Search you can enter any part of the vendor name to get
results.

Vendor Search Results
|image861|

Using the Orders Search you can search for items that have been ordered
with or without the vendor.

Order Search Box
|image862|

You can enter info in one or both fields and you can enter any part of
the title and/or vendor name.

Order Search Results
|image863|

Clicking the plus sign to the right of the Vendor search box will expand
the search and allow you to search for additional fields.

Expanded Orders Search
|image864|

Clicking Advanced Search to the right of the search button will give you
all of the order search options available.

Full Order Search
|image865|

`Budget/Fund Tracking <#fundtracking>`__
----------------------------------------

On the main acquisitions page there will be a table showing you all of
your active funds and a breakdown of what has been ordered or spent
against them.

Fund Table
|image866|

Clicking on the linked amounts under spent or ordered will show you a
summary of the titles ordered/received on that budget.

Titles Spent
|image867|

`EDI Process <#ediprocess>`__
-----------------------------

Previous sections explain all ordering options, this section pulls out
the parts related to EDI or EDIFACT ordering to help those who are only
using EDI for ordering.

    **Important**

    Koha uses the EDIFACT standard not the X12 standard for electronic
    ordering.

`EDI Questions for Vendors <#ediquestions>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You will want to gather the following information from your vendors
before beginning the set up process in Koha.

**EDI Accounts:** *This is the basic connection information for your
vendor. This will be used to fill in the `EDI Accounts <#ediaccounts>`__
section.*

-  **Vendor:** The name of the vendor

-  **Description:** A short description if additional explanation is
   needed ( especially if you have multiple accounts for one vendor ).

-  **Transport:** Does the vendor transmit EDI files via FTP, SFTP, or
   something else the requires special processing?

-  **Remote host:** The URL or IP address of the FTP/SFTP server

-  **Username:** The username for the above server

-  **Password:** The password for the above server

-  **Download directory:** The path on the server that contains files
   for Koha to download and process

-  **Upload directory:** The path on the server that Koha will upload
   files to for your vendor to process

-  **Qualifier:** Who assigned the SAN below?

   -  Choose one of the following:

      (14) EAN International

      (31B) US SAN Agency

      (91) Assigned by supplier

      (92) Assigned by buyer

-  **SAN:** The identifier for the vendor

   *Buyer qualifier and SAN are optional. Some vendors require a second
   buyer identifier in addition to the account EAN*.

-  **Buyer qualifier:** Who assigned the SAN below?

   -  Choose one of the following:

      (14) EAN International

      (31B) US SAN Agency

      (91) Assigned by supplier

      (92) Assigned by buyer

-  **Buyer SAN:** The identifier for the library

-  **Quotes enabled:** [y/n] Does this vendor support sending and
   receiving quotes via EDIfact and do you want to send and receive
   quotes via EDIfact?

-  **Orders enabled:** [y/n] Does this vendor support sending and
   receiving orders via EDIfact and do you want to send and receive
   orders via EDIfact?

-  **Invoices enabled:**\ [y/n] Does this vendor support sending and
   receiving invoices via EDIfact and do you want to send and receive
   invoices via EDIfact?

-  **Order file suffix:** The file suffix for order files

-  **Quote file suffix:** The file suffix for quote files

-  **Invoice file suffix:** The file suffix for invoice files

-  **Account number(s):** (list them all)

-  **Account description(s):** (the summary of what this number is for)

**EANs:** *Each library using EDIfact needs to specify a buyer
identifier know as a SAN or EAN. This will fill in the `Library
EANs <#libraryeans>`__ setting.*

-  **Library**

-  **EAN**

   -  Choose one of the following:

      (14) EAN International

      (31B) US SAN Agency

      (91) Assigned by supplier

      (92) Assigned by buyer

**MARC Order Fields or Grid Ordering:** *These values will fill in the
`MarcFieldsToOrder <#MarcFieldsToOrder>`__ preference.*

-  **price:** MARC21 field that contains the item price

-  **quantity:** MARC21 field that contains the number of items for the
   given record

-  **budget\_code:** MARC21 field that contains the Koha budget code to
   be debited

-  **discount:** MARC21 field the contains the discount as a percentage
   the the price will be discounted by

-  **sort2:** MARC21 field that will populate custom field sort1

-  **sort2:** MARC21 field that will populate custom field sort2

`EDI Setup <#edisetup>`__
~~~~~~~~~~~~~~~~~~~~~~~~~

Before you begin ordering using EDI you will want to take the following
steps:

-  Ask your vendor/bookseller/jobber for `connection
   information <#ediquestions>`__

   -  It might also be beneficial to ask for a few sample EDIFACT files
      from the vendor

-  Share with your vendor/bookseller/jobber your `library
   codes <#libsgroups>`__, `item type codes <#itemtypeadmin>`__, `fund
   codes <#funds>`__, and any other codes or `authorized
   values <#authorizedvalues>`__ they might need for creating your MARC
   order records

-  Communicate with your support provider or the community about whether
   you will need a plugin based on your vendor's answers

   -  For example ByWater Solutions has published plugins for specific
      vendors here:
      https://github.com/bywatersolutions/koha-plugin-edifact-enhanced

-  `Enter the vendor/bookseller/jobber <#addacqvendor>`__ in
   Acquisitions

-  Review your `Acquisitions system preferences <#acqprefs>`__

   -  Be sure to fill in the `MarcFieldsToOrder <#MarcFieldsToOrder>`__
      preference with values for order files

-  Enter your `EDI Accounts <#ediaccounts>`__

-  Enter your `Library EANs <#libraryeans>`__

-  Turn on the `EDI Cron <#edicron>`__ so that it can process files

`EDI Ordering <#ediordering>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The first step in ordering using EDI happens on the book vendor's
website. Each seller will use different language, but you will need to
place your order on their site and then download the MARC order file.
Some language that you might see included "basket", "order", "cart",
and/or "MARC order." Once you have this MARC file downloaded to your
computer you will want to log in to Koha and continue the process there.

Visit the `Stage MARC Records for Import <#stagemarc>`__ tool and upload
your file. Once presented with the confirmation screen proceed to
Acquisitions.

In Acquisitions `create a basket <#createacqbasket>`__ for the vendor
you ordered from. From the basket, choose to `order from a staged
file <#orderfromstagedfile>`__ and click 'Order' next to the file you
downloaded from your vendor and staged in Koha.

From the confirmation screen you will see all of the data in the MARC
file related to your order. If you are not seeing fields such as fund
and quanity filled in then be sure to confirm that your
`MarcFieldsToOrder <#MarcFieldsToOrder>`__ preference is set right.

Once you have added all of the items to the basket you can click the
'Create EDIFACT order' button.

EDIFACT Order

This will generate a pending file in the `EDIFACT
Messages <#edifactmsg>`__ in Koha. The pending files will be processed
by the `EDI Cron Job <#edicron>`__ and sent to your vendor.

`EDI Invoicing <#ediinvoice>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When the book vendor is done processing your files they will send an
invoice via EDI as well. The `EDI Cron Job <#edicron>`__ will grab
invoices and mark items found in the invoice as received and update your
funds without any need for manual intervention.

`EDIFACT Messages <#edifactmsg>`__
----------------------------------

A log of all messages sent and received via EDIFACT can be found under
EDIFACT Messages. EDIFACT Messages
