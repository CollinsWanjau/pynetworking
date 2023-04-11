# 0x0C. Web server

## DNS Records

* DNS records are a set of data stored on a DNS server that maps domain names to IP addresses or other types of info associated with a
domain.When a user types a domain name into a web browser or other
app, the DNS system looks up the associated DNS records to dertemine
the IP addr. of the server hosting the website.

#### Types of DNS records

* A(Address) Record - maps a domain name to an IPv4 ADDRESS.
* AAAA(IPv6 Address) Record - maps a domain name to an IPv6 ADDRESS.
* CNAME(Canonical Name) Record - maps a domain name to another domain name or alias.
1. This can prove convenient when running multiple services (like an
FTP server and a web server, each running on different ports) from
a single IP addresses.
2. The setup has the adv. of allowing you to make changes to the IP
address in one place (the DNS A record for example.com) rather than
having to update multiple DNS records for each subdomain.

* MX record -> a mail exhanger record (MX record) specifies the
mail server responsible for accepting email messages on behalf
of a domain name.
1. It is a resource record in the Domain Name System(DNS).
It is possible to configure several MX records, typically pointing
to an array of mail servers for load balancing and redundancy.
2. Resource records are the basic info element of the Domain Name
System (DNS).An MX record is one of these, and a domain may have
one or more of these set up.
```
Domain			TTL   Class    Type  Priority      Host
example.com.		1936	IN	MX	10         onemail.example.com.
example.com.		1936	IN	MX	10         twomail.example.com.
```
3. The priority field identifies which mailserver should be preferred
- in this case the values (above labelled "Priority"), and the domain
name of a mailserver("Host above").

#### MX preference, distance, and priority

* According to RFC 5321, the lowest-numbered records are the most
preffered.

* The preference number is sometimes refered to as the distance: smaller
distances are more preferable.

##### The basics

* When more than one server is returned for an MX query, the server
with the smallest preference number must be tried first.If there is 
more than one MX record with the same preference number, all of
those must be tried before moving on to lower-priority entries.

* An SMTP  client must be able to try (and retry) each of the
relevant addresses in the list in order, untiil a delivery attempt
succeeds.

##### Load distribution

* The standard approach to disstributing a load of incoming mail
over an array of servers is to return the same preference number for
each server in the set.

* When derterming which server of equal preference to send mail to,
"the sender-SMTP MUST randomize them to spread the load across
multiple mail exchangers for a specific org."

##### "Backup" MX

* Some domains will have several MX records, one of which is intended
as a "backup" - with a higher preference number so that iti would not
normally be picked as the target for email delivery.

```
Domain			TTL   Class    Type  Priority      Host
example.com.		1936	IN	MX	10         onemail.example.com.
example.com.		1936	IN	MX	10         twomail.example.com.
example.com.		1936	IN	MX	100        queue.example.com.
```

### TXT record

* A TXT record is a type of resource record in the DNS used to provide
 the ability to associate arbitrary text with a host or other name,
such as human readable info about server, network, data center, or
other accounting info.

* The DNS protocol specifies that when a client queries for a
specific record type (e.g., TXT) for a certain domain name (e.g.,
example.com), all records of that type must be returned in the
same DNS message.

* There are two ways around this:
1. To specify a domain name prefix to be used when using TXT records
for a specific purpose (e.g. _domainkey.example.com_ in the DKIM case)

#### What is Round Robin DNS?

* Round robin DNS is nothing but a simple technique of load balancing
various Internet services such as Web server, e-mail server by creating
multiple DNS A records with the same name.

##### Round Robin DNS Usage

* You can use round robin DNS for:
1. Load distribution.
2. Load balancing.
3. Fault-tolerance service.

#### NS records

* A type of DNS record that specifies the authoritative name servers
for a particular domain.

* In other words, NS records are used to indicate which servers are
responsible for handling DNS queries for a specific domain

* An NS record delegates a subdomain to a set of name servers.
Whenever you delegate a domain to DNSimple, the TLD authorities place
NS records for your domain in the TLD name servers pointing to us.

#### SOA records

```
ns1.dnsimple.com admin.dnsimple.com 2013022001 86400 7200 604800 300
```

* The SOA is used to provide info about the zone file for a domain,
including the primary name server, the email address of the admin
responsible for the domain, and various timing params.
