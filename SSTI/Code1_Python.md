```python
from flask import Flask, request
from jinja2 import Environment

app = Flask(__name__)
Jinja2 = Environment()

@app.route("/email/unsubscribe")
def page():
    email = request.values.get('email')
    output = Jinja2.from_string('<h1>Are you sure you want to unsubscribe ' + email + '?</h1>' +
            '<button onclick="unsubscribeUser()" style="margin-right: 1rem;">Unsubscribe</button>' +
            '<a href="/">Cancel</a>').render()
    return output

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```
