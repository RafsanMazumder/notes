# Design Tinder

## Capacity Estimation

**Assumptions**
- Total Users: 100 million
- DAU (Daily Active Users): 20 million
- User activity: 10 swipes per user per day
- User profile & preference size: 1 KB
- Swipe data size: 100 bytes

**Storage Calculation**
- User profile & preferences: 100 million users * 1 KB = 100 GB
- Daily swipes (30 days): 20 million users * 10 swipes/day * 30 days = 600 GB

**Bandwidth Calculation**
- Incoming Bandwidth (Swipes) = 20 million users * 10 swipes/day * 100 bytes / 10^5 seconds = 200 KB/s
- Outgoing Bandwidth (1 profile viewed per swipe) = 20 million users * 10 swipes/day * 1 KB / 10^4 seconds = 2 MB/s

**QPS Calculation**
- Swipe QPS (Write operations) = 200 million swipes / day / 10^5 seconds 
- Profile View QPS (Read operations) = 