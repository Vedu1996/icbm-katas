# Persona Story: Estate Manager

## Persona

**Name:** Elena Morris  
**Role:** Estate manager  
**Primary goal:** Keep the whole estate running smoothly, move staff where they are needed, and make smart investment decisions as visitor numbers grow.

## A Day at the Estate

At the start of the day, Elena looks at the staff dashboard. She sees which parts of the estate are busiest, where queue times are building, which animal areas need attention, and which maintenance issues were already raised. The data is shown in broad zones and anonymous counts, so she can spot pressure points without tracking individual visitors.

Later in the afternoon, the estate loses connectivity in a few areas. The dashboard shows some lag, but the important systems keep working. Tickets still scan, rides still operate safely, animals are still monitored, and emergency responses still happen locally. To Elena, this is not a full outage — it is a delay in visibility. Once the connection returns, the missing detail catches up.

Elena then asks for a summary of the busiest areas, queue pressure, repeated welfare concerns, and maintenance issues that deserve attention. The report helps her decide where to move staff, where to add temporary support, and which parts of the estate are attracting the most attention.

These decisions matter because the estate is growing fast. What worked for 5,000 visitors a day will not be enough for 15,000. Elena needs to know where the pressure points are and where to invest next so the estate stays profitable while keeping the experience enjoyable.

## System Interaction

- The dashboard turns raw estate activity into a simple view of what is busy and what needs attention. See [ADR-006](../adrs/%5BADR-006%5D%20Data%20Warehouse%20over%20Data%20Lake.md), [ADR-007](../adrs/%5BADR-007%5D%20Timeseries%20Database.md), and [ADR-011](../adrs/%5BADR-011%5D%20Cloud%20Rules%20Engine%20for%20Anomaly-Driven%20Prioritized%20Tasks.md).
- The estate continues to operate safely even when cloud visibility is delayed. See [ADR-001](../adrs/%5BADR-001%5D%20Event%20Driven%20Architecture.md) and [assumptions/1. Reliable Connectivity.md](../assumptions/1.%20Reliable%20Connectivity.md).
- The system gathers historical patterns so Elena can compare today with previous weekends and busy periods. See [ADR-006](../adrs/%5BADR-006%5D%20Data%20Warehouse%20over%20Data%20Lake.md) and [ADR-007](../adrs/%5BADR-007%5D%20Timeseries%20Database.md).
- More serious issues are grouped into prioritized tasks and sent to the right teams. See [ADR-011](../adrs/%5BADR-011%5D%20Cloud%20Rules%20Engine%20for%20Anomaly-Driven%20Prioritized%20Tasks.md) and [ADR-005](../adrs/%5BADR-005%5D%20Task%20management%20system.md).
- Smart summaries help her plan staffing and improvement work without drowning her in technical detail. See [ADR-010](../adrs/%5BADR-010%5D%20LLM%20Gateway%20for%20Smart%20Reporting%20and%20Chatbot.md).

## Value to Elena

Elena gets a clear picture of the estate without being overwhelmed by data. She can tell the difference between a real operational problem and a temporary reporting delay, and she can use the information to support better staffing, safer operations, and more profitable decision-making as the estate grows.
