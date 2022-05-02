+++
date = "2022-04-28T21:17:21+02:00"
lastmod = "2022-04-28T18:14:00+00:00"
tags = ["projects", "go", "react", "graphql", "postgres"]
categories = ["projects"]
title = "Games Shelf REST/GraphQL App developed with Go and React "
description = "Sharing my opinion about creating a Games Shelf REST/GraphQL project using Golang and React "
meta_image = "images/posts/projects/go-react-rest-graphql-app/go-react-api.png"
+++

![Project image](/images/posts/projects/go-react-rest-graphql-app/go-react-api.png)

Since I started to use Go on a project at my current job, I decided to create some real projects using the language to understand the language more.


After studying some patterns and understanding how the language works, I decided to create a **Games Shelf Application** with REST communication, and then, I added GraphQL to the game.


I decided to use [PostgreSQL](https://www.postgresql.org/) as my database and [React.js](https://reactjs.org/) in the front-end, because, even as I have not used it for a long time, both are my preferred technologies when I think about a database and a front-end framework.

The most difficult things for me to develop when I was developing this API was mainly that I was not a GoLang expert, and, very different than that, I am a newbie in the language, as I started to learn Go nothing more than one month ago, and I am already using it in my job.

Even though Golang is one of the most popular programming languages, we do not have a lot of examples of projects on Github when comparing it with languages like Python and Node.js.


In this way, I had a lot of problems trying to refactor the code to follow a good API structure, mainly trying to share some context variables from the `main` package.

Also, at first I did not try to use an ORM module like [GORM](https://github.com/go-gorm/gorm), because I was trying to understand how to work directly with the database, opening connections and that kind of stuff. In this way, I needed to do all the SQL stuff and in the future I will try to use the `GORM` module to understand its possibilities.

In the front-end I started to create components in react using classes, but after I changed all for class components, after having a lot of problems and understanding that , less boilertplate code and easy to understand.

At the front-end, I started creating components for React using classes, but then I changed all the functional components, after having a lot of problems and understanding that [The future of React is Functional components](https://hackernoon.com/react-functional-components-are-the-future), less boilerplate code and easier to understand.

I learned a lot of Go and I remembered how to use React in a project, so I am happy that I have finally finished it.

###  Application possibilities:


Currently we  have the following possibilities on the application:

- Login using JWT tokens (the user and password need to be managed on the backend project)
- CRUD of the games, platforms and genres
- Connect with a PostgreSQL Database
- Use GraphQL to search for games
- Work with data received from RAWG api, the biggest video game database
  - Data like the games' images and ratings will be got from this API.

I develepod two projects from this application, and both are available on the GitHub and documented how to use and run them.

The source code for the GO API is available here: [**games-shelf-api-go**](https://github.com/coderade/games-shelf-api-go)

And the source code for the React client application is here: [**games-shelf-client-react**](https://github.com/coderade/games-shelf-client-react)