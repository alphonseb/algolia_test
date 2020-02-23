# Formula 1 Race Browser (Algolia Test)

Bootstrapped from Algolia's [vue-instantsearch-starter](https://github.com/algolia/vue-instantsearch-v2-starter)

This is a search UI revolving around Formula 1 and its races from the last two seasons.
I am a **massive** F1 fan and since it is currently the preseason it felt right to chose F1 data to play around with.
It also meant that I could be the end user of my search UI and know what an F1 fan would expect from such a search form. This helped me a lot setting up everything in the Algolia dashboard.

There was not actually many data sources to choose from so I had to put together several datasets to get a final one that would be meaningful to search.

## Facets

Based on my experience as an F1 Fan, I chose the following attribute to be used as filters :

- Season
- Winning driver
- Winning constructor

## Searchable Attributes

For a race browser, of course the race name and location were going to be searchable attributes.

But I also decided to make the top 3 drivers and constructors names searchable because it made sense in a kind of "dataviz" use case. E.g. "I wonder how many podiums did Hamilton get in 2018 ?" -> "Hamilton 2018" and there you have it.

## Feedback

I had a lot of fun building this out. Choosing my own dataset had a good part in that but I also liked discovering the Algolia product. The dashboard is very intuitive and as a new user I was never lost.

The Instant Search components library is also really great and the documentation is very clear. I feel it could maybe be a bit easier to customize the styling. For instance the color of the text seemed to be applied to a lot of deep nested classes and you could not (or at least I did not find how to) change easily the text color for every component at once (like changing the color on `body` would).