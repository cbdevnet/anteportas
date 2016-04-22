# About
anteportas aims to provide a minimal, highly configurable implementation
of a captive portal system. A captive portal forces users to authenticate
or at least actively indicate agreement to conditions before allowing their
packets to traverse a router.

# Why
Out of interest and a (perceived?) lack of alternatives (that are not in some way
integrated into some much larger accounting system).

# Requisites

## On the router
* iptables
* ipset and the iptables ipset module
* HTTP daemon with CGI

## On you
* Some confidence with iptables
* Some experience in bash programming
* A way to authenticate your users from bash
* Some HTML skills probably

# Setup

1. Create `active` ipset using
	```ipset create active hash:mac counters timeout 900```

2. Turn on packet forwarding with
	```sysctl net.ipv4.ip_forward``` and
	```net.ipv6.conf.all.forwarding=1```

3. Check portal.rules for applicability to your setup and customize if needed

4. Load firewall rules from portal.rules using `iptables-restore` or `iptables-apply`
	(and dont forget ip6tables)

5. Copy `process_login` from wwwroot/cgi-bin/ to a location where it can be executed
	using CGI.

5a. Optionally, also copy index.htm and failure.html for use as login panel (they're
	not pretty, though). They may be useful as an example on how to create a login
	page.

6. Edit`process_login`
	* Configure `check()` to match your authentication method
	* Configure the `REDIRECT_*` variables to point to something sensible

# Testing

To see the list of clients currently able to traverse the router, run
	```ipset list active```
