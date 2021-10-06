# Backup Types

## File Backups

Windows: Archive attribute (file footprint)

Full backup:
1. Everything gets backed up
1. Clear attribute bit

Incremental:
1. Backup since last incremental backup

Differential:
1. All files since last full backup

## Incremental

1. Full backup first
1. Small incremental backups since last incremental backup
1. Recovering requires last full backup + all incrementals

## Differential

1. Full backup first
1. Backup since last full backup
1. Keeps growing bigger as time goes on
1. Rcovering requires full + last differential

## Compare

Full:
1. All data
1. Long time
1. Low restoration time 1 tape only
1. Archive bits cleared

Incremental:
1. All data since last incremental
1. Low time
1. High restoration time multiple tapes
1. Archive bits cleared

Differential:
1. All data since last full
1. Moderate time
1. Moderate restoration time 2 tapes only
1. Archive bit not cleared

## Backup media

Magnetic tape:
1. Sequential -> forward data to find what you want
1. 100GB -> Terabytes
1. Easy to ship

Disk:
1. Faster than magnetic
1. Can deduplicate + compress

Copy:
1. Exact system duplicate
1. Can keep offsite

## NAS vs SAN

Network attached storage:
1. Storage over network
1. File-level access: rewrite entire file

Storage area network:
1. Feels like local storage on device
1. Block level access: Don't need to rewrite entire disk

Both ^ requires good bandwidth

## Others

Cloud:
1. Automatic remote devices
1. Limited to bandwidth
1. Can be done using any device

Image:
1. Exact disk replica
1. fs, os, everything

## Backup location

Offline:
1. Local device
1. Fast + secure
1. Must be protected + maintained
1. Offsite for disaster recovery

Online:
1. Over network
1. Encrypted
1. Cloud or others
1. Accessible from anywhere
1. Speed limited by bandwidth
