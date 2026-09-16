# Persona Story: Estate Manager

## Persona

**Name:** Elena Morris  
**Role:** Estate manager  
**Primary goal:** Coordinate the whole estate and make informed operational decisions.

## A Day at the Estate

At the beginning of the day, Elena opens the staff dashboard. She reviews visitor throughput, busy zones, ride queues, open animal welfare tasks, and maintenance issues. Crowd data is presented as anonymous counts and zone-level flow, so she can move staff where they are needed without tracking individual visitors.

During the afternoon, a network outage affects several areas of the estate. Some dashboard metrics become delayed. Local ticketing, ride operations, animal monitoring, and emergency rules continue operating, so Elena treats the dashboard delay as a visibility problem rather than an estate-wide shutdown. Once connectivity returns, buffered events are synchronized and duplicate messages are discarded using their event IDs.

Elena then asks the smart reporting service for a daily summary covering the busiest zones, queue pressure, unresolved welfare concerns, repeated ride anomalies, and tasks requiring management attention.

The report is generated through the LLM gateway using approved operational data. Elena checks important findings against the dashboard and staff reports before using them in the next day's planning meeting.

## System Interaction

- BI dashboards expose operational and historical trends.
- The warehouse and time-series database provide structured data for analysis.
- Predictive models identify demand and maintenance patterns.
- The cloud rules engine converts correlated anomalies into prioritized tasks.
- The LLM gateway controls access, grounding, audit logging, and provider fallback.
- Edge store-and-forward keeps local operations running during outages.

## Value to Elena

Elena gets a single operational picture without being overwhelmed by raw telemetry. She can distinguish an actual operational failure from delayed cloud visibility and use reports, trends, and prioritized work to plan the estate's response.
