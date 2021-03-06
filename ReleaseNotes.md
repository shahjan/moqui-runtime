
# Moqui Runtime Release Notes

## Release 2.1.0 - Not Yet Released

Moqui Runtime 2.1.0 is a minor new feature and bug fix release, in parallel with the release of Moqui Framework.

This release includes a beta (not yet final) version of a new vuet render mode for applications that uses a Vue JS based single-page 
application shell and supports hybrid client/server HTML rendering. The Vue shell (router, etc) is accessible on /vapps while the 
server rendered HTML is still on /apps. The benefit is a significantly better user experience with existing XML Screens and a
foundation for fully client-rendered screens for dynamic in-browser interactivity. Even with hybrid client/server rendering using
XML Screens page loads are faster and there is less load on the server, along with better error handling and some improved widgets. 

### New Features

- Updated various JS libraries
- JS libraries are now downloaded on build if not present instead of included in the git repo; gradle cleanAll removes them
- Changed accordion in form-single to use Bootstrap instead of jQuery UI
- Changed text-line autocomplete to use Twitter Typeahead instead of jQuery UI
- Removed jQuery UI and theme, saves nearly 300KB in JS/CSS/etc files
- JS/CSS/etc libraries in webroot/lib have been removed and are now downloaded on build into webroot/libs
- Added drop-down and modal to navbar to show ScreenDocument configured documentation
- drop-down now supports dynamic-options plus other options for an initial list and async loaded dynamic list for slower responses
- drop-down.dynamic-options now has server-search and related attributes for server-side filtering using the term in the drop down 
  search box; supports pagination (infinite scroll) if server-side transition supports it
- New online documentation support using the ScreenDocument entity with a question mark icon in the header for screens with attached docs
- New DataDocument based screens to define a data doc through a web UI, view its data, etc; because fields on related entities can be
  added an arbitrary number of hops away this is a much more flexible replacement for the Data View screens 

## Release 2.0.0 - 24 Nov 2016

Moqui Runtime 2.0.0 is a major new feature and bug fix release, in parallel with the release of Moqui Framework.

In this version the CSS and JS libraries used are significantly simplified and refined, along with JS and CSS for cleanups and 
workarounds in widgets and styling (including layout, etc).

XML list forms (form-list) have been improved in various ways to make it a functional reporting tools with various options to
parameterize reports and support light user customization of reports including column layout configuration, saved finds, etc.

There are various new System and Tools app screens for system status details, multi-instance management, service job admin and 
history, seeing active users summary, database snapshots, and query statistics.

### Non Backward Compatible Changes

- The webroot/assets directory and all libraries in it, including all Metis files, have been removed and replaced by updated and 
  more limited JS/CSS libraries in webroot/lib
- All JS/CSS libraries used in the default XML Screen macros are included in moqui-runtime, while all libraries not used in XML 
  Screen macros are moved to other components where they are used (ElFinder, Prism, Swagger UI in the tools component; FullCalendar 
  in SimpleScreens; SimpleMDE and Sparkline in HiveMind)
- If you have explicit references to JS/CSS libraries from the assets directory in your code they will need to be updated to the 
  new location under lib, and some libraries may need to be added to your component that are no longer in moqui-runtime
- Existing ScreenThemeResource records will need to be updated for new locations and removed for files that no longer exist (see 
  the updated WebrootThemeData.xml file)
- The webroot.xml screen no longer adds any JS or CSS files leaving it clean for sub-screens outside of the apps.xml screen; this 
  makes it easier to have other screen trees with very different web artifacts, but means any existing ones that are directly under 
  webroot need to be updated to include scripts and stylesheets as needed (like PopCommerce)
- Export on Auto Screen master and entity Data Export now support a master name to export using master def for entities
- Tenant management screens have been removed, replaced by instance management screens

### New Features

- Updated Bootstrap to 3.3.6, jQuery to 2.2.2, jQuery UI to 1.11.4, Font Awesome to 4.5.0, jquery-form to 3.51.0, 
  jquery-validation to 1.15.0, jsTree to 3.3.0, Moment to 2.12.0
- Removed Metis Admin Template files altogether, going back to plain Bootstrap and jQuery UI as a foundation
- Significant cleanups in JS/CSS libraries, in HTML produced in the default XML Screen macros, and in the webroot CSS files
- Now using Select2 (4.0.2) for dropdowns instead of Chosen
- Now using eonasdan/bootstrap-datetimepicker (4.17.37) instead of smalot/bootstrap-datetimepicker for date and date/time widgets, 
  and for time using simple text-line input with a regular expression to validate
- Split default.css file into webroot-layout.css and webroot-theme.css (with all fonts, colors, etc) to make theming easier
- Using icons instead of +/- characters for order-by links
- form-list pagination control now using Bootstrap pagination component, forward/backward icons, and has more convenient links
- Added password change and enable account to UserAccountDetail in System
- XML Form
  - new form-list attributes
    - @header-dialog to put header-field widgets in a dialog instead of in the header
    - @select-columns to allow users to select which fields are displayed in which columns, or not displayed
    - @saved-finds to allow users to save find parameters and selected columns and rerun the finds with a click
    - @show-csv-button, @show-text-button, and @show-pdf-button to show buttons to render the current screen for the given output
  - dramatically improved test and xsl-fo default output macros that use the @print-width, @print-width-type, and @align attributes
    on the field element to support fixed or percent column widths in output
  - added form-list aggregations using the field.@aggregate element with various functions and an option to display certain fields
    in a sub-list within the main list with the main entries grouped by specified fields
  - field.@show-total adds a row to the bottom of a form list (or sub-list if applicable) with a total for the given field
  - added support for form-list.entity-find element to do a find specifically for the form; when this is used all form fields that
    are actually displayed (using @select-columns option) are selected in addition to any explicitly selected fields in the 
    entity-find; this allows for more flexible reporting as grouping is only by selected fields
  - significant cleanups for form-list and form-single macros
- System App
  - Added detailed system status information to the dashboard (much of it also available in JSON file from /status)
  - Added Multi-Instance Management screens to manage external moqui instances running in a container or virtual machine; these use
    services in Moqui Framework which currently support Docker for instance hosting and MySQL for automatic database provisioning;
    support for other instance hosts and databases will be added over time, and can be added by implementing service interfaces
  - New Service Job screens replace the old Quartz Scheduler screens along with the replacement of Quartz by a new 
    java.util.concurrent based scheduler configured with extended cron strings; these have significantly more options
  - Active Users report shows summary of usage by user over a give time period
- Tools App
  - Added database Snapshot screen to export data for most or all entities directly to a .zip file which can then be downloaded; 
    also supports uploading snapshots to import, or snapshots can be imported into an empty database from the command line
  - Added Query Stats screen with statistics on query timing with sort options to see most frequently run queries, queries that
    cumulatively or on average/etc take the most time to run; this is a great tool for code optimization (finding queries that run 
    more often than is needed and may be cacheable, simplified, etc) and for database optimization as a supplement to statistics
    gathered by the database; this is also a nice tool to find queries run against particular entities to run manually, etc 

### Bug Fixes

- Fixed ElFinder rm (GitHub issue #23), and response for upload; also updated ElFinder in tools component to 2.1.11

## Release 1.6.2 - 26 Mar 2016

Moqui Runtime 1.6.2 is a minor new feature and bug fix release, in parallel
with the release of Moqui Framework.

### New Features

- New Moqui logo!
- Removed example component, now in a separate repository (moqui/example)
- Many unused JS libraries removed (from webroot/assets/lib directory)
- The Tools app can now be run in Tenant instances, with the entities in
  the tenantcommon group excluded from various screens
- The Resource ElFinder screen in the System app now restricts access to
  file and classpath resources for all tenants except DEFAULT
- Added basic tenant admin screens, only accessible in the DEFAULT tenant

## Release 1.6.1 - 24 Jan 2016

Moqui Runtime 1.6.1 is the first release of the runtime repository separate
from the rest of Moqui Framework.

Version numbers of this repository will match those of moqui-framework, the
two are developed in parallel. They are separate to make it easier to use
your runtime directory separate from the rest of the framework (for
configuration, custom UI, etc). The easiest way to do this is generally to
fork from this repository (moqui/moqui-runtime).
