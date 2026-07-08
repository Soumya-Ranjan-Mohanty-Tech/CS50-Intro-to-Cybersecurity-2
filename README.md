
# CS50-Intro-to-Cybersecurity-2
Tracking parameters First-party vs third-party cookies Google Analytics tracking Browser privacy protections Incognito mode limitations Supercookies DNS privacy issues DNS over HTTPS (DoH) DNS over TLS (DoT) DNS spoofing VPNs Tor (The Onion Router) App permissions Location tracking How all of these systems interact

Part 1
Tracking Parameters
URL Structure
HTTP GET Requests
Query Strings
Key-Value Pairs
Click IDs
Campaign IDs
How Tracking Parameters Work
Server Logs and Databases

Part 2
Cookies
Session Cookies
Tracking Cookies
First-Party vs Third-Party Cookies
Why Cookies Exist
Virtual Handstamp Analogy
Browser Storage

Part 3
Google Analytics Example
Harvard–Yale–Stanford Example
Embedded Third-Party Content
Image Tags
Referer Header
Cross-Site Tracking
Why Third Parties Become More Powerful

Part 4
Privacy-Focused Browsers
Safari
Firefox
Brave
DuckDuckGo Browser
Edge
Chrome
Private Browsing
Incognito Mode
What Incognito Can and Cannot Do

Part 5
Super Cookies
ISP Injection
Mobile Carrier Tracking
Man-in-the-Middle Concepts
Why HTTPS Stops Super Cookies

Part 6
Cookie Theft
Session Hijacking
Why Passwords Should Not Be Stored in Cookies
Server-Side Sessions
SMS Insecurity
SIM Swapping
End-to-End Encryption
WhatsApp
Signal
Telegram
iMessage

Part 7
Domain Name System (DNS)
DNS Hierarchy
DNS Resolution Process
DNS Caching
Port 53
DNS Privacy Problems
DNS Spoofing
DNS over HTTPS (DoH)
DNS over TLS (DoT)

Part 8
Virtual Private Networks (VPNs)
How VPNs Work
What VPNs Protect Against
What VPNs Cannot Protect Against
IP Address Masking

Part 9
Tor (The Onion Router)
Onion Routing
Multiple Encryption Layers
Relay Nodes
Entry Node
Middle Node
Exit Node
Limitations of Tor

Part 10
Permissions
Camera Permissions
Microphone Permissions
Contact Permissions
Location Permissions
GPS Tracking
Privacy Implications of Mobile Devices

Part 11
Connecting All Topics Together
Complete Cybersecurity & Privacy Ecosystem
How Tracking, Cookies, DNS, VPNs, Tor, HTTPS, Encryption and Permissions Interact
Final Lecture Summary

# 

## Cybersecurity, Privacy, Tracking, DNS, VPNs, Tor, Cookies and Modern Internet Surveillance

---

# 1.1 Introduction

This section of the CS50 Cybersecurity lecture focuses on one of the most important topics in modern computing:

# 1. What Is Privacy?

Privacy means:

Having control over who can collect, store, analyze, share, and profit from information about you.

On the internet, privacy is about controlling information such as:

Websites visited
Search history
Location
Device information
Browser information
Messages
Shopping habits
Personal interests

# 2. Why Privacy Is Difficult on the Internet

The internet was originally designed for:

Communication
Information sharing

It was NOT originally designed for:

Privacy
Anonymity
Tracking prevention

As a result:

Every action leaves traces.

# 3. Digital Footprints

Whenever you:

Visit a website
Search Google
Watch YouTube
Click an advertisement
Open an application

You leave a:

Digital Footprint

A digital footprint is a record of your activity.

Flowchart: Digital Footprint Creation
You
 ↓
Open Browser
 ↓
Visit Website
 ↓
Request Sent
 ↓
Server Receives Request
 ↓
Server Creates Logs
 ↓
Digital Footprint Exists

# 4. Main Entities Watching Internet Activity

The lecture discussed many parties.

Let's rank them.

1. **Websites**

Example:

Amazon
Facebook
Google

**Can see**:

Pages visited
Time spent
Buttons clicked

Cannot directly see:

Other websites visited

(Unless tracking systems are used.)

2. **Third-Party Trackers**

Examples:

Google Analytics
Facebook Pixel
Ad Networks

Can see:

Activity across many websites

This is much more powerful.

3. **Internet Service Provider (ISP)**
   
Examples:

In India:

Jio
Airtel
BSNL
ACT

Can see:

DNS requests
Traffic patterns
IP connections

May not see encrypted content.

4. **Operating System Vendors**
    
Examples:

Microsoft
Apple
Google

Can potentially collect:

Device **telemetry**
Usage statistics
Location information

Depending on settings.

5. Application Developers

Examples:

WhatsApp
Instagram
TikTok

May collect:

Contacts
Camera access
Location
Usage statistics

**Privacy.**

Earlier sections of cybersecurity focus on:

1. Securing accounts
2. Securing data
3. Securing systems
4. Securing software

This lecture focuses on:

**How organizations, websites, advertisers, Internet Service Providers (ISPs), browsers, and applications collect information about users.**

The central question is:

> How are users tracked on the internet even when they think they are anonymous?

To answer this question, we must understand:

* Cookies
* Tracking cookies
* Third-party cookies
* Tracking parameters
* Browser fingerprinting
* Server logs
* DNS
* VPNs
* Tor
* App permissions
* Location tracking

All of these technologies are interconnected.

---

# 1.2 Cookies Review

Before discussing tracking parameters, the lecture revisits cookies.

## What is a Cookie?

A cookie is:

> A small piece of data stored by a web browser on behalf of a website.

A cookie usually contains:

* Session identifiers-------session identifier (Session ID) is a unique random value that **helps a website recognize your current session**.----session_id=8f3a91b7c2d4e5
* Tracking identifiers--------------A tracking identifier is a unique ID used to **recognize the same browser** over time.----------visitor_id=ab12cd34ef56
* Login information------Cookies usually do not store your password directly.Instead **Cookies often store information proving you've already authenticated**.---auth_token=xyz123...
* User preferences--------These cookies **remember settings you've chosen**.--------theme=dark-----language=en------font_size=large----------currency=INR

**Session identifiers**: (Without session cookies, you'd have to log in again on every page.)
**Tracking identifiers**: (Unlike a session ID, this may remain for months or years.)
**Login information**: (Modern websites typically store secure tokens rather than usernames and passwords.)
**User preferences**: (When you return, the website reads the cookie and restores your preferences.)

| Cookie         | Purpose                                  |
| -------------- | ---------------------------------------- |
| **session_id** | Identifies your current browsing session |
| **visitor_id** | Recognizes your browser over time        |
| **auth_token** | Keeps you logged in                      |
| **theme**      | Remembers dark mode preference           |


A cookie is stored on OUR computer or phone.

---

## Why Were Cookies Created?

Cookies were originally created because HTTP is stateless(**Stateless- A protocol property where each request is independent and no memory is retained automatically.**).

**How Cookies Work Technically**

Suppose you visit:example.com

Server responds:
</> http
Set-Cookie: ID=1234ABCD

**What Is Set-Cookie?**

Set-Cookie/Cookie Header is an **HTTP response header**.

Purpose: Tell browser to store a cookie.

Browser stores: ID=1234ABCD

Next request:Browser automatically sends: Cookie: ID=1234ABCD

Server sees: ID=1234ABCD

and says: I know this user.

**Complete Flowchart**

Visit Website
       ↓
Server Sends Set-Cookie
       ↓
Browser Stores Cookie
       ↓
Future Request
       ↓
Browser Sends Cookie
       ↓
Server Recognizes User

**Set-Cookie**

HTTP response header used to create cookies.

**Cookie Header**

HTTP request header used to send cookies back.

### HTTP

HTTP = HyperText Transfer Protocol

HTTP does not naturally remember previous interactions.

Example:

Request 1:

```
Login
```

Request 2:

```
View Profile
```

Without cookies:

The server cannot automatically know:

* Who you are
* Whether you logged in

Cookies solve this problem.

---

# 1.3 The Handstamp Analogy

David Malan repeatedly uses a metaphor:

Imagine entering a nightclub.

A staff member stamps your hand.

Later:

When you re-enter:

You show the stamp.

Nobody needs to ask your identity again.

Cookies work similarly.

Instead of a handstamp:

The server gives your browser a cookie.

The browser automatically sends it back later.

This allows the server to recognize you.

---

# 1.4 Cookies Are Not Automatically Evil

An important point: Cookies themselves are not bad.

Cookies are merely a technology.

They are useful for:

### Login Sessions
 
**Session Cookies** A Session Cookie remembers information during a browsing session.
 
**Purpose**: Maintain state.

Example:

You log into Gmail.

**Google stores a session cookie.----------Google creates (sets) the session identifier/authentication token and the cookie in Set-Cookie HTTP response header to browser, but the browser stores it. -----------------------In future requests to Google, your browser automatically sends the cookie back in a Cookie HTTP request header.**

Now Google remembers:

* Who you are
* That you're authenticated

---

### Shopping Carts

Example: Amazon shopping cart.

You add:

* Rice
* Laptop
* Phone

Cookies help remember your cart contents.

Without cookies:

Your cart would disappear every time you loaded a page.

---

# 1.5 Tracking Cookies

Problems arise when cookies are used not for functionality but for surveillance.

A tracking cookie stores a unique identifier.

Example:

```
ID = 1234ABCD
```

This identifier may uniquely represent:

**YOU**

not millions of users

**but specifically you.**

Whenever your browser visits certain websites:

That identifier is sent again.

This allows organizations to build a profile of:

* Pages visited
* Products viewed
* Ads clicked
* Interests
* Habits

| Feature             | Session Cookie | Tracking Cookie |
| ------------------- | -------------- | --------------- |
| Purpose             | Functionality  | Tracking        |
| Login Support       | Yes            | No              |
| Shopping Cart       | Yes            | No              |
| Privacy Risk        | Low            | High            |
| User Identification | Temporary      | Long-Term       |
| Advertising Use     | No             | Yes             |


---

# 1.6 Tracking Parameters

The lecture then introduces another tracking mechanism:

## HTTP Parameters

Parameters often appear in URLs.

Example:

```
https://google.com/search?q=cats
```

Everything after:

```
?
```

is a parameter.


---

### Parameter Structure

A parameter is:

```
key=value
```

Example:

```
q=cats
```

Here:

Key:

```
q
```

Value:

```
cats
```

Meaning:

Search for cats.

---
**Visual Diagram**
?q=cats
 │   │
 │   └──── Value
 └────── Key
 Search Query = cats

**Without them**

Google would not know what to search for.

Example:
?q=cats
?q=dogs
?q=cybersecurity

Different input.
Different results.


# 1.7 Multiple Parameters

URLs(UNIFORM RESOURCE lOACATOR) can contain multiple parameters.

They are separated using:

```
&
```
**& (Ampersand) separates parameters.**

Example:

```
https://example.com/page?a=1&b=2&c=3
```

Parameters:

```
a=1
b=2
c=3
```
**Question Mark (?)
          ↓
 First Parameter
          ↓
 Ampersand (&)
          ↓
 Second Parameter
          ↓
 Ampersand (&)
          ↓
 Third Parameter**
---


# 8. Legitimate Uses of Parameters

Parameters are often useful.

Examples:

### Search Engines

```
?q=cats
```

---

### Filters

```
?price=1000
```

---

### Pagination

```
?page=5
```

---

### Language Selection

```
?lang=en
```

---

These are normal uses.

---

# 9. Tracking Parameters

However, parameters can also be used for tracking.
**A tracking parameter is a URL(Uniform resource locator) parameter designed primarily to [identify, monitor, or analyze] user behavior across websites and advertising systems.**
Example from the lecture:

```
https://example.com/ad_engagement?
clickID=9x82jsh272j
&campaignID=23
```

Two parameters exist:

### clickID

```
clickID=9x82jsh272j
```

This identifies the individual user.

This is effectively acting as a tracking cookie.

[Advertisement]
 ↓
[You Click]
 ↓
[Unique Number Assigned]
 ↓
[Activity Recorded]

**Real-World Advertising Flow**

Ad Displayed
      ↓
User Clicks
      ↓
Click ID Created
      ↓
User Visits Website
      ↓
Click ID Logged
      ↓
Profile Updated

Future advertisements become more targeted.

---

### campaignID

```
campaignID=23
```

This identifies the advertising campaign.

---

**Campaign IDs identify:**

Advertisement Group

**NOT**

Specific Person

| Feature                | Click ID | Campaign ID |
| ---------------------- | -------- | ----------- |
| Tracks User?           | Yes      | No          |
| Unique Per Person?     | Yes      | Usually     |
| Used For Ads?          | Yes      | Yes         |
| Privacy Concern?       | High     | Low         |
| Identifies Individual? | Yes      | No          |


# 10. Why Tracking Parameters Are Dangerous

**Tracking parameters become useful because servers store them.**

Suppose:

User A gets:

```
clickID=AAA111
```

User B gets:

```
clickID=BBB222
```

User C gets:

```
clickID=CCC333
```

Now every click can be linked back to a specific user.

Advertisers can track:

* What ads you saw
* Which ads you clicked
* Which websites you visited afterward

---

# 11. Tracking Parameters vs Cookies

## **Tracking cookies**:

Stored inside browser storage.

Cookie:

```
ID=1234ABCD
```

may be hidden.

Browser Storage
│
├── Cookies
│
├── **Local Storage
│
├── Session Storage
│
├── Cache
│
├── Browsing History**
│
└── Saved Passwords

## **Tracking parameters**:

Visible directly inside URLs.

Tracking parameter:

```
?clickID=1234ABCD
```

is visible in the URL.

---

# 12. Server Logs

**[      Why do tracking parameters work?            ]**

Because servers keep logs.

Server
   ↓
Creates Cookie
   ↓
Browser Stores Cookie
   ↓
Browser Sends Cookie Back Later

**Visit Website
        ↓
Website Creates Cookie
        ↓
Browser Stores Cookie
        ↓
Future Requests
        ↓
Cookie Sent Back
        ↓
Website Recognizes User**

---

## What Is a Server Log?

A server log is a record of requests.

Example:

```
09:00 User requested:
page1
```

```
09:05 User requested:
page2
```

```
09:10 User requested:
page3
```
The server remembers.

**It records information.**
Timestamp 10:30:15
IP Address: 103.45.67.89
Requested URL (The entire address)---------------https://example.com/products/laptop?utm_source=facebook&id=123
Requested Page (The path (resource) being requested)----------------------/products/laptop
Query Parameters (The data after the ?)---------------------------utm_source=facebook&id=123
Browser Information
Status Code (The server's response to the browser.)
Referrer (The page the user came from) --------**i visited**: https://facebook.com **then clicked**: https://example.com---------**Referrer**: https://facebook.com

| Status Code | Meaning              |
| ----------- | -------------------- |
| 200         | Success (page found) |
| 301         | Permanent redirect   |
| 302         | Temporary redirect   |
| 403         | Forbidden            |
| 404         | Page not found       |
| 500         | Server error         |


---

# 13. Tracking Through Logs

**Why Are Logs Created?**

**Legitimate reasons:**

Troubleshooting
Security
Performance analysis

**But logs can also become:**

Tracking databases

**Suppose every request contains:**

```
clickID=1234ABCD
```

The log becomes:

```
09:00 page1
ID=1234ABCD
```

```
09:05 page2
ID=1234ABCD
```

```
09:10 page3
ID=1234ABCD
```

Now the server can reconstruct your browsing history.

---

# 14. Why Advertisers Love This

Advertisers learn:

### Interests

Examples:

* Agriculture
* Organic farming
* Cybersecurity

---

### Buying Habits

Examples:

* Looking at tractors
* Looking at seeds
* Looking at fertilizers

---

### Behavior Patterns

Examples:

* Active at night
* Active in mornings
* Frequently clicks technology ads

---

This information becomes commercially valuable.

---

**Browser vs Server**

The browser stores things like:

Cookies
Cache
Local Storage

The server stores things like:

User accounts
Website databases
Server logs
Analytics data

**So when people say:**

"Tracking parameters help websites know where visitors came from,"

**they mean:**

The browser sends the tracking parameters to the server, and the server records them in its logs or analytics systems.


# 15. Browser Countermeasures

Modern browsers increasingly fight back.

Examples:

* Safari
* Firefox
* Brave
* DuckDuckGo Browser

These browsers attempt to:

* Remove tracking parameters
* Block tracking cookies
* Reduce cross-site tracking

**Chrome Chriticism**
* Chrome is developed by Google, whose revenue largely comes from advertising. Advertising often relies on understanding user behavior.
* Google benefits from web analytics and advertising ecosystems.
* Therefore, Chrome may not be as aggressive in blocking all forms of tracking as browsers whose primary selling point is privacy.

**Attack Scenario: Complete Tracking Lifecycle**

User Clicks Ad
        ↓
URL Contains Click ID (Tracking Parameters)
        ↓
Server Receives Request
        ↓
Server Creates Log
        ↓
Log Stored in Database
        ↓
User Profile Updated
        ↓
Targeted Ads Generated

---


# 16. What Is a URL?
Full Form

URL = Uniform Resource Locator
URL is a addresss used to loacte a resource on the internet.

**URL Anatomy**
(https://)(www.google.com)(/search)(?q=cats)
│           │                 │       │
│           │                 │       └── Query String
│           │                 └────────── Path
│           └────────────────────── Domain Name
└───────────────────────────── Protocol

**Component 1: Protocol**

Example: https://

Protocol tells the browser: "How should I communicate with this server?"

Common protocols:

Protocol-----Full Form
HTTP---------HyperText Transfer Protocol
HTTPS--------HyperText Transfer Protocol Secure
FTP----------File Transfer Protocol
SSH----------Secure Shell

**Component 2: Domain Name**

Example:google.com

Human-friendly name.

Eventually converted into an IP address using DNS.

**Component 3: Path**

Example: /search

Specifies which resource is requested.

Think:

Website = Building
Path = Room Number

**Component 4: Query String**

Example: ?q=cats

Contains additional information.

This is where tracking usually occurs.



# 16.1 Automatic URL(Uniform Resource Locator) Sanitization

Modern browsers may detect:

```
clickID=
```

or similar tracking values.

Before opening a link:

The browser removes:

```
clickID=...
```

This process is called:

### URL(Uniform resource Locator) Sanitization

or

### URL(Uniform Resource Loactor) Scrubbing

---

# 17. Limitations

This defense is imperfect.

Why?

Because trackers constantly invent:

new names.

Example:

Today:

```
clickID
```

Tomorrow:

```
visitorToken
```

Next week:

```
sessionMarker
```

The browser cannot always know.

---

# 18. Fundamental Privacy Lesson

The lecture emphasizes:

Privacy is a constant battle.

Every time browsers develop protections:

Advertisers develop new tracking techniques.

This creates an ongoing arms race.

---


                 YOU
                  │
                  │
      ┌───────────┼───────────┐
      │           │           │
      ▼           ▼           ▼

  Website      Browser      Device
  Tracking   Fingerprint   Tracking

      │           │           │
      ▼           ▼           ▼

    Cookies    Screen Size   GPS
    Click IDs  Fonts         Sensors
    URLs        Time Zone    Apps

      │           │           │
      └───────────┼───────────┘
                  │
                  ▼

          User Profile

                  │
                  ▼

          Targeted Ads
          

# 19. Databases and User Profiles
  --------------------------------Logs often move into databases.

**A database is an organized collection of data.**

Tracking Database Example
| Click ID | Interest    |
| -------- | ----------- |
| ABC123   | Cats        |
| ABC123   | Farming     |
| ABC123   | Gardening   |
| ABC123   | Agriculture |

# User Profiling

Definition:

User profiling is the process of building a detailed model of a person's behavior, interests, and preferences using collected data.

**Profiling Flowchart**

Click Ad
   ↓
Tracking Parameter
   ↓
Server Log
   ↓
Database
   ↓
Profile Creation
   ↓
Targeted Advertising

### MASTER MEMORY DIAGRAM

**HTTP
(Stateless)
       ↓
Need Memory
       ↓
Cookies
       ↓
Session Cookies
       ↓
Login & Cart Support**

AND

**Cookies
       ↓
Tracking Cookies
       ↓
User Tracking
       ↓
Profiles
       ↓
Targeted Advertising**






# Version A – Ultra-Detailed Notes (Part 2)

## Privacy, DNS, DNS over HTTPS (DoH), DNS over TLS (DoT), VPNs, Tor, Permissions, and Location Tracking

---

# 1. DNS (Domain Name System)

## Full Form

**DNS = Domain Name System**

DNS(Domain Name System) is often called:

> "The phone book of the Internet."

---
**You
 ↓
Click URL
 ↓
Browser creates HTTP Request
 ↓
Request sent to Google
 ↓
Google receives request
 ↓
Google processes query
 ↓
Google returns search results**


## Why DNS Exists

Computers communicate using **IP (Internet Protocol) addresses**, not domain names.

Example:

Instead of remembering:

```
142.250.183.78
```

we remember:

```
google.com
```

Instead of:

```
157.240.xxx.xxx
```

we remember:

```
facebook.com
```

Humans are good at remembering names.

Computers are good at remembering numbers.

DNS bridges the gap---------This is called DNS Resolution.

---

## What DNS Does

DNS translates:

```
Domain Name
        ↓
IP Address
```

Example:

```
google.com
      ↓
142.250.183.78
```

This process is called:

> **DNS Resolution**

---

# Real Example

When you type:

```
https://www.google.com
```

your computer first asks:

```
What is the IP address of google.com?
```

DNS replies:

```
142.250.183.78
```

Only then can your browser contact Google.

---

# How DNS Fits Into Everything

Before HTTPS can begin:

```
Browser
     ↓
DNS Lookup
     ↓
Obtain IP Address
     ↓
HTTPS Connection
     ↓
Website
```

Without DNS:

* No websites
* No web browsing
* No email services
* No online services

would be practical.

---

# DNS Hierarchy

The Internet contains billions of domains.

Your home router cannot know all of them.

So DNS is hierarchical.

---

## Level 1: Local Cache

Your browser may already remember.

Example:

```
google.com
↓
cached
```

No DNS request needed.

---

## Level 2: Operating System Cache

The operating system may remember.

Examples:

* Windows
* macOS
* Linux
* Android
* iOS

---

## Level 3: Router Cache

Your home router may remember.

---

## Level 4: ISP DNS Server

Your Internet Service Provider often maintains DNS servers.

Examples of ISPs:

* Jio
* Airtel
* BSNL
* Vodafone Idea

The ISP may already know:

```text
google.com
↓
142.250.xxx.xxx
```

---

## Level 5: Authoritative DNS Servers

If nobody knows:

The request continues up the hierarchy until an authoritative server provides the answer.

---

# DNS Uses Port 53

Recall:

A port identifies a service.

Examples:

| Service | Port |
| ------- | ---- |
| HTTP    | 80   |
| HTTPS   | 443  |
| SSH     | 22   |
| DNS     | 53   |

Traditionally DNS uses:

```text
Port 53
```

---

# The Privacy Problem With Traditional DNS

Traditional DNS is usually:

```text
Unencrypted
```

Meaning:

When your device asks:

```text
What is the IP address of amazon.com?
```

the request travels openly.

---

# Who Can See DNS Requests?

Potentially:

* ISP
* Coffee shop Wi-Fi
* Airport Wi-Fi
* University network
* Company network

---

# What They Learn

They learn:

```text
User looked up:
google.com

User looked up:
amazon.com

User looked up:
youtube.com
```

Even if website traffic later uses HTTPS.

---

# Important Distinction

Traditional DNS reveals:

```text
Which website?
```

but not:

```text
Which page?
```

Example:

ISP may know:

```text
harvard.edu
```

but not:

```text
harvard.edu/cs50/cybersecurity/week7
```

because the page itself is protected by HTTPS.

---

# DNS Logging

Many DNS servers keep logs.

Logs may contain:

* Time
* IP address
* Requested domain
* Device information

Example:

```text
7:00 PM
User: 49.xxx.xxx.xxx
Requested:
google.com
```

Over time this creates a browsing profile.

---

# DNS Over HTTPS (DoH)

## Full Form

**DoH = DNS over HTTPS**

---

## Problem It Solves

Traditional DNS:

```text
Unencrypted
```

DoH:

```text
Encrypted
```

---

## How It Works

Instead of:

```text
Browser
     ↓
DNS Port 53
     ↓
DNS Server
```

DoH uses:

```text
Browser
     ↓
HTTPS
     ↓
DNS Server
```

---

# Example

Normal DNS query:

```text
What is google.com's IP?
```

can be read by the ISP.

DoH encrypts:

```text
What is google.com's IP?
```

inside HTTPS.

ISP cannot read it.

---

# Benefits of DoH

Protects against:

* ISP monitoring
* Coffee shop Wi-Fi monitoring
* Airport Wi-Fi monitoring
* Public hotspot monitoring

---

# DNS Over TLS (DoT)

## Full Form

**DoT = DNS over TLS**

TLS = Transport Layer Security

---

## Purpose

Same goal as DoH:

Encrypt DNS.

---

# Difference Between DoH and DoT

DoH:

```text
DNS
inside
HTTPS
```

DoT:

```text
DNS
inside
TLS
```

No HTTP involved.

---

# Comparison

| Feature      | DoH  | DoT  |
| ------------ | ---- | ---- |
| Encrypts DNS | Yes  | Yes  |
| Uses HTTPS   | Yes  | No   |
| Uses TLS     | Yes  | Yes  |
| Goal         | Same | Same |

---

# DNS Spoofing

## What Is It?

DNS Spoofing occurs when a DNS server gives a false answer.

---

Example

You ask:

```text
What is the IP of google.com?
```

Malicious server responds:

```text
203.xxx.xxx.xxx
```

which belongs to an attacker.

---

# Consequences

Victim thinks:

```text
I am visiting Google.
```

Actually visits:

```text
Attacker's Website
```

---

# How HTTPS Helps

Even if DNS lies:

The attacker usually cannot provide Google's valid TLS certificate.

Browser shows warning:

```text
Connection is not private
```

or

```text
Certificate invalid
```

Thus:

DNS spoofing becomes much harder.

---

# Virtual Private Network (VPN)

## Full Form

**VPN = Virtual Private Network**

---

# Main Idea

VPN creates:

```text
Encrypted Tunnel
```

between:

```text
Your Device
       ↓
VPN Server
```

---

# Without VPN

```text
Device
   ↓
ISP
   ↓
Website
```

ISP sees substantial metadata.

---

# With VPN

```text
Device
   ↓
Encrypted Tunnel
   ↓
VPN Server
   ↓
Website
```

ISP mainly sees:

```text
Connected to VPN Server
```

---

# What VPN Protects

VPN hides from:

* ISP
* Public Wi-Fi
* Airport networks
* Hotel networks

---

# What VPN Does NOT Hide

Website still sees:

* Pages visited
* Searches performed
* Account activity
* Logins

because you are communicating directly with the website.

---

# VPN and IP Addresses

Without VPN:

```text
Website sees:
Your IP
```

With VPN:

```text
Website sees:
VPN Server IP
```

This can make you appear to be in another country.

Example:

```text
India
   ↓
VPN Singapore
   ↓
Website thinks:
Singapore
```

---

# Important VPN Limitation

VPN does NOT eliminate:

* Cookies
* Tracking parameters
* Browser fingerprinting

Those operate at higher layers.

---

# How DNS, HTTPS, DoH, and VPN Connect Together

When visiting a website:

```text
DNS
     ↓
Find IP Address

HTTPS
     ↓
Secure Communication

DoH
     ↓
Protect DNS Queries

VPN
     ↓
Protect Entire Route To VPN Server
```

Together they improve privacy but solve different problems.

---

**End of Version A – Part 2**

The remaining topics (Tor, Onion Routing, Browser Fingerprinting Deep Dive, Super Cookies, Permissions, Location Tracking, Privacy-Conscious Browsers, and the complete course summary) would form **Part 3**.


# Version A – Ultra-Detailed Notes (Part 3)

## Tor, Onion Routing, Super Cookies, Browser Fingerprinting, Permissions, Location Tracking, Private Browsing, and Complete Privacy Connections

---

# 1. Private Browsing / Incognito Mode

Most modern browsers provide a special browsing mode.

Names vary:

* Incognito Mode (Chrome)
* Private Browsing (Safari)
* Private Window (Firefox)
* Private Window (Brave)

---

## What Private Browsing Does

When you open a private window:

The browser starts with:

```text
No existing cookies
No browsing history
No saved sessions
No cached website data
```

It acts like a fresh browser.

---

## Example

Normal browser:

```text
You log into Gmail.
```

Cookies are stored.

Later:

```text
Gmail remembers you.
```

---

Private window:

```text
Open Incognito
Visit Gmail
Log in
```

After closing:

```text
Cookies deleted
History deleted
Session deleted
```

The next private window starts fresh.

---

# What Private Browsing DOES NOT Do

Many people misunderstand this.

Private browsing does NOT make you anonymous.

The following can still see you:

* Websites
* ISPs
* Employers
* Schools
* Governments (depending on circumstances)
* VPN providers

---

Private browsing only prevents:

```text
Local storage on YOUR device.
```

---

# Example

Suppose you search:

```text
organic farming techniques
```

in Incognito mode.

Google still receives the search.

Google still logs the request.

Google still sees:

* Your IP address
* Browser details
* Device information

The search simply won't remain in your local browser history.

---

# Connection to Cookies

Private browsing creates temporary cookies.

Example:

```text
Session Cookie
```

exists while the window is open.

After closing:

```text
Cookie deleted
```

---

# Limitation: Fingerprinting Still Works

Even if cookies disappear:

Websites may still recognize you using:

* Browser type
* Screen size
* Fonts
* Time zone
* Device information

This is browser fingerprinting.

---

# 2. Super Cookies

David Malan described super cookies as:

> Some of the worst cookies from a privacy perspective.

---

# Normal Cookie

A normal cookie is created by:

```text
Website
    ↓
Browser
```

Example:

```text
Set-Cookie:
ID=1234ABCD
```

The browser stores it.

---

# Super Cookie

A super cookie may be injected by:

* ISP
* Mobile carrier
* Organization providing internet

instead of the website.

---

# Normal Request

```text
Browser
    ↓
Website
```

---

# With Super Cookie Injection

```text
Browser
    ↓
ISP
    ↓
ISP inserts cookie
    ↓
Website
```

---

# Why This Is Dangerous

Even if you:

* Delete cookies
* Clear browser history
* Use Incognito Mode

the ISP can keep inserting:

```text
ID=1234ABCD
```

again and again.

---

# Real Consequence

The website may still recognize:

```text
Same User
```

because the ISP keeps reintroducing the identifier.

---

# Why HTTPS Stops Super Cookies

Recall:

HTTPS provides:

### Confidentiality

No one can read traffic.

### Integrity

No one can modify traffic.

### Authentication

You know who you're talking to.

---

Without HTTPS:

```text
ISP
can read
can modify
can inject
```

---

With HTTPS:

```text
ISP sees encrypted data
```

Example:

Instead of:

```text
GET /page
Cookie: user123
```

ISP sees:

```text
A7F53C1B8D9E...
```

It cannot safely alter it.

Thus:

```text
HTTPS defeats super-cookie injection.
```

---

# 3. Browser Fingerprinting

One of the most powerful tracking techniques.

Unlike cookies:

```text
Nothing needs to be stored.
```

---

# Basic Idea

The website gathers many characteristics.

Each characteristic alone is common.

Together they become unique.

---

# Information Collected

### Browser Type

Examples:

* Chrome
* Firefox
* Safari
* Edge
* Brave

---

### Browser Version

Example:

```text
Chrome 137
```

---

### Operating System

Examples:

* Windows 11
* Windows 10
* macOS
* Linux
* Android
* iOS

---

### Screen Resolution

Examples:

```text
1920 × 1080
2560 × 1440
3840 × 2160
```

---

### Installed Fonts

Examples:

* Arial
* Calibri
* Helvetica
* Roboto

---

### Language Settings

Examples:

```text
en-US
en-IN
or-IN
```

---

### Time Zone

Example:

```text
UTC+05:30
```

---

### Device Type

Examples:

* Desktop
* Laptop
* Smartphone
* Tablet

---

### Hardware Information

Examples:

```text
CPU cores = 12
RAM = 16 GB
```

---

### Graphics Information

Using WebGL.

Examples:

```text
RTX 4070
Intel Iris Xe
```

---

### Canvas Fingerprinting

Browser secretly draws an image.

Small rendering differences create a unique identifier.

---

# Why Fingerprinting Is Powerful

Deleting cookies:

```text
Works
```

---

Deleting browser history:

```text
Works
```

---

Incognito mode:

```text
Works
```

---

But fingerprint remains largely unchanged.

Thus websites may still recognize you.

---

# Connection Between Cookies and Fingerprinting

Cookies:

```text
Store identifier
```

Fingerprinting:

```text
Create identifier from characteristics
```

Both attempt to answer:

```text
Who is this user?
```

---

# 4. Third-Party Cookies

These were a major topic in the lecture.

---

# First-Party Cookie

You visit:

```text
example.com
```

Cookie comes from:

```text
example.com
```

This is first-party.

---

# Third-Party Cookie

You visit:

```text
harvard.edu
```

but Harvard loads:

```text
google.com
```

advertisement.

Google sets:

```text
ID=1234ABCD
```

cookie.

Now Google has a cookie on your machine.

---

# Why This Is Powerful

Suppose:

* Harvard uses Google Ads
* Yale uses Google Ads
* Stanford uses Google Ads

---

You visit:

```text
Harvard
```

Google sees:

```text
ID=1234ABCD
```

---

You visit:

```text
Yale
```

Google sees:

```text
ID=1234ABCD
```

again.

---

You visit:

```text
Stanford
```

Google sees:

```text
ID=1234ABCD
```

again.

---

Google learns:

```text
Same person visited:
Harvard
Yale
Stanford
```

---

This is cross-site tracking.

---

# Modern Browser Defenses

Modern browsers increasingly fight back.

Examples:

* Safari
* Firefox
* Brave
* DuckDuckGo Browser

These browsers attempt to:

* Block third-party cookies
* Block cross-site tracking
* Remove tracking parameters
* Reduce fingerprinting

---

# Chrome

Chrome also includes privacy protections.

However:

Google's business model heavily relies on advertising.

Therefore privacy advocates often consider:

```text
Brave
Firefox
Safari
DuckDuckGo
```

more privacy-focused than Chrome.

---

# 5. Tracking Parameters

Tracking doesn't require cookies.

---

Example URL:

```text
example.com/ad
?clickID=ABC123
&campaignID=23
```

---

Parameter Meaning

```text
campaignID=23
```

Advertising campaign.

---

```text
clickID=ABC123
```

Unique user identifier.

---

Whenever you click links:

The identifier follows you.

---

Server logs store:

```text
clickID
```

allowing tracking.

---

# Modern Protection

Browsers increasingly remove:

```text
utm_source
clickID
tracking parameters
```

before links load.

---

# 6. Permissions

Modern operating systems now ask permission before granting access.

---

Examples

Camera

Microphone

Contacts

Location

Photos

Files

Bluetooth

---

# Why Permissions Matter

Without permissions:

Applications could access sensitive data silently.

Permissions create a security barrier.

---

# Permission Options

Many systems provide:

```text
Always Allow
```

```text
Allow While Using App
```

```text
Never Allow
```

---

# Best Practice

Prefer:

```text
Allow While Using App
```

when possible.

---

# 7. Location Tracking

Smartphones constantly determine location.

Methods include:

### GPS

Global Positioning System

---

### Wi-Fi Positioning

Nearby Wi-Fi networks.

---

### Cell Tower Triangulation

Nearby cellular towers.

---

# Why Apps Want Location

Examples:

Maps

Food delivery

Ride-sharing

Weather

Navigation

---

# Privacy Concern

Many apps request:

```text
Always Allow
```

instead of:

```text
While Using App
```

This allows continuous tracking.

---

# What Companies Can Learn

Potentially:

* Home location
* Workplace
* Daily routine
* Travel habits
* Frequently visited places

---

# 8. Tor

## Full Form

**Tor = The Onion Router**

---

# Goal

Provide stronger privacy and anonymity.

---

# Why "Onion"?

Because traffic is encrypted in layers.

Like an onion has layers.

---

# How Tor Works

Instead of:

```text
You
 ↓
Website
```

Tor uses:

```text
You
 ↓
Node 1
 ↓
Node 2
 ↓
Node 3
 ↓
Website
```

---

# Onion Encryption

Message encrypted:

Layer 1

Layer 2

Layer 3

---

Node 1 removes only Layer 1.

Node 2 removes only Layer 2.

Node 3 removes only Layer 3.

No node knows the complete path.

---

# Benefit

Tracking becomes much harder.

No single relay sees everything.

---

# Limitation

Tor increases privacy.

It does NOT guarantee perfect anonymity.

---

# Complete Privacy Stack

All technologies discussed solve different problems:

| Technology              | Protects Against                  |
| ----------------------- | --------------------------------- |
| HTTPS                   | Reading/modifying website traffic |
| Cookies                 | Maintain state                    |
| Tracking Cookies        | User tracking                     |
| Third-Party Cookies     | Cross-site tracking               |
| Tracking Parameters     | URL-based tracking                |
| DoH                     | DNS monitoring                    |
| DoT                     | DNS monitoring                    |
| VPN                     | ISP visibility and IP exposure    |
| Tor                     | Stronger anonymity                |
| Permissions             | Excessive app access              |
| Private Browsing        | Local history storage             |
| Browser Defenses        | Tracking reduction                |
| Fingerprinting Defenses | Device identification             |

---

# The Big Picture of the Entire Lecture

The central message of David Malan's lecture was:

> Modern Internet technologies were originally created to provide useful functionality (sessions, analytics, DNS, location services, advertisements, personalization, etc.), but the same technologies can also be used to monitor, profile, and track users.

Therefore, understanding:

* Cookies
* Third-party cookies
* Tracking parameters
* DNS
* DoH
* DoT
* HTTPS
* VPNs
* Tor
* Fingerprinting
* Permissions
* Location services

allows you to make informed decisions about your own privacy and to design systems that are more privacy-preserving for future users and customers.

**End of Version A (Complete).**









In your next message, I will continue with **Version A – Part 2 (Third-Party Cookies, Google Analytics, Cross-Site Tracking, and Browser Privacy Protections)** and then later move on to Version B.







# Version A — Part 4

# Privacy-Focused Browsers, Browser Countermeasures, Private Browsing, and Incognito Mode

---

# Introduction

Up to this point in the lecture, we learned that websites, advertisers, analytics companies, and third parties can track users through:

* Cookies
* Third-party cookies
* Tracking parameters
* Server logs
* Referer headers
* Embedded content
* Cross-site tracking

This raises an important question:

> If websites can track us in so many ways, what are browsers doing to protect us?

Modern browsers are no longer just tools for viewing websites.

They have become:

* Privacy tools
* Security tools
* Tracking blockers
* Anti-fingerprinting tools

This section explains:

1. Privacy-focused browsers
2. Browser tracking protections
3. Incognito mode
4. Private browsing
5. What private browsing can do
6. What private browsing cannot do

---

# Browser Countermeasures

## What Is A Countermeasure?

A countermeasure is:

> A protection designed to reduce or prevent a threat.

In cybersecurity:

A countermeasure is anything that reduces:

* Tracking
* Surveillance
* Hacking
* Data collection

Example:

Threat:

* Third-party cookies

Countermeasure:

* Cookie blocking

Threat:

* Tracking parameters

Countermeasure:

* URL cleaning

Threat:

* Browser fingerprinting

Countermeasure:

* Anti-fingerprinting technology

---

# Why Browsers Need Countermeasures

The modern web contains:

* Advertisers
* Analytics companies
* Data brokers
* Social media trackers

Many websites contain code from:

* Google
* Meta (Facebook)
* Advertising networks
* Analytics services

These third parties often attempt to collect information about:

* Which websites you visit
* Which links you click
* How long you stay
* Which products you view
* Which advertisements interest you

Without browser protections:

A user can be tracked across hundreds or thousands of websites.

---

# Major Browsers Mentioned In The Lecture

The instructor discussed several browsers.

---

# Safari

Safari is developed by:

[Apple](https://www.apple.com?utm_source=chatgpt.com)

Default browser on:

* iPhone
* iPad
* Mac

Safari has increasingly focused on privacy.

---

## Safari Privacy Features

### Intelligent Tracking Prevention (ITP)

ITP stands for:

Intelligent Tracking Prevention

Purpose:

Prevent advertisers from tracking users across websites.

Safari automatically:

* Limits cookies
* Restricts tracking
* Reduces cross-site monitoring

---

### Tracking Parameter Removal

The lecture specifically mentioned Apple's feature that removes certain tracking parameters.

Example:

Original URL:

```text
https://example.com/ad?clickid=123456789
```

Browser cleans it to:

```text
https://example.com/ad
```

Result:

Website still works.

Tracking information disappears.

---

# Firefox

Firefox is developed by:

[Mozilla Foundation](https://www.mozilla.org?utm_source=chatgpt.com)

Firefox is generally considered more privacy-conscious than Chrome.

---

## Firefox Protections

Firefox includes:

### Enhanced Tracking Protection

Blocks:

* Tracking cookies
* Fingerprinters
* Cryptocurrency miners
* Cross-site trackers

---

### Total Cookie Protection

Idea:

Every website gets its own cookie jar.

Cookies from one site cannot easily follow you to another.

---

# Brave

Brave is a browser specifically designed around privacy.

Official site:

[Brave Browser](https://brave.com?utm_source=chatgpt.com)

---

## Brave Blocks By Default

* Ads
* Trackers
* Third-party cookies
* Fingerprinting attempts

without requiring extra extensions.

---

# DuckDuckGo Browser

Developed by:

[DuckDuckGo](https://duckduckgo.com?utm_source=chatgpt.com)

Focus:

Privacy-first browsing.

---

## Features

Blocks:

* Trackers
* Advertising scripts
* Third-party tracking technologies

Automatically upgrades connections to HTTPS whenever possible.

---

# Microsoft Edge

Developed by:

[Microsoft Edge](https://www.microsoft.com/edge?utm_source=chatgpt.com)

Includes:

Tracking Prevention Modes:

1. Basic
2. Balanced
3. Strict

The stricter the mode:

The more trackers are blocked.

---

# Google Chrome

Developed by:

[Google Chrome](https://www.google.com/chrome?utm_source=chatgpt.com)

The lecture described Chrome as potentially the "worst offender" from a privacy perspective.

Why?

Because Google's business model heavily relies on:

* Advertising
* Analytics
* User behavior data

---

## Important Clarification

Chrome is not necessarily less secure.

Security and privacy are different.

---

# Security vs Privacy

## Security

Security means:

Protecting data from attackers.

Examples:

* HTTPS
* Encryption
* Malware protection

Question:

"Can hackers steal my information?"

---

## Privacy

Privacy means:

Controlling who collects information about you.

Question:

"Who knows what I am doing?"

---

Chrome can be:

* Very secure

while still being:

* Less privacy-focused

than browsers like Brave or Firefox.

---

# Common Browser Privacy Techniques

Modern browsers attempt to reduce tracking through several mechanisms.

---

# 1. Blocking Third-Party Cookies

Remember:

First-party cookie:

```text
amazon.com → amazon cookie
```

Third-party cookie:

```text
google.com → cookie while visiting amazon.com
```

Browsers increasingly block:

* Third-party cookies
* Cross-site cookies

because these are heavily used for tracking.

---

# 2. Removing Tracking Parameters

Example:

```text
https://shop.com/product?clickid=ABC123
```

Browser removes:

```text
clickid=ABC123
```

Result:

Tracking becomes harder.

---

# 3. Blocking Cross-Site Tracking

Cross-site tracking means:

Following a user across multiple websites.

Example:

You visit:

* Harvard
* Yale
* Stanford

Google Analytics exists on all three.

Google sees activity on all three sites.

Modern browsers try to reduce this visibility.

---

# 4. Anti-Fingerprinting

Recall:

Fingerprinting uses information like:

* Browser type
* Browser version
* Operating system
* Screen resolution
* Fonts
* Language
* Time zone

to identify users.

Browsers increasingly attempt to:

* Hide information
* Standardize information
* Randomize information

to make users harder to distinguish.

---

# Private Browsing

Also called:

* Incognito Mode
* Private Window
* InPrivate Mode

depending on the browser.

---

# What Is Private Browsing?

Private browsing creates:

> A temporary browsing session.

Think of it as:

Starting with a fresh browser.

No history.

No cookies.

No saved sessions.

No remembered logins.

---

# Why Use Private Browsing?

Examples:

### Logging Into Another Account

You can log into:

```text
Account A in normal window
Account B in private window
```

simultaneously.

---

### Testing Websites

Web developers often use private windows.

Why?

To simulate:

* New visitors
* Clean browsers
* Fresh sessions

---

### Temporary Browsing

Useful when:

* Using a shared computer
* Researching something privately
* Avoiding local history storage

---

# What Happens Inside Private Browsing?

When a private window opens:

Browser creates:

A temporary storage area.

This area stores:

* Cookies
* Cache
* Session information

only for that session.

---

# When The Window Closes

Browser deletes:

* Cookies
* History
* Cache
* Session data

stored during that private session.

---

# What Incognito Does Protect Against

Incognito prevents:

### Local History Storage

Someone using your computer later cannot easily see:

* Websites visited
* Searches performed

---

### Local Cookie Persistence

Cookies disappear after closing the session.

---

### Local Session Persistence

Logins are forgotten.

---

# What Incognito Does NOT Protect Against

This is where many people misunderstand private browsing.

---

## Myth 1

### "Incognito Makes Me Anonymous"

False.

Websites still see:

* Your IP address
* Your browser
* Your device

---

## Myth 2

### "My ISP Cannot See Me"

False.

Your ISP still sees:

* Connections
* Traffic patterns
* DNS requests (unless protected by DoH/DoT or VPN)

---

## Myth 3

### "Websites Cannot Track Me"

False.

Websites can still:

* Create new cookies
* Log activity
* Fingerprint your browser

during the session.

---

## Myth 4

### "Server Logs Disappear"

False.

Remember server logs.

The website still records:

* IP address
* Request times
* Pages visited

Those logs are stored on the server.

Not on your computer.

Closing the browser does not erase them.

---

# Fingerprinting Still Works

The lecture specifically mentioned this.

Even in Incognito:

A website can still observe:

* Browser type
* Screen size
* Operating system
* Language settings
* Time zone

These characteristics may still uniquely identify you.

Thus:

Incognito does NOT stop fingerprinting.

---

# IP Addresses Still Exist

Even with Incognito:

Your browser still connects to websites.

That means:

Your IP address is still visible.

Websites can still see:

```text
203.0.113.10
```

(or whatever your IP address is)

unless you use:

* VPN
* Tor

which we will discuss later.

---

# How This Connects To Previous Parts

Tracking parameters identify you through URLs.

↓

Cookies identify you through browser storage.

↓

Third-party cookies identify you across sites.

↓

Browser fingerprinting identifies your device.

↓

Server logs identify your activity.

↓

Privacy-focused browsers attempt to reduce all of these.

↓

Incognito removes local traces but not internet-side traces.

---

# Key Takeaways

1. Modern browsers increasingly include privacy protections.

2. Safari, Firefox, Brave, and DuckDuckGo focus heavily on privacy.

3. Chrome is generally considered less privacy-focused because of Google's advertising ecosystem.

4. Browsers can block:

   * Third-party cookies
   * Tracking parameters
   * Cross-site trackers
   * Fingerprinting attempts

5. Incognito mode only protects against local storage of browsing data.

6. Incognito does NOT hide:

   * IP address
   * Server logs
   * Fingerprinting
   * ISP visibility

7. Privacy and security are related but different concepts.

---

**Next Part: Version A — Part 5: Super Cookies, ISP Injection, Mobile Carrier Tracking, Man-in-the-Middle Concepts, and Why HTTPS Stops Super Cookies.**





# Version A — Part 5

# Super Cookies, ISP Injection, Mobile Carrier Tracking, Man-in-the-Middle Concepts, and Why HTTPS Stops Super Cookies

---

# Introduction

In the previous sections, we learned about:

* Cookies
* Session Cookies
* Tracking Cookies
* Third-Party Cookies
* Browser Tracking
* Incognito Mode

A natural question arises:

> What if I delete all cookies?

or

> What if I use Incognito Mode?

or

> What if I clear my browser history every day?

Will tracking stop?

The lecture introduced a more advanced tracking mechanism called:

# Super Cookies

These are much more difficult to remove because they may not even exist inside your browser.

---

# What Is a Super Cookie?

A Super Cookie is:

> A tracking identifier inserted outside the normal browser cookie system.

Unlike normal cookies:

| Normal Cookie            | Super Cookie                        |
| ------------------------ | ----------------------------------- |
| Stored inside browser    | May be inserted by network provider |
| Can usually be deleted   | Often survives browser cleanup      |
| Created by website       | Created by ISP or intermediary      |
| Visible in browser tools | Often invisible to user             |

---

# Why Are They Called "Super" Cookies?

Not because they are better.

Not because they are larger.

They are called "Super Cookies" because:

They are harder to detect.

They are harder to remove.

They operate outside normal browser controls.

---

# Normal Cookie vs Super Cookie

## Normal Cookie Process

Example:

You visit:

```text
amazon.com
```

Amazon sends:

```http
Set-Cookie:
ID=ABC123
```

Browser stores:

```text
ABC123
```

Later:

Browser returns:

```http
Cookie:
ID=ABC123
```

Amazon recognizes you.

---

## Super Cookie Process

You visit:

```text
example.com
```

Your browser sends request.

But before reaching the website:

Your ISP inserts:

```http
ID=ABC123
```

without your knowledge.

The website receives:

```http
GET /page

Cookie: ID=ABC123
```

Even though:

* Your browser never created it
* You never approved it
* You cannot easily see it

---

# ISP

## Full Form

ISP = Internet Service Provider

Examples:

* Jio
* Airtel
* BSNL
* ACT
* Verizon
* AT&T

An ISP provides:

* Internet connectivity
* Routing
* DNS services
* Traffic forwarding

---

# Why ISPs Can See Your Traffic

Remember the path:

```text
You
↓
Home Router
↓
ISP
↓
Internet
↓
Website
```

Every request must pass through:

```text
ISP
```

Therefore:

ISP can observe traffic flowing through its network.

---

# Mobile Carriers

A Mobile Carrier is a company providing cellular internet.

Examples:

* Jio
* Airtel
* Vi
* AT&T
* Verizon

Mobile carriers are essentially ISPs for mobile devices.

---

# How Mobile Carriers Used Super Cookies

Historically, some mobile carriers inserted identifiers into web traffic.

Purpose:

* Advertising
* Customer tracking
* Marketing analytics

The lecture specifically referenced this behavior.

---

# Example

Suppose:

You open:

```text
http://example.com
```

Browser sends:

```http
GET /page
```

Carrier intercepts traffic.

Carrier adds:

```http
Tracking-ID: 1234ABCD
```

Website now receives:

```http
GET /page
Tracking-ID: 1234ABCD
```

Website can track you.

---

# Why This Is Dangerous

You may think:

"I deleted my cookies."

But:

The identifier comes from the carrier.

Not from your browser.

Therefore:

Deleting cookies changes nothing.

---

# Why Incognito Mode Cannot Stop Super Cookies

Incognito only controls:

* Browser storage
* Browser history
* Browser cookies

But super cookies are injected outside the browser.

Therefore:

Incognito cannot stop them.

---

# Why Clearing Browser History Cannot Stop Them

History exists:

```text
Inside Browser
```

Super Cookies exist:

```text
Inside Network Traffic
```

Different locations.

Deleting one does not affect the other.

---

# How Super Cookies Create Tracking

Suppose carrier injects:

```text
UserID=ABC123
```

every time you browse.

You visit:

```text
news.com
```

↓

```text
shopping.com
```

↓

```text
sports.com
```

↓

```text
travel.com
```

Every website receives:

```text
ABC123
```

Now all those websites know:

Same person visited.

---

# Machine in the Middle

The lecture described this as a type of:

# Man-in-the-Middle (MITM)

---

# Full Form

MITM = Man-in-the-Middle

---

# What Is a Man-in-the-Middle?

A Man-in-the-Middle is:

> Someone positioned between two communicating parties.

---

# Normal Communication

```text
You
 ↓
Website
```

---

# MITM Communication

```text
You
 ↓
Attacker
 ↓
Website
```

The attacker sits in the middle.

---

# Why Is This Dangerous?

Because the middle party can:

* Read messages
* Modify messages
* Inject information
* Track users

---

# ISP as Man-in-the-Middle

In the super-cookie example:

```text
You
 ↓
ISP
 ↓
Website
```

ISP sits between:

* User
* Website

Thus:

ISP can potentially modify traffic.

---

# Packet Inspection

ISPs can inspect packets when traffic is not encrypted.

Packet = Small unit of network data.

Think of packets as:

Individual envelopes.

---

# Without Encryption

Packet contents are visible.

ISP can read:

```text
GET /page
```

and modify it.

---

# Example

Original:

```http
GET /page
```

ISP changes:

```http
GET /page

Tracking-ID=ABC123
```

Website receives modified request.

---

# Why HTTPS Stops This

This was one of the most important points in the lecture.

---

# HTTPS

Full Form:

HyperText Transfer Protocol Secure

---

# What HTTPS Does

HTTPS encrypts communication between:

```text
Browser
↔
Website
```

---

# Encryption

Encryption means:

Converting readable information into unreadable form.

---

# Without HTTPS

Traffic looks like:

```text
Hello Website
```

ISP can read it.

---

# With HTTPS

Traffic looks like:

```text
X8j39!M2kL#Q...
```

ISP sees only encrypted data.

---

# Envelope Analogy

Recall David Malan's envelope analogy.

HTTP:

```text
Transparent envelope
```

Everyone can read contents.

---

HTTPS:

```text
Locked envelope
```

Everyone can see envelope exists.

Nobody can read contents.

---

# Why ISP Cannot Insert Super Cookies Into HTTPS

To modify traffic:

ISP must:

1. Open message
2. Edit message
3. Repackage message

HTTPS prevents step 1.

ISP cannot open encrypted content.

---

# Encryption Keys

HTTPS relies on:

Cryptographic Keys

Specifically:

* Public Key
* Private Key

Only intended website possesses correct private key.

---

# Because ISP Lacks The Key

ISP cannot:

* Decrypt traffic
* Modify traffic
* Insert tracking identifiers

---

# HTTPS Does NOT Hide Everything

Important distinction.

Many beginners misunderstand this.

HTTPS protects:

### Contents

Examples:

* Passwords
* Messages
* Forms
* Emails
* Cookies

---

But HTTPS does NOT necessarily hide:

### Destination Website

ISP often still knows:

```text
youtube.com
```

or

```text
google.com
```

because connections must still be routed.

This becomes important later when discussing:

* DNS
* DNS over HTTPS (DoH)
* VPNs

---

# HTTPS vs VPN

This is where many people become confused.

---

# HTTPS Protects

```text
Browser ↔ Website
```

Connection.

---

# VPN Protects

```text
Device ↔ VPN Server
```

Connection.

---

# Example

Without VPN:

```text
You
 ↓
ISP
 ↓
Google
```

ISP sees:

* Destination
* Traffic patterns

but not HTTPS content.

---

With VPN:

```text
You
 ↓
Encrypted VPN Tunnel
 ↓
VPN Server
 ↓
Google
```

ISP sees:

```text
Connection to VPN
```

Only.

---

# HTTPS + VPN Together

Strongest common setup:

```text
You
 ↓
VPN
 ↓
HTTPS
 ↓
Website
```

Now:

ISP visibility becomes much lower.

---

# Why Super Cookies Became Controversial

Users generally expect:

"If I delete cookies,
tracking should stop."

Super cookies bypassed that expectation.

Because:

Tracking occurred outside browser control.

This created significant privacy concerns.

---

# Opting Out

Some carriers eventually provided:

Opt-Out settings.

Problem:

Often hidden deep inside account settings.

Many users never found them.

---

# Why HTTPS Is One Of The Best Defenses

HTTPS automatically prevents:

* Traffic reading
* Traffic modification
* Traffic injection
* Super cookie insertion

without requiring users to:

* Delete cookies
* Change browsers
* Configure settings

---

# How This Connects To Previous Parts

Tracking Parameters
↓
Cookies
↓
Third-Party Cookies
↓
Cross-Site Tracking
↓
Browser Privacy Features
↓
Incognito Mode
↓
Super Cookies
↓
ISP-Level Tracking
↓
Need for Encryption
↓
HTTPS Protection

The lecture is gradually moving from:

**Website-level tracking**

to

**Network-level tracking**

which sets the stage for the next major topic:

# DNS (Domain Name System)

because even if HTTPS protects content,

your DNS requests can still reveal where you are going.

That is exactly what Part 7 will explore.

---

# Key Takeaways

1. Super Cookies are tracking identifiers inserted outside normal browser cookie storage.

2. They may be injected by:

   * ISPs
   * Mobile carriers
   * Network providers

3. Deleting cookies does not remove Super Cookies.

4. Incognito Mode does not stop Super Cookies.

5. Super Cookies are a form of network-level tracking.

6. This behavior resembles a Man-in-the-Middle scenario.

7. HTTPS encrypts browser-to-website communication.

8. Encryption prevents ISPs from:

   * Reading traffic
   * Modifying traffic
   * Injecting tracking identifiers

9. HTTPS protects content but does not completely hide which websites you connect to.

10. Understanding Super Cookies explains why encryption is essential for privacy on modern networks.

**Next: Version A — Part 6: Cookie Theft, Session Hijacking, Server-Side Sessions, SMS Insecurity, SIM Swapping, End-to-End Encryption, WhatsApp, Signal, Telegram, and iMessage.**







# Version A — Part 6

# Cookie Theft, Session Hijacking, Server-Side Sessions, SMS Insecurity, SIM Swapping, End-to-End Encryption, WhatsApp, Signal, Telegram, and iMessage

---

# 1. Cookie Theft

## Definition

Cookie Theft is the act of stealing a website's cookie from a user's browser and using that cookie to impersonate the user.

Remember:

A cookie often acts like a temporary identity card.

Instead of repeatedly sending:

* Username
* Password

the browser sends a cookie value.

Example:

```text
Cookie:
SessionID = A8F92D71XK
```

The server recognizes:

```text
SessionID = A8F92D71XK
```

and says:

```text
This is Soumya.
He is already logged in.
```

without requiring the password again.

---

## Why Cookies Are Valuable

Cookies often represent:

* Logged-in sessions
* Shopping carts
* User preferences
* Authentication states

Therefore:

Stealing a cookie can sometimes be as powerful as stealing a password.

---

## Example

You log into:

```text
https://bank.com
```

Server sends:

```text
Set-Cookie:
SessionID=XYZ123
```

Browser stores:

```text
SessionID=XYZ123
```

Every future request contains:

```text
Cookie:
SessionID=XYZ123
```

Server sees:

```text
XYZ123
```

and assumes:

```text
Same user
```

Now imagine an attacker steals:

```text
SessionID=XYZ123
```

The attacker can send:

```text
Cookie:
SessionID=XYZ123
```

to the server.

The server may think:

```text
This is the legitimate user.
```

---

# 2. Can Someone Copy a Cookie and Become Me?

This was exactly the student's question in the lecture.

The answer is:

## Sometimes, yes.

If an attacker obtains a valid session cookie:

```text
SessionID=XYZ123
```

they may be able to impersonate you.

This is called:

# Session Hijacking

---

# 3. Session Hijacking

## Definition

Session Hijacking occurs when an attacker steals a valid session identifier and uses it to take over an authenticated session.

---

## Normal Login Process

### Step 1

User logs in.

```text
Username: soumya
Password: mypassword
```

---

### Step 2

Server verifies credentials.

---

### Step 3

Server creates:

```text
SessionID = ABC789
```

---

### Step 4

Server sends cookie.

```text
Set-Cookie:
SessionID=ABC789
```

---

### Step 5

Browser stores it.

---

### Step 6

Future requests send:

```text
Cookie:
SessionID=ABC789
```

---

## Attack Scenario

Attacker steals:

```text
SessionID=ABC789
```

and sends:

```text
Cookie:
SessionID=ABC789
```

to the website.

The website cannot distinguish:

```text
Real User
vs
Attacker
```

because both present the same session token.

---

# Why HTTPS Helps

Without HTTPS:

Traffic travels in plain text.

An attacker on the same network may see:

```text
Cookie:
SessionID=ABC789
```

and steal it.

---

With HTTPS:

Traffic is encrypted.

Attacker sees:

```text
Encrypted gibberish
```

instead of:

```text
SessionID=ABC789
```

making theft far more difficult.

---

# 4. Should Passwords Be Stored Inside Cookies?

David Malan strongly discouraged this.

---

## Bad Practice

Cookie contains:

```text
Username=soumya
Password=mypassword
```

Problems:

If someone accesses browser storage:

they immediately obtain:

* Username
* Password

---

## Better Practice

Cookie contains only:

```text
SessionID=ABC789
```

Server stores:

```text
SessionID → Soumya
Password Hash
Account Information
```

inside the server database.

---

# 5. Server-Side Sessions

## Definition

A Server-Side Session means:

The server stores all important information.

The browser stores only a random identifier.

---

## Example

Browser stores:

```text
SessionID=ABC789
```

Server stores:

| Session ID | User   |
| ---------- | ------ |
| ABC789     | Soumya |

---

When browser sends:

```text
ABC789
```

server looks up:

```text
ABC789 → Soumya
```

and restores the session.

---

# Why This Is Better

If browser only stores:

```text
ABC789
```

then:

* Password remains on server
* Email remains on server
* Sensitive information remains on server

This significantly reduces risk.

---

# 6. SMS (Short Message Service) Insecurity

## What Is SMS?

SMS stands for:

# Short Message Service

Traditional text messaging.

Example:

```text
Hello
How are you?
```

sent through the cellular network.

---

## Common Misconception

Many people assume SMS is secure.

It is not.

---

## Problems with SMS

### Problem 1: Messages Can Be Intercepted

Telecom systems were designed decades ago.

Security was not a major design goal.

---

### Problem 2: Sender ID Can Be Spoofed

Attacker can fake:

```text
From: Bank
```

or

```text
From: Your Friend
```

even when the message originates elsewhere.

---

### Problem 3: SIM Swapping

Attackers can steal your phone number.

More on this shortly.

---

### Problem 4: No End-to-End Encryption

Your telecom provider can potentially access message contents.

---

# 7. SIM Swapping

## Definition

SIM Swapping is an attack where an attacker convinces a mobile carrier to transfer your phone number to a SIM card under the attacker's control.

---

## Normal Situation

Your phone:

```text
Phone Number:
+91 XXXXX XXXXX
```

belongs to your SIM card.

---

## Attack

Attacker contacts mobile carrier.

Claims:

```text
I lost my phone.
Please issue a replacement SIM.
```

If carrier is fooled:

Phone number gets transferred.

---

## Result

Attacker now receives:

* Calls
* SMS
* OTPs (One-Time Passwords)

intended for you.

---

# Why SIM Swapping Is Dangerous

Many websites use:

```text
SMS OTP
```

for password resets.

Attacker receives:

```text
Password Reset Code
```

and gains access to accounts.

---

# Real-World Consequences

Victims have lost:

* Email accounts
* Social media accounts
* Cryptocurrency wallets
* Banking access

through SIM swapping attacks.

---

# 8. End-to-End Encryption (E2EE)

## Full Form

E2EE =

# End-to-End Encryption

---

## Basic Idea

Only:

```text
Sender
and
Receiver
```

can read the message.

Nobody else.

Not even:

* Internet Service Provider
* Wi-Fi owner
* Messaging company

(if implemented correctly)

---

## Example

Without E2EE:

```text
Alice → Messaging Server → Bob
```

Server can read message.

---

With E2EE:

```text
Alice encrypts message.
```

Server receives only encrypted data.

Server forwards encrypted data.

Bob decrypts it.

---

Server sees:

```text
Encrypted ciphertext
```

instead of:

```text
Hello Bob
```

---

# 9. How End-to-End Encryption Works

Simplified process:

### Step 1

Bob has:

```text
Public Key
Private Key
```

---

### Step 2

Alice obtains Bob's Public Key.

---

### Step 3

Alice encrypts message.

---

### Step 4

Server forwards encrypted data.

---

### Step 5

Bob decrypts using Private Key.

---

Only Bob can read it.

---

# 10. WhatsApp

WhatsApp

---

## Owner

[Meta](https://www.meta.com?utm_source=chatgpt.com)

---

## Security

WhatsApp uses:

```text
End-to-End Encryption
```

for personal chats.

---

Meaning:

Meta should not be able to read message contents if encryption is functioning correctly.

---

## What WhatsApp Can Still See

Even if content is encrypted, metadata may still exist.

Examples:

* Who contacted whom
* Time of communication
* Device information
* Phone numbers

This metadata can reveal substantial information.

---

# 11. Signal

Signal

---

## Reputation

Signal is widely regarded as one of the most privacy-focused messaging applications.

---

## Features

* End-to-End Encryption
* Open-source cryptographic protocols
* Minimal metadata collection

---

## Why Security Experts Like Signal

Signal attempts to collect as little user information as possible.

Even Signal itself retains limited metadata compared with many alternatives.

---

# 12. Telegram

Telegram

---

## Important Detail

Many people assume Telegram is automatically end-to-end encrypted.

That is incorrect.

---

## Default Chats

Normal Telegram chats:

```text
Client ↔ Telegram Server ↔ Client
```

are encrypted in transit but not necessarily end-to-end.

Telegram servers can theoretically access message content.

---

## Secret Chats

Telegram's:

```text
Secret Chats
```

use End-to-End Encryption.

Only then does Telegram provide E2EE.

---

# 13. iMessage

iMessage

---

## Owner

[Apple](https://www.apple.com?utm_source=chatgpt.com)

---

## Security

iMessage uses:

```text
End-to-End Encryption
```

between Apple devices.

---

Meaning:

Messages are encrypted before leaving sender's device.

Apple should not be able to read message contents during transmission.

---

# 14. Connection Between Cookies and Messaging Security

Students often confuse these concepts.

They are completely different technologies.

---

## Cookies Solve

Web authentication problems.

Examples:

* Staying logged in
* Shopping carts
* Website sessions

---

## Encryption Solves

Communication privacy problems.

Examples:

* Secure messaging
* Secure email
* Secure banking

---

Cookies:

```text
Who are you?
```

Encryption:

```text
Can others read this message?
```

---

# 15. How This Connects to Previous Parts

### Part 1

Tracking Parameters

tracked your activity through URLs.

---

### Part 2

Cookies

tracked or identified users.

---

### Part 3

Third-Party Cookies

allowed tracking across websites.

---

### Part 4

Privacy Browsers

attempted to block tracking.

---

### Part 5

Super Cookies

allowed ISPs and carriers to track users.

---

### Part 6 (Current Part)

Focus shifts from:

```text
Tracking
```

to:

```text
Identity Theft
Communication Security
Account Takeover
Message Privacy
```

Key ideas:

* Stolen cookies can hijack sessions.
* Passwords should stay on servers.
* SMS is insecure.
* SIM swapping can steal accounts.
* End-to-End Encryption protects message contents.
* WhatsApp, Signal, Telegram, and iMessage use encryption differently.
* Cookies and encryption solve entirely different problems.

This completes **Version A — Part 6** exactly covering the lecture section from the cookie-theft question through the discussion of messaging security and end-to-end encryption.






# Version A — Part 7

# Domain Name System (DNS), DNS Hierarchy, DNS Resolution Process, DNS Caching, Port 53, DNS Privacy Problems, DNS Spoofing, DNS over HTTPS (DoH), and DNS over TLS (DoT)

---

# 1. Introduction: Why DNS Exists

Before understanding DNS, we must understand a fundamental problem.

Humans prefer names.

Computers prefer numbers.

For example:

Humans remember:

```text
google.com
amazon.com
harvard.edu
```

Computers communicate using:

```text
142.250.190.14
52.94.236.248
23.35.103.27
```

These numerical addresses are called:

# IP Addresses

(IP = Internet Protocol)

---

## The Problem

Imagine if every website required remembering an IP address.

Instead of typing:

```text
google.com
```

you would need:

```text
142.250.190.14
```

This would be extremely difficult.

Humans are good at remembering names.

Computers are good at remembering numbers.

DNS was created to bridge this gap.

---

# 2. What Is DNS?

DNS stands for:

# Domain Name System

---

## Definition

DNS is the internet's distributed naming system that translates domain names into IP addresses.

---

## Simple Analogy

Think of DNS as the internet's phone book.

Old Phone Book:

| Person | Phone Number |
| ------ | ------------ |
| John   | 555-1234     |
| Sarah  | 555-5678     |

DNS:

| Domain Name | IP Address      |
| ----------- | --------------- |
| google.com  | 142.250.xxx.xxx |
| amazon.com  | 52.xxx.xxx.xxx  |

DNS answers the question:

```text
What is the IP address of google.com?
```

---

# 3. Why DNS Is Required

Suppose you type:

```text
https://google.com
```

into your browser.

The browser cannot contact:

```text
google.com
```

directly.

Network devices understand IP addresses, not names.

Therefore:

Before connecting:

```text
google.com
```

must be converted into:

```text
142.250.xxx.xxx
```

DNS performs this translation.

---

# 4. DNS Resolution Process

## Definition

DNS Resolution is the process of finding the IP address associated with a domain name.

---

## Example

You type:

```text
google.com
```

into Chrome.

---

### Step 1

Browser asks:

```text
What is the IP address of google.com?
```

---

### Step 2

DNS server responds:

```text
142.250.xxx.xxx
```

---

### Step 3

Browser now knows where Google's server is located.

---

### Step 4

Browser sends:

```text
HTTPS Request
```

to Google's IP address.

---

### Step 5

Google responds with the webpage.

---

# Complete Flow

```text
User
 ↓
Browser
 ↓
DNS Query
 ↓
DNS Server
 ↓
IP Address Returned
 ↓
Browser Connects
 ↓
Website Opens
```

---

# 5. DNS Hierarchy

In the lecture, David explained that DNS is hierarchical.

No single DNS server stores every domain name on Earth.

Instead:

Many DNS servers work together.

---

## Why Hierarchy Is Needed

There are:

* Hundreds of millions of websites
* Billions of DNS requests daily

One server could never handle everything.

Therefore DNS is distributed.

---

# Simplified DNS Hierarchy

```text
Root DNS Servers
       ↓
Top-Level Domain Servers
       ↓
Authoritative DNS Servers
```

---

# Root DNS Servers

These are the highest-level DNS servers.

They know where to find:

```text
.com
.org
.edu
.in
.gov
```

servers.

---

# Top-Level Domain (TLD) Servers

TLD = Top-Level Domain

Examples:

```text
.com
.org
.edu
.in
.net
```

TLD servers know where the domain's authoritative server exists.

---

# Authoritative DNS Servers

These servers contain the actual answer.

Example:

```text
google.com
→ 142.250.xxx.xxx
```

---

# Example Resolution

You ask:

```text
Where is google.com?
```

Root Server:

```text
Ask .com server.
```

↓

.com Server:

```text
Ask Google's DNS server.
```

↓

Google DNS Server:

```text
142.250.xxx.xxx
```

↓

Answer returned to you.

---

# 6. DNS Caching

David mentioned that systems remember answers.

This is called:

# DNS Caching

---

## Definition

DNS Caching means temporarily storing DNS results to avoid repeating the same lookup.

---

## Why Caching Exists

Without caching:

Every website visit would require:

```text
Root Server
→ TLD Server
→ Authoritative Server
```

again and again.

This would be slow.

---

## Example

Today:

```text
google.com
→ 142.250.xxx.xxx
```

is discovered.

The answer is stored.

Tomorrow:

Browser remembers.

No lookup needed.

---

# Where DNS Caching Occurs

Multiple places store cached answers.

---

## Browser Cache

Chrome remembers DNS answers.

Firefox remembers DNS answers.

Edge remembers DNS answers.

---

## Operating System Cache

Windows remembers.

Linux remembers.

macOS remembers.

---

## Router Cache

Home router remembers.

---

## ISP Cache

Internet Service Provider remembers.

---

# Result

Future requests become much faster.

---

# 7. DNS Uses Port 53

In networking:

Applications communicate using ports.

---

## What Is a Port?

A port is a logical communication endpoint.

Think of:

IP Address = Building

Port Number = Apartment Number

---

Example:

```text
192.168.1.10
```

might be the building.

Port:

```text
53
```

identifies DNS.

---

# Common Ports

| Service | Port |
| ------- | ---- |
| HTTP    | 80   |
| HTTPS   | 443  |
| SSH     | 22   |
| DNS     | 53   |

---

DNS traditionally uses:

# Port 53

---

# 8. DNS Privacy Problem

This was one of the lecture's most important points.

---

## The Hidden Issue

Most DNS requests have historically been:

# Unencrypted

---

Suppose you type:

```text
harvard.edu
```

Your device asks:

```text
What is the IP address of harvard.edu?
```

If DNS is unencrypted:

Anyone observing traffic can see:

```text
User is visiting harvard.edu
```

---

# Who Can Potentially See This?

* ISP
* Coffee shop Wi-Fi owner
* Airport Wi-Fi provider
* Hotel network
* Corporate network
* University network

---

Even if HTTPS protects website content later,

the DNS request may already reveal:

```text
Which website you intend to visit
```

---

# Why This Is Important

Your ISP can build a profile of:

* News sites visited
* Shopping sites visited
* Banking sites visited
* Social media sites visited

without seeing actual webpage contents.

---

# 9. DNS Logging

DNS servers often keep records.

These records are called:

# DNS Logs

---

Example

```text
09:00
google.com

09:05
youtube.com

09:10
amazon.com
```

This reveals browsing habits.

---

David specifically emphasized:

ISPs may know nearly every website you visit because DNS queries pass through them.

---

# 10. DNS Spoofing

A student asked about DNS Spoofing.

---

## Definition

DNS Spoofing occurs when a malicious DNS server provides a false IP address.

---

# Example

You ask:

```text
What is the IP address of bank.com?
```

Correct Answer:

```text
52.44.10.8
```

Attacker replies:

```text
99.99.99.99
```

instead.

---

Now:

You connect to attacker's server.

You think:

```text
I'm at bank.com
```

Reality:

```text
You're on the attacker's website.
```

---

# Goal

Steal:

* Passwords
* Banking information
* Credit card information

---

# Why HTTPS Helps

Even if DNS is spoofed:

The fake server usually cannot present the legitimate website's TLS certificate.

The browser displays warnings such as:

```text
Your connection is not private
```

or

```text
Certificate mismatch
```

This is one reason HTTPS is extremely important.

---

# 11. DNS over HTTPS (DoH)

David introduced:

# DNS over HTTPS

Abbreviated:

# DoH

---

## Definition

DNS over HTTPS sends DNS requests through encrypted HTTPS connections.

---

## Traditional DNS

```text
DNS Query
↓
Port 53
↓
Unencrypted
```

Anyone can inspect it.

---

## DoH

```text
DNS Query
↓
HTTPS
↓
TLS Encryption
↓
Encrypted
```

Nobody in the middle can read it.

---

# Example

Normal DNS:

ISP sees:

```text
google.com
youtube.com
amazon.com
```

---

With DoH:

ISP sees only:

```text
Encrypted traffic
```

The domain lookup itself is hidden.

---

# Benefits

Protects DNS requests from:

* ISPs
* Public Wi-Fi
* Hotels
* Airports
* Coffee shops

---

# 12. DNS over TLS (DoT)

Another solution is:

# DNS over TLS

Abbreviated:

# DoT

---

## Definition

DoT encrypts DNS traffic using:

TLS

(Transport Layer Security)

without wrapping it inside HTTPS.

---

# Traditional DNS

```text
DNS
Port 53
Unencrypted
```

---

# DoT

```text
DNS
+
TLS Encryption
```

---

# Key Difference

### DoH

```text
DNS inside HTTPS
```

---

### DoT

```text
DNS directly encrypted using TLS
```

---

Both solve the same privacy problem.

---

# 13. DoH vs DoT

| Feature               | DoH | DoT |
| --------------------- | --- | --- |
| Encrypts DNS          | Yes | Yes |
| Uses HTTPS            | Yes | No  |
| Uses TLS              | Yes | Yes |
| Prevents ISP Snooping | Yes | Yes |
| Protects DNS Queries  | Yes | Yes |

---

# 14. How DNS Connects to Everything Learned So Far

Before visiting any website:

DNS must work.

---

Connection chain:

```text
DNS
↓
IP Address
↓
TCP Connection
↓
HTTPS
↓
Cookies
↓
Tracking
↓
Web Session
```

Everything starts with DNS.

---

# 15. Complete DNS Privacy Flow

Without Protection:

```text
User
 ↓
ISP sees DNS query
 ↓
Website contacted
 ↓
Tracking begins
```

---

With DoH or DoT:

```text
User
 ↓
Encrypted DNS Query
 ↓
ISP cannot read query
 ↓
Website contacted
 ↓
HTTPS protects content
```

---

# Part 7 Summary

You learned:

* DNS = Domain Name System.
* DNS translates domain names into IP addresses.
* DNS resolution finds a website's IP address.
* DNS uses a hierarchical structure.
* DNS caching speeds up lookups.
* Traditional DNS uses Port 53.
* Traditional DNS is often unencrypted.
* ISPs can observe DNS requests.
* DNS spoofing can redirect users to malicious sites.
* HTTPS helps detect many spoofing attacks through certificate validation.
* DNS over HTTPS (DoH) encrypts DNS queries using HTTPS.
* DNS over TLS (DoT) encrypts DNS queries using TLS.
* DNS is one of the first steps that occurs before visiting any website.

This completes **Version A — Part 7**. The next section is **Part 8: Virtual Private Networks (VPNs), How VPNs Work, What VPNs Protect Against, What VPNs Cannot Protect Against, and IP Address Masking.**









# Version A — Part 8

# Virtual Private Networks (VPNs), How VPNs Work, What VPNs Protect Against, What VPNs Cannot Protect Against, and IP Address Masking

---

# 1. Introduction to VPNs

In the lecture, after discussing DNS privacy issues, David Malan introduced another technology frequently advertised as a privacy solution:

# VPN

VPN stands for:

# Virtual Private Network

---

Many advertisements claim:

```text
Use a VPN.
Become completely anonymous.
Nobody can track you.
Become invisible online.
```

These claims are often exaggerated.

A VPN is useful.

A VPN improves privacy in certain situations.

However:

A VPN does **not** make you invisible.

A VPN does **not** solve every privacy problem.

Understanding exactly what a VPN does and does not do is extremely important.

---

# 2. Breaking Down the Name

## Virtual

The connection exists through software rather than a dedicated physical cable.

There is no special wire connecting you directly to the VPN provider.

The connection is created logically through software.

---

## Private

Traffic inside the VPN tunnel is encrypted.

People in the middle should not be able to read it.

---

## Network

The VPN creates a private network connection across the public Internet.

---

# Simple Definition

A VPN is a technology that creates an encrypted tunnel between your device and another network over the Internet.

---

# 3. Why VPNs Were Originally Created

Many people think VPNs were invented for hiding identities.

That was not their original purpose.

VPNs were primarily designed to allow remote access to private networks.

---

## Example: Company Network

Suppose:

```text
Company Headquarters
↓
Internal Servers
↓
Private Databases
↓
Employee Resources
```

These resources are intended only for employees.

---

An employee travels abroad.

They are now sitting in:

```text
Coffee Shop
Airport
Hotel
Home
```

How can they securely access the company's internal systems?

Solution:

VPN.

---

The VPN creates a secure connection from:

```text
Employee Laptop
↓
Internet
↓
Company VPN Server
↓
Company Internal Network
```

The employee effectively becomes part of the company network.

---

# 4. The VPN Tunnel Concept

The most important VPN concept is:

# Tunnel

---

## What Is a Tunnel?

A VPN tunnel is an encrypted pathway through which traffic travels.

Imagine:

Normal Internet Traffic:

```text
You
↓
ISP
↓
Router
↓
Internet
↓
Website
```

Many devices handle your traffic.

---

With VPN:

```text
You
↓
Encrypted Tunnel
↓
VPN Server
↓
Website
```

---

Everything between:

```text
You
and
VPN Server
```

is encrypted.

---

# Why "Tunnel"?

Imagine a mountain.

Without a tunnel:

You travel over the mountain.

Everyone can see you.

---

With a tunnel:

You travel inside the mountain.

Observers outside cannot see what you are doing.

---

A VPN tunnel works similarly.

---

# 5. How a VPN Works

Suppose you want to visit:

```text
google.com
```

---

## Without VPN

```text
Your Device
↓
ISP
↓
Google
```

Google sees:

```text
Your Real IP Address
```

ISP sees:

```text
Traffic Destination
```

---

## With VPN

```text
Your Device
↓
VPN Server
↓
Google
```

---

Google now sees:

```text
VPN IP Address
```

instead of:

```text
Your Real IP Address
```

---

This is one of the VPN's biggest features.

---

# 6. VPN Encryption Process

Suppose your web request is:

```text
GET /search?q=cats
```

---

Without VPN:

Traffic may travel through:

```text
ISP
Routers
Network Equipment
```

before reaching destination.

---

With VPN:

Your request becomes:

```text
Encrypted Data
```

before leaving your device.

---

Only the VPN server can decrypt it.

---

Flow:

```text
Device
↓ Encrypt
VPN Tunnel
↓ Decrypt
VPN Server
↓
Website
```

---

# 7. What VPNs Protect Against

This is one of the most important exam-style concepts.

---

## Protection #1: Local Network Snooping

Suppose you use:

```text
Airport Wi-Fi
Hotel Wi-Fi
Coffee Shop Wi-Fi
```

The network operator can potentially observe traffic patterns.

---

VPN encrypts traffic before it reaches them.

They see:

```text
Encrypted Data
```

instead of readable content.

---

## Protection #2: ISP Monitoring

Without VPN:

ISP can observe substantial information.

---

With VPN:

ISP primarily sees:

```text
Connection to VPN Server
```

instead of every website you visit.

---

## Protection #3: IP Address Exposure

Without VPN:

Website sees:

```text
Your Real IP Address
```

---

With VPN:

Website sees:

```text
VPN Server IP Address
```

---

## Protection #4: Geographic Restrictions

Suppose VPN server is located in:

```text
United States
```

while you are in:

```text
India
```

Websites may believe you are browsing from the United States.

---

# 8. IP Address Masking

David specifically discussed this.

---

## What Is IP Masking?

IP Masking means hiding your actual IP address from the destination website.

---

## Example

Real IP:

```text
49.xxx.xxx.xxx
```

VPN Server:

```text
104.xxx.xxx.xxx
```

---

Without VPN:

Website sees:

```text
49.xxx.xxx.xxx
```

---

With VPN:

Website sees:

```text
104.xxx.xxx.xxx
```

---

Website believes:

```text
User = VPN Server
```

---

# Why This Matters

IP addresses often reveal:

* Country
* Region
* ISP
* Approximate location

VPN reduces direct exposure of this information.

---

# 9. Country Switching Effect

Suppose:

You live in India.

VPN server exists in:

```text
Germany
```

---

Traffic flow:

```text
India
↓
VPN Server (Germany)
↓
Website
```

---

Website sees:

```text
Germany
```

instead of:

```text
India
```

---

This is why VPNs are often used for:

* Testing websites
* Accessing region-specific services
* Privacy enhancement

---

# 10. What VPNs Do NOT Protect Against

This is where many misconceptions begin.

David emphasized that VPNs solve specific problems only.

---

## VPN Does NOT Stop Malware

Suppose your computer is infected.

```text
Keylogger
Spyware
Trojan
```

exists on your machine.

---

VPN cannot help.

---

Because:

```text
Infected Device
↓
Encrypted VPN Tunnel
↓
Internet
```

The malware still exists.

---

VPN protects traffic.

VPN does not clean infected devices.

---

## VPN Does NOT Stop Phishing

Suppose you visit:

```text
fake-bank.com
```

instead of:

```text
real-bank.com
```

VPN cannot detect this automatically.

---

You can still be tricked.

---

## VPN Does NOT Stop Cookie Tracking

Remember Parts 1–3.

Tracking Cookies:

```text
Google
Facebook
Analytics Systems
```

can still identify users.

---

VPN does not automatically block:

* Cookies
* Tracking parameters
* Browser fingerprinting

---

## VPN Does NOT Stop Browser Fingerprinting

Websites can still collect:

* Browser type
* Browser version
* Screen size
* Language
* Time zone
* Installed fonts
* Device characteristics

---

Even if your IP changes,

your browser may still look unique.

---

# 11. VPN Does NOT Make You Anonymous

This is extremely important.

Many people confuse:

```text
Privacy
```

with

```text
Anonymity
```

---

VPN provides:

```text
More Privacy
```

---

VPN does not guarantee:

```text
Complete Anonymity
```

---

Why?

Because websites can still use:

* Cookies
* Accounts
* Browser Fingerprinting
* Login Information

to identify you.

---

# Example

Suppose:

You connect using VPN.

Then log into:

```text
Facebook
Gmail
Instagram
```

---

Those services already know who you are.

The VPN does not magically erase your identity.

---

# 12. VPN and HTTPS Together

A common confusion.

---

## HTTPS

Protects:

```text
Your Device
↓
Website
```

---

## VPN

Protects:

```text
Your Device
↓
VPN Server
```

---

They are different layers.

---

Without VPN:

```text
Device
↓ HTTPS
Website
```

---

With VPN:

```text
Device
↓ VPN Tunnel
VPN Server
↓ HTTPS
Website
```

---

They can work together.

In fact, most modern VPN traffic also carries HTTPS traffic inside it.

---

# 13. VPN and DNS

Recall Part 7.

DNS requests reveal websites you visit.

---

Without VPN:

```text
Device
↓
ISP DNS
```

ISP may see DNS requests.

---

With VPN:

DNS requests often travel through the VPN tunnel.

---

Flow:

```text
Device
↓
Encrypted VPN Tunnel
↓
VPN DNS Server
```

---

ISP may no longer see individual DNS lookups.

---

# 14. VPN Trust Problem

VPN removes trust from:

```text
ISP
```

but creates trust in:

```text
VPN Provider
```

---

Without VPN:

ISP may see activity.

---

With VPN:

VPN provider may see activity.

---

Therefore:

You are shifting trust.

Not eliminating trust.

---

This is one reason privacy experts emphasize:

```text
Choose VPN providers carefully.
```

---

# 15. VPN vs ISP Visibility

Without VPN:

```text
ISP sees:
- Your IP
- DNS requests
- Traffic patterns
- Many connection details
```

---

With VPN:

```text
ISP sees:
- VPN connection
- Amount of data transferred
```

but not necessarily individual websites.

---

# 16. How VPN Connects to Previous Topics

Recall everything learned so far:

---

Part 1

Tracking Parameters

```text
URL Tracking
```

---

Part 2

Cookies

```text
Session Tracking
```

---

Part 3

Third-Party Cookies

```text
Cross-Site Tracking
```

---

Part 4

Private Browsing

```text
Local Privacy
```

---

Part 5

Super Cookies

```text
ISP-Level Tracking
```

---

Part 6

Messaging Security

```text
End-to-End Encryption
```

---

Part 7

DNS Privacy

```text
Website Lookup Privacy
```

---

Part 8

VPN

```text
Traffic Tunnel Privacy
IP Address Masking
Network Protection
```

---

# Complete VPN Flow

```text
User
↓
VPN Encrypts Traffic
↓
Encrypted Tunnel
↓
VPN Server
↓
Website
```

Website sees:

```text
VPN IP Address
```

instead of:

```text
User's Real IP Address
```

---

# Part 8 Summary

You learned:

* VPN stands for Virtual Private Network.
* A VPN creates an encrypted tunnel between your device and a VPN server.
* VPNs were originally designed for secure remote access.
* VPNs encrypt traffic between you and the VPN server.
* VPNs can hide your real IP address from websites.
* VPNs can make websites think you are in another country.
* VPNs protect against some ISP and public Wi-Fi monitoring.
* VPNs do not stop malware.
* VPNs do not stop phishing.
* VPNs do not stop cookies.
* VPNs do not stop browser fingerprinting.
* VPNs do not provide complete anonymity.
* VPNs and HTTPS solve different problems and often work together.
* VPNs may improve DNS privacy.
* VPNs shift trust from the ISP to the VPN provider.

This completes **Version A — Part 8**. The next section is **Part 9: Tor (The Onion Router), Onion Routing, Multiple Encryption Layers, Relay Nodes, Entry Node, Middle Node, Exit Node, and the Limitations of Tor.**






# Version A — Part 9

# Tor (The Onion Router), Onion Routing, Multiple Encryption Layers, Relay Nodes, Entry Node, Middle Node, Exit Node, and the Limitations of Tor

---

# 1. Introduction to Tor

Tor is one of the most famous privacy technologies on the Internet.

Tor stands for:

**The Onion Router**

It is a privacy-preserving network designed to make it much harder for websites, governments, Internet Service Providers (ISPs), advertisers, and attackers to determine:

* Who you are
* Where you are
* Which websites you visit
* Where your traffic originates

The lecture introduced Tor after discussing:

* Cookies
* Tracking parameters
* Browser fingerprinting
* DNS tracking
* Virtual Private Networks (VPNs)

because Tor attempts to solve privacy problems that those technologies cannot fully solve.

---

# 2. Why Tor Was Created

Even if you use:

* HTTPS
* DNS over HTTPS (DoH)
* DNS over TLS (DoT)
* VPNs

many organizations can still potentially learn information about you.

Examples:

### Websites

May know:

* Your IP address
* Browser fingerprint
* Behavior patterns

### ISP

May know:

* You are using a VPN
* Traffic volume
* Connection timing

### VPN Provider

May know:

* Your real IP address
* Websites you access

Therefore:

You are still trusting someone.

Tor tries to reduce that trust requirement.

---

# 3. Core Goal of Tor

Tor's goal is:

> Make it extremely difficult to connect a user with their online activity.

Instead of sending traffic directly:

```
You
 ↓
Website
```

Tor routes traffic through multiple computers.

```
You
 ↓
Computer A
 ↓
Computer B
 ↓
Computer C
 ↓
Website
```

As a result:

No single computer knows the complete story.

---

# 4. Why It Is Called "The Onion Router"

An onion has many layers.

Tor works similarly.

Your data is wrapped in multiple layers of encryption.

Example:

```
Layer 3
 Layer 2
  Layer 1
   Original Data
```

Each relay removes only one layer.

Just like peeling an onion.

This is why it is called:

**Onion Routing**

---

# 5. What Is Onion Routing?

Onion Routing is a technique where:

1. Traffic is encrypted multiple times.
2. Traffic is sent through multiple relays.
3. Each relay removes one encryption layer.
4. No relay knows the complete route.

This creates anonymity.

---

# 6. Tor Network Architecture

A Tor connection typically involves:

### 1. Entry Node (Guard Node)

First relay.

### 2. Middle Node

Intermediate relay.

### 3. Exit Node

Final relay.

### 4. Destination Server

Website or service.

Diagram:

```
You
 ↓
Entry Node
 ↓
Middle Node
 ↓
Exit Node
 ↓
Website
```

This path is called a:

**Tor Circuit**

---

# 7. How Tor Builds a Circuit

When Tor starts:

It first downloads a list of available Tor relays.

These relays are computers run by volunteers worldwide.

Tor then randomly chooses:

* One Entry Node
* One Middle Node
* One Exit Node

Example:

```
You
 ↓
Germany Relay
 ↓
Brazil Relay
 ↓
Canada Relay
 ↓
Website
```

Tomorrow:

```
You
 ↓
Japan Relay
 ↓
France Relay
 ↓
Australia Relay
 ↓
Website
```

Routes constantly change.

This increases privacy.

---

# 8. Multiple Encryption Layers

Suppose:

You want to access:

```
https://example.com
```

Tor creates three encryption layers.

---

## Layer 1

Encrypted using Exit Node's public key.

Only Exit Node can decrypt.

---

## Layer 2

Encrypted using Middle Node's public key.

Only Middle Node can decrypt.

---

## Layer 3

Encrypted using Entry Node's public key.

Only Entry Node can decrypt.

---

Result:

```
Layer 3
 Layer 2
  Layer 1
   Request
```

This is the onion.

---

# 9. Journey Through the Tor Network

## Step 1

Your computer sends the onion.

```
You
 ↓
Entry Node
```

Entry Node removes outer layer.

It now learns:

* Your IP address
* Next relay

But not:

* Final destination

---

## Step 2

Traffic reaches Middle Node.

```
Entry Node
 ↓
Middle Node
```

Middle Node removes second layer.

Middle Node learns:

* Previous relay
* Next relay

But not:

* Your IP
* Final website

---

## Step 3

Traffic reaches Exit Node.

```
Middle Node
 ↓
Exit Node
```

Exit Node removes final layer.

Exit Node learns:

* Destination website

But not:

* Your IP address

---

## Step 4

Traffic reaches Website.

Website sees:

```
Exit Node IP
```

instead of:

```
Your IP
```

Therefore website cannot directly identify you.

---

# 10. What Each Node Knows

## Entry Node Knows

✓ Your IP

✓ Next relay

✗ Final destination

---

## Middle Node Knows

✓ Previous relay

✓ Next relay

✗ Your IP

✗ Destination

---

## Exit Node Knows

✓ Destination

✓ Traffic leaving Tor

✗ Your IP

---

This separation is Tor's major privacy advantage.

---

# 11. Why Websites Cannot Easily Identify You

Normally:

```
You
 ↓
Website
```

Website sees:

* Your IP
* Region
* ISP

With Tor:

```
You
 ↓
Entry
 ↓
Middle
 ↓
Exit
 ↓
Website
```

Website sees:

* Exit Node IP only

Therefore:

Your true location becomes much harder to determine.

---

# 12. IP Address Masking in Tor

Tor masks your IP address.

Instead of seeing:

```
49.205.x.x
```

the website might see:

```
185.x.x.x
```

belonging to a relay in another country.

This provides anonymity.

---

# 13. Why Tor Is Stronger Than a VPN for Anonymity

VPN:

```
You
 ↓
VPN
 ↓
Website
```

VPN provider knows:

* Your IP
* Destination

Tor:

```
You
 ↓
Entry
 ↓
Middle
 ↓
Exit
 ↓
Website
```

No single relay knows everything.

This greatly reduces trust requirements.

---

# 14. What Tor Protects Against

Tor helps protect against:

### ISP Monitoring

ISP sees:

```
You → Tor Network
```

ISP does not easily see final websites.

---

### Location Tracking

Websites cannot easily see your real location.

---

### IP-Based Tracking

Real IP address becomes hidden.

---

### Government Surveillance

Mass surveillance becomes more difficult.

---

### Advertiser Tracking

IP-based profiling becomes harder.

---

# 15. Why Tor Is Called Anonymous Communication

Anonymous means:

> Activity cannot easily be linked to an individual.

Tor's purpose is:

```
User Identity
      ↔
 Online Activity
```

Break the connection between them.

---

# 16. The Exit Node Problem

The Exit Node is powerful.

It sees traffic leaving Tor.

If traffic is not encrypted:

```
HTTP
```

the Exit Node can read it.

Example:

```
HTTP Login
```

Exit Node may see:

* Username
* Password
* Messages

Therefore:

### Never use HTTP over Tor.

Use:

```
HTTPS
```

instead.

---

# 17. Tor + HTTPS = Much Stronger Privacy

Best combination:

```
Tor
+
HTTPS
```

Tor hides:

* Who you are

HTTPS protects:

* What you send

Together they provide significantly stronger privacy.

---

# 18. Why Tor Is Slower

Tor traffic takes a longer route.

Normal:

```
You → Website
```

Tor:

```
You
 ↓
Relay 1
 ↓
Relay 2
 ↓
Relay 3
 ↓
Website
```

More hops mean:

* More latency
* More delay
* Slower downloads

This is the main performance cost of Tor.

---

# 19. Limitations of Tor

The lecture emphasized:

**Tor is not magic.**

It increases privacy.

It does not guarantee perfect anonymity.

---

## Limitation 1: Browser Fingerprinting

Even through Tor:

Websites can still examine:

* Screen size
* Fonts
* Language
* Time zone
* Browser behavior

This may help identify users.

---

## Limitation 2: User Mistakes

If you log into:

* Social media
* Gmail
* Banking

while using Tor,

you may identify yourself voluntarily.

Example:

```
Anonymous Tor User
      ↓
Logs into personal account
      ↓
Identity revealed
```

---

## Limitation 3: Malicious Exit Nodes

Exit nodes can observe unencrypted traffic.

Therefore HTTPS remains essential.

---

## Limitation 4: Traffic Analysis

Powerful attackers may analyze:

* Timing
* Traffic volume
* Packet patterns

to infer connections.

---

## Limitation 5: Not Invisible

People can still know you are using Tor.

Example:

Your:

* ISP
* University
* Employer

may notice:

```
User → Tor Entry Node
```

Even if they cannot see the final destination.

---

# 20. The Lecture's Important Warning

The instructor specifically explained:

> Tor raises the bar for tracking and surveillance, but it does not eliminate them.

This is one of the most important cybersecurity principles.

There is rarely:

```
100% security
100% privacy
100% anonymity
```

Instead there is:

```
Lower Risk
Higher Cost For Attackers
More Privacy
```

Tor is an example of this philosophy.

---

# 21. How Tor Connects to Previous Topics

Tor builds on almost every concept discussed earlier.

### Cookies

Tor helps reduce tracking through IP addresses, but cookies can still identify you if they are present.

### Tracking Parameters

Tor hides your IP but tracking parameters can still follow you across websites.

### Browser Fingerprinting

Tor helps, but fingerprinting may still work.

### DNS

Tor performs DNS resolution through the Tor network rather than exposing requests directly to your ISP.

### HTTPS

HTTPS protects data content.

### VPN

VPN hides activity from your ISP.

Tor attempts to provide stronger anonymity by using multiple relays.

### Encryption

Tor relies heavily on public-key cryptography and layered encryption.

---

# Final Summary

Tor (**The Onion Router**) is a privacy network that routes Internet traffic through multiple relays using **Onion Routing**. Traffic is wrapped in multiple encryption layers, and each relay removes only one layer. A typical Tor circuit contains an **Entry Node**, **Middle Node**, and **Exit Node**. No single relay knows both who you are and where you are going.

Tor helps protect against ISP monitoring, IP-based tracking, location tracking, and some forms of surveillance. However, it is not perfect. Browser fingerprinting, user mistakes, traffic analysis, and malicious exit nodes remain risks. Tor works best when combined with **HTTPS**, because Tor hides your identity while HTTPS protects the contents of your communications.

---

**Next Part:** Version A — Part 10: Permissions, Camera Permissions, Microphone Permissions, Contact Permissions, Location Permissions, GPS Tracking, and Privacy Implications of Mobile Devices.








# Version A — Part 10

# Permissions, Camera Permissions, Microphone Permissions, Contact Permissions, Location Permissions, GPS Tracking, and Privacy Implications of Mobile Devices

---

# 1. Introduction

In the final section of the lecture, the instructor moved away from:

* Cookies
* DNS
* VPNs
* Tor
* Browser tracking

and discussed another major privacy topic:

# Permissions

Modern smartphones and operating systems (OSs) increasingly ask users for permission before applications (apps) can access sensitive information or hardware.

Examples:

* Camera
* Microphone
* Contacts
* Photos
* Files
* GPS Location
* Bluetooth
* Motion Sensors

The goal is to give users more control over their privacy.

However, this also means that users must make informed decisions about what they allow apps to access.

---

# 2. What Are Permissions?

## Definition

A permission is:

> A rule enforced by the operating system that determines whether an application can access a specific resource, piece of data, or hardware component.

Examples:

| Resource   | Permission Needed        |
| ---------- | ------------------------ |
| Camera     | Camera Permission        |
| Microphone | Microphone Permission    |
| Contacts   | Contacts Permission      |
| GPS        | Location Permission      |
| Photos     | Photo Library Permission |

Without permission:

The app is blocked.

With permission:

The app can access that resource.

---

# 3. Why Permissions Exist

Imagine a phone without permissions.

Every app could automatically access:

* Camera
* Microphone
* Contacts
* Messages
* Photos
* Location

without your knowledge.

This would create enormous privacy risks.

Permissions were introduced to prevent this.

The operating system acts as a security guard between:

```text
User Data
     ↑
Operating System
     ↑
Applications
```

Apps must request access.

The operating system asks the user.

The user decides.

---

# 4. Operating System (OS)

## Definition

An Operating System (OS) is:

> The main software that manages a computer or smartphone and controls access to hardware and system resources.

Examples:

### Mobile Operating Systems

* Android
* iOS

### Desktop Operating Systems

* Windows
* macOS
* Linux

The OS enforces permissions.

Apps cannot simply bypass the OS.

---

# 5. Why Modern Operating Systems Ask for Permissions

Earlier smartphones gave apps broad access.

Modern systems increasingly ask:

```text
Allow Camera Access?
```

```text
Allow Microphone Access?
```

```text
Allow Location Access?
```

This is known as:

# Fine-Grained Permission Control

Fine-grained means:

Very specific control over what an app can access.

---

# 6. Camera Permissions

## Definition

Camera Permission allows an application to access the device's camera.

---

### Why Legitimate Apps Need Camera Access

Examples:

### WhatsApp

Needs camera access to:

* Take photos
* Record videos
* Video calls

### Google Meet

Needs camera access for:

* Video conferencing

### QR Scanner

Needs camera access to:

* Scan QR codes

---

# 7. Camera Privacy Risks

If camera permission is abused:

An app may potentially:

* Capture images
* Record videos
* Observe surroundings

This is why operating systems increasingly show indicators when cameras are active.

Examples:

### Android

Camera icon appears.

### iPhone

Green indicator light appears.

---

# 8. Microphone Permissions

## Definition

Microphone Permission allows an application to access the phone's microphone.

---

### Legitimate Uses

Examples:

### WhatsApp

* Voice messages
* Calls

### Zoom

* Meetings

### Voice Assistants

* Siri
* Google Assistant

---

# 9. Microphone Privacy Risks

If abused:

The app may record:

* Conversations
* Meetings
* Personal discussions

This is why operating systems now display indicators when the microphone is active.

Examples:

### iPhone

Orange indicator.

### Android

Microphone notification.

---

# 10. Contact Permissions

## Definition

Contact Permission allows an app to access your address book.

The address book contains:

* Names
* Phone numbers
* Email addresses
* Contact details

---

# 11. Why Apps Request Contact Access

Examples:

### WhatsApp

Uses contacts to determine:

* Which friends already use WhatsApp

### Telegram

Uses contacts to:

* Discover users

### Messaging Apps

Use contacts for easier communication.

---

# 12. Contact Privacy Risks

Giving access means:

The app may learn:

* Who you know
* Relationship patterns
* Social connections

This creates valuable data.

Example:

If an app knows:

```text
You ↔ Friend A
You ↔ Friend B
You ↔ Friend C
```

it can construct a social graph.

---

# 13. What Is a Social Graph?

## Definition

A Social Graph is:

> A map showing relationships between people.

Example:

```text
You
├── Friend A
├── Friend B
└── Friend C
```

Companies may use social graphs for:

* Recommendations
* Advertising
* Analytics

---

# 14. Photo Library Permissions

Many apps request access to:

* Photos
* Videos

Reasons:

### Legitimate

* Uploading images
* Editing photos

### Privacy Concern

Photos may contain:

* Faces
* Documents
* Locations
* Metadata

---

# 15. File Permissions

Apps may request access to:

* Documents
* Downloads
* Internal storage

Risk:

Sensitive files may become accessible.

Examples:

* Personal documents
* Financial information
* Identification documents

---

# 16. Location Permissions

The lecture spent considerable time discussing location permissions.

---

## Definition

Location Permission allows an app to determine where you are physically located.

This is often done through:

* GPS
* Wi-Fi
* Cellular networks
* Bluetooth beacons

---

# 17. Why Apps Want Location Data

Legitimate examples:

### Google Maps

Needs location for navigation.

### Uber

Needs location to find drivers.

### Weather Apps

Need location for forecasts.

### Food Delivery Apps

Need location for deliveries.

---

# 18. The Different Levels of Location Access

Modern operating systems often provide multiple options.

### Always

```text
Allow Always
```

App can track location continuously.

---

### While Using App

```text
Allow Only While Using App
```

Location is available only when the app is open.

---

### Ask Every Time

```text
Ask Each Time
```

User decides repeatedly.

---

### Never

```text
Deny Access
```

No location access.

---

# 19. Why "Always Allow" Can Be Risky

The instructor specifically emphasized this concern.

If location access is:

```text
Always Allowed
```

then tracking can continue:

* Day
* Night
* Weekends
* Holidays

even when you are not actively using the app.

---

# 20. GPS (Global Positioning System)

## Full Form

GPS = Global Positioning System

---

## Definition

GPS is:

> A satellite-based navigation system used to determine a device's geographic position.

---

# 21. How GPS Works

Many satellites orbit Earth.

Your phone receives signals from multiple satellites.

By comparing:

* Signal arrival times
* Satellite positions

the phone calculates:

* Latitude
* Longitude
* Altitude

This process is called:

# Trilateration

---

# 22. What Is Trilateration?

Trilateration is:

> Determining a location by measuring distance from multiple known points.

Example:

If your phone knows its distance from:

* Satellite A
* Satellite B
* Satellite C

it can calculate its position.

---

# 23. GPS Is Not the Only Location Technology

Phones also use:

### Wi-Fi Positioning

Nearby Wi-Fi networks help estimate location.

### Cellular Towers

Nearby cell towers help estimate location.

### Bluetooth Beacons

Short-range devices help determine position.

This often works indoors where GPS signals may be weak.

---

# 24. Why Location Data Is Extremely Sensitive

Location data can reveal:

* Home address
* Workplace
* School
* Travel patterns
* Daily routine
* Religious visits
* Shopping habits
* Social activities

Location data can sometimes reveal more about a person than browsing history.

---

# 25. The Lecture's Main Privacy Warning

The instructor emphasized:

Many users enable location access without thinking about its implications.

Example:

You may only want:

```text
Navigation
```

but the app may continuously learn:

```text
Where you live
Where you work
Where you travel
Where you spend weekends
```

---

# 26. Location History

Some companies maintain:

# Location History

Location history is:

> A stored record of where a device has been over time.

Example:

```text
8:00 AM → Home
9:00 AM → Office
1:00 PM → Restaurant
6:00 PM → Gym
8:00 PM → Home
```

Over months or years this becomes extremely detailed.

---

# 27. Mobile Devices as Tracking Devices

The lecture encouraged students to think carefully about smartphones.

A smartphone is simultaneously:

* Communication device
* Camera
* Microphone
* GPS receiver
* Internet-connected computer

This combination makes smartphones extremely powerful.

It also creates privacy concerns.

---

# 28. Why Privacy Is a Trade-Off

Many services require data.

Example:

Navigation requires:

```text
Location
```

Video calls require:

```text
Camera
Microphone
```

Messaging apps require:

```text
Contacts
```

Therefore:

There is a balance between:

```text
Convenience
        vs
Privacy
```

More convenience often requires more data sharing.

---

# 29. Best Practices for Permissions

### Grant Only Necessary Permissions

Ask:

```text
Does this app truly need this permission?
```

---

### Prefer "While Using App"

Instead of:

```text
Always Allow
```

use:

```text
While Using App
```

when possible.

---

### Review Permissions Regularly

Check:

* Camera
* Microphone
* Location
* Contacts

periodically.

---

### Remove Unused Apps

Unused apps may still have permissions.

---

### Be Skeptical

If a flashlight app requests:

```text
Contacts
Location
Microphone
```

that should raise questions.

---

# 30. How This Connects to Earlier Topics

Permissions connect directly to almost every topic in the lecture.

### Cookies

Track online behavior.

### Browser Fingerprinting

Identifies devices.

### DNS

Reveals websites visited.

### VPNs

Hide network paths.

### Tor

Hides identity through routing.

### Permissions

Control what data applications can collect directly from your device.

Thus:

```text
Cookies → Online Tracking

Fingerprinting → Device Identification

DNS → Website Discovery

VPN → Traffic Protection

Tor → Anonymity

Permissions → Device Privacy
```

All are different pieces of the privacy ecosystem.

---

# Final Summary

Permissions are operating-system-controlled rules that determine whether applications can access sensitive resources such as cameras, microphones, contacts, photos, files, and location data. Modern operating systems like Android and iOS increasingly provide fine-grained control over these permissions.

Location permissions are particularly sensitive because GPS (Global Positioning System), Wi-Fi positioning, cellular towers, and Bluetooth beacons can reveal a person's movements and daily routines. While permissions improve privacy, they also create a trade-off between convenience and security. Understanding and managing permissions is therefore a critical part of modern cybersecurity and privacy protection.

---

**Next (Final) Section:** **Version A — Part 11: Connecting All Topics Together, Complete Cybersecurity & Privacy Ecosystem, How Tracking Parameters, Cookies, Browser Fingerprinting, DNS, HTTPS, DoH, VPNs, Tor, Encryption, and Permissions All Interact, plus the Complete Final Lecture Summary.**











# Version A — Part 11 (Final)

# Connecting All Topics Together: The Complete Cybersecurity & Privacy Ecosystem

This final section ties together **everything covered throughout the lecture**. The goal is not merely to understand each technology individually, but to understand **how they interact**, **what problems each solves**, **what problems they do not solve**, and **how they collectively affect your privacy and security**.

---

# 1. The Big Picture

Whenever you use the Internet, many entities may potentially learn information about you:

* Websites
* Advertisers
* Analytics companies
* Internet Service Providers (ISPs)
* Wi-Fi operators
* Governments
* App developers
* Data brokers

The lecture showed that privacy is not threatened by one thing.

Instead, privacy can be affected by many different layers:

```text
Application Layer
↓
Browser Layer
↓
Operating System Layer
↓
Network Layer
↓
Internet Infrastructure Layer
↓
Physical Device Layer
```

Each layer leaks different information.

Each privacy technology attempts to protect one or more layers.

---

# 2. The Complete Journey of a Website Visit

Suppose you visit:

```text
https://www.example.com
```

Many things happen simultaneously.

---

## Step 1: DNS Resolution

Your device first asks:

```text
What is the IP address of example.com?
```

This process uses:

**DNS (Domain Name System)**

Without DNS:

You would need to memorize IP addresses.

Example:

```text
example.com
↓
93.184.216.34
```

---

### Privacy Problem

Traditional DNS is often unencrypted.

Your ISP may see:

```text
User requested example.com
```

even before you visit the website.

---

### Solution

Use:

* DNS over HTTPS (DoH)
* DNS over TLS (DoT)

These encrypt DNS requests.

---

# 3. Website Connection

Once the IP address is known:

Your browser connects to the website.

If the website uses:

```text
HTTP
```

traffic is unencrypted.

If the website uses:

```text
HTTPS
```

traffic is encrypted.

---

# 4. What HTTPS Actually Protects

HTTPS stands for:

**HyperText Transfer Protocol Secure**

HTTPS protects:

✔ Webpage contents

✔ Login credentials

✔ Messages

✔ Form submissions

✔ Cookies

✔ URLs after connection begins

---

HTTPS prevents:

* Eavesdropping
* Modification
* Cookie injection
* Super cookie insertion

by people in the middle.

---

# 5. What HTTPS Does NOT Hide

HTTPS does NOT hide:

* Your IP address
* The website you're connecting to
* Traffic volume
* Connection timing

The website still knows:

```text
You connected.
```

---

# 6. Tracking Parameters

Suppose you click:

```text
example.com/ad?click_id=ABC123&campaign=23
```

The website receives:

* click_id
* campaign

---

### Purpose

Campaign ID:

Tracks advertising campaign.

Click ID:

Tracks specific user behavior.

---

### Result

The server can record:

```text
User clicked ad
↓
Visited page
↓
Purchased item
```

---

Tracking parameters become part of:

* Server logs
* Databases
* Analytics systems

---

# 7. Cookies

The website may then place a cookie in your browser.

Example:

```text
ID=1234ABCD
```

The lecture compared cookies to:

# Virtual Handstamps

```text
Website stamps your hand.
```

Every future visit:

```text
Browser shows stamp again.
```

The website recognizes you.

---

# 8. Why Cookies Exist

Cookies are not inherently bad.

They enable:

* Logins
* Shopping carts
* User preferences
* Sessions

Without cookies:

Many websites would constantly forget who you are.

---

# 9. Session Cookies

Session cookies maintain:

```text
Logged In State
```

Example:

```text
Login Once
↓
Cookie Stored
↓
Stay Logged In
```

---

# 10. Tracking Cookies

Tracking cookies have a different purpose.

Instead of functionality:

They track behavior.

Example:

```text
Visited Site A
Visited Site B
Visited Site C
```

The same cookie follows you.

---

# 11. First-Party vs Third-Party Cookies

## First-Party Cookie

Created by:

```text
example.com
```

while visiting:

```text
example.com
```

Usually necessary.

---

## Third-Party Cookie

Created by:

```text
analytics-company.com
```

while visiting:

```text
example.com
```

Often used for tracking.

---

# 12. The Harvard–Yale–Stanford Example

The lecture used:

* Harvard
* Yale
* Stanford

All embedding:

```html
<img src="example.com/ad.gif">
```

The third-party server appears on all sites.

Therefore:

```text
Harvard Visit
↓
Yale Visit
↓
Stanford Visit
```

becomes visible to the third party.

---

# 13. Why Third Parties Become Powerful

Harvard only sees:

```text
Harvard Activity
```

Yale only sees:

```text
Yale Activity
```

But Google Analytics might see:

```text
Harvard
+
Yale
+
Stanford
+
Thousands of Other Sites
```

Therefore:

Third parties may know more than the websites themselves.

---

# 14. Browser Fingerprinting

Suppose cookies are deleted.

Tracking can still occur.

---

## Browser Fingerprinting

A website collects:

* Browser type
* Browser version
* Operating system
* Screen resolution
* Installed fonts
* Language
* Time zone
* Device type

---

Example:

```text
Chrome 136
Windows 11
1920×1080
English
UTC+5:30
```

Combined together:

This can uniquely identify you.

---

# 15. Privacy Browsers

Modern browsers increasingly fight back.

Examples:

* Safari
* Firefox
* Brave
* DuckDuckGo Browser

They attempt to:

* Block trackers
* Remove tracking parameters
* Block third-party cookies
* Reduce fingerprinting

---

Chrome generally offers fewer privacy protections by default because Google's business relies heavily on advertising and analytics.

---

# 16. Private Browsing / Incognito Mode

Many people misunderstand Incognito Mode.

It does NOT make you anonymous.

---

It only removes local traces.

When the session ends:

* Cookies disappear
* History disappears
* Cached files disappear

---

But websites can still see:

* Your IP
* Browser fingerprint
* Activity during the session

---

# 17. Super Cookies

Normal cookies are stored:

```text
Browser
```

Super cookies are injected by:

```text
ISP
Company
University
Mobile Carrier
```

---

These are difficult to remove because they are inserted into traffic.

---

# 18. Why HTTPS Stops Super Cookies

Without HTTPS:

```text
ISP
↓
Reads Traffic
↓
Modifies Traffic
↓
Injects Cookie
```

---

With HTTPS:

```text
Traffic Encrypted
↓
ISP Cannot Read
↓
ISP Cannot Modify
↓
ISP Cannot Inject
```

because encryption protects the contents.

---

# 19. Cookie Theft and Session Hijacking

If an attacker steals a session cookie:

```text
Cookie = Identity
```

they may impersonate the user.

This is called:

# Session Hijacking

---

Best practice:

Store:

```text
Random Session ID
```

inside cookies.

Store actual user data on the server.

---

# 20. SMS Insecurity

Traditional SMS:

* Is not end-to-end encrypted
* Can be intercepted
* Can be spoofed

---

# 21. SIM Swapping

Attackers trick a carrier into moving your phone number.

Result:

```text
Your Number
↓
Attacker's SIM
```

The attacker receives:

* Calls
* Texts
* Verification codes

---

# 22. End-to-End Encryption

End-to-End Encryption (E2EE) means:

Only sender and receiver possess decryption keys.

Not even the service provider can read messages.

---

Examples:

* WhatsApp
* Signal
* iMessage

---

### Telegram

Telegram only provides E2EE in:

**Secret Chats**

Normal Telegram chats are cloud-based and not end-to-end encrypted by default.

---

# 23. DNS Privacy Problems

DNS reveals:

```text
What website you want
```

before the connection even begins.

Your ISP may see:

```text
example.com
```

even if HTTPS is used later.

---

# 24. DNS Over HTTPS (DoH)

DNS queries travel inside HTTPS encryption.

Benefits:

* ISP cannot easily read DNS requests.
* Public Wi-Fi cannot easily monitor DNS activity.

---

# 25. DNS Over TLS (DoT)

Similar goal:

Encrypt DNS traffic.

Difference:

Uses TLS directly rather than HTTPS.

---

# 26. Virtual Private Networks (VPNs)

VPN stands for:

**Virtual Private Network**

Creates:

```text
Encrypted Tunnel
```

between:

```text
You
↓
VPN Server
```

---

# 27. What VPN Protects Against

VPN hides activity from:

* ISP
* Coffee shop Wi-Fi
* Airport Wi-Fi

Your ISP sees:

```text
Encrypted Tunnel
```

instead of individual websites.

---

# 28. What VPN Does NOT Protect Against

The VPN provider can still potentially see:

* Your activity
* Your connections

Therefore:

Trust shifts from:

```text
ISP
```

to:

```text
VPN Provider
```

---

# 29. IP Address Masking

VPN changes visible IP address.

Website sees:

```text
VPN IP
```

instead of:

```text
Your IP
```

---

# 30. Tor (The Onion Router)

Tor improves anonymity further.

Traffic travels through:

* Entry Node
* Middle Node
* Exit Node

---

Each layer is encrypted.

No single node knows:

* Who you are
* Where you are going

at the same time.

---

# 31. Onion Routing

Traffic is encrypted multiple times.

Like layers of an onion.

Each relay removes only one layer.

---

# 32. Tor vs VPN

VPN:

```text
One Trusted Party
```

Tor:

```text
Multiple Relays
```

reducing trust requirements.

---

# 33. Tor Limitations

Tor is not magic.

It does not eliminate:

* Browser fingerprinting
* User mistakes
* Traffic analysis
* Malicious exit nodes

It merely increases privacy.

---

# 34. Permissions

Permissions protect data stored directly on your device.

Examples:

* Camera
* Microphone
* Contacts
* Files
* Photos
* Location

---

# 35. Camera Permissions

Allow apps to:

* Take photos
* Record video
* Conduct video calls

Risk:

Potential surveillance.

---

# 36. Microphone Permissions

Allow apps to:

* Record audio
* Conduct calls
* Use voice assistants

Risk:

Listening to conversations.

---

# 37. Contact Permissions

Allow apps to access:

* Names
* Numbers
* Emails

Risk:

Building social graphs.

---

# 38. Location Permissions

Allow apps to know:

* Current location
* Movement history

through:

* GPS
* Wi-Fi
* Cellular networks

---

# 39. Why Location Is So Sensitive

Location data may reveal:

* Home
* Workplace
* Habits
* Daily routines
* Travel patterns

Sometimes location data reveals more than browsing history.

---

# 40. The Complete Privacy Ecosystem

Everything connects.

```text
DNS
↓
Find Website

HTTPS
↓
Protect Content

Cookies
↓
Remember User

Tracking Parameters
↓
Track Clicks

Analytics
↓
Track Behavior

Fingerprinting
↓
Identify Devices

Permissions
↓
Access Device Data

VPN
↓
Hide Activity From ISP

Tor
↓
Increase Anonymity

End-to-End Encryption
↓
Protect Messages

Privacy Browsers
↓
Reduce Tracking
```

---

# 41. The Central Lesson of the Entire Lecture

The single most important lesson is:

> Privacy is not one technology.

Privacy is a collection of technologies, policies, decisions, and trade-offs.

No single tool solves everything.

---

## HTTPS protects content

But not identity.

---

## VPN protects traffic from your ISP

But requires trusting the VPN provider.

---

## Tor increases anonymity

But is not perfect.

---

## Privacy browsers reduce tracking

But cannot eliminate all fingerprinting.

---

## Permissions protect device data

But users must make good decisions.

---

## End-to-End Encryption protects messages

But not necessarily metadata.

---

# 42. Final Cybersecurity Philosophy

The lecture repeatedly emphasized a key cybersecurity principle:

```text
There is no perfect privacy.
There is no perfect security.
```

Instead:

```text
Reduce Risk
Increase Protection
Raise the Cost of Attack
Make Tracking Harder
Make Surveillance Harder
Make Identification Harder
```

Good cybersecurity is about layers.

---

# Final Lecture Summary

The lecture began with tracking parameters and cookies, moved through third-party tracking, browser fingerprinting, privacy-focused browsers, incognito mode, super cookies, session hijacking, SMS insecurity, end-to-end encryption, DNS privacy, DNS over HTTPS, DNS over TLS, VPNs, Tor, permissions, GPS tracking, and mobile privacy.

Together, these topics form a complete picture of modern Internet privacy. Every website visit, every DNS query, every app permission, every cookie, and every network connection potentially reveals information. Technologies such as HTTPS, DoH, VPNs, Tor, privacy browsers, end-to-end encryption, and permission controls each protect different parts of that information. Understanding how all of these pieces fit together is the foundation for making informed privacy and cybersecurity decisions in the real world.

**This completes Version A (Parts 1–11) of the CS50 Cybersecurity Privacy Lecture notes.**


































































































































