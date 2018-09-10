After a talk I gave at a recent Hack Reactor I ran into someone who gave a pitch I've heard and made my self countless times. Given all the people working on it, the product has het to emerge and here is a few reasons I think it never will. 

The pitch is for a perfect project management system. Something less fucked than Jira but more fuckable than Basecamp.

## The elevator pitch

Instead of creating a monster system, create a basic system and make expanding it modular. 

## Why this is a sinkhole that will never get filled. 

The kind of problems that PMS's have are only partly technical

1. **Getting people to enter data into it reliably is one of the biggest problems in a PMS**. Most people are hard put to manage a "To Do" list much less a full blown PMS. They are too busy actually doing things to want to work on the paper trail. A developer who can fix 5-10 issues a day doesn't want a management task that will halve his throughput. In general even asking for a worker to give a one line status update once a day is as good as you can hope for -- asking them to do so in a formatted app-driven update and create records, etc. is often beyond what the rank and file will put up with over the long haul. 
2. **Even when a basic system is adopted, the first thing PM's do is add overhead fields that make the process a drag.** Project Managers are notorious for adding multiple fields to a task system to facilitate their own work and upwards reporting. After they have had their way, the field entry process becomes so byzantine that only they themselves are willing to maintain it. The venn diagram between a system so simple that non PM's will use it and a system fully featured enough to allow PM's do do PMmy stuff has no overlap.
3. **Making a system modular enough to handle a range of use cases often becomes "Dev Inception"**. Think of it this way. 
  * Ruby is designed to develop almost any web app kind of task you can think of
  * A project manager written in Ruby is designed to handle a smaller problem. 
  * As the PM tool becomes used more and more, modules develop to handle specific cases
  * At some point, the ground that the modules cover and the ground that Ruby covers becomes more and more the same ground
  * As the expected deliverables expand, the value of the common PM code becomes less and less significant
  * Also, the common code becomes a performance burden as (a) some pre-decided schema becomes slow and inefficient and (b) the requirement to satisfy shared APIs becomes more tiresome than starting from scratch. 
That is a problem with many frameworks not just PMS tools: the shared assumptions accelerate early stage development but once business dynamics become well understood the frameworks are stripped away in favor of optimized code that directly address specific needs. 

## Functional Fixedness 

The problem is that "Project Management" is not so much an industry as it is a pattern of application development. Managing time-based tasks is a quality of nearly any app that involves real world resources. A ticketing app like Fandango is in some ways a kind of Project Management system -- it creates tasks of interaction with a ticket vendor, it has projects that are events, etc. etc. Similarly, an app you design for a DJ to build a runlist could be considered a task/calendar app. As could a hair stylists booking app. 

So starting with a quality that a bunch of apps have in common and saying "We're going to build a cross-system solution that all these things combine" sounds like something that would help a lot of people, but the amount of tasks it solves compared with the amount of customization that different people require means that pretty soon, the DJ has to accept system constraints that the Hairstylists' model requires and vice versa, and this constraint web becomes exponential. 

For instance, a hair appointment often has two periods, and a gap of useful time in-between, as with a perm. So for a hairstylist, having time periods in separate records that you link to the appointment make sense. Also, each stylist may have different times for a given service -- some might schedule the same job (perm) as 45 minutes, 30 minute gap, 20 minute followup; some might schedule the job as 30 minutes, 45 minute gap, 30 minutes. 

When you consider the amount of granularity describing a hairstylist task requires and compare it to a DJ whose task (playing "Stairway to Heaven") always has a fixed duration and only one period, a DJ app developer looking at the deeply denormalized model that has been derived for Hairstylist will really not see using hairstylist models as a big time save for building his DJ app. Or, the Hairstylist developer looking at the more simple model that the DJ app uses might decide it is overly normalized and too simple for their stylist. 

A third group (People planning social outings) might even find duration/end time to be a useless concept and chafe at fulfilling the requirement of duration/end time based entry. 

In short, just because two activities share a thing in common (tasks with duration, times, repeats) doesn't mean that there is an overarching schema that can be applied to all the use cases and will save everyone time and energy by adopting that schema. And that is the premise that a broad-based PMS asserts, and that premise is wrong.