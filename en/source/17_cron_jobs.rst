.. include:: images.rst


Cron Jobs
=========

A cron job is a Linux command for scheduling a command or script on your
server to complete repetitive tasks automatically. Scripts executed as a
cron job are typically used to modify files or databases; however, they
can perform other tasks that do not modify data on the server, like
sending out email notifications. Koha has many cron jobs in place that
you can enable (search engine indexing, overdue notice generation, data
cleanup and more), this chapter will explain those for you.

`Cron Jobs <#cronjobs>`__
-------------------------

The locations below assume a dev install which puts the crons in misc/,
if you have a standard install you may want to look in bin/ for these
files if you cannot find them in misc/

`Search <#searchcron>`__
~~~~~~~~~~~~~~~~~~~~~~~~

`Rebuild Index <#rebuildsearchcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/migration\_tools/rebuild\_zebra.pl

Does: Updates Zebra indexes with recently changed data.

Required by: Zebra

Frequency suggestion: every x minutes, (between 5-15 minutes) depending
on performance needs

`Circulation <#circcron>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~

`Holds Queue <#buildholdscron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/holds/build\_holds\_queue.pl

Does: Updates holds queue report

Required by: `Holds Queue Report <#holdsqueue>`__

Frequency suggestion: every 1-4 hours

Description:

-  A script that should be run periodically if your library system
   allows borrowers to place on-shelf holds. This script decides which
   library should be responsible for fulfilling a given hold request.

   It's behavior is controlled by the system preferences
   `StaticHoldsQueueWeight <#holdqueueweight>`__ and
   `RandomizeHoldsQueueWeight <#holdqueueweight>`__.

   If you do not want all of your libraries to participate in the
   on-shelf holds fulfillment process, you should list the the libraries
   that \*do\* participate in the process here by inputting all the
   participating library's branchcodes, separated by commas ( e.g.
   "MPL,CPL,SPL,BML" etc. ).

   By default, the holds queue will be generated such that the system
   will first attempt to hold fulfillment using items already at the
   pickup library if possible. If there are no items available at the
   pickup library to fill a hold, build\_holds\_queue.pl will then use
   the list of libraries defined in StaticHoldsQueueWeight. If
   RandomizeHoldsQueueWeight is disabled ( which it is by default ), the
   script will assign fulfillment requests in the order the branches are
   placed in the StaticHoldsQueueWeight system preference.

   For example, if your system has three libraries, of varying sizes (
   small, medium and large ) and you want the burden of holds
   fulfillment to be on larger libraries before smaller libraries, you
   would want StaticHoldsQueueWeight to look something like
   "LRG,MED,SML".

   If you want the burden of holds fulfillment to be spread out equally
   throughout your library system, simply enable
   RandomizeHoldsQueueWeight. When this system preference is enabled,
   the order in which libraries will be requested to fulfill an on-shelf
   hold will be randomized each time the list is regenerated.

   Leaving StaticHoldsQueueWeight empty is contraindicated at this time.
   Doing so will cause the build\_holds\_queue script to ignore
   RandomizeHoldsQueueWeight, causing the script to request hold
   fulfillment not randomly, but by alphabetical order.

`Perl Documentation <#buildholdscronperl>`__
''''''''''''''''''''''''''''''''''''''''''''

**GetBibsWithPendingHoldRequests**

my $biblionumber\_aref = GetBibsWithPendingHoldRequests();

Return an arrayref of the biblionumbers of all bibs that have one or
more unfilled hold requests.

**GetPendingHoldRequestsForBib**

my $requests = GetPendingHoldRequestsForBib($biblionumber);

Returns an arrayref of hashrefs to pending, unfilled hold requests on

the bib identified by $biblionumber. The following keys are present in
each hashref:

-  biblionumber

-  borrowernumber

-  itemnumber

-  priority

-  branchcode

-  reservedate

-  reservenotes

-  borrowerbranch

The arrayref is sorted in order of increasing priority.

**GetItemsAvailableToFillHoldRequestsForBib**

my $available\_items =

GetItemsAvailableToFillHoldRequestsForBib($biblionumber);

Returns an arrayref of items available to fill hold requests for the bib
identified by $biblionumber. An item is available to fill a hold

request if and only if:

-  it is not on loan

-  it is not withdrawn

-  it is not marked notforloan

-  it is not currently in transit

-  it is not lost

-  it is not sitting on the hold shelf

**MapItemsToHoldRequests**

MapItemsToHoldRequests($hold\_requests, $available\_items);

**CreatePickListFromItemMap**

**AddToHoldTargetMap**

**\_get\_branches\_to\_pull\_from**

Query system preferences to get ordered list of branches to use to fill
hold requests.

`Expired Holds <#expiredholdscron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/holds/cancel\_expired\_holds.pl

Does: By default, this cron job will only automatically cancel holds
where the user has set an expiration date. If the library is using the
`ExpireReservesMaxPickUpDelay <#ExpireReservesMaxPickUpDelay>`__ and
`ExpireReservesMaxPickUpDelayCharge <#ExpireReservesMaxPickUpDelayCharge>`__
preferences then this script will also cancel holds that have been
sitting on the hold shelf for too long and will (if the library does)
charge the patron for not picking up the hold.

Frequency suggestion: daily

`Unsuspend Holds <#unsuspendholdcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/holds/auto\_unsuspend\_holds.pl

Does: This script checks to find holds that should no longer be
suspended and removes the suspension if the
`AutoResumeSuspendedHolds <#AutoResumeSuspendedHolds>`__ preference is
set to 'allow'. This puts the patron back in to the queue where they
were when the hold was suspended.

Frequency suggestion: daily

`Fines <#finescronjob>`__
^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/fines.pl

Does: Calculates and posts fines to patron accounts.

Required by: `finesMode <#finesMode>`__ system preference

Frequency suggestion: nightly

`Long Overdues <#longoverduecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/longoverdue.pl

Does: allows one to specify delays for changing items to different lost
statuses, and optionally charge for them using the replacement price
listed on the item record.

Frequency suggestion: nightly

    **Note**

    Staff can control some of the parameters for the longoverdue cron
    job with the
    `DefaultLongOverdueLostValue <#DefaultLongOverdueLostValue>`__ and
    `DefaultLongOverdueChargeValue <#DefaultLongOverdueChargeValue>`__
    preferences.

`Perl Documentation <#longoverduecronperl>`__
'''''''''''''''''''''''''''''''''''''''''''''

**NAME**

longoverdue.pl cron script to set lost statuses on overdue materials.
Execute without options for help.

**DESCRIPTION**

--lost \| -l

This option takes the form of n=lv, where n is num days overdue, and lv
is the lost value. See warning below.

--charge \| -c

This specifies what lost value triggers Koha to charge the account for
the lost item. Replacement costs are not charged if this is not
specified.

--verbose \| v

verbose.

--confirm

confirm. without this option, the script will report the number of
affected items and return without modifying any records.

--quiet

suppress summary output.

--maxdays

Specifies the end of the range of overdue days to deal with (defaults to
366). This value is universal to all lost num days overdue passed.

--mark-returned

When an item is marked lost, remove it from the borrowers issued items.

`Track total checkouts <#updateissuescron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/update\_totalissues.pl

Does: updates the biblioitems.totalissues field in the database with the
latest tally of checkouts.

Frequency suggestion: nightly

`Perl Documentation <#updateissuesperldoc>`__
'''''''''''''''''''''''''''''''''''''''''''''

**NAME**

update\_totalissues.pl

**SYNOPSIS**

update\_totalissues.pl --use-stats

update\_totalissues.pl --use-items

update\_totalissues.pl --commit=1000

update\_totalissues.pl --since='2012-01-01'

update\_totalissues.pl --interval=30d

**DESCRIPTION**

This batch job populates bibliographic records' total issues count based
on historical issue statistics.

--help Prints this help

-v\|--verbose

Provide verbose log information (list every bib modified).

--use-stats

Use the data in the statistics table for populating total issues.

--use-items

Use items.issues data for populating total issues. Note that issues data
from the items table does not respect the --since or --interval options,
by definition. Also note that if both --use-stats and --use-items are
specified, the count of biblios processed will be misleading.

-s\|--since=DATE

Only process issues recorded in the statistics table since DATE.

-i\|--interval=S

Only process issues recorded in the statistics table in the last N units
of time. The interval should consist of a number with a one- letter unit
suffix. The valid suffixes are h (hours), d (days), w (weeks), m
(months), and y (years). The default unit is days.

--incremental

Add the number of issues found in the statistics table to the existing
total issues count. Intended so that this script can be used as a cron
job to update popularity information during low-usage periods. If
neither --since or --interval are specified, incremental mode will
default to processing the last twenty-four hours.

--commit=N

Commit the results to the database after every N records are processed.

--test

Only test the popularity population script.

**WARNING**

If the time on your database server does not match the time on your Koha
server you will need to take that into account, and probably use the
--since argument instead of the --interval argument for incremental
updating.

**CREDITS**

This patch to Koha was sponsored by the Arcadia Public Library and the
Arcadia Public Library Foundation in honor of Jackie Faust-Moreno, late
director of the Arcadia Public Library.

**AUTHOR**

Jared Camins-Esakov <jcamins AT cpbibliography DOT com>

`Generate Patron File for Offline Circulation <#offlinecirccron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/create\_koc\_db.pl

Does: Generates the borrowers.db file for use with the `Koha Offline
Circulation <#windowsofflinecirc>`__ tool

Frequency suggestion: weekly

`Automatic renewal <#autorenewcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/automatic\_renewals.pl

Does: Will automatically renew items if you're allowing automatic
renewal with your `circulation and fine rules <#circfinerules>`__.

Frequency suggestion: nightly

`Perl Documentation <#autorenewcronperl>`__
'''''''''''''''''''''''''''''''''''''''''''

**NAME**

automatic\_renewals.pl - cron script to renew loans

**SYNOPSIS**

./automatic\_renewals.pl

or, in crontab: 0 3 \* \* \* automatic\_renewals.pl

**DESCRIPTION**

This script searches for issues scheduled for automatic renewal
(issues.auto\_renew). If there are still renews left (Renewals allowed)
and the renewal isn't premature (No Renewal before) the issue is
renewed.

**OPTIONS**

No options.

`Patrons <#patroncrons>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~

`Unverified Registrations <#deleteexpiredregistrationcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/delete\_expired\_opac\_registrations.pl

Does: Deletes patron registrations that were submitted via the OPAC but
not reviewed by the library within the number of days entered in the
`PatronSelfRegistrationExpireTemporaryAccountsDelay <#PatronSelfRegistrationExpireTemporaryAccountsDelay>`__
preference.

Frequency suggestion: nightly

`Unconfirmed Registrations <#deleteunverifiedcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/delete\_unverified\_opac\_registrations.pl

Does: Deletes patron self registrations that were submitted via the OPAC
but not confirmed via email within 24 hours. This is only necessary if
you are requiring patrons to confirm their registrations via email with
the
`PatronSelfRegistrationVerifyByEmail <#PatronSelfRegistrationVerifyByEmail>`__
preference.

Frequency suggestion: hourly

`Anonymize Patron Data <#anonymizecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/batch\_anonymise.pl

Does: Used to anonymize patron data. This will remove borrowernumbers
from circulation history so that the stats are kept, but the patron
information is removed for privacy reasons.

`Update Child to Adult Patron Type <#j2acron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/j2a.pl

Does: Convert juvenile/child patrons from juvenile patron category and
category code to corresponding adult patron category and category code
when they reach the upper age limit defined in the Patron Categories.

Frequency suggestion: nightly

`Perl Documentation <#j2aperldoc>`__
''''''''''''''''''''''''''''''''''''

**NAME**

juv2adult.pl - convert juvenile/child patrons from juvenile patron
category and category code to corresponding adult patron category and
category code when they reach the upper age limit defined in the Patron
Categories.

**SYNOPSIS**

juv2adult.pl [ -b=<branchcode> -f=<categorycode> -t=<categorycode> ]

Options:

--help brief help message

--man full documentation

-v verbose mode

-n take no action, display only

-b <branchname> only deal with patrons from this library/branch

-f <categorycode> change patron category from this category

-t <categorycode> change patron category to this category

=head1 OPTIONS

--help Print a brief help message and exits.

--man Prints the manual page and exits.

-v Verbose. Without this flag set, only fatal errors are reported.

-n No Action. With this flag set, script will report changes but not
actually execute them on the database.

-b changes patrons for one specific branch. Use the value in the
branches.branchcode table.

-f \*required\* defines the juvenile category to update. Expects the
code from categories.categorycode.

-t \*required\* defines the category juvenile patrons will be converted
to. Expects the code from categories.categorycode.

**DESCRIPTION**

This script is designed to update patrons from juvenile to adult patron
types, remove the guarantor, and update their category codes
appropriately when they reach the upper age limit defined in the Patron
Categories.

**USAGE EXAMPLES**

"juv2adult.pl" - Suggests that you read this help. :)

"juv2adult.pl" -b=<branchcode> -f=<categorycode> -t=<categorycode> -
Processes a single branch, and updates the patron categories from
fromcat to tocat.

"juv2adult.pl" -f=<categorycode> -t=<categorycode> -v -n - Processes all
branches, shows all messages, and reports the patrons who would be
affected. Takes no action on the database.

`Notices <#noticescron>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~

`Message Queue <#msgqueuecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/process\_message\_queue.pl

Does: processes the message queue to send the check out, check in and
hold filled emails and SMS message to users and sends outgoing emails to
patrons. requires
`EnhancedMessagingPreferences <#EnhancedMessagingPreferences>`__ to be
on

Frequency suggestion: 1-4 hours

    **Important**

    Item due and Advanced due notices are controlled by the
    `advance\_notices cron <#advnoticecron>`__.

`Advanced Notice <#advnoticecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/advance\_notices.pl

Does: prepares "pre-due" notices and "item due" notices for patrons who
request them prepares notices for patrons for items just due or coming
due soon. requires
`EnhancedMessagingPreferences <#EnhancedMessagingPreferences>`__ to be
on

Frequency suggestion: nightly

    **Note**

    This script does not actually send the notices. It queues them in
    the `message queue <#msgqueuecron>`__ for later

`Perl Documentation <#advnoticecronperl>`__
'''''''''''''''''''''''''''''''''''''''''''

**NAME**

advance\_notices.pl - cron script to put item due reminders into message
queue

**SYNOPSIS**

./advance\_notices.pl -c

or, in crontab: 0 1 \* \* \* advance\_notices.pl -c

**DESCRIPTION**

This script prepares pre-due and item due reminders to be sent to
patrons. It queues them in the message queue, which is processed by the
process\_message\_queue.pl cronjob. The type and timing of the messages
can be configured by the patrons in their "My Alerts" tab in the OPAC.

**METHODS**

parse\_letter

`Overdue Notice <#overduenoticecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/overdue\_notices.pl

Does: prepares messages to alert patrons of overdue messages (both via
email and print)

Frequency suggestion: nightly

    **Note**

    This script does not actually send the notices. It queues them in
    the `message queue <#msgqueuecron>`__ for later or generates the
    HTML for later printing

`Perl Documentation <#overduenoticecronperl>`__
'''''''''''''''''''''''''''''''''''''''''''''''

**NAME**

overdue\_notices.pl - prepare messages to be sent to patrons for overdue
items

**SYNOPSIS**

overdue\_notices.pl [ -n ] [ -library <branchcode> ] [ -library
<branchcode>...] [ -max <number of days> ] [ -csv [ <filename> ] ]
[-itemscontent <field list> ]

Options:

-help brief help message

-man full documentation

-n No email will be sent

-max <days> maximum days overdue to deal with

-library <branchname> only deal with overdues from this library
(repeatable : several libraries can be given)

-csv <filename> populate CSV file

-html <filename> Output html to file

-itemscontent <list of fields> item information in templates

-borcat <categorycode> category code that must be included

-borcatout <categorycode> category code that must be excluded

**OPTIONS**

-help Print a brief help message and exits.

-man Prints the manual page and exits.

-v Verbose. Without this flag set, only fatal errors are reported.

-n Do not send any email. Overdue notices that would have been sent to
the patrons or to the admin are printed to standard out. CSV data (if
the -csv flag is set) is written to standard out or to any csv filename
given.

-max Items older than max days are assumed to be handled somewhere else,
probably the longoverdues.pl script. They are therefore ignored by this
program. No notices are sent for them, and they are not added to any CSV
files. Defaults to 90 to match longoverdues.pl.

-library

select overdues for one specific library. Use the value in the
branches.branchcode table. This option can be repeated in order to
select overdues for a group of libraries.

-csv Produces CSV data. if -n (no mail) flag is set, then this CSV data
is sent to standard out or to a filename if provided. Otherwise, only
overdues that could not be emailed are sent in CSV format to the admin.

-itemscontent

comma separated list of fields that get substituted into templates in
places of the <<items.content>> placeholder. This defaults to
issuedate,title,barcode,author

Other possible values come from fields in the biblios, items, and issues
tables.

-borcat Repetable field, that permit to select only few of patrons
categories.

-borcatout

Repetable field, permis to exclude some patrons categories.

-t \| --triggered

This option causes a notice to be generated if and only if an item is
overdue by the number of days defined in a notice trigger.

By default, a notice is sent each time the script runs, which is
suitable for less frequent run cron script, but requires syncing notice
triggers with the cron schedule to ensure proper behavior. Add the
--triggered option for daily cron, at the risk of no notice being
generated if the cron fails to run on time.

-list-all

Default items.content lists only those items that fall in the range of
the currently processing notice. Choose list-all to include all overdue
items in the list (limited by -max setting).

**DESCRIPTION**

This script is designed to alert patrons and administrators of overdue
items.

Configuration

This script pays attention to the overdue notice configuration performed
in the "Overdue notice/status triggers" section of the "Tools" area of
the staff interface to Koha. There, you can choose which letter
templates are sent out after a configurable number of days to patrons of
each library. More information about the use of this section of Koha is
available in the Koha manual.

The templates used to craft the emails are defined in the "Tools:
Notices" section of the staff interface to Koha.

Outgoing emails

Typically, messages are prepared for each patron with overdue items.
Messages for whom there is no email address on file are collected and
sent as attachments in a single email to each library administrator, or
if that is not set, then to the email address in the
"KohaAdminEmailAddress" system preference.

These emails are staged in the outgoing message queue, as are messages
produced by other features of Koha. This message queue must be processed
regularly by the misc/cronjobs/process\_message\_queue.pl program.

In the event that the "-n" flag is passed to this program, no emails are
sent. Instead, messages are sent on standard output from this program.
They may be redirected to a file if desired.

Templates

Templates can contain variables enclosed in double angle brackets like
<<this>>. Those variables will be replaced with values specific to the
overdue items or relevant patron. Available variables are:

<<bib>>

the name of the library

<<items.content>>

one line for each item, each line containing a tab separated list of
title, author, barcode, issuedate

<<borrowers.\*>>

any field from the borrowers table

<<branches.\*>>

any field from the branches table

CSV output

The "-csv" command line option lets you specify a file to which overdues
data should be output in CSV format.

With the "-n" flag set, data about all overdues is written to the file.
Without that flag, only information about overdues that were unable to
be sent directly to the patrons will be written. In other words, this
CSV file replaces the data that is typically sent to the administrator
email address.

**USAGE EXAMPLES**

"overdue\_notices.pl" - In this most basic usage, with no command line
arguments, all libraries are processed individually, and notices are
prepared for all patrons with overdue items for whom we have email
addresses. Messages for those patrons for whom we have no email address
are sent in a single attachment to the library administrator's email
address, or to the address in the KohaAdminEmailAddress system
preference.

"overdue\_notices.pl -n -csv /tmp/overdues.csv" - sends no email and
populates /tmp/overdues.csv with information about all overdue items.

"overdue\_notices.pl -library MAIN max 14" - prepare notices of overdues
in the last 2 weeks for the MAIN library.

**SEE ALSO**

The misc/cronjobs/advance\_notices.pl program allows you to send
messages to patrons in advance of their items becoming due, or to alert
them of items that have just become due.

**INTERNAL METHODS**

These methods are internal to the operation of overdue\_notices.pl.

parse\_letter

parses the letter template, replacing the placeholders with data
specific to this patron, biblio, or item

named parameters:

letter - required hashref

borrowernumber - required integer

substitute - optional hashref of other key/value pairs that should be
substituted in the letter content

returns the "letter" hashref, with the content updated to reflect the
substituted keys and values.

prepare\_letter\_for\_printing

returns a string of text appropriate for printing in the event that an
overdue notice will not be sent to the patron's email address. Depending
on the desired output format, this may be a CSV string, or a
human-readable representation of the notice.

required parameters:

letter

borrowernumber

optional parameters:

outputformat

`Print Hold Notices <#printholdcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/gather\_print\_notices.pl

Does: looks through the message queue for hold notices that didn't go
through because the patron didn't have an email address and generates a
print notice

Frequency suggestion: nightly

`Talking Tech <#talkingtechcrons>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To learn more about setting up this third party product view the
`Talking Tech Appendix <#talkingtechappendix>`__.

`Sending Notices File <#talkingtechsendcron>`__
'''''''''''''''''''''''''''''''''''''''''''''''

Script path: misc/cronjobs/thirdparty/TalkingTech\_itiva\_outbound.pl

Does: Script to generate Spec C outbound notifications file for Talking
Tech i-tiva phone notification system.

Required by:
`TalkingTechItivaPhoneNotification <#TalkingTechItivaPhoneNotification>`__

Frequency suggestion: nightly

`Perl Documentation <#talkingsendcronperl>`__


**USAGE**

TalkingTech\_itiva\_outbound.pl

TalkingTech\_itiva\_outbound.pl --type=OVERDUE -w 0 -w 2 -w 6
--output=/tmp/talkingtech/outbound.csv

TalkingTech\_itiva\_outbound.pl --type=RESERVE --type=PREOVERDUE
--lang=FR

Script to generate Spec C outbound notifications file for Talking Tech
i-tiva phone notification system.

--help -h

Prints this help

-v Provide verbose log information.

--output -o

Destination for outbound notifications file (CSV format). If no value is
specified, output is dumped to screen.

--lang

Sets the language for all outbound messages. Currently supported values
are EN, FR and ES. If no value is specified, EN will be used by default.

--type

REQUIRED. Sets which messaging types are to be used. Can be given
multiple times, to specify multiple types in a single output file.
Currently supported values are RESERVE, PREOVERDUE and OVERDUE. If no
value is given, this script will not produce any outbound notifications.

--waiting-hold-day -w

OPTIONAL for --type=RESERVE. Sets the days after a hold has been set to
waiting on which to call. Use switch as many times as desired. For
example, passing "-w 0 -w 2 -w 6" will cause calls to be placed on the
day the hold was set to waiting, 2 days after the waiting date, and 6
days after. See example above. If this switch is not used with
--type=RESERVE, calls will be placed every day until the waiting reserve
is picked up or canceled.

--library-code --code -c

OPTIONAL The code of the source library of the message. The library code
is used to group notices together for consortium purposes and apply
library specific settings, such as prompts, to those notices. This field
can be blank if all messages are from a single library.

`Receiving Notices File <#talkingtechreceivecron>`__
''''''''''''''''''''''''''''''''''''''''''''''''''''

Script path: misc/cronjobs/thirdparty/TalkingTech\_itiva\_inbound.pl

Does: Script to process received Results files for Talking Tech i-tiva
phone notification system.

Required by:
`TalkingTechItivaPhoneNotification <#TalkingTechItivaPhoneNotification>`__

Frequency suggestion: nightly

`Perl Documentation <#talkingreceivecronperl>`__


**USAGE**

TalkingTech\_itiva\_inbound.pl

TalkingTech\_itiva\_inbound.pl -v --input=/tmp/talkingtech/results.csv

Script to process received Results files for Talking Tech i-tiva phone
notification system.

--help -h

Prints this help

-v Provide verbose log information.

--input -i

REQUIRED. Path to incoming results file.

`Notify Patrons of Expiration <#patronexpirycron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/membership\_expiry.pl

Does: Sends messages to warn patrons of their card expiration to the
`messaage queue <#msgqueuecron>`__ cron.

Requires: `MembershipExpiryDaysNotice <#MembershipExpiryDaysNotice>`__

Frequency: nightly

`In Processing/Book Cart <#proccartcron>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Script path: misc/cronjobs/cart\_to\_shelf.pl

Does: Updates all items with a location of CART to the item's permanent
location.

Required by: `NewItemsDefaultLocation <#NewItemsDefaultLocation>`__,
`InProcessingToShelvingCart <#InProcessingToShelvingCart>`__, &
`ReturnToShelvingCart <#ReturnToShelvingCart>`__ system preferences

Frequency suggestion: hourly

`Perl Documentation <#proccartcronperl>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**NAME**

cart\_to\_shelf.pl cron script to set items with location of CART to
original shelving location after X hours. Execute without options for
help.

`Catalog <#catalogcron>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~

`Check URLs <#checkurlcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/check-url.pl

Does: checks URLs in 856$u field. Script output can now be formatted in
CSV or HTML. The HTML version links directly to MARC biblio record
editor.

Frequency suggestion: monthly

Learn more: http://wiki.koha-community.org/wiki/Check-url_enhancements

`Perl Documentation <#checkurlcronperl>`__
''''''''''''''''''''''''''''''''''''''''''

**NAME**

C4::URL::Checker - base object for checking URL stored in Koha DB

**SYNOPSIS**

::

    use C4::URL::Checker;

            my $checker = C4::URL::Checker->new( );
            $checker->{ host_default } = 'http://mylib.kohalibrary.com';
            my $checked_urls = $checker->check_biblio( 123 );
            foreach my $url ( @$checked_urls ) {
                print "url:        ", $url->{ url       A }, "\n",
                      "is_success: ", $url->{ is_success }, "\n",
                      "status:     ", $url->{ status     }, "\n";
            }

**FUNCTIONS**

new

Create a URL Checker. The returned object can be used to set default
host variable :

::

    my $checker = C4::URL::Checker->new( );
            $checker->{ host_default } = 'http://mylib.kohalibrary.com';

check\_biblio

Check all URL from a biblio record. Returns a pointer to an array
containing all URLs with checking for each of them.

::

     my $checked_urls = $checker->check_biblio( 123 );

With 2 URLs, the returned array will look like that:

::

         [
               {
                 'url' => 'http://mylib.tamil.fr/img/62265_0055B.JPG',
                 'is_success' => 1,
                 'status' => 'ok'
               },
               {
                 'url' => 'http://mylib.tamil.fr//img/62265_0055C.JPG',
                 'is_success' => 0,
                 'status' => '404 - Page not found'
               }
             ],

**NAME**

check-url.pl - Check URLs from 856$u field.

**USAGE**

check-url.pl [--verbose\|--help] [--host=http://default.tld]

Scan all URLs found in 856$u of bib records and display if resources are
available or not.

**PARAMETERS**

--host=http://default.tld

Server host used when URL doesn't have one, ie doesn't begin with
'http:'. For example, if --host=http://www.mylib.com, then when 856$u
contains 'img/image.jpg', the url checked is:
http://www.mylib.com/image.jpg'.

--verbose\|-v

Outputs both successful and failed URLs.

--html

Formats output in HTML. The result can be redirected to a file
accessible by http. This way, it's possible to link directly to biblio
record in edit mode. With this parameter --host-pro is required.

--host-pro=http://koha-pro.tld

Server host used to link to biblio record editing page.

--help\|-h

Print this help page.

`Update Authorities <#mergeauthcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/migration\_tools/merge\_authority.pl

Does: Updates biblio data with changes to authorities records

    **Note**

    The name of this script is misleading. This script does not merge
    authorities together it instead merges authority data with linked
    bib records. Edits to authority records will be applied to
    bibliographic records that use that authority when this script is
    run.

Required by: `dontmerge <#dontmerge>`__ system preference

Frequency suggestion: nightly

`Serials Update <#serialscron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/serialsUpdate.pl

Does: checks if there is a "late" issue on active subscriptions, and if
there is, the script will set it as late, and add the next one as
expected.

Frequency suggestion: nightly

`Automatic item update <#autoitemupdatecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/automatic\_item\_modification\_by\_age.pl

Does: updates items based on the list of rules set forth in the
`Automatic item modifications by age <#autoitemagemod>`__ tool

Required by: `Automatic item modifications by age <#autoitemagemod>`__

Frequency suggestions: nightly

`Perl Documentation <#autoitemupdatecrondoc>`__
'''''''''''''''''''''''''''''''''''''''''''''''

**NAME**

automatic\_item\_modification\_by\_age.pl

**SYNOPSIS**

./automatic\_item\_modification\_by\_age.pl -h

Toggle recent acquisitions status. Use this script to delete "new"
status for items.

**OPTIONS**

-h\|--help Prints this help message.

-v\|--verbose Set the verbose flag.

-c\|--confirm The script will modify the items.

**AUTHOR**

Jonathan Druart <jonathan.druart@biblibre.com>

**COPYRIGHT**

Copyright 2013 BibLibre

**LICENSE**

This file is part of Koha.

Koha is free software; you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation; either version 3 of the License, or (at your
option) any later version.

Koha is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or
FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for
more details.

You should have received a copy of the GNU General Public License along
with Koha; if not, see <http://www.gnu.org/licenses>.

`OPAC <#opaccrons>`__
~~~~~~~~~~~~~~~~~~~~~

`RSS Feeds <#customrssfeedcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/rss/rss.pl

Does: Produces an RSS XML document for any SQL query (not used for
search results RSS feed). `Learn more <#customrss>`__.

Frequency suggestion: hourly

`Authorities Browser <#authbrowsercron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/build\_browser\_and\_cloud.pl

Does: Generate content for authorities browse in OPAC

Required by: `OpacBrowser <#OpacBrowser>`__ system preference

    **Important**

    This preference and cron job should only be used on French systems.

`Subject/Author Clouds <#keywordclouds>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/cloud-kw.pl

Does: Generates HTML keywords clouds from Koha Zebra indexes.
misc/cronjobs/cloud-sample.conf has a sample of how this script
operates.

Frequency: This is the type of script you can run once a month or so,
the content generated isn't going to change very much over time.

`Perl Documentation <#keywordcloudsperl>`__
'''''''''''''''''''''''''''''''''''''''''''

**NAME**

cloud-kw.pl - Creates HTML keywords clouds from Koha Zebra Indexes

**USAGE**

cloud-kw.pl [--verbose\|--help] --conf=cloud.conf

Creates multiple HTML files containing keywords cloud with top terms
sorted by their logarithmic weight. cloud.conf is a YAML configuration
file driving cloud generation process.

**PARAMETERS**

--conf=configuration file

Specify configuration file name

--verbose\|-v

Enable script verbose mode.

--help\|-h

Print this help page.

**CONFIGURATION**

Configuration file looks like that:

::

      ---
             # Koha configuration file for a specific installation
             # If not present, defaults to KOHA_CONF
             KohaConf: /home/koha/mylibray/etc/koha-conf.xml
             # Zebra index to scan
             ZebraIndex: Author
             # Koha index used to link found kewords with an opac search URL
             KohaIndex: au
             # Number of top keyword to use for the cloud
             Count: 50
             # Include CSS style directives with the cloud
             # This could be used as a model and then CSS directives are
             # put in the appropriate CSS file directly.
             Withcss: Yes
             # HTML file where to output the cloud
             Output: /home/koha/mylibrary/koharoot/koha-tmpl/cloud-author.html
            ---
             KohaConf: /home/koha/yourlibray/etc/koha-conf.xml
             ZebraIndex: Subject
             KohaIndex: su
             Count: 200
             Withcss: no
             Output: /home/koha/yourlibrary/koharoot/koha-tmpl/cloud-subject.html

**IMPROVEMENTS**

Generated top terms have more informations than those outputted from the
time being. Some parameters could be easily added to improve this
script:

WithCount

In order to output terms with the number of occurrences they have been
found in Koha Catalogue by Zebra.

CloudLevels

Number of levels in the cloud. Now 24 levels are hardcoded.

Weighting

Weighting method used to distribute terms in the cloud. We could have
two values: Logarithmic and Linear. Now it's Logarithmic by default.

Order

Now terms are outputted in the lexical order. They could be sorted by
their weight.

`System Administration <#systemcron>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`Clean up Database <#cleandbcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/cleanup\_database.pl

Does: Truncates the sessions table, cleans out old zebraqueue entries,
action logs and staged MARC files.

Perl Documentation
''''''''''''''''''

**USAGE**

kohaclone/misc/cronjobs/cleanup\_database.pl [-h\|--help] [--sessions]
[--sessdays DAYS] [-v\|--verbose] [--zebraqueue DAYS] [-m\|--mail]
[--merged] [--import DAYS] [--logs DAYS] [--searchhistory DAYS]

**PARAMETERS**

-h --help

prints this help message, and exits, ignoring all other options

--sessions

purge the sessions table. If you use this while users are logged into
Koha, they will have to reconnect.

--sessdays DAYS

purge only sessions older than DAYS days

-v --verbose

will cause the script to give you a bit more information about the run.

--zebraqueue DAYS

purge completed zebraqueue entries older than DAYS days. Defaults to 30
days if no days specified.

-m --mail DAYS

purge items from the mail queue that are older than DAYS days. Defaults
to 30 days if no days specified.

--merged

purged completed entries from need\_merge\_authorities.

--import DAYS

purge records from import tables older than DAYS days. Defaults to 60
days if no days specified

--z3950

purge records from import tables that are the result of Z39.50 searches

--logs DAYS

purge entries from action\_logs older than DAYS days. Defaults to 180
days if no days specified

--searchhistory DAYS

purge entries from search\_history older than DAYS days. Defaults to 30
days if no days specified

--list-invites DAYS

purge (unaccepted) list share invites older than DAYS days. Defaults to
14 days if no days specified.

--restrictions DAYS

purge patrons restrictions expired since more than DAYS days. Defaults
to 30 days if no days specified.

`Share Usage Stats <#usagecron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/share\_usage\_with\_koha\_community.pl

Does: If you're sharing information via the `UsageStats <#UsageStats>`__
feature this will send your info to the `Hea
website <http://hea.koha-community.org/>`__.

Frequency: monthly

`Perl Documentation <#usagecronperl>`__
'''''''''''''''''''''''''''''''''''''''

**NAME**

share\_usage\_with\_koha\_community.pl - Share your library's usage with
the Koha community

**SYNOPSIS**

share\_usage\_with\_koha\_community.pl [-h\|--help] [-v\|--verbose]

If the `UsageStats <#UsageStats>`__ system preference is set, you can
launch this script to share your usage data anonymously with the Koha
community.

Collecting Koha usage statistics will help developers to know how Koha
is used across the world.

This script will send the usage data for the bibliographic and authority
records, checkouts, holds, orders, and subscriptions.

Only the total number is retrieved. In no case will private data be
shared!

In order to know which parts of Koha modules are used, this script will
collect some system preference values.

If you want to tell us who you are, you can fill the
`UsageStatsLibraryName <#UsageStatsLibraryName>`__ system preference
with your library name,
`UsageStatsLibraryUrl <#UsageStatsLibraryUrl>`__,
`UsageStatsLibraryType <#UsageStatsLibraryType>`__ and/or
`UsageStatsCountry <#UsageStatsCountry>`__.

All these data will be analyzed on the http://hea.koha-community.org
Koha community website.

IMPORTANT : please do NOT run the cron on the 1st, but on another day.
The idea is to avoid all Koha libraries sending their data at the same
time ! So choose any day between 1 and 28 !

**OPTIONS**

-h\|--help

Print a brief help message

-v\|--verbose

Verbose mode.

-f\|--force

Force the update.

**AUTHOR**

Alex Arnaud <alex.arnaud@biblibre.com>

Jonathan Druart <jonathan.druart@biblibre.com>

**COPYRIGHT**

Copyright 2014 BibLibre

**LICENSE**

This file is part of Koha.

Koha is free software; you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation; either version 3 of the License, or (at your
option) any later version.

You should have received a copy of the GNU General Public License along
with Koha; if not, write to the Free Software Foundation, Inc., 51
Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

`Acquisitions <#acqcrons>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`Clean up old suggestions <#cleansuggcron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/purge\_suggestions.pl

Does: Removes old (defined by you) suggestions from the suggestion
management area.

`Email suggestions to process <#emailsuggestfund>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/notice\_unprocessed\_suggestions.pl

Does: Generates a notice to the fund owner that there are suggestions in
need of processing

`Perl Documentation <#emailsuggestfundperl>`__
''''''''''''''''''''''''''''''''''''''''''''''

**NAME**

notice\_unprocessed\_suggestions.pl - Generate notification for
unprocessed suggestions.

The budget owner will be notified.

The letter template 'TO\_PROCESS' will be used.

**SYNOPSIS**

notice\_unprocessed\_suggestions.pl [-h\|--help] [-v\|--verbose]
[-c\|--confirm] [--days=NUMBER\_OF\_DAYS]

**OPTIONS**

-h\|--help Print a brief help message

-c\|--confirm This flag must be provided in order for the script to
actually generate notices. If it is not supplied, the script will only
report on the patron it would have noticed.

--days This parameter is mandatory. It must contain an integer
representing the number of days elapsed since the last modification of
suggestions to process.

-v\|--verbose Verbose mode.

`EDI Message Processing <#edicron>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Script path: misc/cronjobs/edi\_cron.pl

Does: Sends and received EDI messages

Frequency: Every 15 minutes

`Deprecated scripts <#deprecatedcrons>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These should not be run without modification:

Script path: misc/cronjobs/update\_items.pl

Script path:misc/cronjobs/smsoverdues.pl

Script path:misc/cronjobs/notifyMailsOp.pl

Script path:misc/cronjobs/reservefix.pl

Script path:misc/cronjobs/zebraqueue\_start.pl
