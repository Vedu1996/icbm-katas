# Persona Story: Zookeeper

## Persona

**Name:** Aisha Khan  
**Role:** Zookeeper and enclosure care specialist  
**Primary goal:** Keep animals healthy and identify welfare problems early.

## A Day at the Estate

Aisha starts her shift by opening the task-management app on her phone. It shows her feeding, cleaning, and inspection tasks for the day. Because the app supports offline work, she can still see and update assignments in remote parts of the estate where Wi-Fi is unreliable.

At the piranha display, Aisha begins the morning feeding. The enclosure cameras detect feeding-related movement, the feeder scale records a weight change, and the water sensors report normal conditions. The local edge system combines these signals and records a feeding event without sending raw video to the cloud.

Later, the system detects reduced movement in another enclosure at the same time as an unusual temperature reading. A local alert appears on Aisha's device and sounds at the enclosure. She inspects the habitat, adds her observations to the task, and escalates it to the veterinary team.

The event is also stored for longer-term analysis. If the pattern continues over several days, the cloud rules engine can create a higher-priority welfare task using the historical trend.

## System Interaction

- Edge computer vision extracts behavior and movement signals locally.
- Feeder scales and environmental probes contribute supporting evidence.
- Sensor-fusion logic turns several readings into a compact structured event.
- The local rules engine handles urgent alerts without waiting for cloud connectivity.
- The commercial task platform provides Aisha's offline-capable work list.
- The time-series database preserves trends for veterinary and management review.

## Value to Aisha

Aisha receives fewer noisy alerts and more useful context. She can keep working during connectivity problems, respond quickly to urgent changes, and use historical evidence when deciding whether an animal needs further care.
