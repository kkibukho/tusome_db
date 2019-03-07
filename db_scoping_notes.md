# Tusome M&E Database: Notes on data sources and required capabilities

## Data Sources

The data Tusome generates, collects, and consumes falls broadly into a few categories: entity-based, event-based, and ??

### Entity-based data

The data here is relatively stable. While zones may be split and renamed, or schools may be reassigned, or enrollment may change, these entities and their attributes are fairly stable. Except for the enrollment figures, these data might expect to be updated on a quarterly basis or less frequently than that.

1. _Location Lists_
    + This is the term we use to refer to geographic hierarchies. From the national level down to the school level, the structure is a simple 1:1 nesting.
        * **Counties**: (roughly 50) each of which contains several subcounties
        * **Subcounties**: (roughly 330) each of which contains several zones
        * **Zones/Clusters**: (roughly 1300) each of which contains several schools
        * **Schools**: (roughly 26,000) each of which contains 3 grades, potentially with multiple streams in each grade
    + Each entity in this hierarchy should contain a number of attributes. At a minimum, these would include
        * **unique identifier**: for use within the Tangerine data-collection ecosystem
        * **name**: for use as a human-readable label associated with the unique identifier in reports, summaries, etc.
        * **school type**: one of {public, private, APBET, other} for use in groupings and analyses
        * **rurality**: one of {urban, peri-urban, rural} for use in groupings and analyses
        * **directions** or **nearby location**: this could be the name of a nearby town, landmark, road to take, etc.
        * **latitude** and **longitude**: for schools, not geographic areas
        * **shapefile data**: for geographic areas, not schools
        * **books received**: for grades within schools, split by language/subject (English, Kiswahili, Maths)
        * **SNE status**: one of {mainstream, special unit, sne school} for schools
        * **enrollment**: by sex, for streams or grades within schools
        * **residential status**: for each school, one of {residential, non-residential}. Informs budgeting and logistical planning.
        * **materials drop-off point**: for each school. Usually but not always one per zone. Informs budgeting and logistical planning.
        * **assigned

1. _Lists of Persons_
    + These are primarily reference lists of personnel in whom Tusome has an interest. They would include but not necessarily be limited to
        * **teachers**: mapped to their streams, grades, and schools
        * **CSOs** or **coaches**: mapped to their zones
        * **education officials**: mapped to their geographic area of responsibility and their institution
        * **RTI personnel**: mapped to their geographic area of responsibility and their unit/team
    + Each entity included in one of these lists should contain a number of attributes. At a minimum, these would include
        * **unique identifier**: for use within Tusome's database
        * **employer identifier**: for use when mapping this individual to external partners. These might include, for instance, TSC (teachers' service commission) numbers.
        * **phone number for communication**: the best number at which to reach them
        * **phone number for payment**: linked to Mpesa or Airtel Mobile Money or some such mobile money payment system
        * **common name**: for use in attendance lists, summaries, etc. (e.g., _Tim Slade_, _Mike McKay_)
        * **validated name**: for use in matching with Mpesa or Airtel Mobile Money payment processes/systems (e.g. _Timothy Slade_, _Michael Kay_)
        * **primary role**: for use in analyses and budgeting (e.g. teacher, head teacher, county director of education, subcounty director of education, curriculum support officer)
        * **job group**: for use in budgeting. Not relevant for all entities.
        * **Tusome training role**: for use in analyses and budgeting (e.g. master trainer, CSO trainer, star teacher, etc.). Not relevant for all entities.
        * **grade taught**: for use in analyses and logistical planning. Not relevant for all entities.
        * **tablet possession**: for use in analyses and logistical planning. Not relevant for all entities.
        * **tablet identifier**: for use in logistical planning. Not relevant for all entities.
        * **duty station**: for use in budgeting and logistical planning.

## Activity-based data

These are data that are relatively less permanent. They may vary substantially from one round of activity or season of implementation to another.

1. _Training venues_
    + These are broadly consistent from one round of training to another, but they change at the margins.
    + For each training venue identified, we require at least the following attributes:
        * **catchment area**: the list of zones/schools whose teachers and HTs will be participating in the training at this venue. Most training venues serve the school staff of a single zone, but not all; in some cases zones are combined, and in others they are split. The best mapping of venue::area would therefore be at the school level, but in the majority of cases the schools at the venue could be easily summarized by reference to the zone in which they are located.
        * **materials drop-off point**: training materials will sometimes but not always be dropped off at the venue.
        * **host trainer**: there is typically a CSO who is designated the "host" trainer because the venue is located within their area of responsibility.
        * **secondary trainer**: there is typically a CSO who is designated the "visiting" trainer because their duty station is in a different zone, but they have been assigned to support training at this venue
        * **additional trainer**: there is typically a "star teacher" who serves as an extra set of hands to help lead/guide trainings at the venue. They are usually residents of the training venue's catchment area.
        * **contact information**: the venues are often schools, conference facilities, etc. At a minimum, each venue's point of contact and that POC's phone number should be easily accessible in case plans change.
        * **training dates**: the dates on which a given training took place. e.g., _Dec 20-23_ for _Term 1 2019_, _April 17-19_ for _Term 2 2019_, etc.
        * **estimated participant count**: for budgeting purposes, calculation of attendance rates, etc. This would simply be an aggregation of the personnel counts for the schools listed in the venue's catchment area.
        * **residential status**: for budgeting and _Strawberry_-based payment purposes. This would be one of {residential|non-residential|mixed} depending on the statuses of the schools in the catchment area.

1. _Event participation_
    + This would include but not be restricted to individual-level data.
    + Event-registration data is available through Gooseberry for at least medium-scale, non-teacher-training events (such as workshops with government officials), but some of the metadata regarding event duration, content, purpose, etc. may be less readily available and is definitely less consistent in structure.
    + For teacher-training purposes, much of the required individual-level data is already available through the Gooseberry participant registration system, which stores participant records in JSON format. The need would be to pipe the Gooseberry data into our database to enable analyses of overall treatment "dosage" that incorporate not only coaching data but also training data.
    + These events would include but not be limited to training workshops, materials-development workshops, policy engagement-workshops, sustainability meetings at the regional, county, and subcounty levels, engagements with central Ministry and SAGA officials, etc.
    + For each event, we would require at least the following:
        * **activity name**: to be able to map the database record to other reporting and discussions about the event
        * **workplan identifier**: to be able to map the database record to projected and actual financial outlays for the event, as well as reporting by IR/Sub-IR/Activity
        * **participation ct**: a count of entries on the participant list, disaggregated by sex (pulled in via a merge from the individual participant data). 
            - **N.B.**: for some events, such as cluster meetings, Tusome's practice has been to _not_ require either participant lists or Gooseberry registration. For those categories of events, participant count data may need to be entered directly rather than calculated from the participant list.
        * **participants**: drawn from the Gooseberry registration data, but linked as well to scans of the attendance sheet.
        * **event report**: where appropriate, any minutes, reports, or products generated by the event. These would be binary large objects (BLOBs); if the RDBMS only supports text fields, it could be a field that links to the documents, which could be housed in OneDrive or SharePoint.

I'm not quite sure how to think about it, but it would also be useful to track the dissemination/response to our sharing of the Tangerine Dashboard link to various stakeholders. We would care about recipients, modality of delivery to the recipient, and response/click-through/conversion rate. (Since we don't currently have a good way of gathering the latter, we might want to consider using our free-to-the-user shortcode to poll recipients a week after dissemination.)

## Required joins

Much of the data we care about for analytic purposes lies at the intersection of multiple proposed tables/lists. This is a non-exhaustive list of the kinds of join operations we would want to perform on our data.

1. **\[Role\] assigned to \[location\]**, where the role could be a government official, CSO, etc. and the role could be a county, zone, school, etc. The purpose of this join would be to enable us to rapidly identify points of contact and issue notices via SMS / email / WhatsApp with location-based content; identify and document areas where a staffing gap has emerged, so it can be addressed in discussions with government personnel; etc.
1. **"residential" status of each individual at a training venue based on their associated school** <-- This is relevant because reimbursement rates for training attendance vary as a function of a participant's duty station.
1. **Contact information for trainers at a venue** <-- This is relevant in cases where rapid, large-scale communication needs to be provided, such as updates to plans regarding schedule, content, logistics, registration, etc.
1. **individual demographic data pulled into training participation records by way of individual unique IDs** <-- For the purposes of reporting/disaggregation by sex, and for tracking intervention dosage across events and time
1. **Tangerine-based data by unique IDs for individuals and schools** <-- To enable a holistic view of contact across intervention modalities
