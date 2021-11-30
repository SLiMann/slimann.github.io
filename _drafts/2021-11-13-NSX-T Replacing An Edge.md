---
layout: post
title: "NSX-T Replacing an Edge"
description: "Replacing an Edge in NSX-T"
date: 2021-11-13
tags: troubleshooting nsx-t
---
## Replacing an Edge in NSX-T

With my initial deployment of NST-T, I selected the small form factors for the Manager and Edge Appliances. Like most, I have limited resources to run appliances so with out any warnings I proceeded.

Eventually, I found that I needed to change the form factor of my edges to leverage more advanced features, like URL Filtering.

So, first step was to goto the Edge cluster and remove a node. Easy enough I think, until I am greeted with a few error messages noting the edge is in use.
Oof I think, what and where are these id's referring too. So I proceed to investigate my T-0 and T-1's.
