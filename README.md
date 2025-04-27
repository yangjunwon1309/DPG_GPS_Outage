# DPG_GPS_Outage
 
1. Algorithm Overview

This project restores vehicle driving records in GPS-denied areas using DTG (Digital Tachograph) data sampled at 1 Hz. 
Even when GPS signals are unavailable (e.g., tunnels), the DTG’s internal IMU continues to record longitudinal and lateral accelerations. 
Additionally, GPS data (latitude, longitude, altitude, and speed) at the start and end of the GPS-denied section are still available.
The algorithm reconstructs the driving path by minimizing the discrepancy between predicted and actual GPS values at the section’s boundaries through optimization.

![Group 20](https://github.com/user-attachments/assets/9f20683e-d88c-4a1c-baab-1d7f98331492)

2. Detection of GPS-Denied Sections

GPS-denied sections are detected based on the number of visible satellites. 
A section where the number of satellites is 10 or fewer is considered unreliable. 
The algorithm identifies the start and end points of these sections, and the position and speed values between them are replaced through the reconstruction process.

4. Initial Path Estimation and Optimization

An initial path is generated using Dead Reckoning (DR) based on the known start (or end) point, speed, and heading information.
The ENU (East-North-Up) coordinate system is used for position updates.

Since IMU-based measurements accumulate drift over time, the predicted end point from DR may differ from the actual end point.
To mitigate cumulative IMU errors, backward smoothing is applied during optimization, 
with the discrepancy between the endpoint and the predicted endpoint serving as the cost function.
The decision variables include slope, Δheading, and Δspeed, with constraints imposed to limit their absolute values.

The resulting optimization problem is solved using the Primal-Dual Interior Point Method.
Finally, the optimized path is converted back to latitude and longitude coordinates to complete the reconstruction.
