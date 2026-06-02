# CS50-Intro-to-Cybersecurity-2
Tracking parameters First-party vs third-party cookies Google Analytics tracking Browser privacy protections Incognito mode limitations Supercookies DNS privacy issues DNS over HTTPS (DoH) DNS over TLS (DoT) DNS spoofing VPNs Tor (The Onion Router) App permissions Location tracking How all of these systems interact

A complete **Version A** that truly includes *every concept, explanation, example, student question, answer, connection, and additional background material* from that transcript would be far larger than what can fit in a single response (it would be closer to a small book chapter, roughly 25,000–30,000+ words).

To do it properly without omitting content, I need to deliver Version A in multiple parts.

# 

## Cybersecurity, Privacy, Tracking, DNS, VPNs, Tor, Cookies and Modern Internet Surveillance

---

# 1.1 Introduction

This section of the CS50 Cybersecurity lecture focuses on one of the most important topics in modern computing:

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

* Session identifiers
* User preferences
* Login information
* Tracking identifiers

A cookie is stored on your computer or phone.

---

## Why Were Cookies Created?

Cookies were originally created because HTTP is stateless.

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

An important point:

Cookies themselves are not bad.

Cookies are merely a technology.

They are useful for:

### Login Sessions

Example:

You log into Gmail.

Google stores a session cookie.

Now Google remembers:

* Who you are
* That you're authenticated

---

### Shopping Carts

Example:

Amazon shopping cart.

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

YOU

not millions of users

but specifically you.

Whenever your browser visits certain websites:

That identifier is sent again.

This allows organizations to build a profile of:

* Pages visited
* Products viewed
* Ads clicked
* Interests
* Habits

---

# 1.6 Tracking Parameters

The lecture then introduces another tracking mechanism:

## HTTP Parameters

Parameters often appear in URLs.

Example:

```text
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

# 1.7 Multiple Parameters

URLs can contain multiple parameters.

They are separated using:

```
&
```

Example:

```
https://example.com/page?a=1&b=2&c=3
```

Parameters:

```text
a=1
b=2
c=3
```

---

# 8. Legitimate Uses of Parameters

Parameters are often useful.

Examples:

### Search Engines

```text
?q=cats
```

---

### Filters

```text
?price=1000
```

---

### Pagination

```text
?page=5
```

---

### Language Selection

```text
?lang=en
```

---

These are normal uses.

---

# 9. Tracking Parameters

However, parameters can also be used for tracking.

Example from the lecture:

```text
https://example.com/ad_engagement?
clickID=9x82jsh272j
&campaignID=23
```

Two parameters exist:

### campaignID

```text
campaignID=23
```

This identifies the advertising campaign.

---

### clickID

```text
clickID=9x82jsh272j
```

This identifies the individual user.

This is effectively acting as a tracking cookie.

---

# 10. Why Tracking Parameters Are Dangerous

Suppose:

User A gets:

```text
clickID=AAA111
```

User B gets:

```text
clickID=BBB222
```

User C gets:

```text
clickID=CCC333
```

Now every click can be linked back to a specific user.

Advertisers can track:

* What ads you saw
* Which ads you clicked
* Which websites you visited afterward

---

# 11. Tracking Parameters vs Cookies

Tracking cookies:

Stored inside browser storage.

Tracking parameters:

Visible directly inside URLs.

Example:

Cookie:

```text
ID=1234ABCD
```

may be hidden.

Tracking parameter:

```text
?clickID=1234ABCD
```

is visible in the URL.

---

# 12. Server Logs

Why do tracking parameters work?

Because servers keep logs.

---

## What Is a Server Log?

A server log is a record of requests.

Example:

```text
09:00 User requested:
page1
```

```text
09:05 User requested:
page2
```

```text
09:10 User requested:
page3
```

The server remembers.

---

# 13. Tracking Through Logs

Suppose every request contains:

```text
clickID=1234ABCD
```

The log becomes:

```text
09:00 page1
ID=1234ABCD
```

```text
09:05 page2
ID=1234ABCD
```

```text
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

---

# 16. Automatic URL Sanitization

Modern browsers may detect:

```text
clickID=
```

or similar tracking values.

Before opening a link:

The browser removes:

```text
clickID=...
```

This process is called:

### URL Sanitization

or

### URL Scrubbing

---

# 17. Limitations

This defense is imperfect.

Why?

Because trackers constantly invent:

new names.

Example:

Today:

```text
clickID
```

Tomorrow:

```text
visitorToken
```

Next week:

```text
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

## End of Version A – Part 1

The remaining sections still include:

* First-party vs Third-party Cookies
* Google Analytics
* Harvard/Yale/Stanford example
* Cross-site tracking
* Incognito Mode
* Supercookies
* SMS insecurity
* End-to-End Encryption
* DNS
* DNS Over HTTPS
* DNS Over TLS
* DNS Spoofing
* VPNs
* Tor (The Onion Router)
* Layered Encryption
* Permissions
* Camera/Microphone access
* GPS tracking
* Final cybersecurity principles

These will require several more parts to cover with the level of detail you requested.

In your next message, I will continue with **Version A – Part 2 (Third-Party Cookies, Google Analytics, Cross-Site Tracking, and Browser Privacy Protections)** and then later move on to Version B.
