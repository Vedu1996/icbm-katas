# Persona Story: Zookeeper

## Persona

**Name:** Aisha Khan  
**Role:** Zookeeper and enclosure care specialist  
**Primary goal:** Keep animals healthy, respond early to problems, and prevent costly emergencies.

## A Day at the Estate

Aisha starts her shift by opening her work list on her phone. It shows the feeding, cleaning, and check-up tasks for the day. Even in the quieter parts of the estate where the Wi‑Fi is unreliable, she can still see what she needs to do and update her notes.

At the piranha display, she begins the morning feeding. The system quietly watches the enclosure and records that feeding happened, without sending raw video anywhere. It also notes the change in feed weight and the normal condition of the water. Aisha only sees a simple summary that helps her understand what is happening.

Later, one of the other enclosures starts to look different. The animals are moving less than usual and the temperature is a little off. A quick alert pops up on Aisha’s device and also sounds at the enclosure itself, so she knows something needs attention immediately. She checks the habitat, adds her observations, and escalates the issue to the veterinary team.

The event is also saved for later review. If this pattern repeats over several days, the system can recognise it as a developing welfare issue and raise a more urgent task for the team. That matters because spotting a problem early can reduce treatment costs and help keep the animals calm and healthy.

## System Interaction

- The estate quietly watches key animal areas and turns noisy sensor readings into a clear, useful alert. See [ADR-008](../adrs/%5BADR-008%5D%20Sensor%20Fused%20Edge%20Computer%20Vision%20for%20Animal%20welfare%20monitoring.md) and [ADR-004](../adrs/%5BADR-004%5D%20Local%20Edge%20Rules%20Engine.md).
- Feeding and water changes are logged without storing raw video, keeping the process privacy-safe and operationally useful. See [ADR-002](../adrs/%5BADR-002%5D%20Edge%20Computer%20Vision.md).
- When the network is weak, the local system still keeps working and raises immediate alerts. See [ADR-001](../adrs/%5BADR-001%5D%20Event%20Driven%20Architecture.md) and [ADR-004](../adrs/%5BADR-004%5D%20Local%20Edge%20Rules%20Engine.md).
- The task list stays available to Aisha even when the cloud connection is poor. See [ADR-005](../adrs/%5BADR-005%5D%20Task%20management%20system.md).
- Over time, the wider system looks for patterns and helps staff act before a small problem becomes a serious one. See [ADR-011](../adrs/%5BADR-011%5D%20Cloud%20Rules%20Engine%20for%20Anomaly-Driven%20Prioritized%20Tasks.md).

## Value to Aisha

Aisha gets fewer false alarms and better context. She can keep working through patchy connectivity, respond quickly to urgent changes, and use the longer-term pattern data to decide when an animal needs extra care. That helps protect the animals and keeps the estate running more efficiently.
