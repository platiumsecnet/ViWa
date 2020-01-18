Forcepoint NGFW vs pfSense Comparison
---

*_Forcepoint NGFW_*
	If you are looking for a smaller network/security team, the ease and low complexity create an easy to manage environment. 
	One engineer can easily manage 100 nodes/locations. 
	If you are just starting to get security conscious and predict regular adjustments to policy, routing, and access, this is a very good system for making easy to understand and low impact changes on a regular basis without operations interruption.

*_pfSense_*
	For fast-growing or SME companies, pfSense is quite suitable because pfSense already had many advanced features such as VPN and multiple WAN / LAN. 
	As a result, we just need to pay for expensive router frequently to upgrade our infrastructure.

# 1. Feature Rating Comparison
|No|--Criterial--|--Forcepoint NGFW score--|--pfSense score--|
|--|-------------|-------------------------|-----------------|
|1|Total|8.1|7.2|
|2|Identification Technologies|5.0|*3.0*|
|3|Visualization Tools|5.0|6.0|
|4|Content Inspection|10.0|*8.0*|
|5|Policy-based Controls|10.0|*7.0*|
|6|Active Directory and LDAP|8.0|*4.0*|
|7|Firewall Management Console|10.0|*8.0*|
|8|Reporting and Logging|8.0|*6.0*|
|9|VPN|9.0|10.0|
|10|High Availability|9.0|9.0|
|11|Stateful Inspection|7.0|9.0|
|12|Proxy Server|—|9.0

# 2. Pros
|No|--Forcepoint NGFW--|--pfSense--|
|--|-------------------|-----------|
|1|Easy to manage and make changes on - ACL's are done with ease.|pfSense is an excellent firewall - It logs all of your traffic. It has packages you can install to snort bad traffic.|
|2|Easy USB initial configuration - The easy initial setup of a new location and firewall saves massive time. Settings are automatically pushed to new nodes upon contact with the controller.|pfSense has a tool called "p0f" which allows you to see what type of OS is trying to connect to you. You can filter these results and you can also block a specific OS from connecting to you.|
|3|Low Complexity - This system does not have a lot of complexity requiring extra hours, training, or personnel to manage.|pfSense is an excellent load-balancer: (Multi-WAN and Server Load Balancing) The fail-over/aggregation works very well. This is perfect if your business uses multiple ISP's to ensure your customers are always able to access their data. Also helps with bandwidth distribution as well.|
|4||VPN's - I am not entirely sure if this package was free with pfSense, but it does offer the ability to use OpenVPN which is what I am familiar with.|
|5||They also have IPsec in the settings as well, but I am not familiar with that enough to go into any detail with it.|
|6||As I mentioned I do use OpenVPN the only thing I don't care for with it is I can create OpenVPN configs for each user I want to be able to VPN into the network and I assumed each one would be "unique" but this does not seem to be the case. I could be doing it wrong, but if I create a config for a specific employee I would expect only that employee should be able to use that config, but I have been able to login to everyone that I made using my credentials.|
|7||I mentioned earlier that pfSense had a GUI.|
|8||I personally really think it is cool because it has a bunch of reporting graphs for monitoring your networks. I think when I become the full-time admin at the company I am going to try to talk them into getting me a TV I can mount on the wall and display all the graphs and real-time info pfSense shows so I can monitor what is going on with the network(s) at all times. Plus I think it would look rad.|

# 3. Cons
|No|--Forcepoint NGFW--|--pfSense--|
|--|-------------------|-----------|
|1|Poor Reporting - It exists but even when calling in to support for assistance, they have no idea how to tackle customizing reports or searching for specific data.|There is no API for making changes. This can be a hindrance in environments where auto-deploying something needs firewall rules or HAProxy configs updated. Since all settings are stored in an XML file and then configs are generated from that, even manually updating config files cannot be done.|
|2||Beware that some network cards can have issues. pfSense is based on FreeBSD, so it's best to look on their compatibility list before deploying.|
