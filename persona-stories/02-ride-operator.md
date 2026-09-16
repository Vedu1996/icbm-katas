# Persona Story: Ride Operator

## Persona

**Name:** Lewis Grant  
**Role:** Ferris wheel ride operator  
**Primary goal:** Keep the ride safe, keep guests moving, and prevent downtime from turning into lost revenue.

## A Day at the Estate

Before opening, Lewis checks the ride panel. It shows the Ferris wheel is running normally and gives him a quick view of recent cycle counts and vibration readings. Nearby, the queue camera estimates how busy the line is, so he can ask another staff member to help if the wait grows too long.

Later that afternoon, a safety sensor picks up a problem that is serious enough to require immediate action. The ride automatically moves into its safe state and a nearby announcement tells guests to stay back. The decision happens right away, without waiting for a cloud connection or a remote system to respond.

Once the immediate risk is controlled, Lewis records what happened. The event is saved locally and sent to the cloud later when the connection is back. Meanwhile, the wider system sees the pattern building over time: the ride has been used more, the vibration has been climbing, and the last inspection was some time ago. It turns that into a maintenance task before the problem becomes a major failure.

Lewis then completes the inspection and updates the task from his phone. If he is in a poorer signal area, the app still lets him work and syncs the information later. That keeps the ride operational and helps the estate avoid both safety issues and unnecessary closures.

## System Interaction

- The ride’s own safety signals trigger quick local action when something is wrong. See [ADR-004](../adrs/%5BADR-004%5D%20Local%20Edge%20Rules%20Engine.md).
- The system estimates queue pressure without identifying individual visitors. See [ADR-002](../adrs/%5BADR-002%5D%20Edge%20Computer%20Vision.md).
- The local rules handle immediate response even when the wider network is weak. See [ADR-001](../adrs/%5BADR-001%5D%20Event%20Driven%20Architecture.md) and [ADR-004](../adrs/%5BADR-004%5D%20Local%20Edge%20Rules%20Engine.md).
- The cloud looks at longer-term patterns and helps raise maintenance work before failure happens. See [ADR-011](../adrs/%5BADR-011%5D%20Cloud%20Rules%20Engine%20for%20Anomaly-Driven%20Prioritized%20Tasks.md).
- The task system turns the issue into a clear action for the maintenance team. See [ADR-005](../adrs/%5BADR-005%5D%20Task%20management%20system.md).

## Value to Lewis

Lewis can focus on running the ride and managing the queue instead of staring at raw data. The estate stays safer, the guest experience is smoother, and maintenance issues are caught early enough to reduce disruption and protect the business.
