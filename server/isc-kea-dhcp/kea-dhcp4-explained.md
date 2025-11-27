[Available hooks](https://kea.readthedocs.io/en/latest/arm/hooks.html#available-hook-libraries)

## Host reservation
Another aspect of host reservations is the different types of identifiers. Kea currently supports four types of identifiers: hw-address, duid, client-id, and circuit-id. This is beneficial from a usability perspective; however, there is one drawback. For each incoming packet, Kea has to extract each identifier type and then query the database to see if there is a reservation by this particular identifier. If nothing is found, the next identifier is extracted and the next query is issued. This process continues until either a reservation is found or all identifier types have been checked. Over time, with an increasing number of supported identifier types, Kea would become slower and slower.

To address this problem, a parameter called host-reservation-identifiers is available. It takes a list of identifier types as a parameter. Kea checks only those identifier types enumerated in host-reservation-identifiers. From a performance perspective, the number of identifier types should be kept to a minimum, ideally one. If the deployment uses several reservation types, please enumerate them from most- to least-frequently used, as this increases the chances of Kea finding the reservation using the fewest queries. An example of a host-reservation-identifiers configuration looks as follows:

## Many of the config options

https://kea.readthedocs.io/en/latest/arm/config-templates.html#template-secure-high-availability-kea-dhcp-with-multi-threading

## Adding db backend eg postgres

https://kea.readthedocs.io/en/latest/arm/admin.html