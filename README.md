# Frontend Engineering Task | Maps

## Technologies Used

- MapboxGl
- Algolia (to fetch member details)
- NuxtJS
- Figma (for design)

## Objective

To create a well integrated map based view of all the members within a community. The view should be informative and interactive and help the user visualize the globalness of the community.

## Features

- A list of members laid out on top of the Map.
- Search to allow members to find fellow community members

### Features that can be added

- Allow user to discover similar users based on their currently selected user.
- Allow the user to find other members based on company name, designation and location.
- Display upcoming events on the dash and on the map (if is an in-person meetup)
- Allow the user to select whether to view members in a clustered view or as unclustered points.

## Components

The app has two main components:

- The Search
- The Map

### The Search

Search for this app has been powered by Algolia and implemented with Vue-InstantSearch. We build a database of the community members on Algolia which is indexed to provide fast search capabilities. The search provides the user a way to search through the member database and jump to the location of a certain member.

### The Map

The map displays and helps in visualizing the number and spread of the community members around the world. Each user appears as an individual marker on the map, which can be focused on. The user can know more about a user by clicking on the marker and having a quick look at the member profile.

## How it works

The records for the community members are stored inside an index in Algolia, on which we perform the search. The received results of the search is then rendered upon the map as a symbol layer, with each member being represented by a feature on the map. As we keep refining the search, the layer is redrawn to display the up to date results.

A single icon has been used to mark an individual member on the map. A small set of icons can be used if there are multiple types of users i.e. admins, moderators and normal users.

## Design

Community members can be shown in the overworld view in 3 main ways:

### Default View

Markers can be used to create more interactive and custom styled points on the map, but it comes with a significant overhead and slows down the app to a significant degree.

The points can be placed on the map using a Symbol Layer. It provides a speedup to the app but gives up the ability to customize individual markers to a great extent.

#### Using a small set of common icons

![unclustered](/designs/unclustered.png)

Reference: https://docs.mapbox.com/mapbox-gl-js/example/geojson-markers/

- Advantages
  - All the user images don't need to be loaded into mapbox, thus decreasing the map load time.
  - As the icons don't take up much space, and the text is visible only when the space is available, it looks relatively uncluttered.
- Cons
  - Lack of individuality at a glance.

#### Using clusters at lower zoom levels

![clustered](/designs/clustered.png)

Reference: https://docs.mapbox.com/mapbox-gl-js/example/cluster/

- Advantages

  - Clean and concise view.
  - Prevents overlap of icons and text.

- Cons
  - Lack of information about individual users at a glance.

#### Using individual custom icons

![custom-icon](/designs/custom-icons.png)

- Advantages

  - Individual members are well represented on the map.

- Cons
  - Each icon has to be individually loaded into mapbox, thus increasing the load time
  - Lots of network requests need to be made in the case of large number of users.

Keeping the performance in mind, I've used the unclustered point view with a common icon.

### User location

Reference: https://docs.mapbox.com/mapbox-gl-js/example/locate-user/

The user's current location can be identified and the user can explore and find users near their location.

![](/designs/user-location.png)

### Search Filters (Concept)

Reference: https://www.algolia.com/doc/guides/managing-results/refine-results/filtering/

Filters can be added to the search query search for that keyword in a specific category i.e. company name, job title, city or country.

![](/designs/search-filters.png)

### User Profile

On interacting with either a search result or a marker, the user can view the profile of the selected member.

##### Simplified Design with Alternate Color Scheme

![](/designs/profile-1-alt.png)

##### Simplified Design

![](/designs/profile-1.png)

##### Alternate Design 1 (shows more information about the user)

![](/designs/profile-2.png)

##### Alternate Design 2 (shows more information about the user)

![](/designs/profile-3.png)
