# Design YouTube

## Capacity Estimation

**Assumptions**
- MAU (Monthly Active Users): 2 billion
- DAU (Daily Active Users): 50% of MAU = 1 billion
- Average video size: 50 MB
- Video consumption per DAU: 5 videos
- View to Upload ratio: 200 : 1

**Video Views & Uploads per Day**
- Daily Video Views = 1 billion DAU * 5 videos = 5 billion views/day
- Daily Video Uploads = 5 billion views/day / 200 = 25 million uploads/day

**Video Storage Calculation**
- Daily Storage = 25 million uploads/day * 50 MB = 1250 TB/day
- With 3x encoding overhead = 3750 TB/day = 3.75 PB/day

**Bandwidth Calculation**
- Views Bandwidth = 5 billion views/day * 50 MB / 10^4 seconds = 5 * 10^9 * 50 * 10^6 / 10^4 = 25 TB/s
- Upload Bandwidth = 25 TB / 200 / s = 125 GB/s

**QPS**
- Video Views per Second = 5 billion / 10^4 seconds = 500,000 QPS
- Video Uploads per Second = 500,000 QPS / 200 = 2500 QPS

**Metadata Storage Calculation**
- Metadata Size = 1 KB per video & 0.5 KB per user
- Storage time = 5 years = 5 * 400 days = 2000 days
- 1 million new users/day (Assumption)
- Video metadata storage = 2000 days * 25 million uploads/day * 1 KB = 50 TB
- User metadata storage = 2000 days * 1 million new users/day * 0.5 KB = 1 TB