# DPG_GPS_Outage
 
## Overview

This project restores vehicle driving records in GPS-denied areas using DTG (Digital Tachograph) data sampled at 1 Hz. Even when GPS signals are unavailable (e.g., tunnels), the DTG’s internal IMU continues to record longitudinal and lateral accelerations. Additionally, GPS data (latitude, longitude, altitude, and speed) at the start and end of the GPS-denied section are still available.  
The algorithm reconstructs the driving path by minimizing the discrepancy between predicted and actual GPS values at the section’s boundaries through optimization.

## Algorithm Description

![Group 20](https://github.com/user-attachments/assets/9f20683e-d88c-4a1c-baab-1d7f98331492)

### 1. Detection of GPS-Denied Sections
- GPS-denied sections are detected based on the number of visible satellites.
- A section where the number of satellites is 10 or fewer is considered unreliable.
- The algorithm identifies the start and end points of these sections.
- Position and speed values between these points are considered unreliable and are reconstructed.

### 2. Initial Path Estimation via Dead Reckoning (DR)
- An initial path is generated using Dead Reckoning (DR) based on the known start (or end) point, speed, and heading.
- The ENU (East-North-Up) coordinate system is used for position updates.

### 3. Optimization
- To mitigate cumulative IMU errors, **backward smoothing** is applied.
- The **cost function** is the discrepancy between the predicted and actual endpoints.
- **Decision variables**: slope, Δheading, Δspeed.
- **Constraints**: absolute values of decision variables are bounded.
- The optimization problem is solved using the **Primal-Dual Interior Point Method**.

### 4. Final Step
- The optimized ENU path is converted back into latitude, longitude, and altitude coordinates.
- The reconstructed driving record replaces unreliable data in GPS-denied sections.
