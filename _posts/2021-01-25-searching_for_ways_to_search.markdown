---
layout: post
title:      "Searching for Ways to Search"
date:       2021-01-25 23:41:31 +0000
permalink:  searching_for_ways_to_search
---

## Two Methods Of Performing A Search With A Rails Back-End And A React-Redux Front-End

My time at Flatiron School has been quite the adventure. Six months ago, as I was just beginning to embark of my journey at Flatiron, the mere idea of having learned so much in such a short time was absolutely wild to even consider. So as I built my final project, I decided to make it an homage to my first ever app, a CLI called “Cocktail Buddy.” (You can read about that experience here: [https://maxjacobzander.github.io/my_first_app](http://) or check out the demo here: [https://youtu.be/Mid918ODF0U](http://).)  Having so many more tools at my disposal than I did when I wrote “Cocktail Buddy”, I created “Liquor Cabinet”, an app with a React with Redux front-end and a Ruby on Rails back-end. As someone who enjoys a good cocktail, but can’t always think of what I can or should make for myself, this seemed like a great opportunity to solve that problem. To me, the idea was simple: the backend would store a database of cocktails and drinks (and the recipes to make them) and a user would be able to tell the front-end a kind of liquor they wanted to use or feature, and the app would spit back recipes (from the database) that they could make. Now, while the idea was easy enough to come up with, executing was a whole other experience. Ultimately, I accomplished my goal in two different ways (both of which are used in the app) and, since I definitely had to piecemeal it together, I want to walk you (and probably my future self) through exactly what I did.

### Method #1:
###### (The initial workaround)

When I was still trying to accomplish exactly what I wanted for the search feature, I came up with a workaround to achieve a somewhat similar goal. JavaScript has a wonderful built-in function called filter(). Filter essentially just does what you would expect: it takes in an array and filters out things that meet a set of parameters that you set.  Initially, I had my index component written as follows:


```
export class Drinks extends Component {
    componentDidMount(){
        this.props.fetchDrinks()
    }
    render() {
        const drinks = this.props.drinks.map(( drink, i ) => <DrinkItem key={i} drink={drink} />)
				return (
            <div>
                <h3>Drinks</h3>
                <ul classname="DrinkCollection">
                    { drinks }
                </ul>
            </div>
        );
    }
}
const mapStateToProps = state => {
    return {
        drinks: state
    }
}

export default connect(mapStateToProps, { fetchDrinks })(Drinks);
```

(For reference, DrinkItem.js looks like this:)

```
import React from 'react'
import { connect } from 'react-redux';

const DrinkItem = (props) => {
    console.log(props)
    return (
    <ul className="collection-item">
      <li>Name: {props.drink.name}</li><br/>
      <li>Main Liquor: {props.drink.main_liquor}</li><br />
      <li>Ingredients: {props.drink.ingredients}</li><br />
      <li>Instructions: {props.drink.instructions}</li><br />
    </ul>
    );
}

export default connect(null, { editDrink })(DrinkItem)
```

In order to add what would appear to be search functionality to the component, I added an input field to give the user a place to type their search:

```
                <LogoHeader />
                <h2>Drinks</h2>
                <h3>View All Drinks Here or Enter A Type of Liquor Below to Search</h3>
                <input type="text" id="allDrinkSearchBar"
                    placeholder="Please Enter A Type Of Liquor (ie. Whiskey, Tequila, etc.)"
                    value={this.state.search}
                    onChange={this.updateSearch.bind(this)} />
```

You'll note that this is merely a text input and *not* a form. *(More on using a form later!)

You'll also note that the value needed to be assigned, so I set that to `{this.state.search}`. This also meant that I needed to set up a constructor, where I set the value of `state.search` to an empty string:

```
    constructor() {
        super();
        this.state = {
            search: ''
        }
    }
```

Additionally, I defined a function called `updateSearch()` which would `setState` based on an event, and I setState to be the value of the text input field. 

```
    updateSearch(event){
        this.setState({search: event.target.value})
    }
```

Now for the fun part! Moving some pieces around, I redefined my `const drinks`. This time I called on `this.props.drinks.filter` to begin my filtering process. I then passed in an individual drink and, from that, returned two different sets of parameters: the current search state would have to be matched within the drink’s `ingredients` or its `main_liquor`. I used JavaScript’s `indexOf()` method to search both of those places for matches and, should the match be found, have those matches returned.

```
const drinks = this.props.drinks.filter(
            (drink) => {
                return drink.ingredients.toLowerCase().indexOf(this.state.search) !== -1 || drink.main_liquor.indexOf(this.state.search) !== -1;
            }
        );
```

Then I repurposed my initial `const drink` as `let filteredDrinks` as follows:

```
let filteredDrinks = drinks.map(( drink, i ) => <DrinkItem key={i} drink={drink} />)
```

This maps through the array of matches and lists out each individual entry. At this point, the only thing left to do was to change what I was displaying from `drinks` to `filteredDrinks`, thereby displaying all drinks until a "search" (filter) was made!

All together, this solution looks like this:

```
import React, { Component } from 'react';
import DrinkItem from '../components/DrinkItem';
import { connect } from 'react-redux';
import { fetchDrinks } from '../actions/actions';
import LogoHeader from '../components/LogoHeader'
import NavBar from '../components/NavBar'

export class Drinks extends Component {
    constructor() {
        super();
        this.state = {
            search: ''
        }
    }

    updateSearch(event){
        this.setState({search: event.target.value})
    }

    componentDidMount(){
        this.props.fetchDrinks()
    }

    render() {
        const drinks = this.props.drinks.filter(
            (drink) => {
                return drink.ingredients.toLowerCase().indexOf(this.state.search) !== -1 || drink.main_liquor.indexOf(this.state.search) !== -1;
            }
        );
        let filteredDrinks = drinks.map(( drink, i ) => <DrinkItem key={i} drink={drink} />)
        return (
            <div>
                <LogoHeader />
                <h2>Drinks</h2>
                <h3>View All Drinks Here or Enter A Type of Liquor Below to Search</h3>
                <input type="text" id="allDrinkSearchBar"
                    placeholder="Please Enter A Type Of Liquor (ie. Whiskey, Tequila, etc.)"
                    value={this.state.search}
                    onChange={this.updateSearch.bind(this)} />
                <br /> <br />
                <div classname="DrinkCollection">
                        { filteredDrinks }
                </div>
                <NavBar />
            </div>
        );
    }
}


const mapStateToProps = state => {
    return {
        drinks: state
    }
}

export default connect(mapStateToProps, { fetchDrinks })(Drinks);
```


---

### Method #2:
###### (The real search)

So the real search was definitely more complicated, but not at all unachievable!

Unlike the solution presented in Method #1, I was not trying to merely filter out matches from an otherwise visible list. I wanted to be able to search from my home component and then return just the matches right there on the page. Ultimately, I was able to accomplish this as using a Rails simple search and then fetching that properly from the front-end. But let’s not get ahead of ourselves...

Within my `drinks_controller.rb`, I defined a new method “search”. One of the attributes I gave to the Drink class is a `main_liquor` and I figured that that would be a good way to search for drinks featuring a given liquor in one’s liquor cabinet. To be able to find matches, I used the `.where` method and passed in the `main_liquor` with `LIKE` followed by a placeholder, followed by the query as a parameter between two `%`s so that, should a user accidentally miss a first or last letter, the app would still be able to complete their intended search. Since I wanted it to be able to potentially return multiple drinks, I set this all to `@drinks` and, finally, I made sure to render `@drinks` as json.

```
  def search
    @drinks = Drink.where("main_liquor LIKE ?", "%" + params[:q] + "%")
    render json: @drinks
  end
```

The next move was to go into my routes and add both `get` and `post` routes for my newly defined search method.

```
  get "/api/v1/search", to: "api/v1/drinks#search"
  post "/api/v1/search", to: "api/v1/drinks#search"
```

Now, following “/api/v1/search” by itself won’t actually find anything. In order to do that, we needed a query. So we tack on “?q=" followed by the query to make that happen. As an example, “/api/v1/search?q=whiskey” will give us a list of matches from the database that have “Whiskey” listed as the `main_liquor`!

So what now? Well, now we jump to the front-end! 

I created a new action called `searchDrinks`, which takes in a liquor and, since we’re sending data to the backend, makes a POST request. The site for the fetch becomes the search URL from the previous step with the passed-in liquor interpolated into the query spot and the data is stringified and sent to the backend. We parse out the received data as JSON, then take that data and, apply our reducer to update the state.

```
export const searchDrinks = liquor => {

    return(dispatch) => {
        return fetch(`http://localhost:3001/api/v1/search?q=${liquor}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({liquor})
        })
        .then(resp => resp.json())
        .then(liquor => {
            dispatch({ type: "FIND_DRINK", payload: liquor })
        })
    }
}
```

Like my initial `SET_DRINK` case, the `FIND_DRINK` case in my reducer just returns `[…action.payload]` (essentially just a Redux way of saying the data from the action).

```
export default (state = [], action) => {
    switch(action.type){
        ...
        case "FIND_DRINK":
            return [...action.payload]
```

Within my Search.js component, I also wrote out a very important piece to this puzzle: the actual search bar. Unlike Method #1 where I just used a text-input field, this time I used a form, changing the value of the submit button to “search”, and defining a `handleChange` to (like the  `updateSearch` of Method #1), `setState` of `main_liquor` to be whatever the value of the filled in form field is at that moment. When the form is submitted, I prevented the default refresh from happening and then called my `searchDrinks` action, passing in `this.state.main_liquor` to appropriately interpolate the correct value into the fetch request.

All of that can be seen here:

```
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { searchDrinks } from '../actions/actions'

class Search extends Component {

    constructor(props) {
        super(props);
        this.state = {
            main_liquor: ''
        }
    }

    handleChange = event => {
		this.setState({
            main_liquor: event.target.value
        });
    };

    handleSubmit = event => {
        event.preventDefault()
        this.props.searchDrinks(this.state.main_liquor);
    }
    
    

    render() {
        return (
            <div className="search-form">
                <form onSubmit={this.handleSubmit}>
                    <h3>
                        <label>
                        What's in your cabinet?
                        <input type="text" name="q" placeholder="Please Enter A Type Of Liquor (ie. Whiskey, Tequila, etc.)" onChange={this.handleChange} value={this.state.main_liquor}/>
                        </label>
                        <input type="submit" value="search" />
                    </h3>
                </form>
            </div>
        );
    }
}

export default connect(null, {searchDrinks})(Search);
```

Finally, I created a functional component for my results that displays each drink and it’s attributes in a `<ul>` and then put that component into my Home.js under the search bar to render the results on the page upon the submit.

```
import React from 'react'
import { connect } from 'react-redux';

const Results = (props) => {
    if (props.drinks.length < 1){
    return null;
    }
    return props.drinks.map((drink) => {
      return( <div>
      <ul className="collection-item">
        <li>Name: {drink.name}</li><br/>
        <li>Main Liquor: {drink.main_liquor}</li><br />
        <li>Ingredients: {drink.ingredients}</li><br />
        <li>Instructions: {drink.instructions}</li><br />
        </ul>
      </div> )
    })
}

const mapStateToProps = state => {
  return {
      drinks: state
  }
}

export default connect(mapStateToProps)(Results);
```

All of this to say, a lot of work and a lot of time later, not only had I achieved my goal, but I had even figured out another way to achieve something similar! What a way to end my time at Flatiron School! I am super proud of the work I did on this project and hope that this post proves helpful for anyone trying to do a search of a Rails back-end with a React-Redux front-end!
