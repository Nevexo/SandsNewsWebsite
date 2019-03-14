# Sandsnews Website API

## Feature list

    - Prices of news paper delivery
    - Available locations for delivery
    - Delivery Registration
    - Office news
    - Prices of photographs
    - Opening hours
    - Local failback after timeout

## Endpoints

### /api/home/manifest

{
    "opening_hours": {
        "monday": {"state": "OPEN", "open": "11AM", "close": "5:30PM"},
        "tuesday": {"state": "OPEN", "open": "11AM", "close": "5:30PM"},
        "wednesday": {"state": "OPEN", "open": "11AM", "close": "5:30PM"},
        "thursday": {"state": "OPEN", "open": "11AM", "close": "5:30PM"},
        "friday": {"state": "OPEN", "open": "11AM", "close": "5:30PM"},
        "saturday": {"state": "OPEN", "open": "11AM", "close": "5:30PM"},
        "sunday": {"state": "CLOSED"}
    },
    "news": [
        {
            "title": "New website launched",
            "author": "Cameron Fleming",
            "timestamp": "ISO timestamp"
        }
    ]
}

### /api/delivieries

{
    "area_prices": {
        "Caton": "",
        "Silverdale": "".
        "Arkholme": "",
        "Slyne": "",
        "Warton": "",
        "Kellet": "",
        "Yealand": "",
        "Capernwaray": "",
        "Carnforth": "",
        "Borwick": "",
        "Hest Bank": "",
        "Halton": "",
        "Bolton-le-Sands": ""
    }
}

### /api/photos

{
    "print_prices": {
        "6x4": "40p",
        "7x5": "70p",
        "A4": "£1.20",
        "A4 Collage": "£3.99"
    },
    "block_prices": {
        "6x4": "£6.99",
        "7x5": "£8.99",
        "A4": "£11.99",
        "A4 Collage": "£17.99"
    }
}

### /api/manifest

> this endpoint should be requsted once and placed into a cookie
> with a 1 day experation no matter which page loads it.

{
    "phone_numbers": {
        "mobile": "07869 291372",
        "landline": "01524 822435"
    },
    "email_address": "info@sandsnews.com",
    "address": "28 Main Road, Bolton-le-Sands, Carnforth, LA5 8DL",
    "site_status": "active"
}

### GET /api/areas
This endpoint returns an array of available locations for news rounds
[
    {
        "name": "Yealand",
        "type": "BilledByDayType",
        "weekday_price": "45p",
        "weekend_price": "55p"
    }
]

## User Control endpoints

These endpoints require authentication, see below

### POST /api/authenticate
This endpoint takes the following:
{
    "username": "Name",
    "password": "Password (over SSL)"
}

and returns a token to be stored on the client for 1 day
unless 'remember me' is checked where the token is marked as
always-activated

### POST /api/logout
This endpoint takes no data but will de-activate the users token.

## Paper Delivery Endpoints

### POST /api/area/{location_name}
This endpoint allows the user to change/create a location
Type can include: BilledDaily or BilledByDayType (weekend/week charges)
It must recieve this body:

{
    "type": "BilledByDayType",
    "weekday_price": "45p",
    "weekend_price": "55p",
}
or
{
    "type": "BilledDaily",
    "daily_price": "45p"
}

### DELETE /api/location/{location_name}
This endpoint will simply delete a location

## Sandsnews Photos management endpoints

### POST /api/photos/print/{size}
This endpoint allows the administrator to set the price for a size of photo

{
    "price": "40p"
}

### DELETE /api/photos/print/{size}
This endpoint will simply delete a size of print/block

> NOTE: The above endpoints can be altered for /block/ as well.

## User account management

### GET /api/users
Returns a list of users
[
    {
        "username": "Alex",
        "created_date": "ISO timestamp",
        "enabled": true
    }
]

### POST /api/users/{username}
Allows a users information to be modified

{
    "username": "name",
    "password": "",
    "enabled": true
}

### DELETE /api/users/{username}
Deletes a user.

## Server management endpoints

### GET /api/status

{
    "api": {
        "active_version": "1.0",
        "available_version": "1.1"
    },
    "website": {
        "active_version": "1.0",
        "available_version": "1.1"
    }
}

### POST /api/control/restart
Restarts the service

### POST /api/control/upgrade
Has the server git pull & perform a restart.

### POST /api/control/web-upgrade
Has the server git pull the website