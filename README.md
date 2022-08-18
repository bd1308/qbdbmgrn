# qbdbmgrn

## Purpose
It's painfully evident that Intuit and/or Quickbooks could care less about supporting Linux users. After debugging some issues, I've found out how janky the Linux server DB manager is, and modified it to work for Quickbooks Enterprise 2022

## History
There's some lookup-table that Quickbooks uses to translate Quickbooks version (e.g. Quickbooks 2022 == v 32) and the filemonitor process uses this to determine what ports the desktop client should use to connect to the "qbdbmgrn_XX" daemon, where XX is the two digit version mentioned above. 

## Gameplan
I'm posting this to help others configure and manage Quickbooks Enterprise DB Manager servers on Linux, so we can continue to use this in the future.

## Version Chart
| Quickbooks Enterprise Version | Internal QB Version Number |
|-------------------------------|----------------------------|
| 8.0                           | 18                         |
| 9.0			                      | 19                         |
| 10.0			                    | 20                         |
| 11.0(2011)		                | 21                         |
| 12.0(2012)		                | 22                         |
| 13.0			                    | 23                         |
| 14.0			                    | 24                         |
| 15.0			                    | 25                         |
| 16.0			                    | 26                         |
| 17.0			                    | 27                         |
| 18.0			                    | 28                         |
| 19.0			                    | 29                         |
| 20.0			                    | 30                         |
| 21.0			                    | 31                         |
| 22.0(2022)		                | 32                         |

## Motivation
I've spent the last 10 years of my personal time helping SMBs manage Quickbooks Enterprise, on Linux + Samba boxes, and have encountered so SO many problems. I'm posting this here and will continue to add to it in hopes somebody else will find it useful

## Future Plans
* Collect recent/old qbdbm RPMs (and DEB files via Alien) and upload them to a dist/ folder.
* Wrap qbdbmgrn/qbdbfilemon/samba into a Docker container and k8s Helm chart
* Release updated versions of qbdbmgrn/qbdbfilemon (for example for Quickbooks Enterprise 2022)

## Step By Step functionality of Linux QBES
* End-User double-clicks on company file from mapped drive or otherwise opens company file from Samba network share
* QB Desktop Enterprise uses the server name of the network share and contacts port that is bound by qbmonitord (qbdbfilemon)
** qbdbfilemon maintains a qbdata.dat file that maintains network connectivity information, and creates/manages ND files that maintain metadata for each company file that is opened.
* qbmonitord then greps ps output for qbdbmgrn_XX (where XX is 'Internal QB Version Number') and looks for the port or port range qbdbmgrn has bound, and returns this information to QB Desktop
* QB Desktop then uses this new port/connection to maintain consistency between mutliple users (multi-user mode), likely through some bizarre process that is likely completely insane.

## Common Issues 
* H202 Error - 
  * Check firewall to make sure all proper ports are opened
  * Check that qbdbfilemon and qbdbmgrn_XX are running
  * Check logs, especially when attempting multi-user mode. Look for errors in grep output for `port=`. This is how I found out that QBES 2022 doesn't work with the "latest available" release of qbdbm RPM, and inspired this repo.
* -6190,-77 - Ensure all users of QB Enterprise have the network drive mapped and are being used the same way. qbdbfilemon/qbdbmgrn cannot support DNS names and IP mappings
* -6190,-83 - Ensure all users can read/write files (including ND and other metadata files) on the network share. This typically indicates some permission/ownership issue with Samba.
* -6190,-816 - I restarted both qbdbmgrn/qbdbfilemon and this went away.
