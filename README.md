# Frontend Engineering Task | Maps

## Technologies Used

- MapboxGL
- Algolia
- NuxtJS

## Objective

To create a well integrated map based view of all the members within a community. The view should be interactive and help the user visualize the globalness of the community.

## Features

- Allow the user to search through all the members of the community.
- Display the community members on a map and mark their locations in a comprehensible way.
- Allow the user to explore his location and discover new users around them.

### Proposed Features

- Display upcoming events on the dash and on the map (if in person)
- Allow user to discover similar users based on their currently selected user.
- Allow the user to select whether to view members in a clustered view or as unclustered points.
- Allow the user to find other members based on company name, designation and location.

## How it works

The app has two main components:

- The Search
- The Map

### The Search

Search for this app has been powered by Algolia. We build a database of the community members on Algolia which is indexed to provide fast search capabilities. The search provides the user a way to search through the member database and jump to the location of a certain member.

### The Map

The map displays the count and spread of the community members around the world using a visual. Each user appears as an individual marker on the map, which can be focused on. The user can know more about a user by clicking on the pointer and having a quick look at the member profile.

## The Technical Stuff

The Dataset for the community members come from the algolia servers, which is then rendered upon the map, showing the users in their accurate locations.

## Design

Community members can be shown in the overworld view in 3 main ways:

1. Using individual custom icons
   [custom-icon](/designs/custom-icons.png)
2. Using clusters at lower zoom levels
   [clustered](/designs/clustered.png)
3. Using a small set of common icons
   [unclustered](/designs/unclustered.png)
