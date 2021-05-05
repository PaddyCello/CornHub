# CornHub
Gardening app for hardcore gardeners

# Project Brief

In one week, build a full-stack app (PostgreSQL, Python/Django, React) with CRUD functionality, working solo, in a pair, or in a group of three. I opted for the latter, and recruited Jonty Ward and Michael Henderson to form a team.

# Link to Deployed Project

[cornhubapp.herokuapp.com](http://cornhubapp.herokuapp.com/)

# Overview and Concept

I am an avid gardener, and find that I often have to consult multiple sources in order to find all of the information I need concerning sowing, fertilizing, watering and any number of other crucial matters. I thought that it would be wonderful to have an app where all of this information was at my fingertips, with several different approaches to searching and filtering - for example, by month, name, species, temperature or difficulty. Additionally, I wanted it to have a dark, humorous aesthetic, as my own experience of tilling clay-like soil in 30 degree heat with thrash metal in my headphones didn’t really match with the cosy, middle-class image that gardening can often have. Accordingly, I called it CornHub.

![screenshot](https://github.com/PaddyCello/CornHub/blob/fc04d2a2de8a5afbe36b9899879d7c9caedd862a/screenshots/Screenshot%202021-05-02%20at%2008.15.04.png)

![screenshot](https://github.com/PaddyCello/CornHub/blob/818e2cb25a6a66f568ac9ccec76cd772453c804a/screenshots/Screenshot%202021-05-02%20at%2008.15.51.png)

![screenshot](https://github.com/PaddyCello/CornHub/blob/818e2cb25a6a66f568ac9ccec76cd772453c804a/screenshots/Screenshot%202021-05-02%20at%2008.16.53.png)

# Technologies Used

Python
Django
PostgreSQL
React
JavaScript
CSS
React-Bootstrap
GitHub
Heroku

# Approach Taken

I actually came up with the concept a few days before we were set to begin the project, which enabled me to write some high-level pseudocode on a Google Doc ahead of time. This also meant that I had more leverage when I was finding people for the team, as I had an idea that I could show to them and talk them through. Finding the right people was vital, and I had a few criteria that needed to be met:

- My teammates had to be keen gardeners, otherwise the project would be less enjoyable for all involved, and potentially the database smaller and less accurate. Additionally, this would make it more likely that all three of us would bring ideas to the table, well and truly making it into a team venture.
- My teammates had to have a sufficiently warped sense of humour.
- My teammates had to be skillful enough as developers to meet, and add to, the ambition of the project.

Fortunately, Jonty and Michael turned out to be perfect for the team, and were both sold on the concept pretty much immediately when I told them about it. More pseudocode was promptly written, with post-MVP features even listed in order of preference, and wireframes of design layout and user flow were made on Exacalidraw. We started a Trello for visibility, task assignment and assessment of priorities, and a Google Sheet for collecting information for our database. Crucially, we outlined models for our User and our Plants with every field that we felt we might conceivably need, so as to avoid having to make subsequent changes.

![screenshot](https://github.com/PaddyCello/CornHub/blob/6c468435f15c3b90438d8e01de51a4bd933a1928/screenshots/Screenshot%202021-05-05%20at%2015.46.44.png)

We were at this stage still getting to grips with Django, and were looking to avoid potential problems; this also meant that our starting point was to work in a group until our back end was built out completely (albeit with some fairly test data) and our front end was attached, with a skeleton of components waiting to be filled with code. We also spent an evening individually researching CSS frameworks, and decided that React-Bootstrap was the one for the job.

With all of this done, it became much easier to assign solo tasks. As a general rule throughout the project, we worked as a full group on the most complex tickets, and saved the more straightforward ones for evenings and the weekend. This was not our original plan, but we discovered while building the back end together that we had a terrific dynamic as a group, both in terms of skills and morale, and it made sense to take advantage of this as much as possible.

One of our ongoing solo tasks was of course adding to our data set. This proved to be quite time consuming, owing to the number of fields on the Plants model, and our emphasis on accuracy. Thankfully, spending hours researching potatoes and courgettes was hugely enjoyable.

One of my first tasks was to build out the main index page for the plants. I had wanted something that was searchable in multiple ways, and would also filter data according to current month (one category each for sow, plant and harvest), with a carousel of random plants as a hero image - perhaps a loose homage to Netflix. The selections for current month proved to be straightforward, as I had the foresight to consider JavaScript date objects when building out the model in Django - months are numbers instead of strings, and zero-indexed, thus making them easy to evaluate against a new Date(). However, displaying the current month as a string took a bit more research; the solution is as follows:

```
thisDate.toLocaleString('default', { month: 'long' })
```
