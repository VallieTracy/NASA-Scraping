# Current Mars News
<b>PROJECT SCOPE:</b> Utilizing Flask API to build a web application that scrapes various websites for data related to Mars and then display the information in a single HTML page.

![mission_to_mars](Images/mission_to_mars.png)

### In order to run this application from your own desktop, please follow the steps below. 
*(This was a complex project, so I've written a detailed description of the process below should it interest you.  You can either clone the repo as layed out below, or scroll to "PROCESS" to delve into said detailed description.  You can navigate to the Screenshots folder to view the finished product.)*

#### Please clone this repository to your desktop and then do the following:    
1. Navigate to Missions_to_Mars, the folder that contains ``app.py`` and launch a GitBash (Windows) or Terminal (Mac). 
2. Type ``source activate PythonData`` and then hit ENTER.
3. Type ``export FLASK_APP=app.py`` and then hit ENTER.  
4. Type ``flask run`` and then hit ENTER.      
5. Observe that the Flask server starts and tells you which port it's running on. Don't close this window.     
6. With the Flask server running, enter this address in your Chrome browser: http://127.0.0.1:5000/. You'll see that it loads the index page.      
7. After you've navigated to the page, click 'Scrape New Data'. You will notice a new chrome browser window will open.  Do not close this window because this is how the Flask server is gathering the updated info. If you like, you can open it and watch it cycle through the various web pages!
8. You will now see that the page will update with the most current:  
      + featured image on NASA's Mars homepage     
      + weather from NASA's Mars twitter feed  
      + and the latest Mars headline     
9. And you can scroll to the bottom of the page at any time to view snapshots of Mars' four hemispheres.     
10. ENJOY!

#### Additional notes:
* You'll need [mongodb compass](https://www.mongodb.com/products/compass) installed on your computer. 
* You can find and adjust the chromdriver path in ``scrape_mars.py`` line 11.   
* Should you need chromedriver, you can [follow this link](https://sites.google.com/a/chromium.org/chromedriver/downloads)




# PROCESS
I broke the process into two parts.  The first part I performed solely in Jupyter Notebook.  The second part is where I utilzed Flask and MongoDB to create a web application. 



## STEP 1 - Scraping

I performed my initial scraping using Jupyter Notebook, BeautifulSoup, Pandas, and Requests/Splinter.  The code is found in my [`mission_to_mars`](https://github.com/VallieTracy/NASA-Scraping/blob/master/Missions_to_Mars/mission_to_mars.ipynb) Jupyter notebook.  I collected information on current Mars news, featured image, weather, planet facts, and hemisphere titles & images.  You can read about each of those below, or skip to Step 2, which talks about Flask and MongoDB.

### (a.) Mars News Feed     

Website used: https://mars.nasa.gov/news/

* I collected the latest News Title & Paragraph Text in this step.  I stored the text to a variable for later reference (thinking ahead to the Flask portion).
```python
# My resulting Title and Text:

news_title = "NASA's Curiosity Keeps Rolling As Team Operates Rover From Home"

news_p = "The team has learned to meet new challenges as they work remotely on the Mars mission."
```

### (b.) JPL Mars Space Images - Featured Image     

Website used: https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars

* The JPL featured image of Mars changes multiple times per day.  In order to store that image into a variable, I used splinter to navigate the site and find the image url.  I then assigned the url string to a variable, making sure to use the full-size `.jpg` image and capture the complete url string.

```python
# My resulting url string:

featured_image_url = 'https://www.jpl.nasa.gov/spaceimages/images/wallpaper/PIA16565-1920x1200.jpg'
```

### (c.) Mars Weather     
Website used: https://twitter.com/marswxreport?lang=en

* I accessed Mars' Twitter feed to get the current weather.  The most current weather is in the top pinned tweet, so I scraped the site for that saved that tweet text into a variable called `mars_weather`.     
* NOTE: I had difficulty with this portion when I used the chromedriver method, so I switched to the requests.get method.  You'll see the same note in the code.

```python
# Weather variable:

mars_weather = 'InSight sol 496 (2020-04-18) low -94.6ºC (-138.4ºF) high -6.2ºC (20.9ºF)
winds from the SW at 4.6 m/s (10.3 mph) gusting to 15.7 m/s (35.2 mph)
pressure at 6.60 hPapic.twitter.com/mrkGEj5txc'
```

### (d.) Mars Facts     
Website used: https://space-facts.com/mars/

* There's a website that contains facts about Mars, some written in basic text format, but also there are tables within the text.  I used Pandas to scrape the first table which contains details about the planet diameter, mass, etc.  

* I then used Pandas to convert the data to an HTML table string.     

* Please see my `mission_to_mars.ipynb` to see the code.

### (e.) Mars Hemispheres     
Website used: https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars

* Mars has four hemispheres.  My goal here was to obtain high resolution images for each.  I found that I had to click each of the links to the individual hemispheres in order to find the image url to the full resolution image.

* I then saved both the image url string for the full resolution hemisphere image, and the Hemisphere title containing the hemisphere name. I used a Python dictionary to store the data using the keys `img_url` and `title`.

* Finally, I appended the dictionary with the image url string and the hemisphere title to a list. The list ultimately contained one dictionary for each hemisphere.

```python
# The resulting list of dictionaries:

hemisphere_image_urls = [
    {"title": "Valles Marineris Hemisphere", "img_url": "..."},
    {"title": "Cerberus Hemisphere", "img_url": "..."},
    {"title": "Schiaparelli Hemisphere", "img_url": "..."},
    {"title": "Syrtis Major Hemisphere", "img_url": "..."},
]
```

- - -

## STEP 2 - MongoDB and Flask Application

Step 2 was to use MongoDB with Flask templating to create a new HTML page that displays all of the information that was scraped from the URLs above.

* I first converted my Jupyter notebook into a Python script called `scrape_mars.py` with a function called `scrape` that executed all of my scraping code from above and returned one Python dictionary, called `mars_dictionary`, containing all of the scraped data.

* Next, I created a route called `/scrape` that imported my `scrape_mars.py` script and called my `scrape` function.

* I created a root route `/` that querried my Mongo database and passed the mars data into an HTML template to display the data.

* I finally created a template HTML file called `index.html` that takes the mars data dictionary and displays all of the data in the appropriate HTML elements.  I used Bootstrap in my HTML template.


- - -

# SCREENSHOTS OF MY FINAL WORK: 


![mission_to_mars](Images/final_app_part1.png)

![mission_to_mars](Images/final_app_part2.png)




