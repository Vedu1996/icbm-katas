# Persona Story: Ride Operator

## Persona

**Name:** Lewis Grant  
**Role:** Ferris wheel ride operator  
**Primary goal:** Operate the ride safely while keeping queues moving.

## A Day at the Estate

Before opening, Lewis checks the local ride panel. It shows that the Ferris wheel is receiving cycle counts and vibration readings. The queue camera reports high occupancy and an estimated wait time, so Lewis asks another staff member to help manage the queue.

During the afternoon, a wired safety sensor detects a dangerous condition. The local rules engine responds immediately, placing the ride into its safety state and triggering a nearby announcement. The response does not depend on the cloud, Wi-Fi, or a task-management API.

Once the immediate situation is under control, Lewis records what happened. The event is queued locally and later synchronized with the cloud. Separately, the cloud rules engine notices that vibration has gradually increased, the ride has completed many cycles, and its last inspection was several weeks ago. It creates a prioritized maintenance task rather than waiting for a serious failure.

Lewis completes the inspection using the task-management mobile app. If he is working in a low-connectivity zone, his notes are stored locally and synchronized later.

## System Interaction

- Wired safety signals support sub-second ride protection.
- Local computer vision provides anonymous queue and occupancy estimates.
- The local rules engine handles immediate alerts and announcements.
- The cloud rules engine correlates telemetry over time.
- The task platform turns maintenance findings into assigned work.
- Store-and-forward queues protect events during network interruptions.

## Value to Lewis

Lewis can focus on operating the ride rather than watching raw sensor feeds. Safety decisions happen locally and immediately, while longer-term telemetry helps maintenance teams address problems before they become emergencies.
