# TOR hidden services network/graph Analysis using NetworkX
Tor hidden services (.onion) analysis

Today, we live in a world where we can access the Internet from virtually every device we own. However, as the Internet expands and becomes more available, so do the assaults on people who use it, by spying on and stealing their content. Tor is a volunteer-based anonymity network in which Tor network members can join to expand the network's scale and randomness. The Tor network is a network of relays that routes a client's traffic before delivering it to the client's final destination. When a client's traffic is traversing the Tor network, the origin of the traffic is no longer known, and the final destination is only known at the Tor network's exit point. Tor even encrypts device traffic as it is in the Tor network. It defends users from threats who spy on a network to attempt to intercept a user's information in this manner.



## Methods
### Data Collection
This section describes the crawling method used to collect the data, as well as its pre-processing. The generation of the network is also described in this section.  

#### Web Crawling
There is no available data on the Tor web services network, so in order to analyze the network, the data was generated using scraping/crawling techniques. The data for the analysis was gathered using a handcrafted web crawler. The crawler was written in Python using [Selenium](https://www.selenium.dev) library for connection to the [Tor Browser](https://www.torproject.org) via a [SOCKS proxy](https://en.wikipedia.org/wiki/SOCKS). The crawler received the initial onion domains (websites) from the surface web's publicly accessible list of [Tor links](https://thehiddenwiki.org). It scrapes all *.onion* links (hosted on hidden servers) on a web page and saves connections between the main domain of that page and all the links' domains discovered. Then new links are crawled again to find more new connections to each of those links. The scraping of new connections is repeated until the network converges (i.e. no more links and nodes could be added). The crawler did not scrape further for secret utilities that needed user authentication or were behind subscription pay-walls.

Note, since hidden services occasionally relocate their websites to different addresses, the scrapped web links often are not accessible anymore, such websites are removed from the network.

Lastly, after the web crawler finished the process of collecting the web links, all the websites were revisited to label the theme/category of the service manually. 

#### Network
The network structure is simple. *Nodes* represent the hidden domains (websites) and *links/edges* represent connections and references between those websites. The network is *directed*. The final generated network consists of **N=197** nodes and **E=221** edges. The overall picture of the network is represented in Figure.

![alt text](https://github.com/dalyapraz/TOR_networkx/blob/main/2.png "Network")


According to Tor Project Inc. estimates, there are on average [100 000](https://metrics.torproject.org/hidserv-dir-onions-seen.html}) hidden services accessible online on any given day. However, my crawler was only able to find around **N=197** such websites online. The difference in the number of secret services may be due to Tor-based messaging platforms like [TorChat](https://en.wikipedia.org/wiki/TorChat), where each active user is given a special 16-character *.onion* address that can't be differentiated from the website address. On top of that, the crawler did not go beyond the websites with login systems and pay-walls. Another limitation is the initial seed/website link that originated the web crawler. Since there might be websites that are disconnected at all from the original seed, such websites were not found by the crawler. 

This project studies the nature of the dark network at the domain level, so all sub-domains of a certain domain are aggregated under it and denoted as a single individual node in a graph. So this could also have shrunk the number of nodes also. For instance, a domain *ABC.onion* with sub-domains *en.ABC.onion* and *ru.ABC.onion*, each of which and their all internal web pages, such as *ABC.onion/feed* and *ABC.onion/blog*, are viewed as a single node of the network. As a result, every edge in the graph from node *X* to node *Y* represents a hyperlink within domain *X*'s web page leading to a web page contained in the domain *Y*. All nodes are hidden web domains, domains that do exist on the Internet surface are not included in the network (i.e. github.com, facebook.com, etc.). The links that point within the web pages of the same domain (self-loops) are dropped from the network. 

Sources: [1] [Web Crawling with Selenium](https://towardsdatascience.com/how-to-scrape-the-dark-web-53145add7033) 