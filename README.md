# Adventure Project Week

This week you'll be implementing a frontend interface for a multi-user
dungeon (MUD) game called _LambdaMUD_. The backend is partially written
but needs to be completed.

Using API requests, clients are able to create, read, update and delete
data on remote servers but what if the server needs to initiate a
request to the client? Say, to alert them that another player has
entered their room or that they have received a chat message. This is
where WebSockets come in.

WebSocket is a computer communications protocol, providing full-duplex
communication channels over a single TCP connection. You will be using
the Pusher service to handle the WebSocket connections for your project.
You can read more about WebSockets [here](https://pusher.com/websockets).

You are to treat this week as if you are working at a company and the
instructor is your client. The project managers will be your main
support throughout the week.

The main objective of this week is to develop the MVP feature set listed
below using any other technologies you have learned here at Lambda
School. There are design files in this repository you should use as a
creative guide.

## Git Commits

- You are required to showcase progress with at least 1 commit a day.
  This will let your project manager know where you are and if you need
  help. This also allows the client to get progress reports from the
  company in a real world setting.


Upon your first commit, please submit a Pull Request and add _both_ the
**Trello Set Up** and **MVP Features** Task lists to your first Pull
Request comment:

```markdown
## Trello Set Up:

- [ ] Create a Trello account if you don't have one already
- [ ] Create a new board called "LambdaMUD - {Your Name}"
- [ ] Create lists titled `backlog`,`To Do`, `In Progress`, and `Done`
- [ ] Fill in the `To Do` list with the MVP features listed below
- [ ] Fill in the `backlog` list with all the extra features listed below
- [ ] Share your board with the project manager that has been assigned to you. If you have not been assigned yet, reach out to your lead PM for guidance
- [ ] Add your Trello URL to your project's README.md file. Commit the change, push it to your repository & submit a pull request

## MVP Features:

#### Client
- [ ] Create a standalone frontend app that communicates with the server via API calls
- [ ] Be able to create a new account on the server (implemented on server)
- [ ] Be able to log in to the server (implemented on server)
- [ ] Create an interface that displays the current room name, its description and the other players in the room
- [ ] Be able to move between rooms and update the display accordingly (implemented on server)
- [ ] Be able to use a `say` command to say things that other people in the room will see (server implementation incomplete)
- [ ] Upon login, subscribe to a Pusher channel based on the player's id: `p-channel-<id>`
- [ ] Bind the player channel to `broadcast` events and display the messages to the player
- [ ] Alert the player when someone enters and leaves the current room (implemented on server)
- [ ] Alert the player when someone in the current room says something (server implementation incomplete)

#### Server
- [ ] Create a new API endpoint for `say` which broadcasts a message to other players in the current room

```

---

**Once you have completed the Minimum Viable Product requirements,
direct message your project manager for approval. If approved, you may
continue working on the Extra Features.**

Once your MVP has been approved, you have been given a feature list that
the client would love to have completed. Your goal would be to finish
MVP as soon as you can and get working the list of features.

## Extra Features:


- [ ] Add a `shout` command that broadcasts a message to every player
- [ ] Add a `whisper` command that sends a private message to a single player
- [ ] Disconnect inactive players
- [ ] Replace text-based movement with graphical navigation
- [ ] Expand the world map
- [ ] Add a search function to find the shortest path to another player (hint: BFS)
- [ ] Generate and display a map of the world
- [ ] Add the ability to pick up and drop objects
- [ ] Add NPCs that wander around the world (hint: look into [Celery](http://docs.celeryproject.org/en/latest/index.html))
- [ ] Add weapons
- [ ] Add combat with NPCs
- [ ] Add PvP combat


---

# Directions


## Set up a Pusher account
* Sign up for a free account on pusher.com
* Create a new app
* Take note of your credentials
  * app_id, key, secret, cluster
* Look through the provided sample code and documentation


## Set up your local server
* Set up your virtual environment
  * `pipenv --three`
  * `pipenv install`
  * `pipenv shell`

* Add your secret credentials
  * Create `.env` in the root directory of your project
  * Add your pusher credentials and secret key
    ```
    SECRET_KEY='<your_secret_key>'
    DEBUG=True
    PUSHER_APP_ID=<your_app_id>
    PUSHER_KEY=<your_pusher_key>
    PUSHER_SECRET=<your_secret_key>
    PUSHER_CLUSTER=<your_cluster>
    ```

* Run database migrations
  * `./manage.py makemigrations`
  * `./manage.py migrate`

* Add rooms to your database
  * `./manage.py shell`
  * Copy/paste the contents of `util/create_world.py` into the Python interpreter
  * Exit the interpreter

* Run the server
  * `./manage.py runserver`


## Run API commands
### Registration
* `curl -X POST -H "Content-Type: application/json" -d '{"username":"testuser", "password1":"testpassword", "password2":"testpassword"}' localhost:8000/api/registration/`
* Response: `{"key":"2330ee38f77a42e2ef3799c4e9783662987e2347"}`

### Login
* `curl -X POST -H "Content-Type: application/json" -d '{"username":"testuser", "password":"testpassword"}' localhost:8000/api/login/`
* Response: `{"key":"2330ee38f77a42e2ef3799c4e9783662987e2347"}`

### Initialize
* `curl -X GET -H 'Authorization: Token 2330ee38f77a42e2ef3799c4e9783662987e2347' localhost:8000/api/adv/init/`
  * NOTE: Use your own auth token here
* Response: `{"id": 1, "name": "testuser", "title": "Outside Cave Entrance", "description": "North of you, the cave mount beckons", "players": []}`

### Move
* `curl -X POST -H 'Authorization: Token 2330ee38f77a42e2ef3799c4e9783662987e2347' -H "Content-Type: application/json" -d '{"direction":"s"}' localhost:8000/api/adv/move/`
  * NOTE: Use your own auth token here
* Response: `{"name": "testuser", "title": "Foyer", "description": "Dim light filters in from the south. Dusty\npassages run north and east.", "players": [], "error_msg": ""}`

### Say (NOT YET IMPLEMENTED)
* `curl -X POST -H 'Authorization: Token 2330ee38f77a42e2ef3799c4e9783662987e2347' -H "Content-Type: application/json" -d '{"message":"Hello, world!"}' localhost:8000/api/adv/say/`
  * NOTE: Use your own auth token here
