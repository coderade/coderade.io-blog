+++
date = "2022-04-28T21:17:21+02:00"
lastmod = "2022-04-28T18:14:00+00:00"
tags = ["projects", "go", "react", "graphql", "postgres"]
categories = ["projects"]
title = "Games Shelf REST/GraphQL App developed with Go and React "
description = "In this post I will try to share my experience creating a Games Shelf REST/GraphQL project using Golang and React functional and class components."
meta_image = "images/posts/projects/go-react-rest-graphql-app/go-react-api.png"
+++

![Project image](/images/posts/projects/go-react-rest-graphql-app/go-react-api.png)

Since I started using Go on a project at my current job, I decided to create some real projects with the language to understand it better.

After studying some patterns and understanding how Go works, I decided to create a **Games Shelf Application** with REST communication, and later, I added GraphQL to the mix.

I chose [PostgreSQL](https://www.postgresql.org/) as my database and [React.js](https://reactjs.org/) for the front-end. Even though I haven't used them for a long time, both are my preferred technologies for databases and front-end frameworks.

The most challenging part of developing this API was my lack of expertise in Go. In fact, I am a newbie in the language, having started learning Go just one month ago, and I'm already using it at my job.

Even though Go is one of the most popular programming languages, there are not as many example projects on GitHub compared to languages like Python and Node.js.

I encountered many problems trying to refactor the code to follow a good API structure, especially when trying to share some context variables from the `main` package.

Initially, I did not use an ORM module like [GORM](https://github.com/go-gorm/gorm) because I wanted to understand how to work directly with the database, including opening connections and similar tasks. As a result, I had to handle all the SQL operations manually. In the future, I plan to use the `GORM` module to explore its possibilities.

For the front-end, I started by creating React components using classes but later switched to functional components. This change came after facing many issues and realizing that functional components have less boilerplate code and are easier to understand. [The future of React is Functional components](https://hackernoon.com/react-functional-components-are-the-future).

I learned a lot about Go and refreshed my React skills through this project, so I am happy to have finally finished it.

### Application Possibilities:

Currently, the application offers the following features:

- Login using JWT tokens (the user and password need to be managed on the backend project)
- CRUD operations for games, platforms, and genres
- Connect with a PostgreSQL database
- Use GraphQL to search for games
- Work with data received from the RAWG API, the largest video game database
- Data like game images and ratings are fetched from this API.

I developed two projects for this application, and both are available on GitHub with documentation on how to use and run them.

The source code for the Go API is available here: [**games-shelf-api-go**](https://github.com/coderade/games-shelf-api-go).

And the source code for the React client application is here: [**games-shelf-client-react**](https://github.com/coderade/games-shelf-client-react).
