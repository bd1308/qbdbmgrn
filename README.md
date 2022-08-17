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
