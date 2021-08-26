# Watery-Minder-back-end
Watery-Minder back end

## ☝️ **Pitch**

Ensuring that all your plants are consistently watered is actually pretty difficult. Water My Plants is an app that helps to solve those problems. 

With an easy to use interface for creating a plant watering schedule tailored to each individual plant, **Water My Plants** will remind users when it's time to feed that foliage and quench your plants' thirst.

## ✅  **MVP**

1. `user` can sign-up / create an account by providing a unique `username`, a valid mobile `phoneNumber` and a `password`. 
2. `user` can login to an authenticated session using the credentials provided at account creation / signup.
3. Authenticated `user` can Create, Update and Delete a `plant` object. At a minimum, each `plant` must have the following properties: 
    - `id`: Integer
    - `nickname`: String
    - `species` : String
    - `h2oFrequency`: Type determined by implementation
    - `image`: (optional)
4. Authenticated `user` can view a list of created `plants`.  A `plant` can be deleted or selected to present `user` with a detail view where `user` can then update any property of the selected `plant`. 
5. Authenticated `user` can update their `phoneNumber` and `password`.

## 🏃‍♀️ **Stretch**

1. Authenticated `user` can set up push notifications to be triggered when an `h2oFrequency` of any `plant` arrives / has elapsed. 

2. Implement a feature that allows an authenticated `user` to see an appropriate suggested `h2oFrequency` based on `species` using the API of your choice. 

3. Authenticated `user` can upload `image`s of a `plant`. If no user `image` is provided, a placeholder `image` of a plant of the same `species` populates the view.

# API
Disclaimer: this documentation last updated on August 26, 2021 and does not take into effect changes after that date.

## Core
The core app is located at: https://wmp-api.herokuapp.com/
A GET call to this address will return a message "Yip yip, Appa!" to let you know you are connected.

## /api/auth
### register

Requires JSON {
    username,
    password,
    phoneNumber
}

### login

Requires JSON {
    username,
    password
}

Response includes an auth token that expires in 24 hours

## Note: below this line, the API requires a valid auth token

## /api/users
### GET

A user can only get their own information; their userID is pulled from the auth token.

### PUT
User does not need all fields in the JSON.

Only the user in the auth token may be changed.

Changed passwords are encrypted before being saved to the database.

Phone number checked for valid format by phonejs node module.

## /api/plants
### GET

Returns a list of plants created by the user, verified by the auth token.

### POST

Requires JSON {
    h2oInterval,
    h2oAmount
}

plantID and ownerID are bigint assigned by back-end.

speciesID is a string that does NOT currently link to the species table

nickname and lastWatered are optional strings

image is a placeholder string, would be replaced by blob to actually hold image

### PUT/:plantID

Full JSON not required

plantID must match a plant owned by userID in auth token

### DELETE/:plantID

userID in auth token must match ownerID of plant

## /api/species

Warning: currently does not check ownerID; use PUT and DELETE with caution

Disclaimer: Was unwired from plants to enable direct string entry.  This is bad programming, and would not happen in a real project.

### GET
Returns a list of all plant species

### POST

Requires JSON {
    speciesName,
    h2oInterval,
    h2oAmount
}

speciesID assigned by back end

image field would be converted to blob to hold actual image data

### PUT/:speciesID

Warning: currently changes species without checking for plants not owned by userID in auth token

### DELETE/:speciesID

Warning: currently removes without checking for plants not owned by userID in auth token
