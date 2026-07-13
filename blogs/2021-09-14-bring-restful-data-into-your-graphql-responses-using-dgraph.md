---
title: "Bring RESTful Data Into Your GraphQL Responses Using Dgraph Lambda - Dgraph Blog"
url: "https://discuss.dgraph.io/t/bring-restful-data-into-your-graphql-responses-using-dgraph-lambda-dgraph-blog/15558"
date: "2021-09-14"
author: "diggy"
feed_url: "https://discuss.dgraph.io/c/blog/.rss"
---
If you have a RESTful API that performs some business logic or 3rd party info that isn’t within your data, you’ve likely thought about how to use this data within your GraphQL API. Most of the time, a RESTful service may be used by sources outside of just your application, so moving it over to GraphQL may not make sense. Conversely, you may not even own the RESTful service but still want to expose that data through your GraphQL API for your users.
