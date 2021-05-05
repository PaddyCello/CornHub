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

```
class Plants(models.Model):
    plant_name = models.CharField(max_length=50)
    subspecies = models.CharField(max_length=80, blank=True)
    type = models.CharField(max_length=50, blank=True)
    sow_month = models.PositiveSmallIntegerField(blank=True)
    plant_month = models.PositiveSmallIntegerField(blank=True)
    harvest_month = models.PositiveSmallIntegerField(blank=True)
    sunlight = models.CharField(max_length=50, blank=True)
    soil_acidity = models.CharField(max_length=50, blank=True)
    watering_frequency = models.PositiveSmallIntegerField(blank=True)
    fertilizing_frequency = models.PositiveSmallIntegerField(blank=True)
    fertilizer_type = models.CharField(max_length=80, blank=True)
    germination_temperature = models.PositiveSmallIntegerField(blank=True)
    image = models.CharField(max_length=300, blank=True)
    description = models.TextField(blank=True)
    difficulty = models.PositiveSmallIntegerField(blank=True,)
    owner = models.ForeignKey(
        User,
        related_name='created_plants',
        on_delete = models.CASCADE
    )
    verified_by_admin = models.BooleanField(default=False)
```

We were at this stage still getting to grips with Django, and were looking to avoid potential problems; this also meant that our starting point was to work in a group until our back end was built out completely (albeit with some fairly test data) and our front end was attached, with a skeleton of components waiting to be filled with code. We also spent an evening individually researching CSS frameworks, and decided that React-Bootstrap was the one for the job.

With all of this done, it became much easier to assign solo tasks. As a general rule throughout the project, we worked as a full group on the most complex tickets, and saved the more straightforward ones for evenings and the weekend. This was not our original plan, but we discovered while building the back end together that we had a terrific dynamic as a group, both in terms of skills and morale, and it made sense to take advantage of this as much as possible.

One of our ongoing solo tasks was of course adding to our data set. This proved to be quite time consuming, owing to the number of fields on the Plants model, and our emphasis on accuracy. Thankfully, spending hours researching potatoes and courgettes was hugely enjoyable.

One of my first tasks was to build out the main index page for the plants. I had wanted something that was searchable in multiple ways, and would also filter data according to current month (one category each for sow, plant and harvest), with a carousel of random plants as a hero image - perhaps a loose homage to Netflix. The selections for current month proved to be straightforward, as I had the foresight to consider JavaScript date objects when building out the model in Django - months are numbers instead of strings, and zero-indexed, thus making them easy to evaluate against a new Date(). However, displaying the current month as a string took a bit more research; the solution is as follows:

```
thisDate.toLocaleString('default', { month: 'long' })
```

Post-MVP, I did also add in functionality for the user to select other months than the current one; however, the page will always default to the current month when it loads.

Deciding on search conditions for the entire set of plants took a bit more thought. Eventually I settled on an initial filter based off of difficulty level, returning progressively more plants as the 
difficulty level increased; plus a simple text field that would search for strings in Name, Subspecies and Type.

```
const handleChange = (event) => {
    setDifficulty(Number(event.target.value))
  }
  const handleSearch = (event) => {
    const searchList = plantData.filter(plant => {
      return plant.plant_name.toUpperCase().includes(event.target.value.toUpperCase()) ||
      plant.subspecies.toUpperCase().includes(event.target.value.toUpperCase()) ||
      plant.type.toUpperCase().includes(event.target.value.toUpperCase())
    })
    setRightPlants(searchList)
  }
```
```
<Table responsive>
  <tbody>
    <tr>
          {rightPlants.filter(plant => {
            return plant.difficulty <= difficulty
          }).map(plant => {
            return (
              <PlantTile
              key={plant.id}
              { ...plant}
              />
            )
          })}
              </tr>
    </tbody>
</Table>
```

As the above snippet also demonstrates, I used React-Bootstrap <Table> for horizontal scrolls. I did have to hide the scroll bar though (this also helped take care of visible scroll bars on the Weather page):

```
::-webkit-scrollbar {
  display: none;
}
```

The hero image carousel was another React-Bootstrap component, and populated with a random set of six plants from the GET request for all plants. I added a little bit of error handling to avoid duplicates and ensure that the number of plants is the same on every page load:

```
      const randomNums = []
      for (let i = 0; randomNums.length <= 5; i++) {
        const num = Math.floor(Math.random() * response.data.length)
        if (!randomNums.includes(num)) {
          randomNums.push(num)
        }
      }
      const coolPlants = response.data.filter((item, index) => {
        return randomNums.includes(index)
      })
      setRandomPlants(coolPlants)
```

Additionally, every entry in the scrollbars and carousel serves as a fastlink to a page that will display all of the information on that plant, with the option to save the plant to the user profile if the user is logged in (and the option to log in if they aren’t), plus a button to go back to the index page. It was pointed out later that this component is also reachable from the Weather page (which was built by one of my teammates), and that this should be reflected in the behaviour of the ‘Back’ button. After a good hour of Googling, I discovered that the solution to this is ridiculously simple:

```
history.goBack()
```

With all of this done, my next task was to design our landing page. The design is simple, but sets an example for the other pages in terms of fonts, layouts and hex colours. (Subsequently, we did also style the Auth page together as a group, to further ensure consensus on styling.) As a fun extra feature, I incorporated a random subtitle generator with a Math.random() and an array of quotes, so that different subtitles will appear on page load:

```
const Title = () => {
  const subtitles = ['For hardcore gardeners.', 'Nothing seedy... only plants.', 'Full of photos of dirty, filthy... root vegetables.']
  const [subtitle, setSubtitle] = useState(subtitles[0])

  useEffect(() => {
    const changeSub = () => {
      setSubtitle(subtitles[Math.floor(Math.random() * 3)])
    }
    changeSub()
  }, [])
```

Another important feature that I added was the POST request for new plants, only available to the user once they are logged in. This feature mattered, as the subject matter for the app is vast, and open-source contribution would be a quick and effective way to build a comprehensive database. As shown on the Plants model earlier, I have included a field, verified_by_admin, that defaults to false; the idea being that this could be changed to true through the Django admin panel once we have had the opportunity to verify the information provided. Until that point, the plant would still display on the app, but with a disclaimer:

```
{onePlant.verified_by_admin === false
      ? <p className="not-yet">Not yet verified by CornHub</p>
      : <p></p>
    }
```

I also set some specific, arbitrary defaults for numerical fields (which Django did not allow us to leave blank), so that erroneous information would not display on the PlantShow page if the user was submitting a partial data set - something that we had permitted, as the model actually has no required fields. These were then handled in PlantShow through conditional renders. For example, here is what happens when the user doesn’t know the sowing month of the plant they are submitting (remember that months are zero-indexed):

(in form data default state for POST request)
```
sow_month: 12,
```
(conditional render for PlantShow)
```
{onePlant.sow_month !== 12
      ? <p>Sow in: {monthNames[onePlant.sow_month]}</p>
      : <p></p>
}
```

One problem that adding the NewPlant form revealed, however, was that we had used a reserved word when defining our Plants model - somehow, it had not caused any problems until this point! This did still mean that we had to go to the plants/models.py and plants/seeds.json files in the back end, and change every instance of ‘name’ to ‘plant_name’.

A crucial styling feature that we incorporated across the board was responsive design. This was due, almost entirely, to the fact that I wanted to integrate a Spotify plugin (complete with a playlist of predominantly metal songs with gardening pun names), and knew that gardeners would be listening on a mobile device:

![photo](https://github.com/PaddyCello/CornHub/blob/2f19458324b0ff571058f45f5e1daa60a9c0ec7a/screenshots/IMG_5050.JPG)

Purely because of this one feature, the entire site works on mobile. Similarly, we made a point of being as comprehensive with error handling and conditional rendering as we could possibly think of.

The most ambitious feature of our app, by far, was the optional watering reminder for each saved plant. Building this involved several steps and a lot of experimentation, and was absolutely a collective effort. In a nutshell, this is how it works:

- Clicking the reminder button generates a Date.now() timestamp and saves it to local storage, along with a payload of the plant ID, the watering frequency, the plant name and an instruction to water
- On page load, a new Date.now() is generated and saved to state
- On page load, the reminder payload is retrieved from local storage
- The watering frequency is multiplied by 86.4 million (the number of milliseconds in a day) and added to the timestamp from the payload
- This figure is compared to the current Date.now()
- If the payload figure is the lower of the two numbers, a toast reminder is displayed, with a reminder to water, and the name of the plant to water; it will display a list if multiple plants need watering
- When the toast appears, the payload is removed from local storage; accordingly, the toast also displays an instruction to click the reminder button again if further reminders are wanted

For presentation purposes, we expedited the watering_frequency * 86.4 * 100000 figure to a mere five minutes; but in the deployed version, the original figure remains.

# Bugs, Blockers and Wins

Known or suspected bugs:
- One or two images from the database don’t load
- Geolocation settings on Weather component don’t always automatically default to user’s location
- cornhubapp.herokuapp.com may not always redirect to the page unless the prefix http:// is specifically entered as part of the URL
- Scrollbars may not stay hidden on Firefox
- Default germination temperature is not the same on PlantShow as it is on NewPlant

Blockers:

- We did have to abandon profile images for the user, as we were unable to get our requests to Cloudinary to work as intended; however, we also realised we didn’t have much of a need for this feature in the first place
- Deployment to Heroku resulted in the PlantShow page remaining blank unless the user was logged in. Eventually we figured out that we needed to refactor our GET request for the current user, so that the app wouldn’t hang if local storage was empty

Wins:

- By complete coincidence, the default font used in React-Bootstrap is apparently identical to the font in the PornHub logo
- The team I assembled believed in the project every bit as much as I did, and brought a ton of great ideas

# Key Learning and Future Features

Learning-wise, my biggest takeaway is the power of teamwork. Without Jonty and Michael, this project would have been substantially smaller and far less interesting, and some of the features (especially the watering reminder) would have been impossible to build without the combined mental power of the three of us. I also learned to never hold back when asking specific people to work with me, as this would have been a missed opportunity had I done so.

In terms of future features, on the whole the three of us are extremely happy with the end result; however, some additions could include:

- Reminders for fertilization
- Some fine-tuning on the aspect ratios for the plant photos
- Changing PayPal integration from test to live
- A mailto: link for users to make suggestions about plant edits

