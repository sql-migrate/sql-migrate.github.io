---
---

<h2>Downloads</h2>
<h3>Desktop app</h3>
<h4>
          <a
          onclick="var that=this;ga('send', 'event','Download','macOS 1.1.85',this.href);setTimeout(function(){location.href=that.href;},200);return false;"
          href="https://downloads.postgrescompare.com/builds/PostgresCompare.pkg">Download for macOS</a>
</h4>
<h4>
          <a
          onclick="var that=this;ga('send', 'event','Download','Windows 1.1.85',this.href);setTimeout(function(){location.href=that.href;},200);return false;"
          href="https://d10jucl19ljf5k.cloudfront.net/PostgresCompareSetup.exe">Download for Windows</a>
</h4>
<h4>
          <a
          onclick="var that=this;ga('send', 'event','Download','Linux 1.1.83',this.href);setTimeout(function(){location.href=that.href;},200);return false;" href="https://downloads.postgrescompare.com/builds/PostgresCompare-linux-x64_1.1.83.zip">Download for Linux</a>
</h4>

<h3>Command line</h3>
<h4>
          <a
          onclick="var that=this;ga('send', 'event','Download','macOS 1.1.76 cli',this.href);setTimeout(function(){location.href=that.href;},200);return false;"
          href="https://downloads.postgrescompare.com/builds/cli/mac/1.1.76/pgc">Download for macOS</a>
</h4>
<h4>
          <a
          onclick="var that=this;ga('send', 'event','Download','Windows 1.1.76 cli',this.href);setTimeout(function(){location.href=that.href;},200);return false;"
          href="https://downloads.postgrescompare.com/builds/cli/windows/1.1.76/pgc.exe">Download for Windows</a>
</h4>
<h4>
          <a
          onclick="var that=this;ga('send', 'event','Download','Linux 1.1.79 cli',this.href);setTimeout(function(){location.href=that.href;},200);return false;"
          href="https://downloads.postgrescompare.com/builds/cli/linux/1.1.79/pgc">Download for Linux</a>
</h4>

<h2>Changelog</h2>


<h3>v1.1.85 (September 22nd 2021)</h3>
  - Added comparison of Configuration Parameters

<h3>v1.1.84 (September 13th 2021)</h3>
  - Added history view

<h3>v1.1.83 (August 5th 2021)</h3>
  - Fix comparison of partitions when comparing single schemas

<h3>v1.1.82 (July 31st 2021)</h3>
  - Ignore differing schema names when comparing single schemas

<h3>v1.1.80 (June 20th 2021)</h3>
  - Compare and deploy table partitions
  - Fix bug deploying  timestamp vs timestamp(0) differences

<h3>v1.1.79 (June 13th 2021)</h3>
  - Ensure that adding a column comes before adding a foreign key

<h3>v1.1.78 (June 4th 2021)</h3>
  - Resolve issue comparing domains

<h3>v1.1.76 (June 2nd 2021)</h3>
  - Resolve 'keynotfound' issue with data comparison
  - Resolve 'keynotfound' issue with schema comparison

<h3>v1.1.75 (May 31st 2021)</h3>
  - Performance improvements, filtering by schema

<h3>v1.1.74 (May 2nd 2021)</h3>
  - Index include columns
  - Index column  operator class names
  - Fix issue where an index implementing a fk constraint was being excluded
  - Fix ordering of deployment script when DROP/CREATE a view

<h3>v1.1.71 (April 2nd 2021)</h3>
  - Compare range types
  - Store and recover window state between launches
  - Include extensions when comparing single schemas

<h3>v1.1.69 (March 22nd 2021)</h3>
  - Add sample project option
  - Add ignore default arguments option

<h3>v1.1.68 (March 15th 2021)</h3>
  - Grant/Revoke privileges in deploy script
  - Show script folder progress
  - Add a description for a project

<h3>v1.1.67 (March 8th 2021)</h3>
  - Sort differences with identical last
  - Added edit project button
  - Add view dependency planning

<h3>v1.1.66 (March 1st 2021)</h3>
  - Add remaining object types to scripts folder creation
  - Fix issue comparing check constraint definitions between postgres versions

<h3>v1.1.65 (Feb 22nd 2021)</h3>
  - Added first pass at scripts folder creation

<h3>v1.1.64 (Feb 15th 2021)</h3>
  - Fix data compare issue where no options presented for tables to compare
  - Sort schemas alphabetically when setting up projects

<h3>v1.1.63 (Feb 8th 2021)</h3>
  - Add scripting of owner of sequences, enums, domains and aggregates

<h3>v1.1.61 (Feb 1st 2021)</h3>
  - Add comparison of Aggregates
  - New dependency calculation when generating scripts

<h3>v1.1.60 (Jan 25th 2021)</h3>
  - Add comparison of privileges for functions, views, sequences, schemas, databases, tablespaces.

<h3>v1.1.59 (Jan 21st 2021)</h3>
  - Fixed test connection feedback

<h3>v1.1.58 (Jan 18th 2021)</h3>
  - Improved comparison performance
  - Fixed v12 issue where WHEN clause was missing

<h3>v1.1.57 (Jan 6th 2021)</h3>
  - Add comparison of privileges

<h3>v1.1.52 (Dec 1st 2020)</h3>
  - Fix data compare 'Sequence contains no elements'

<h3>v1.1.51 (Nov 1st 2020)</h3>
  - Fix deployment of CREATE POLICY
  - Added deployment of altered primary key

<h3>v1.1.50 (Oct 12th 2020)</h3>
  - Fix title bar on MacOS
  - Improve query performance

<h3>v1.1.49 (Oct 3rd 2020)</h3>
  - Fix export report/save script
  - DROP POLICY
  - CREATE PUBLICATION

<h3>v1.1.48 (Sept 16th 2020)</h3>
  - Fix frozen UI issue after editing project
  - Add new onboarding

<h3>v1.1.47 (Sept 8th 2020)</h3>
  - Detect differences in the partition by clause.
  - Include command line interface

<h3>v1.1.46 (August 30th 2020)</h3>
  - Fix issus where Citus tables were being ignored
  - UI refactoring

<h3>v1.1.42 (August 8th 2020)</h3>
  - Fix issues comparing data using unique index and when some columns are missing
  - Persist filter on projects list
  - Select first non-identical object when navigating to a comparison

<h3>v1.1.41 (August 3rd 2020)</h3>
  - Add environment variables to control logging/data locations
  - Add comments on Domain, Composite Type and MView

<h3>v1.1.40 (July 19th 2020)</h3>
  - Add CSV report of comparison results
  - Improved keyboard navigation
  - Fix duplicate CHECK keywords when adding check constraints in pg12
  - Add CREATE/DROP of Policies

<h3>v1.1.39 (July 6th 2020)</h3>
  - Fix comparing views where underlying column has changed
  - Fix scripting of Procedure signature change
  - Fix scripting of NOT NULL when column is primary key
  - Fix draft data compare options

<h3>v1.1.38 (June 15th 2020)</h3>
  - Remove Easter Egg
  - Fix missing properties OnUpdate, ReturnsSet

<h3>v1.1.37 (June 4th 2020)</h3>
  - Add Easter Egg

<h3>v1.1.35 (May 31st 2020)</h3>
  - Improve performance of populating comments
  - Fix fetching argument defaults
  - Fix issue where UI can become responsive on click of "view comparison"

<h3>v1.1.34 (May 25th 2020)</h3>
  - Add data compare beta

<h3>v1.1.33 (March 29th 2020)</h3>
  - Fix deploying column comments
  - Refactor options

<h3>v1.1.32 (March 24th 2020)</h3>
  - Deploy precision differences

<h3>v1.1.31 (March 10th 2020)</h3>
  - UI improvements
  - Fix column name escaping in unique constraint
  - Fix expression index parsing

<h3>v1.1.30 (February 24th 2020)</h3>
  - UI improvements
  - Add  'ignore column order' option

<h3>v1.1.28 (January 21st 2020)</h3>
  - Deploy serial datatype changes
  - Support v9.3

<h3>v1.1.27 (January 14th 2020)</h3>
  Fixes:
  - Running comparison restarting

Improvements:

- Deploy identity column differences

<h3>v1.1.26 (January 13th 2020)</h3>
  Fixes:
  - Null exception being thrown from CheckConstraintBuilder
  - Max length missing when altering column datatype
  - COMMENT ON PROCEDURE using incorrect name

Improvements:

- Order columns and comments alphabetically for a cleaner diff
- Deploy new primary key constraint
- Alter deferrable constraint (instead of drop/create)
- Reset scroll when filtering

<h3>v1.1.25 (January 7th 2020)</h3>
  - Fix issue where script was appearing blank

<h3>v1.1.24 (January 5th 2020)</h3>
  - Add ignore case option
  - Fix issue where column storage type was being altered before data type

<h3>v1.1.23 (January 1st 2020)</h3>
  - Add deployment of deleted check, unique and fk constraints
  - Fix issue where inline code comments were not being ignore correctly

<h3>v1.1.22 (December 30th 2019)</h3>
  - Add deployment of altered check and unique constraints
  - Fix issue on Pg < 10 where regexpr_match does not exist

<h3>v1.1.21 (December 26th 2019)</h3>
  - New option to ignore comments in code when comparing
  - Handle triggers on partitions

<h3>v1.1.20 (December 16th 2019)</h3>
  - Include schema name for function return types
  - Include constraints and collations for domains
  - Include schema name for domains

<h3>v1.1.19 (October 28th 2019)</h3>
  - Fixed scripting of precision for time and timestamp columns

<h3>v1.1.18 (October 27th 2019)</h3>
  - Fixed issue where PostgresCompare was adding whitespace to functions

<h3>v1.1.17 (October 24th 2019)</h3>
  - Fixed Language and FunctionVolatility display properties
  - Fixed issue where new indexes were not being deployed
  - Added copy button to script create screen

<h3>v1.1.16 (October 15th 2019)</h3>
  - Add PostgreSQL 12 support

<h3>v1.1.15 (September 30th 2019)</h3>
  - Fix bug where comparing indexes threw 'sequence has no matching elements' exception

<h3>v1.1.14 (September 30th 2019)</h3>
  - Add client ssl certificate support
  - Fix bug where attributes of composite types were not being compared

<h3>v1.1.13 (September 23rd 2019)</h3>
  - Add option to ignore tablespaces
  - Add updated at and number of comparisons to project tile
  - Bug fix: deploy new Materialized View
  - Bug fix: composite type owners not being ignored
  - Bug fix: exception while comparing "failed to find type with id ..."
  - Bug fix: failed comparison tile showed "in progress"
  - Script fixes: partition by, trigger function, partial indexes

<h3>v1.1.12 (September 16th 2019)</h3>
  - Add shared projects

<h3>v1.1.11 (August 4th 2019)</h3>
  - Add ability to copy a project
  - Add DROP and CREATE enum with deleted values
  - Drop Triggers first, functions last

<h3>v1.1.10 (July 28th 2019)</h3>
  - Drop and create views and functions when arguments are changed
  - Order constraints so that they are added after columns

<h3>v1.1.9 (July 25th 2019)</h3>
  - Support indexes on expressions
  - Properly quote column names in RETURNS TABLE functions

<h3>v1.1.8 (July 21st 2019)</h3>
  - Escape passwords correctly
  - Fix issue where deployment script ui would crash

<h3>v1.1.7 (July 18th 2019)</h3>
  - Order constraints alphabetically for a cleaner script
  - Performance improvements when rendering the create SQL

<h3>v1.1.6 (July 11th 2019)</h3>
  - Support adding new foreign keys
  - Support altering table and column comments

<h3>v1.1.5 (July 8th 2019)</h3>
  - Support HTTPS_PROXY and HTTP_PROXY environment variables
  - When using "ignore owner" don't output ALTER OWNER statement
  - Fix incorrectly quoted datatypes in CREATE FUNCTION

<h3>v1.1.4 (July 3rd 2019)</h3>
  - Add support for new unique constraints
  - Fix issue re: arrays as datatypes in UDTs
  - Add split/inline view option

<h3>v1.1.3 (June 30th 2019)</h3>
  - Add deploy of dropped and altered views
  - Exclude sequences, operators, operator families and text search objects that are internal

<h3>v1.1.2 (June 28th 2019)</h3>
  - Fix issue where deployment script would detect a dependency cycle and throw an Exception

<h3>v1.1.1 (June 26th 2019)</h3>
  - Replaced editor component
  - Fix issue creating deployment script when Foreign Tables present
  - Fix "invalid date" issue

<h3>v1.1.0 (June 24th 2019)</h3>
  - Replaced editor component
  - Fixed object filters

<h3>v1.0.55 (June 19th 2019)</h3>
  - Fix comparison of single schemas

<h3>v1.0.54 (June 14th 2019)</h3>
  - Fix parsing of default function arguments

<h3>v1.0.53 (June 10th 2019)</h3>
  - Fix problem scripting Materialized View refresh
  - Fix scripting issues with sequences, types, functions and arrays

<h3>v1.0.52 (June 2nd 2019)</h3>
  - Fix bug setting argument defaults
  - Add resize bar to comparison details
  - Persist default pagesize of differences list

<h3>v1.0.51 (May 29th 2019)</h3>
  - Fix issue where scripting ui was not working when comparing single schemas

<h3>v1.0.50 (May 19th 2019)</h3>
  - Add Materialized View deployment
  - Add Enum deployment

<h3>v1.0.49 (May 12th 2019)</h3>
  - Show spinner when checking for updates
  - Add "Show identical objects" link

<h3>v1.0.48 (May 10th 2019)</h3>
  - Add "Object Types to Compare" project tab

<h3>v1.0.47 (May 5th 2019)</h3>
  - Fix graphs on deployment page
  - Fix recompare after deployment
  - Add filters to script page
  - Start Data Compare beta

<h3>v1.0.46 (April 29th 2019)</h3>
  - Fixed issue scripting precision and scale differences

<h3>v1.0.45 (April 27th 2019)</h3>
  - Added licensing

<h3>v1.0.44-beta (April 2nd 2019)</h3>
  - Added auto-update to the Windows build
  - Signed the Windows executables
  - Now shipping installers for Windows and mac

<h3>v1.0.43-beta (March 24th 2019)</h3>
  - Added friendly names and pie chart to comparison tiles
  - Fixed issue creating script where Snapshot navigation property could not be found
  - Added button to check all connections at once
  - Added button to clear all previous comparisons
  - Aligned colours everywhere to have same meaning 'new, different, dropeed, identical'

<h3>v1.0.42-beta (March 15th 2019)</h3>
  - Added overview tab to comparison view
  - Fixed issue where delete was not cascading correctly
  - Added tablespace scripting to tables
  - Added ability to alter varchar size
  - Allow triggers with same name but different table

<h3>v1.0.41-beta (February 24th 2019)</h3>
  - Added auto-update for macOS
  - Fixed issue where comparison appeared to freeze
  - Ignore exception when reading from sequence user doesn't have access to

<h3>v1.0.40-beta (February 17th 2019)</h3>
  - Deployment screen now highlights errors

<h3>v1.0.39-beta (February 10th 2019)</h3>
  - Add option to ignore object owner
  - Add filter to exclude tables that match a pattern
  - Fix error while querying pg_partitioned_table

<h3>v1.0.38-alpha (February 3rd 2019)</h3>
  - Make comparisons and deployments cancellable
  - Add filter for just different objects

<h3>v1.0.37-alpha (January 23rd 2019)</h3>
  - Fix issue querying pg_partitioned_table pre v10
  - Add detailed and change sql panels

<h3>v1.0.36-alpha (January 20th 2019)</h3>
  - Add option to ignore column order
  - Add option to ignore table partitions

<h3>v1.0.35-alpha (January 8th 2019)</h3>
  - Fix null reference exception when comparing single schemas

<h3>v1.0.34-alpha (January 6th 2019)</h3>
  - Add support for comparing single schemas

<h3>v1.0.33-alpha (December 31st 2018)</h3>
  - Fixed issue where script window would not return with result
  - Fixed issue with reading sequences on versions < 10
  - Added icons to comparison view

<h3>v1.0.32-alpha (December 26th 2018)</h3>
  - Added comparison of access methods, casts, conversions, event triggers, foreign data wrappers, operator families
  - Added comparison of tablespaces, foreign servers, text search parsers, text search templates, text search configs
  - Added comparison of text search dictionaries, user mappings, publications, subscriptions, statistics, policies
  - Added comparison enums, collations, sequences, operators and foreign tables
  - Fixed issue showing UTC time on artifacts instead of local time

<h3>v1.0.31-alpha (November 22nd 2018)</h3>
  - Fix PostgreSQL 9.4

<h3>v1.0.30-alpha (November 19th 2018)</h3>
  - Improve scripting of functions
  - Add comparison of procedures for Pg 11

<h3>v1.0.29-alpha (October 18th 2018)</h3>
  - Added comparison before and after deployment to UI
  - Fix bug where columns were not being compared
  - Fix bug where comparison results were being duplicated in the UI

<h3>v1.0.28-alpha (October 10th 2018)</h3>
  - Added real time comparison results feature

<h3>v1.0.27-alpha (October 1st 2018)</h3>
  - Added option to ignore whitespace differences when comparing functions
  - Added more detailed progress reporting
  - Added deployment of functions
  - Fixed bug where serial columns were considered different due to their default value differing

<h3>v1.0.26-alpha (September 23rd 2018)</h3>
  - Added dropdown selects to filter fields on comparison screen
  - Added "Most Recent" and "Previous" to comparison and deployment list
  - Added alter column datatype, default, statistics, not null, storage
  - Added alter table inherits, owner, comments, cluster
  - Changed "Deployment..." CTAs to "Script.." so it's not so scary :)
  - Improved performance of comparison and deployments list fetching
  - Fixed issue where removing system triggers at snapshot time would cause an exception later when comparing

<h3>v1.0.25-alpha (September 9th 2018)</h3>
  - Added searchable/sortable table to the comparison and deployments screens
  - Fixed bug where self-referencing foreign keys were producing circular dependencies
  - Fixed bug where indexes were being created before the table they depend on
  - Fixed bug where removing extension types for comparison was causing a crash (KeyNotFoundException) when those types were needed for a table definition later

<h3>v1.0.24-alpha (September 3rd 2018)</h3>
  - Performance improvements
  - Added progress reporting to notifications
  - Defaults and placeholders added to new environment and new project forms
  - Removed snapshots tab

<h3>v1.0.23-alpha (August 26th 2018)</h3>
  - Replace tables with cards for environments, projects, comparisons, deployments and snapshots
  - Added schema name to comparison and new script pages
  - Fixed v11 snapshotting due to removed attributes

<h3>v1.0.22-alpha (August 19th 2018)</h3>
  - Added deployment step to new script window
  - Added support for Domains, Extensions and Materialized Views
  - Added ordering script by dependencies

<h3>v1.0.21-alpha (July 8th 2018)</h3>
  - Added new script window

<h3>v1.0.20-alpha (June 17th 2018)</h3>
  - Added editable projects
  - Added navigate via notifications

<h3>v1.0.19-alpha (June 11th 2018)</h3>
  - Added projects
  - Added editable environments

<h3>v1.0.18-alpha (May 17th 2018)</h3>
  - Improved error reporting

<h3>v1.0.17-alpha (May 7th 2018)</h3>
  - Changed: buttons now available alongside each other
  - Fixed: constraint indexes appearing in comparison
  - Changed: improved performance of snapshots and comparison

<h3>v1.0.16-alpha (April 29th 2018)</h3>
  - Added: comparison of Composite Types
  - Fixed: quoted identifiers not being used

<h3>v1.0.15-alpha (April 21st 2018)</h3>
  - Added: comparison of Schemas
  - Fixed: support for PostgreSQL 9.4
  - Fixed: toast tables and indexes appearing in comparison
  - Changed: removed db objects other than connected database

<h3>v1.0.14-alpha (April 17th 2018)</h3>
  - Added: comparison of Indexes

<h3>v1.0.13-alpha (April 15th 2018)</h3>
  - Added: comparison of Roles/Users

<h3>v1.0.12-alpha (April 12th 2018)</h3>
  - Fixed: function comparison failing when trying to compare a function with no arguments

<h3>v1.0.11-alpha (April 12th 2018)</h3>
  - Added: basic Function comparison and snapshotting
  - Changed: excluded pg_catalog procedures from queries for better performance

<h3>v1.0.10-alpha (April 10th 2018)</h3>
  - Fixed: SchemaName throwing an error on script compare
  - Fixed: incorrect default for columns

<h3>v1.0.9-alpha (April 8th 2018)</h3>
  - Fixed: objects being matched from different schemas
  - Fixed: invisible differences due to different schema owner
  - Fixed: ordering of differences
  - Added: muted text for identical objects
  - Added: improved performance for large comparisons

<h3>v1.0.8-alpha (April 5th 2018)</h3>
  - Fixed: 'System.InvalidCastException: Object must implement IConvertible.' error

<h3>v1.0.7-alpha (April 2nd 2018)</h3>
  - Added: Persistence of data across builds
  - Added: macOS build signing
  - Fixed: Triggers appearing in every schema
  - Fixed: 'Can't convert infinite timestamptz values to DateTime' error
  - Fixed: macOS build not saving environments

<h3>v1.0.6-alpha (March 27th 2018)</h3>
  - Added: better scripting of Triggers
  - Added: basic comparison of Triggers
  - Fixed: start comparison from dropdown menu on list of environments

<h3>v1.0.5-alpha (March 26th 2018)</h3>
  - Fixed: attempting to read from pg_authid throwing an error, replaced with pg_roles
  - Added: snapshot and comparison of Views
  - Added: basic snapshot of Triggers
  - Added: ability to report an error via email

<h3>v1.0.4-alpha (March 21st 2018)</h3>
  - Fixed: Internal triggers (to support constraints) causing tables to appear as different

<h3>v1.0.3-alpha (March 19th 2018)</h3>
  - Fixed: Test connection button remains checked after closing New Environment modal
  - Added: Logging of exceptions during snapshot, comparison, script and deploy to file

<h3>v1.0.2-alpha (March 18th 2018)</h3>
  - Fixed: Comparison an snapshot tasks not informing the UI when an error occured.
  - Fixed: Phantom snapshots appearing as artifacts of comparisons.
  - Changed: Updated notifications to show a progress bar for the running task.

<h3>v1.0.1-alpha (March 16th 2018)</h3>
  - Fixed: Comparisons and snapshots seemingly running forever. This was caused by an issue reading pg_catalog.pg_index where indpred was not NULL. The app would throw an exception but not inform the
  ui that the task had failed.

<h3>v1.0.0-alpha (March 14th 2018)</h3>
