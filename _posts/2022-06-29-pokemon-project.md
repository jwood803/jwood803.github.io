Pokemon Project
================

## Project

This project involved using the Pokemon API
(<https://pokeapi.co/docs/v2>) and creating functions to call specific
endpoints to get the data. There are several endpoints in the API
involving from one that can give Pokemon characteristics all the way to
one that can give details on locations that are in all of the games.

The project page can be found
[here](https://jwood803.github.io/pokemonproject/) and the GitHub repo
is [here](https://github.com/jwood803/pokemonproject)

### Difficulties

There were a couple of difficulties when doing this project.

The first involved trying to find an endpoint that can be useful for
exploratory data exploration. I still haven’t been able to find a decent
endpoint for this but I did try to combine some data.

The second difficulty was a code one. This involved trying to get the
resulting data from the API into an easily readable and usable format.
The best thing that helped with this was finding the `unnest` method in
the `dplyr` package. This allowed nested data frames to be flattened
into the current data frame which made returning that data much easier.

### Post-mortem

The main thing I would do differently when approaching this project or
even this API is to look more closely at what other languages do with
certain endpoints. How do they handle the data, especially the nested
data? Which endpoints did they do first?
