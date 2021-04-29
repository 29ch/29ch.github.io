# Responder
A familiar HTTP Service Framework for Python

### install
```
$ pipenv install responder
```

### Declare a Web Service
```
import responder

api = responder.API()
```

### hello world
```
@api.route("/")
def hello_world(req, resp):
    resp.text = "hello, world!"
```

### Run the Server
```
api.run()
```

### routing
#### Accept Route Arguments
```
@api.route("/hello/{who}")
def hello_to(req, resp, *, who):
    resp.text = f"hello, {who}!"
```

#### Returning JSON / YAML
```
@api.route("/hello/{who}/json")
def hello_to_json(req, resp, *, who):
    resp.media = {"hello": who}
```

#### Rendering a Template
```
@api.route("/hello/{who}/html")
def hello_to_html(req, resp, *, who):
    resp.html = api.template('hello.html', who=who)
```
templates/hello.html
```
Hello, {{ who }}
```

```
from responder.templates import Templates
templates = Templates()

@api.route("/hello/{name}/html")
def hello(req, resp, name):
    resp.html = templates.render("hello.html", name=name)
```

```
from responder.templates import Templates
templates = Templates(enable_async=True)

resp.html = await templates.render_async("hello.html", name=name)
```

#### port, address, debug
```
api.run(port=8080, address="127.0.0.2", debug=True)
```

#### request method
```
@api.route("/check_method")
def check_method(req, resp):
    if req.method == "get":
        resp.text = "this is get"
    elif req.method == "post":
        resp.text = "this is post"
```

#### post data
```
@api.route("/take_post")
async def take_post(req, resp):
    if req.method == "get":
        resp.text = "please send post"
    else:
        data = await req.media()
        resp.media = data
        resp.text = "I'm giving you back :) "
```

#### Setting Response Status Code
```
@api.route("/416")
def teapot(req, resp)
    resp.status_code = api.status_codes.HTTP_416   # ...or 416
```

#### Setting Response Headers
```
@api.route("/pizza")
def pizza_pizza(req, resp):
    resp.headers['X-Pizza'] = '42'
```

### async
```
@api.route("/taking")
async def some_function(req, resp):

    @api.background.task
    def take_long_time():
        # 時間のかかる処理

    take_long_time()

    resp.media = {'success': True}
```

#### Receiving Data & Background Tasks
```
import time

@api.route("/incoming")
async def receive_incoming(req, resp):

    @api.background.task
    def process_data(data):
        """Just sleeps for three seconds, as a demo."""
        time.sleep(3)


    # Parse the incoming data as form-encoded.
    # Note: 'json' and 'yaml' formats are also automatically supported.
    data = await req.media()

    # Process the data (in the background).
    process_data(data)

    # Immediately respond that upload was successful.
    resp.media = {'success': True}
```
