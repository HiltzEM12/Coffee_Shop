# Coffee Shop Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

On Windows, run the following:
    py -m pip install --user virtualenv
    py -m venv env
The last variable above is the name of the virtual environment.  In this case 'env'
Then add the env folder to the gitignore
Then activate the virtual environment by running:
    .\env\Scripts\activate
If the above doesn't work, use:
    source env/Scripts/activate
Check to see if its running, run:
    where python
It should display something allong the lines of (...env\Scripts\python.exe) if it's running.
To leave the virtual environment, run:
    deactivate

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) and [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) are libraries to handle the lightweight sqlite database. Since we want you to focus on auth, we handle the heavy lift for you in `./src/database/models.py`. We recommend skimming this code first so you know how to interface with the Drink model.

- [jose](https://python-jose.readthedocs.io/en/latest/) JavaScript Object Signing and Encryption for JWTs. Useful for encoding, decoding, and verifying JWTS.

## Running the server

From within the `./src` directory first ensure you are working using your created virtual environment.

Each time you open a new terminal session, run:

```bash
export FLASK_APP=api.py;
```

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

## Tasks

### Setup Auth0

1. Create a new Auth0 Account
2. Select a unique tenant domain
    - roofuseat.us.auth0.com
3. Create a new, single page web application
    - Name: CoffeeShop
    - ClientID: dJReaIrxbUbm0kFRliVy0NJsijlKVS0f
4. Create a new API
    - Name: coffee
    - Identifier: coffee
    - in API Settings:
        - Enable RBAC
        - Enable Add Permissions in the Access Token
5. Create new API permissions:
    - `get:drinks-detail`
    - `post:drinks`
    - `patch:drinks`
    - `delete:drinks`
6. Create new roles for:
    - Barista
        - can `get:drinks-detail`
    - Manager 
        - can perform all actions
7. Test your endpoints with [Postman](https://getpostman.com). 
    - Register 2 users - assign the Barista role to one and Manager role to the other.
         - Not ideal for an actual API, but here are the test users:
            - barista@test.com 1234asdfQWER
            - manager@test.com 1234asdfQWER
    - Sign into each account and make note of the JWT.
    - Import the postman collection `./starter_code/backend/udacity-fsnd-udaspicelatte.postman_collection.json`
    - Right-clicking the collection folder for barista and manager, navigate to the authorization tab, and including the JWT in the token field (you should have noted these JWTs).
    - Run the collection and correct any errors.
    - Export the collection overwriting the one we've included so that we have your proper JWTs during review!

### Implement The Server

There are `@TODO` comments throughout the `./backend/src`. We recommend tackling the files in order and from top to bottom:

1. `./src/auth/auth.py`
2. `./src/api.py`

### Notes
To get the jwt use
https://{{YOUR_DOMAIN}}/authorize?audience={{API_IDENTIFIER}}&response_type=token&client_id={{YOUR_CLIENT_ID}}&
redirect_uri={{YOUR_CALLBACK_URI}}
For this project use: https://roofuseat.us.auth0.com/authorize?audience=coffee&response_type=token&client_id=dJReaIrxbUbm0kFRliVy0NJsijlKVS0f&redirect_uri=https://localhost:8100/tabs/user-page
Go to that url, login, and retrive the jwt from the url after login.
Use this in a private browser session to prevent auto sign in

Clock is not an attribute of the time module.  Error in sqlalchemy.  To fix change the following:
in backend\env\Lib\site-packages\sqlalchemy\util\compat.py
replace time_func = time.clock
with time_func = time.perf_counter

Barista: https://localhost:8100/login-results#access_token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Im5abkRKUkhQUmhMZnY0US13Y3VyMyJ9.eyJpc3MiOiJodHRwczovL3Jvb2Z1c2VhdC51cy5hdXRoMC5jb20vIiwic3ViIjoiYXV0aDB8NWVmMzhiM2U3Nzk3YzEwMDEzNzAyMTNkIiwiYXVkIjoiY29mZmVlIiwiaWF0IjoxNTkzMDk5MDEyLCJleHAiOjE1OTMxODU0MTIsImF6cCI6ImRKUmVhSXJ4YlVibTBrRlJsaVZ5ME5Kc2lqbEtWUzBmIiwic2NvcGUiOiIiLCJwZXJtaXNzaW9ucyI6WyJnZXQ6ZHJpbmtzLWRldGFpbCJdfQ.0WaTJfxGbscyfLA3JaN_kimOvYNJMySm1a01mzIj9f2emdtQrpR_Xbb97ozXlb9iXMX21J7oMXtadGmodqp3elDPN8dT9Ai6yHTQ4qeMF39m7_UvI-xh5lkHc9K0Tdczn1gahPAekFdP91nR6MZ7rnqYobVNJk6A3xlbtY2dOCWtl6z-tD8RLPVvAYSpplVPySvh_0QUZRMTnBrRG--E4bQkKXerw2YP7ivZy6Q384Bscn3jSMLkOnf7dX4xxjVfAAXPMp7GfVHksecnx-xIR7Crt0HHY3Zy4X5u8kM42gRbp5XcWKIXCzUkL7DgKmEoHy5GMgsw-_mp68MCVhme0Q&expires_in=86400&token_type=Bearer

Manager: https://localhost:8100/login-results#access_token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Im5abkRKUkhQUmhMZnY0US13Y3VyMyJ9.eyJpc3MiOiJodHRwczovL3Jvb2Z1c2VhdC51cy5hdXRoMC5jb20vIiwic3ViIjoiYXV0aDB8NWVmMzhiNjI3Nzk3YzEwMDEzNzAyMTNmIiwiYXVkIjoiY29mZmVlIiwiaWF0IjoxNTkzMDk5MjcxLCJleHAiOjE1OTMxODU2NzEsImF6cCI6ImRKUmVhSXJ4YlVibTBrRlJsaVZ5ME5Kc2lqbEtWUzBmIiwic2NvcGUiOiIiLCJwZXJtaXNzaW9ucyI6WyJkZWxldGU6ZHJpbmtzIiwiZ2V0OmRyaW5rcy1kZXRhaWwiLCJwYXRjaDpkcmlua3MiLCJwb3N0OmRyaW5rcyJdfQ.AbQ_MhzW70f5aMJcgBNqHIZ4nz9DeN-fZUuOPDrW7HZJ6UdtUCK0QNlNZMzdxa9FnmrO6xCsIwpXidY1n5kyi921NGjlDxbRaCqABRPFBkKUYvMe4Z9e9otqQVBSLTH5CY-mlNwsuElHlPxmoPrk_VLLOQp25rpBgtBretPkqpKp1QXN2oFS4JupdFnwye8P35DHkAxNOdy9vuVgO-AxtRg90o7cD1XblrlD172Bfha8Zj7AcWvJoMjAjDkPlXcErl5bS45GH9bM-lqu0Pj5lMHBFtxmBnRaLFmpvDEYZeHNgQiZH8LzIXzZKKVIPOutPf20Z4GngZHLW6MgeeWBHQ&expires_in=86400&token_type=Bearer