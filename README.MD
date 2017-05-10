# Shakespeare Reader

## Table Of Contents
- [Purpose](#purpose)
- [Design](#design)
- [Development Status](#development-status)
- [Dependencies](#dependencies)
- [Installation and Usage](#installation-and-usage)

## Purpose

As part of my last semester of undergrad I took a Shakespeare for non-majors course, and struggled through the reading. I found that fully comprehend the text to a level that I could be tested on I was using many tools to understand the text. At all times I would be going back and forth between multiple browser tabs looking up information. I would start with the sparknotes character analysis to have an idea who everyone was when I started reading, then the summary to have a background of the plot before I began reading. The Shakespeare text was still dense, and I found I was reading the modern english translations more than the original. This application makes this process more streamlined by providing all of the context directly linked from the reading. Actions like mouse-over on a character's names will be able to reveal scraped data inside the same page, rather than having to do another google search. Unknown shakespearian language can be searched. There are so many ways to make the text interactive with the combination of outside data sources.

## Design
I am using a node backend with mongodb, and ejs templates. This software stack lends nicely to creating the interactive user expierience I am seeking. The shakespeare text sources will be scraped from [Open Source Shakespeare](http://www.opensourceshakespeare.org/), and for the time being this is all I am pulling from this site, however there are tons of other recources available to be embedded with the text. From there the text will be cleaned and stored in mongo. A user entering the site can either select a pre-loaded shakespeare text, or upload or paste their own. Due to the different spellings of words in different shakespeare printings I will be using Levenshtein distance to compute the similarity percentage and make sure the response is within a given range. 

After the target text has been identified I will request all of the scraped information required for that view. This would not include some scraping tasks that would have to happen in real time such as dictionary references. This information will contain the plot overview, a list of characters with a brief corresponding description, and key facts. All of this data is scraped from the respective sparknotes study guide. The plot summary is shown before the text, and the character desciptions are shown if the user clicks on that characters name. Key facts are compressed to a list of headings, and at any time the user can drop down text with the specific key fact. The no fear shakespeare modern translations are scraped, and only displayed on request. This is to encourage reading the original text. A user can highlight a section of text and have it changed to the no fear version. Oxford english dictionary queries can be done through their api directly, which means it doesn't have to be scraped. This is a premium service however, and will stay on the backburner till the very end. Another stretch goal would be implementing user entered data for live feedback and discussions. The software is straightforward, but moderating is difficult.

## Development Status

#### Application
- [x] Mongo Config
- [x] Default Express Route
- [x] Local user creation and authentication
- [x] Google authentication
- [x] Facebook authentication
- [x] Twitter authentication
- [x] Save account to DB
- [x] Account Page
- [x] Link existing accounts
- [ ] Unlink existing accounts
- [x] File Uploader
- [x] File Parser
- [ ] Textfield uploader
- [ ] Reading UI
- [ ] OED API (shakesperian specific words)
- [ ] Longman Dictionary (standard english words)
- [ ] User Discussion

#### Scraping
- [x] Open Source Shakespeare
- [ ] Sparknotes Plots
- [ ] Sparknotes Characters
- [ ] Sparknotes Key Facts
- [ ] No Fear Translation

## Dependencies
The following dependencies are installed on my system with corresponding version numbers.
- ##### [NodeJS](https://nodejs.org/en/)
    - Server Side Javascript
    - Version: 6.10.2
    - [Installation Information](https://nodejs.org/en/download/package-manager/)
- ##### [NPM](https://www.npmjs.com/)
    - NodeJS Package Manager
    - Version: 3.10.10
    - [Installation Information](http://blog.npmjs.org/post/85484771375/how-to-install-npm)
- ##### [MongoDB](https://www.mongodb.com/)
    - NoSQL Database
    - Version: 3.4.4
    - [Installation Information](https://docs.mongodb.com/manual/installation/)
- ##### Other Javascript Dependencies
    - Everything else is handled by npm, and configured in package.json. 
    
## Installation and Usage

##### Clone the repository

    git clone https://github.com/JustinWayneOlson/shakespeare-reader
    
##### Install JavaScript Dependencies

    npm install
    
##### Set up config files

There are two files in the .config folder that don't get pushed to github. Those contain the API information for authentication, as well as the DB url string. In the config folder create the following two files.

The first file contains the API information for google facebook and twitter. Register a developer account with the three domains and create a new application on their respective portals. Copy the keys, and secrets into this config file, and set the redirect page in the portal's settings to http://localhost:8080/profile. For more information about this see this [tutorial series](https://scotch.io/tutorials/easy-node-authentication-setup-and-local).

###### auth.js

    module.exports = {

        'facebookAuth' : {
            'clientID'      : 'APP ID', // your App ID
            'clientSecret'  : 'SECRET', // your App Secret
            'callbackURL'   : 'http://localhost:8080/auth/facebook/callback'
        },
    
        'twitterAuth' : {
            'consumerKey'       : 'APP KEY',
            'consumerSecret'    : 'SECRET',
            'callbackURL'       : 'http://localhost:8080/auth/twitter/callback'
        },
    
        'googleAuth' : {
            'clientID'      : 'APP ID',
            'clientSecret'  : 'SECRET',
            'callbackURL'   : 'http://localhost:8080/auth/google/callback'
        }

};

The second file is the MongoDB config. This just contains the connection string to the database. For information on what this should look like read these [docs](http://mongoosejs.com/docs/connections.html).

###### database.js

    module.exports = {
        'url' : 'CONNECTION STRING'
    };
    
##### Start the Mongo Service in the background

    sudo mongod &

##### Start the web server

    node server.js
    
##### Point Browser to localhost:8080/






