---
title: "Dynamic AutoScaling of GitHub Runners - Dgraph Blog"
url: "https://discuss.dgraph.io/t/dynamic-autoscaling-of-github-runners-dgraph-blog/18309"
date: "2023-02-21"
author: "diggy"
feed_url: "https://discuss.dgraph.io/c/blog/.rss"
---
In this article we explain our transition to GitHub Actions for our CI/CD needs at Dgraph Labs Inc . As a part of this effort we have built (in-house) & implemented a new architecture for “Dynamic AutoScaling of GitHub Runners” to power this setup. In the past, our CI/CD was powered by a self-hosted on-prem TeamCity setup - this turned out to be a little difficult to operate & manage in a startup setting like ours.
