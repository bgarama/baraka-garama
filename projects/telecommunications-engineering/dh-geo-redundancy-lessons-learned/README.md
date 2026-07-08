Overview

This document summarizes lessons learned from a successful GEO-redundancy validation performed in a laboratory environment using:

2 SBCs (Primary and Secondary)
2 IPPBXs (Primary and Secondary)
Shared Directory Number (DN)
SIP registration on both redundant paths

The objective was to validate service continuity during failover scenarios while maintaining inbound and outbound call functionality.

Architecture

      +------------------+
      |   Primary SBC    |
      +------------------+
                |
      +------------------+
      |  Primary IPPBX   |
      +------------------+

                ||

      +------------------+
      | Secondary IPPBX  |
      +------------------+
                |
      +------------------+
      | Secondary SBC    |
      +------------------+

Key Finding
The primary issue encountered during deployment was related to SIP trust configuration.
The Secondary IPPBX was initially configured to accept signaling from the Primary SBC.
The successful resolution consisted of updating the SIP peer/trusted endpoint configuration so that the Secondary IPPBX accepted signaling from the Secondary SBC.
Result
After updating the trusted SBC IP configuration:

Registration completed successfully.
Inbound calls completed successfully.
Outbound calls completed successfully.
Failover scenarios behaved as expected.

Use Case: Primary IPPBX Failure
Objective
Verify that call service remains available when the Primary IPPBX becomes unavailable.
Test Steps

Establish SIP registrations on both redundant paths.
Validate baseline inbound and outbound calls.
Deactivate the Primary IPPBX.
Place new inbound and outbound calls.
Verify call routing toward the Secondary IPPBX.

Expected Result
Calls should continue processing through the Secondary path without manual reconfiguration.
Actual Result
  Calls successfully failed over to the Secondary IPPBX.
  Call setup and release completed normally.
  Service continuity maintained.

Lessons Learned
Registration Validation First
Always verify:

SIP registration status
Reachability
OPTIONS responses
Route availability

before executing failover tests.
Trust Relationships Matter
Redundant systems require proper trust definitions between SBCs and IPPBXs.
A valid registration does not guarantee successful call processing if signaling is received from an untrusted source.
Test Multiple Failure Types
Failover testing should include:

Administrative deactivation
SIP service interruption
Network interface failure
Registration loss scenarios

Keep Configurations Symmetrical
Primary and Secondary systems should maintain equivalent configurations wherever possible to reduce operational complexity.

Conclusion
The GEO-redundancy solution successfully demonstrated service continuity across dual SBC and dual IPPBX deployments using a shared DN. Correct SIP trust configuration between the Secondary IPPBX and Secondary SBC was the key requirement for successful failover operation.
