# Current Mars News
<b>PROJECT SCOPE:</b> Utilizing Flask API to build a web application that scrapes various websites for data related to Mars and then display the information in a single HTML page.

![mission_to_mars](Images/mission_to_mars.png)

### In order to run this application from your own desktop, please follow the steps below. 
*(Otherwise scroll further to see a detailed description of my process, and then navigate to the Screenshots folder to view the finished product.)*

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
I broke the process into two parts.  The first part I performed solely in Jupyter Notebook.  The second part is where I utilzed Flask and MongoDB to create a web application. Below, I break it down further for you to follow along.



## Step 1 - Scraping

I performed my initial scraping using Jupyter Notebook, BeautifulSoup, Pandas, and Requests/Splinter.  The code is found in my [`mission_to_mars`](https://github.com/VallieTracy/NASA-Scraping/blob/master/Missions_to_Mars/mission_to_mars.ipynb) Jupyter notebook.

### (a.) Mars News Feed     

Website used: https://mars.nasa.gov/news/

* I used the [NASA Mars News Site](https://mars.nasa.gov/news/) and collect the latest News Title and Paragraph Text. Assign the text to variables that you can reference later.

```python
# Example:
news_title = "NASA's Next Mars Mission to Investigate Interior of Red Planet"

news_p = "Preparation of NASA's next spacecraft to Mars, InSight, has ramped up this summer, on course for launch next May from Vandenberg Air Force Base in central California -- the first interplanetary launch in history from America's West Coast."
```

### (b.) JPL Mars Space Images - Featured Image     

Website used: https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars

Tasks performed:     
* Used splinter to navigate the site and found the image url for the current Featured Mars Image and assigned the url string to a variable.

* Make sure to find the image url to the full size `.jpg` image.

* Make sure to save a complete url string for this image.

```python
# Example:
featured_image_url = 'https://www.jpl.nasa.gov/spaceimages/images/largesize/PIA16225_hires.jpg'
```

### (c.) Mars Weather

* Visit the Mars Weather twitter account [here](https://twitter.com/marswxreport?lang=en) and scrape the latest Mars weather tweet from the page. Save the tweet text for the weather report as a variable called `mars_weather`.

```python
# Example:
mars_weather = 'Sol 1801 (Aug 30, 2017), Sunny, high -21C/-5F, low -80C/-112F, pressure at 8.82 hPa, daylight 06:09-17:55'
```

### (d.) Mars Facts

* Visit the Mars Facts webpage [here](https://space-facts.com/mars/) and use Pandas to scrape the table containing facts about the planet including Diameter, Mass, etc.

* Use Pandas to convert the data to a HTML table string.

### (e.) Mars Hemispheres

* Visit the USGS Astrogeology site [here](https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars) to obtain high resolution images for each of Mar's hemispheres.

* You will need to click each of the links to the hemispheres in order to find the image url to the full resolution image.

* Save both the image url string for the full resolution hemisphere image, and the Hemisphere title containing the hemisphere name. Use a Python dictionary to store the data using the keys `img_url` and `title`.

* Append the dictionary with the image url string and the hemisphere title to a list. This list will contain one dictionary for each hemisphere.

```python
# Example:
hemisphere_image_urls = [
    {"title": "Valles Marineris Hemisphere", "img_url": "..."},
    {"title": "Cerberus Hemisphere", "img_url": "..."},
    {"title": "Schiaparelli Hemisphere", "img_url": "..."},
    {"title": "Syrtis Major Hemisphere", "img_url": "..."},
]
```

- - -

## Step 2 - MongoDB and Flask Application

Use MongoDB with Flask templating to create a new HTML page that displays all of the information that was scraped from the URLs above.

* Start by converting your Jupyter notebook into a Python script called `scrape_mars.py` with a function called `scrape` that will execute all of your scraping code from above and return one Python dictionary containing all of the scraped data.

* Next, create a route called `/scrape` that will import your `scrape_mars.py` script and call your `scrape` function.

  * Store the return value in Mongo as a Python dictionary.

* Create a root route `/` that will query your Mongo database and pass the mars data into an HTML template to display the data.

* Create a template HTML file called `index.html` that will take the mars data dictionary and display all of the data in the appropriate HTML elements. Use the following as a guide for what the final product should look like, but feel free to create your own design.

![final_app_part1.png](Images/final_app_part1.png)
![final_app_part2.png](Images/final_app_part2.png)

- - -

## Step 3 - Submission

To submit your work to BootCampSpot, create a new GitHub repository and upload the following:

1. The Jupyter Notebook containing the scraping code used.

2. Screenshots of your final application.

3. Submit the link to your new repository to BootCampSpot.

## Hints

* Use Splinter to navigate the sites when needed and BeautifulSoup to help find and parse out the necessary data.

* Use Pymongo for CRUD applications for your database. For this homework, you can simply overwrite the existing document each time the `/scrape` url is visited and new data is obtained.

* Use Bootstrap to structure your HTML template.

### Copyright

Trilogy Education Services © 2019. All Rights Reserved.

