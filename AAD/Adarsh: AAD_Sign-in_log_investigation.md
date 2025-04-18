// This KQL query analyzes non-interactive logon activities of a specific user, 
// focusing on the location details of each logon attempt. It pulls data from 
// the "AADNonInteractiveUserSignInLogs" table and extracts detailed location 
// information such as city, country/region, and state from the "LocationDetails" field.

// Replace the user principal name (UPN) with the specific user's email to filter results.
AADNonInteractiveUserSignInLogs
| where UserPrincipalName == "adarsh@xyz.com" // Filter by specific user (replace with the correct UPN)
| extend city = tostring(parse_json(LocationDetails).city) // Extract city from LocationDetails JSON field
| extend countryOrRegion = tostring(parse_json(LocationDetails).countryOrRegion) // Extract country/region from LocationDetails
| extend state = tostring(parse_json(LocationDetails).state) // Extract state from LocationDetails
| sort by TimeGenerated desc // Sort results by time of logon in descending order
| project TimeGenerated, UserDisplayName, UserPrincipalName, city, countryOrRegion, state, Location, Identity // Display relevant fields
